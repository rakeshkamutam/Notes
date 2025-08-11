# Angular Signals - Beginner's Guide (Accurate Version)

## What are Signals?

Angular Signals are a **reactive state management system** inspired by SolidJS. They act as a **wrapper around a value** and automatically notify all linked components when that value changes.

### Key Benefits
- **Better Performance**: Change detection happens only at component level when signal changes, **without traversing the entire component tree**
- **Push-Pull Pattern**: `push` for setting/mutating values, `pull` for retrieving values
- **Reactive Programming**: Automatic notifications when dependencies change

---

## 1. Basic Writable Signals

### Creating and Reading Signals

```typescript
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <div>
      <!-- IMPORTANT: Use parentheses to read signal value -->
      <h2>Counter: {{ count() }}</h2>
      <button (click)="increment()">+</button>
      <button (click)="decrement()">-</button>
      <button (click)="reset()">Reset</button>
    </div>
  `
})
export class CounterComponent {
  // Signal MUST have an initial value
  count = signal(0);
  
  increment() {
    // set() method: Replace entire signal value
    this.count.set(this.count() + 1);
  }
  
  decrement() {
    // update() method: Modify based on current value
    this.count.update(currentValue => currentValue - 1);
  }
  
  reset() {
    this.count.set(0);
  }
}
```

### Working with Objects and Arrays

```typescript
import { Component, signal } from '@angular/core';

interface User {
  name: string;
  age: number;
  email: string;
}

@Component({
  selector: 'app-user-profile',
  template: `
    <div>
      <h2>User: {{ user().name }}</h2>
      <p>Age: {{ user().age }}</p>
      <p>Email: {{ user().email }}</p>
      
      <h3>Todo List ({{ todos().length }} items)</h3>
      <ul>
        <li *ngFor="let todo of todos()">{{ todo }}</li>
      </ul>
      
      <button (click)="updateAge()">Birthday!</button>
      <button (click)="addTodo()">Add Todo</button>
      <button (click)="replaceUser()">Replace User</button>
    </div>
  `
})
export class UserProfileComponent {
  // Signal with object - must have initial value
  user = signal<User>({
    name: 'John Doe',
    age: 25,
    email: 'john@example.com'
  });
  
  // Signal with array - must have initial value
  todos = signal<string[]>(['Learn Angular', 'Build an app']);
  
  updateAge() {
    // update() receives current value and returns new value
    this.user.update(currentUser => ({
      ...currentUser,
      age: currentUser.age + 1
    }));
  }
  
  addTodo() {
    // update() works with arrays too
    this.todos.update(currentTodos => [...currentTodos, `Task ${currentTodos.length + 1}`]);
  }
  
  replaceUser() {
    // set() replaces entire value
    this.user.set({
      name: 'Jane Smith',
      age: 30,
      email: 'jane@example.com'
    });
  }
}
```

---

## 2. Computed Signals (Read-Only)

**IMPORTANT**: Computed signals are **read-only** and **derive their value from other signals**. You **cannot** call `set()` or `update()` on them - this will cause a compilation error. The Computed signal can not be mutable like **writable signals.**.

```typescript
import { Component, signal, computed } from '@angular/core';

@Component({
  selector: 'app-shopping-cart',
  template: `
    <div>
      <h2>Shopping Cart</h2>
      
      <div *ngFor="let item of items()">
        <span>{{ item.name }} - ${{ item.price }} x {{ item.quantity }}</span>
        <button (click)="updateQuantity(item.id, item.quantity + 1)">+</button>
        <button (click)="updateQuantity(item.id, item.quantity - 1)">-</button>
      </div>
      
      <hr>
      <!-- All these are computed signals - read-only -->
      <p><strong>Total Items: {{ totalItems() }}</strong></p>
      <p><strong>Total Price: ${{ totalPrice() }}</strong></p>
      <p><strong>Average Price: ${{ averagePrice() }}</strong></p>
      <p><strong>Status: {{ cartStatus() }}</strong></p>
    </div>
  `
})
export class ShoppingCartComponent {
  // This is the only writable signal
  items = signal([
    { id: 1, name: 'Laptop', price: 999, quantity: 1 },
    { id: 2, name: 'Mouse', price: 25, quantity: 2 }
  ]);
  
  // Computed signals take a callback function and are READ-ONLY
  totalItems = computed(() => {
    return this.items().reduce((sum, item) => sum + item.quantity, 0);
  });
  
  // This computed signal depends on items signal
  totalPrice = computed(() => {
    return this.items().reduce((sum, item) => sum + (item.price * item.quantity), 0);
  });
  
  // This computed signal depends on other computed signals
  averagePrice = computed(() => {
    const total = this.totalPrice(); // Reading another computed signal
    const count = this.totalItems(); // Reading another computed signal
    return count > 0 ? (total / count).toFixed(2) : '0.00';
  });
  
  // Computed signals automatically update when dependencies change
  cartStatus = computed(() => {
    const total = this.totalPrice();
    if (total === 0) return 'Empty';
    if (total < 100) return 'Small order';
    if (total < 500) return 'Medium order';
    return 'Large order';
  });
  
  updateQuantity(id: number, newQuantity: number) {
    // Only the writable signal can be updated
    this.items.update(items => 
      items.map(item => 
        item.id === id 
          ? { ...item, quantity: Math.max(0, newQuantity) }
          : item
      )
    );
    // All computed signals will automatically update!
  }
  
  // ❌ This would cause a compilation error:
  // someMethod() {
  //   this.totalPrice.set(100); // ERROR! Computed signals are read-only
  // }
}
```

---

## 3. Effects - Side Operations

Effects are used for **side operations** when signals change - like logging, HTTP requests, DOM updates, or timers.

```typescript
import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-user-settings',
  template: `
    <div>
      <h2>User Settings</h2>
      
      <label>
        Theme:
        <select [value]="theme()" (change)="changeTheme($any($event.target).value)">
          <option value="light">Light</option>
          <option value="dark">Dark</option>
        </select>
      </label>
      
      <label>
        Language:
        <select [value]="language()" (change)="changeLanguage($any($event.target).value)">
          <option value="en">English</option>
          <option value="es">Spanish</option>
          <option value="fr">French</option>
        </select>
      </label>
      
      <p>Settings saved: {{ saveCount() }} times</p>
      <p>Check console for effect logs!</p>
    </div>
  `
})
export class UserSettingsComponent {
  theme = signal('light');
  language = signal('en');
  saveCount = signal(0);
  
  // Computed signal for settings object
  settings = computed(() => ({
    theme: this.theme(),
    language: this.language()
  }));
  
  constructor() {
    // Effect runs at least once and whenever tracked signals change
    effect(() => {
      console.log(`Theme changed to: ${this.theme()}`);
      // Side effect: Update DOM
      document.body.className = `theme-${this.theme()}`;
    });
    
    // Effect with multiple signal dependencies
    effect(() => {
      const currentSettings = this.settings();
      console.log('Settings changed:', currentSettings);
      
      // Side effect: Save to localStorage
      localStorage.setItem('userSettings', JSON.stringify(currentSettings));
      
      // Update save counter
      this.saveCount.update(count => count + 1);
    });
    
    // Effect with cleanup using onCleanup
    effect((onCleanup) => {
      const lang = this.language();
      console.log(`Loading translations for ${lang}`);
      
      // Simulate setting up a subscription or timer
      const timer = setInterval(() => {
        console.log(`Checking for ${lang} translation updates...`);
      }, 5000);
      
      // Cleanup function runs before next effect execution or on destroy
      onCleanup(() => {
        console.log(`Cleaning up timer for ${lang}`);
        clearInterval(timer);
      });
    });
  }
  
  changeTheme(newTheme: string) {
    this.theme.set(newTheme);
    // Effect will automatically run because theme signal changed
  }
  
  changeLanguage(newLanguage: string) {
    this.language.set(newLanguage);
    // Effects will automatically run because language signal changed
  }
}
```

---

## 4. Advanced Features

### Untracked Function - Non-Reactive Reading

Use `untracked()` to read signal values **without creating dependencies**.

```typescript
import { Component, signal, computed, effect, untracked } from '@angular/core';

@Component({
  selector: 'app-performance-demo',
  template: `
    <div>
      <p>Counter: {{ counter() }}</p>
      <p>Debug Mode: {{ debugMode() ? 'ON' : 'OFF' }}</p>
      <p>Expensive Result: {{ expensiveComputation() }}</p>
      
      <button (click)="increment()">Increment Counter</button>
      <button (click)="toggleDebug()">Toggle Debug</button>
    </div>
  `
})
export class PerformanceDemoComponent {
  counter = signal(0);
  debugMode = signal(false);
  
  // This computed only recalculates when counter changes
  // debugMode changes are ignored because of untracked()
  expensiveComputation = computed(() => {
    const value = this.counter(); // This creates a dependency
    
    // untracked() prevents debugMode from being a dependency
    const isDebug = untracked(() => this.debugMode());
    
    if (isDebug) {
      console.log('Performing expensive calculation...');
    }
    
    // Simulate expensive operation
    return value * value;
  });
  
  constructor() {
    // Effect to demonstrate untracked behavior
    effect(() => {
      console.log(`Counter is now: ${this.counter()}`);
      
      // Reading debugMode with untracked won't cause this effect to re-run
      const debug = untracked(() => this.debugMode());
      if (debug) {
        console.log('Debug info: Effect triggered by counter change');
      }
    });
  }
  
  increment() {
    this.counter.update(c => c + 1);
    // expensiveComputation will recalculate
    // effect will run
  }
  
  toggleDebug() {
    this.debugMode.update(d => !d);
    // expensiveComputation will NOT recalculate (because untracked)
    // effect will NOT run (because untracked)
  }
}
```

### Converting Observables to Signals with toSignal

```typescript
import { Component, inject, signal } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { toSignal } from '@angular/core/rxjs-interop';
import { interval } from 'rxjs';

interface Post {
  id: number;
  title: string;
  body: string;
}

@Component({
  selector: 'app-data-demo',
  template: `
    <div>
      <h2>Data from Observable</h2>
      
      <!-- toSignal creates a read-only signal -->
      <p>Current Time: {{ currentTime() }}</p>
      <p>Posts Count: {{ posts()?.length || 0 }}</p>
      
      <div *ngIf="posts()">
        <h3>First Post:</h3>
        <p>{{ posts()![0]?.title }}</p>
      </div>
      
      <button (click)="manualRefresh()">Manual Refresh</button>
    </div>
  `
})
export class DataDemoComponent {
  private http = inject(HttpClient);
  
  // Convert interval observable to signal
  private time$ = interval(1000);
  currentTime = toSignal(this.time$, { initialValue: 0 });
  
  // Convert HTTP observable to signal
  private posts$ = this.http.get<Post[]>('https://jsonplaceholder.typicode.com/posts');
  posts = toSignal(this.posts$); // No initial value - will be undefined initially
  
  // For manual operations, use regular writable signals
  refreshCount = signal(0);
  
  manualRefresh() {
    // Note: toSignal creates read-only signals
    // You cannot do: this.posts.set(newData) 
    // For manual updates, you need separate writable signals
    this.refreshCount.update(count => count + 1);
    console.log(`Refreshed ${this.refreshCount()} times`);
  }
}
```

---

## 5. Key Rules and Best Practices

### Signal Reading Rules

```typescript
// ✅ CORRECT: Always use parentheses to read signal value
const value = this.mySignal();
const userName = this.user().name;

// ❌ WRONG: Without parentheses, you get the signal function
const wrong = this.mySignal; // This is the signal function, not the value
```

### Writable vs Computed Signal Rules

```typescript
export class ExampleComponent {
  // ✅ Writable signal - can use set() and update()
  counter = signal(0);
  
  // ✅ Computed signal - READ-ONLY, takes callback function
  doubled = computed(() => this.counter() * 2);
  
  someMethod() {
    // ✅ ALLOWED: Writable signals
    this.counter.set(10);
    this.counter.update(current => current + 1);
    
    // ❌ COMPILATION ERROR: Computed signals are read-only
    // this.doubled.set(20); // ERROR!
    // this.doubled.update(current => current + 1); // ERROR!
    
    // ✅ ALLOWED: Reading any signal
    console.log(this.counter()); // Works
    console.log(this.doubled()); // Works
  }
}
```

### Effect Rules

```typescript
export class EffectExampleComponent {
  data = signal('initial');
  
  constructor() {
    // ✅ CORRECT: Use effects for side operations
    effect(() => {
      console.log('Data changed:', this.data());
      // Side effects: logging, HTTP calls, DOM updates, etc.
      this.saveToLocalStorage(this.data());
    });
    
    // ❌ WRONG: Don't use effects to compute values
    // Use computed signals instead
    effect(() => {
      // Don't do this for computing values!
      const computed = this.data().toUpperCase();
      // ...
    });
  }
  
  private saveToLocalStorage(value: string) {
    localStorage.setItem('data', value);
  }
}
```

### Signal Initialization Rules

```typescript
// ✅ CORRECT: All signals must have initial values
counter = signal(0);
user = signal({ name: '', age: 0 });
items = signal<string[]>([]);
loading = signal(false);

// ❌ WRONG: Signals cannot be undefined initially
// counter = signal(); // ERROR!
```

---

## Summary

**Key Concepts:**

1. **Writable Signals**: Created with `signal()`, can use `set()` and `update()`
2. **Computed Signals**: Created with `computed()`, take callback functions, are **READ-ONLY**
3. **Effects**: Created with `effect()`, used for **side operations**, run asynchronously
4. **Signal Reading**: Always use parentheses `mySignal()` to get the value
5. **Performance**: Only components using changed signals re-render
6. **Untracked**: Use `untracked()` to read without creating dependencies
7. **Observable Integration**: Use `toSignal()` to convert observables (creates read-only signals)

**Essential Rules:**
- Signals must have initial values
- Use parentheses to read signal values
- Computed signals are read-only
- Effects are for side operations, not computations
- `untracked()` prevents dependency tracking



##  also need to learn toSingal() & untracked() 