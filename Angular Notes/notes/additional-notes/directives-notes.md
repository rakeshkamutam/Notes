# Angular Directives - Complete Guide

## 📘 Overview

**Directives** are instructions in the DOM that tell Angular how to manipulate elements. They are TypeScript classes decorated with `@Directive` and can be used as attribute selectors, CSS classes, or HTML elements.

### Types of Directives

- **🔹 Structural Directives**: Modify DOM layout by adding/removing elements
  - Examples: `*ngIf`, `*ngFor`, `*ngSwitch`
- **🔹 Attribute Directives**: Change element appearance or behavior
  - Examples: `ngStyle`, `ngClass`, `ngModel`

---

## 1. `*ngIf` - Conditional Rendering

### Basic Example

```typescript
// app.component.ts
export class AppComponent {
  isLoggedIn = true;
  showMessage = false;
}
```

```html
<!-- app.component.html -->
<div *ngIf="isLoggedIn">
  <p>Welcome back, user!</p>
</div>

<div *ngIf="!isLoggedIn">
  <p>Please log in to continue.</p>
</div>
```

### With Else Block

```html
<div *ngIf="isLoggedIn; else loginBlock">
  <p>Welcome back!</p>
</div>

<ng-template #loginBlock>
  <p>Please log in.</p>
</ng-template>
```

### Key Points
- **Purpose**: Shows/hides DOM elements based on conditions
- **Syntax**: `*ngIf="condition"`
- **Structural**: Yes - modifies DOM structure
- **Performance**: Elements are completely removed from DOM when condition is false

---

## 2. `*ngFor` - Loop Through Data

### Basic Example

```typescript
// products.component.ts
export class ProductsComponent {
  numbers = [1, 3, 5, 17, 20];
  products = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Phone', price: 699 },
    { id: 3, name: 'Tablet', price: 399 }
  ];
}
```

```html
<!-- Simple array iteration -->
<div *ngFor="let number of numbers">
  <p>Number is {{ number }}</p>
</div>

<!-- Object array iteration -->
<div *ngFor="let product of products">
  <h3>{{ product.name }}</h3>
  <p>Price: ${{ product.price }}</p>
</div>
```

### With Index

```html
<div *ngFor="let number of numbers; let i = index">
  <p>{{ i + 1 }}. Number is {{ number }}</p>
</div>
```

### Advanced Features

```html
<div *ngFor="let item of items; let i = index; let first = first; let last = last; let even = even; let odd = odd">
  <p [ngClass]="{ 'first-item': first, 'last-item': last, 'even-item': even, 'odd-item': odd }">
    {{ i + 1 }}. {{ item }}
  </p>
</div>
```

### Key Points
- **Purpose**: Loop over arrays/iterables to generate HTML
- **Syntax**: `*ngFor="let item of items"`
- **Index Access**: `let i = index`
- **Performance**: Use `trackBy` for large lists
- **Updates**: DOM updates automatically when data changes

---

## 3. `ngStyle` - Dynamic Inline Styles

### Basic Example

```typescript
// app.component.ts
export class AppComponent {
  isError = true;
  fontSize = 18;
  backgroundColor = '#f0f0f0';
}
```

```html
<!-- Single style -->
<p [ngStyle]="{ 'color': isError ? 'red' : 'green' }">
  Status message
</p>

<!-- Multiple styles -->
<div [ngStyle]="{ 
  'font-size': fontSize + 'px',
  'background-color': backgroundColor,
  'padding': '10px',
  'border-radius': '5px'
}">
  Styled content
</div>
```

### Using Component Properties

```typescript
export class AppComponent {
  getStyles() {
    return {
      'color': this.isError ? 'red' : 'green',
      'font-weight': this.isError ? 'bold' : 'normal',
      'font-size': '16px'
    };
  }
}
```

```html
<p [ngStyle]="getStyles()">Dynamic styles from method</p>
```

### Key Points
- **Purpose**: Apply inline CSS styles dynamically
- **Syntax**: `[ngStyle]="{ 'property': value }"`
- **Type**: Attribute directive
- **Flexibility**: Can use expressions, methods, or objects

---

## 4. `ngClass` - Dynamic CSS Classes

### Basic Example

```typescript
// app.component.ts
export class AppComponent {
  isActive = true;
  isError = false;
  userType = 'admin';
}
```

```html
<!-- Object syntax -->
<div [ngClass]="{ 
  'active': isActive, 
  'error': isError, 
  'admin-user': userType === 'admin' 
}">
  Dynamic classes
</div>

<!-- String syntax -->
<div [ngClass]="'btn btn-primary'">
  Static classes
</div>

<!-- Array syntax -->
<div [ngClass]="['btn', 'btn-large', isActive ? 'active' : '']">
  Array of classes
</div>
```

### Using Component Methods

```typescript
export class AppComponent {
  getClasses() {
    return {
      'text-success': this.isSuccess,
      'text-danger': this.isError,
      'font-bold': this.isBold
    };
  }
}
```

```html
<p [ngClass]="getClasses()">Text with dynamic classes</p>
```

### Key Points
- **Purpose**: Add/remove CSS classes dynamically
- **Syntax**: `[ngClass]="{ 'className': condition }"`
- **Multiple formats**: Object, string, or array
- **Type**: Attribute directive

---

## 5. `*ngSwitch` - Multiple Conditions

### Example

```typescript
// app.component.ts
export class AppComponent {
  viewMode = 'list'; // 'list', 'grid', 'card'
}
```

```html
<div [ngSwitch]="viewMode">
  <div *ngSwitchCase="'list'">
    <p>List View</p>
  </div>
  <div *ngSwitchCase="'grid'">
    <p>Grid View</p>
  </div>
  <div *ngSwitchCase="'card'">
    <p>Card View</p>
  </div>
  <div *ngSwitchDefault>
    <p>Default View</p>
  </div>
</div>
```

---

## 📊 Quick Reference Table

| Directive | Type | Purpose | Example |
|-----------|------|---------|---------|
| `*ngIf` | Structural | Conditional rendering | `<div *ngIf="condition">Content</div>` |
| `*ngFor` | Structural | Loop through data | `<div *ngFor="let item of items">{{ item }}</div>` |
| `*ngSwitch` | Structural | Multiple conditions | `<div [ngSwitch]="value">` |
| `ngStyle` | Attribute | Dynamic inline styles | `<p [ngStyle]="{ 'color': 'red' }">Text</p>` |
| `ngClass` | Attribute | Dynamic CSS classes | `<p [ngClass]="{ 'active': isActive }">Text</p>` |

---

## 🎯 Best Practices

1. **Use `*ngIf` for conditional rendering** instead of hiding with CSS
2. **Implement `trackBy` for large `*ngFor` lists** to improve performance
3. **Prefer `ngClass` over `ngStyle`** when possible for better maintainability
4. **Use `*ngSwitch` for multiple conditions** instead of chaining `*ngIf`
5. **Keep directive expressions simple** - move complex logic to component methods

---

## 🔧 Custom Directive Example

```typescript
// highlight.directive.ts
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() highlightColor = 'yellow';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

```html
<!-- Usage -->
<p appHighlight highlightColor="lightblue">Hover over me!</p>
```