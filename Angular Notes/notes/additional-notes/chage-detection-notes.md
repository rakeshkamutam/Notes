# Angular Change Detection - Comprehensive Guide

## Table of Contents
1. [Fundamentals](#fundamentals)
2. [How Change Detection Works](#how-change-detection-works)
3. [Change Detection Strategies](#change-detection-strategies)
4. [Performance Optimization Techniques](#performance-optimization-techniques)
5. [Advanced Topics](#advanced-topics)
6. [Zone-less Angular](#zone-less-angular)
7. [Best Practices](#best-practices)
8. [Common Pitfalls](#common-pitfalls)

---

## Fundamentals

### What is Change Detection?

Change detection is Angular's core mechanism that keeps your user interface synchronized with your application's data model. It's an automated process that runs behind the scenes, detecting when data changes and updating the DOM accordingly.

**Key Characteristics:**
- **Automatic Process**: Works without direct developer interaction
- **UI Synchronization**: Ensures views reflect current data state
- **Event-Driven**: Triggered by user interactions, HTTP requests, timers, and promises
- **Tree Traversal**: Checks components from root to leaves in a depth-first manner

### The Change Detection Cycle

Angular's change detection follows a predictable cycle:

1. **Trigger Event**: User interaction, async operation, or manual trigger
2. **Zone Notification**: Zone.js notifies Angular of potential changes
3. **Tree Traversal**: Angular walks the component tree from root to leaf
4. **Expression Evaluation**: Checks all bindings and expressions
5. **DOM Update**: Updates the DOM where values have changed
6. **Lifecycle Hooks**: Calls relevant lifecycle hooks (ngAfterViewChecked, etc.)

---

## How Change Detection Works

### Zone.js Integration

**Zone.js** is Angular's monkey-patching library that automatically detects asynchronous operations:

```typescript
// Zone.js patches these APIs:
- setTimeout/setInterval
- Promise.then()
- DOM events
- XMLHttpRequest
- WebSocket
```

**How Zone.js Works:**
- Wraps asynchronous APIs to create "zones"
- Provides hooks for when async operations start/complete
- Notifies Angular when potential state changes occur
- Only triggers when there are active event listeners

### Default Change Detection Strategy

Angular uses the **`ChangeDetectionStrategy.Default`** (also called `CheckAlways`) by default:

```typescript
@Component({
  selector: 'app-example',
  template: `
    <div>{{ expensiveCalculation() }}</div>
    <div>{{ user.name }}</div>
  `,
  changeDetection: ChangeDetectionStrategy.Default // This is the default
})
export class ExampleComponent {
  // This getter runs on EVERY change detection cycle
  get expensiveCalculation() {
    console.log('Expensive calculation running...');
    return Math.random() * 1000;
  }
}
```

**Characteristics of Default Strategy:**
- Checks **all** components on every change detection cycle
- Re-evaluates **all** template expressions and bindings
- Can be performance-intensive in large applications
- Provides maximum reactivity but at a cost

---

## Change Detection Strategies

### OnPush Strategy

The **`ChangeDetectionStrategy.OnPush`** strategy significantly improves performance by limiting when change detection runs:

```typescript
@Component({
  selector: 'app-optimized',
  template: `
    <div>{{ message }}</div>
    <button (click)="updateMessage()">Update</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  @Input() data: any;
  message = 'Hello';

  updateMessage() {
    this.message = 'Updated!';
  }
}
```

**OnPush Triggers Change Detection When:**
1. **Input Properties Change**: Any `@Input()` receives a new reference
2. **Local Events**: Events triggered within the component
3. **Manual Detection**: Using `ChangeDetectorRef.markForCheck()`
4. **Observable Emissions**: When using `async` pipe
5. **Signal Updates**: When signal values change (Angular 16+)

### Input Reference Changes

**Important**: OnPush only detects reference changes, not property mutations:

```typescript
// ❌ This WON'T trigger change detection with OnPush
updateUserMutation() {
  this.user.name = 'New Name'; // Same object reference
}

// ✅ This WILL trigger change detection with OnPush
updateUserImmutable() {
  this.user = { ...this.user, name: 'New Name' }; // New object reference
}
```

---

## Performance Optimization Techniques

### 1. Template Expression Optimization

**Avoid Heavy Computations in Templates:**

```typescript
// ❌ Bad: Heavy computation in template
@Component({
  template: `<div>{{ calculateExpensiveValue() }}</div>`
})
export class BadComponent {
  calculateExpensiveValue() {
    // This runs on every change detection cycle!
    return this.data.reduce((sum, item) => sum + item.value, 0);
  }
}

// ✅ Good: Compute once and cache
@Component({
  template: `<div>{{ cachedValue }}</div>`
})
export class GoodComponent implements OnInit {
  cachedValue: number;

  ngOnInit() {
    this.cachedValue = this.calculateExpensiveValue();
  }

  private calculateExpensiveValue() {
    return this.data.reduce((sum, item) => sum + item.value, 0);
  }
}
```

### 2. NgZone.runOutsideAngular()

Prevent unnecessary change detection for operations that don't affect the UI:

```typescript
import { NgZone } from '@angular/core';

@Component({...})
export class ComponentWithTimers {
  constructor(private ngZone: NgZone) {}

  startPolling() {
    // ❌ This triggers change detection every second
    setInterval(() => {
      console.log('Polling...');
    }, 1000);

    // ✅ This doesn't trigger change detection
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => {
        console.log('Polling outside Angular zone...');
      }, 1000);
    });
  }

  // If you need to update UI from outside zone
  updateUI() {
    this.ngZone.run(() => {
      // Code here will trigger change detection
      this.message = 'Updated from outside zone';
    });
  }
}
```

### 3. TrackBy Functions for ngFor

Optimize list rendering with trackBy functions:

```typescript
@Component({
  template: `
    <div *ngFor="let item of items; trackBy: trackByFn">
      {{ item.name }}
    </div>
  `
})
export class ListComponent {
  items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' }
  ];

  // ✅ Angular can track items by ID instead of recreating DOM elements
  trackByFn(index: number, item: any) {
    return item.id; // or index if no unique identifier
  }
}
```

### 4. Immutable Data Patterns

Use immutable updates for better OnPush performance:

```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ImmutableComponent {
  items: Item[] = [];

  // ✅ Immutable update - creates new array reference
  addItem(newItem: Item) {
    this.items = [...this.items, newItem];
  }

  // ✅ Immutable update for nested objects
  updateItem(id: number, updates: Partial<Item>) {
    this.items = this.items.map(item => 
      item.id === id ? { ...item, ...updates } : item
    );
  }

  // ❌ Mutates existing array - OnPush won't detect
  addItemMutable(newItem: Item) {
    this.items.push(newItem);
  }
}
```

---

## Advanced Topics

### Manual Change Detection with ChangeDetectorRef

```typescript
import { ChangeDetectorRef, ChangeDetectionStrategy } from '@angular/core';

@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ManualDetectionComponent implements OnInit {
  data: any[] = [];

  constructor(private cdr: ChangeDetectorRef, private dataService: DataService) {}

  ngOnInit() {
    // Method 1: markForCheck() - marks component and ancestors for checking
    this.dataService.getData().subscribe(data => {
      this.data = data;
      this.cdr.markForCheck(); // Schedules change detection
    });

    // Method 2: detectChanges() - immediately runs change detection
    this.loadCriticalData().then(data => {
      this.data = data;
      this.cdr.detectChanges(); // Runs immediately
    });
  }

  // Method 3: detach/reattach for fine-grained control
  toggleDetection() {
    this.cdr.detach(); // Stops automatic change detection
    
    setTimeout(() => {
      this.cdr.reattach(); // Resumes automatic change detection
    }, 5000);
  }
}
```

### Working with Observables and OnPush

**Using Async Pipe (Recommended):**

```typescript
@Component({
  template: `
    <div *ngFor="let message of messages$ | async">
      {{ message.text }}
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class AsyncComponent {
  messages$ = this.messageService.getMessages();

  constructor(private messageService: MessageService) {}
}
```

**Manual Subscription with ChangeDetectorRef:**

```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ManualSubscriptionComponent implements OnInit, OnDestroy {
  messages: Message[] = [];
  private subscription = new Subscription();

  constructor(
    private messageService: MessageService,
    private cdr: ChangeDetectorRef
  ) {}

  ngOnInit() {
    this.subscription.add(
      this.messageService.getMessages().subscribe(messages => {
        this.messages = messages;
        this.cdr.markForCheck(); // Required for OnPush
      })
    );
  }

  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}
```

### Signals and Change Detection (Angular 16+)

```typescript
import { Component, computed, signal } from '@angular/core';

@Component({
  template: `
    <div>Count: {{ count() }}</div>
    <div>Double: {{ doubleCount() }}</div>
    <button (click)="increment()">Increment</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class SignalComponent {
  // Signals automatically trigger change detection
  count = signal(0);
  
  // Computed signals update when dependencies change
  doubleCount = computed(() => this.count() * 2);

  increment() {
    this.count.update(value => value + 1);
    // No need for markForCheck() - signals handle this automatically
  }
}
```

---

## Zone-less Angular

### Setting Up Zone-less Angular

**1. Remove Zone.js from angular.json:**
```json
{
  "projects": {
    "your-app": {
      "architect": {
        "build": {
          "options": {
            "polyfills": [
              // Remove "zone.js" from this array
            ]
          }
        }
      }
    }
  }
}
```

**2. Configure in app.config.ts:**
```typescript
import { provideExperimentalZonelessChangeDetection } from '@angular/core';

export const appConfig: ApplicationConfig = {
  providers: [
    provideExperimentalZonelessChangeDetection(),
    // ... other providers
  ]
};
```

### Zone-less Behavior

**What Still Works:**
- Signal updates
- Event bindings in templates
- Manual `ChangeDetectorRef` calls
- `async` pipe

**What Doesn't Work:**
- Automatic detection of `setTimeout`, `Promise.then()`, etc.
- Third-party library events (unless they use Angular's event system)

**Zone-less Example:**
```typescript
@Component({
  template: `
    <div>{{ count() }}</div>
    <button (click)="increment()">Increment</button>
    <button (click)="delayedIncrement()">Delayed Increment</button>
  `
})
export class ZonelessComponent {
  count = signal(0);

  constructor(private cdr: ChangeDetectorRef) {}

  increment() {
    this.count.update(c => c + 1); // ✅ Works - signals trigger detection
  }

  delayedIncrement() {
    setTimeout(() => {
      this.count.update(c => c + 1); // ❌ Won't update UI automatically
      this.cdr.markForCheck(); // ✅ Manual trigger needed
    }, 1000);
  }
}
```

---

## Best Practices

### 1. Choose the Right Strategy
- **Default Strategy**: Small to medium apps, rapid prototyping
- **OnPush Strategy**: Large apps, performance-critical components
- **Zone-less**: Maximum performance, full control over change detection

### 2. Component Design Patterns
- **Smart/Dumb Component Pattern**: Use OnPush for presentation components
- **Immutable Data Flow**: Always create new object references for OnPush
- **Signal-First**: Prefer signals over traditional reactive patterns (Angular 16+)

### 3. Performance Monitoring
```typescript
import { ApplicationRef } from '@angular/core';

@Injectable()
export class ChangeDetectionProfiler {
  constructor(private appRef: ApplicationRef) {
    // Monitor change detection cycles
    this.appRef.isStable.subscribe(stable => {
      if (stable) {
        console.log('Change detection cycle completed');
      }
    });
  }
}
```

### 4. Testing Change Detection
```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { ChangeDetectionStrategy } from '@angular/core';

describe('OnPushComponent', () => {
  let component: OnPushComponent;
  let fixture: ComponentFixture<OnPushComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [OnPushComponent]
    });
    fixture = TestBed.createComponent(OnPushComponent);
    component = fixture.componentInstance;
  });

  it('should update when input changes', () => {
    component.data = { value: 'initial' };
    fixture.detectChanges();
    
    component.data = { value: 'updated' };
    fixture.detectChanges(); // Manually trigger change detection
    
    expect(fixture.nativeElement.textContent).toContain('updated');
  });
});
```

---

## Common Pitfalls

### 1. ExpressionChangedAfterItHasBeenCheckedError

```typescript
// ❌ This causes the error in development mode
@Component({
  template: `<div>{{ randomValue }}</div>`
})
export class ProblematicComponent {
  get randomValue() {
    return Math.random(); // Different value each time!
  }
}

// ✅ Fix: Store the value
@Component({
  template: `<div>{{ randomValue }}</div>`
})
export class FixedComponent implements OnInit {
  randomValue: number;

  ngOnInit() {
    this.randomValue = Math.random();
  }
}
```

### 2. OnPush with Mutable Objects

```typescript
// ❌ OnPush won't detect this change
@Component({
  template: `<div>{{ user.name }}</div>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MutableComponent {
  @Input() user: User;

  updateUser() {
    this.user.name = 'New Name'; // Same object reference
  }
}

// ✅ OnPush will detect this change
updateUser() {
  this.user = { ...this.user, name: 'New Name' }; // New object reference
}
```

### 3. Memory Leaks with Manual Subscriptions

```typescript
// ❌ Memory leak - subscription not cleaned up
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class LeakyComponent implements OnInit {
  ngOnInit() {
    this.dataService.getData().subscribe(data => {
      this.data = data;
      this.cdr.markForCheck();
    }); // Subscription never unsubscribed!
  }
}

// ✅ Proper cleanup
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class CleanComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit() {
    this.dataService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        this.data = data;
        this.cdr.markForCheck();
      });
  }

  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

### 4. Overusing markForCheck()

```typescript
// ❌ Overusing manual change detection
updateData() {
  this.data = newData;
  this.cdr.markForCheck(); // Unnecessary if using OnPush correctly
  this.cdr.detectChanges(); // Avoid unless absolutely necessary
}

// ✅ Let Angular handle it naturally
updateData() {
  this.data = newData; // OnPush will detect the change automatically
}
```

---

## Summary

Understanding Angular's change detection is crucial for building performant applications. Key takeaways:

1. **Start with Default strategy** for simplicity, migrate to OnPush for performance
2. **Use immutable data patterns** with OnPush strategy
3. **Leverage Zone.runOutsideAngular()** for non-UI operations
4. **Prefer async pipe** over manual subscriptions
5. **Consider Zone-less Angular** for maximum performance control
6. **Monitor and profile** your application's change detection cycles
7. **Test your change detection** strategy thoroughly

Remember: Premature optimization is the root of all evil. Profile your application first, then optimize where needed.