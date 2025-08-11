# Angular 13 Content Projection & @ContentChild Guide

## Table of Contents
1. [Overview](#overview)
2. [Basic @ContentChild Usage](#basic-contentchild-usage)
3. [Multiple Component Queries](#multiple-component-queries)
4. [Template Reference Variables](#template-reference-variables)
5. [Safe Usage Patterns](#safe-usage-patterns)
6. [Advanced Examples](#advanced-examples)
7. [Best Practices](#best-practices)

## Overview

Content projection in Angular allows parent components to pass content into child components using `<ng-content>`. The `@ContentChild` and `@ContentChildren` decorators enable you to query and interact with projected content programmatically.

### Key Concepts
- **Content Projection**: Using `<ng-content>` to accept external content
- **@ContentChild**: Query for a single projected component/element
- **@ContentChildren**: Query for multiple projected components/elements
- **Lifecycle**: Use `ngAfterContentInit()` to access projected content

## Basic @ContentChild Usage

### Step 1: Create a Projectable Component

```typescript
// special-button.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-special-button',
  template: `<button class="special-btn">Click Me!</button>`,
  styles: [`
    .special-btn {
      background: #007bff;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class SpecialButtonComponent {
  sayHello() {
    console.log('Hello from Special Button!');
  }
  
  getText() {
    return 'Special Button Text';
  }
}
```

### Step 2: Create a Container Component with Content Projection

```typescript
// my-card.component.ts
import { Component, ContentChild, AfterContentInit } from '@angular/core';
import { SpecialButtonComponent } from './special-button.component';

@Component({
  selector: 'app-my-card',
  template: `
    <div class="card">
      <div class="card-header">
        <h3>{{ title }}</h3>
      </div>
      <div class="card-body">
        <ng-content></ng-content>
      </div>
      <div class="card-footer" *ngIf="hasProjectedButton">
        <small>Button projected: {{ buttonText }}</small>
      </div>
    </div>
  `,
  styles: [`
    .card {
      border: 1px solid #ddd;
      border-radius: 8px;
      margin: 16px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .card-header {
      background: #f8f9fa;
      padding: 16px;
      border-bottom: 1px solid #ddd;
    }
    .card-body {
      padding: 16px;
    }
    .card-footer {
      background: #f8f9fa;
      padding: 8px 16px;
      border-top: 1px solid #ddd;
      font-size: 0.8em;
      color: #666;
    }
  `]
})
export class MyCardComponent implements AfterContentInit {
  @ContentChild(SpecialButtonComponent) specialButton!: SpecialButtonComponent;
  
  title = 'My Card Component';
  hasProjectedButton = false;
  buttonText = '';

  ngAfterContentInit() {
    if (this.specialButton) {
      this.hasProjectedButton = true;
      this.buttonText = this.specialButton.getText();
      this.specialButton.sayHello();
    }
  }
}
```

### Step 3: Usage in Parent Component

```typescript
// app.component.html
<app-my-card>
  <app-special-button></app-special-button>
  <p>This is additional projected content!</p>
</app-my-card>
```

## Multiple Component Queries

### Creating Multiple Button Types

```typescript
// special-button-a.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-special-button-a',
  template: `<button class="special-btn-a">Button A</button>`,
  styles: [`
    .special-btn-a {
      background: #28a745;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class SpecialButtonAComponent {
  sayHi() {
    console.log('Hi from Special Button A!');
  }
  
  getType() {
    return 'Button Type A';
  }
}

// special-button-b.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-special-button-b',
  template: `<button class="special-btn-b">Button B</button>`,
  styles: [`
    .special-btn-b {
      background: #dc3545;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class SpecialButtonBComponent {
  sayGoodbye() {
    console.log('Goodbye from Special Button B!');
  }
  
  getType() {
    return 'Button Type B';
  }
}
```

### Querying Multiple Component Types

```typescript
// enhanced-card.component.ts
import { Component, ContentChild, AfterContentInit } from '@angular/core';
import { SpecialButtonComponent } from './special-button.component';
import { SpecialButtonAComponent } from './special-button-a.component';
import { SpecialButtonBComponent } from './special-button-b.component';

@Component({
  selector: 'app-enhanced-card',
  template: `
    <div class="enhanced-card">
      <h3>Enhanced Card with Multiple Queries</h3>
      <div class="button-info">
        <p *ngIf="hasSpecialButton">✅ Special Button found</p>
        <p *ngIf="hasSpecialButtonA">✅ Special Button A found</p>
        <p *ngIf="hasSpecialButtonB">✅ Special Button B found</p>
        <p *ngIf="!hasAnyButton">❌ No special buttons found</p>
      </div>
      <div class="content">
        <ng-content></ng-content>
      </div>
    </div>
  `,
  styles: [`
    .enhanced-card {
      border: 2px solid #007bff;
      border-radius: 8px;
      padding: 16px;
      margin: 16px;
    }
    .button-info {
      background: #f8f9fa;
      padding: 8px;
      border-radius: 4px;
      margin-bottom: 16px;
    }
    .content {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }
  `]
})
export class EnhancedCardComponent implements AfterContentInit {
  @ContentChild(SpecialButtonComponent) specialButton?: SpecialButtonComponent;
  @ContentChild(SpecialButtonAComponent) specialButtonA?: SpecialButtonAComponent;
  @ContentChild(SpecialButtonBComponent) specialButtonB?: SpecialButtonBComponent;
//   Since there's no <app-special-button-a> projected, Angular finds nothing.
//   this.specialButtona will be undefined, and calling any method on it will throw an error.
//   (need to handle null properly)

  hasSpecialButton = false;
  hasSpecialButtonA = false;
  hasSpecialButtonB = false;
  hasAnyButton = false;

  ngAfterContentInit() {
    this.hasSpecialButton = !!this.specialButton;
    this.hasSpecialButtonA = !!this.specialButtonA;
    this.hasSpecialButtonB = !!this.specialButtonB;
    this.hasAnyButton = this.hasSpecialButton || this.hasSpecialButtonA || this.hasSpecialButtonB;

    // Safely call methods on each button type
    this.specialButton?.sayHello();
    this.specialButtonA?.sayHi();
    this.specialButtonB?.sayGoodbye();
  }
}
```

### Usage Example

```html
<!-- app.component.html -->

<!-- Example 1: Only Special Button -->
<app-enhanced-card>
  <app-special-button></app-special-button>
</app-enhanced-card>

<!-- Example 2: Multiple Button Types -->
<app-enhanced-card>
  <app-special-button></app-special-button>
  <app-special-button-a></app-special-button-a>
  <app-special-button-b></app-special-button-b>
</app-enhanced-card>

<!-- Example 3: Mixed Content -->
<app-enhanced-card>
  <app-special-button-a></app-special-button-a>
  <p>Some text content</p>
  <span>More content</span>
</app-enhanced-card>
```

## Template Reference Variables

### Using Template References with @ContentChild

```typescript
// ref-card.component.ts
import { Component, ContentChild, AfterContentInit, ElementRef } from '@angular/core';
import { SpecialButtonComponent } from './special-button.component';

@Component({
  selector: 'app-ref-card',
  template: `
    <div class="ref-card">
      <h3>Template Reference Example</h3>
      <div class="ref-info">
        <p>Button Element: {{ buttonElementInfo }}</p>
        <p>Button Component: {{ buttonComponentInfo }}</p>
      </div>
      <ng-content></ng-content>
      <button (click)="interactWithProjectedButton()">
        Interact with Projected Button
      </button>
    </div>
  `,
  styles: [`
    .ref-card {
      border: 2px solid #28a745;
      border-radius: 8px;
      padding: 16px;
      margin: 16px;
    }
    .ref-info {
      background: #f8f9fa;
      padding: 8px;
      border-radius: 4px;
      margin-bottom: 16px;
    }
  `]
})
export class RefCardComponent implements AfterContentInit {
  // Query by component type
  @ContentChild(SpecialButtonComponent) buttonComponent?: SpecialButtonComponent;
  
  // Query by template reference variable
  @ContentChild('myButton') buttonElement?: ElementRef<HTMLElement>;
  @ContentChild('mySpecialBtn') specialButtonRef?: SpecialButtonComponent;

  buttonElementInfo = 'Not found';
  buttonComponentInfo = 'Not found';

  ngAfterContentInit() {
    if (this.buttonComponent) {
      this.buttonComponentInfo = 'Found via component type';
    }

    if (this.buttonElement) {
      this.buttonElementInfo = `Found: ${this.buttonElement.nativeElement.tagName}`;
    }

    if (this.specialButtonRef) {
      console.log('Found special button via template ref');
    }
  }

  interactWithProjectedButton() {
    // Interact with component
    this.buttonComponent?.sayHello();
    
    // Interact with element
    if (this.buttonElement) {
      this.buttonElement.nativeElement.style.backgroundColor = '#ff6b6b';
    }
  }
}
```

### Usage with Template References

```html
<!-- Using template references -->
<app-ref-card>
  <app-special-button #mySpecialBtn></app-special-button>
  <button #myButton>Regular Button</button>
  <p>Some other content</p>
</app-ref-card>
```

## Safe Usage Patterns

### Defensive Programming with Projected Content

```typescript
// safe-card.component.ts
import { Component, ContentChild, AfterContentInit, OnDestroy } from '@angular/core';
import { SpecialButtonComponent } from './special-button.component';

@Component({
  selector: 'app-safe-card',
  template: `
    <div class="safe-card">
      <h3>Safe Usage Patterns</h3>
      <div class="status" [ngClass]="statusClass">
        {{ statusMessage }}
      </div>
      <ng-content></ng-content>
      <div class="actions" *ngIf="hasValidButton">
        <button (click)="safeInteraction()">Safe Interaction</button>
        <button (click)="chainedOperations()">Chained Operations</button>
      </div>
    </div>
  `,
  styles: [`
    .safe-card {
      border: 2px solid #ffc107;
      border-radius: 8px;
      padding: 16px;
      margin: 16px;
    }
    .status {
      padding: 8px;
      border-radius: 4px;
      margin-bottom: 16px;
    }
    .status.success { background: #d4edda; color: #155724; }
    .status.warning { background: #fff3cd; color: #856404; }
    .status.error { background: #f8d7da; color: #721c24; }
    .actions {
      margin-top: 16px;
      display: flex;
      gap: 8px;
    }
  `]
})
export class SafeCardComponent implements AfterContentInit, OnDestroy {
  @ContentChild(SpecialButtonComponent) specialButton?: SpecialButtonComponent;

  hasValidButton = false;
  statusMessage = 'Checking projected content...';
  statusClass = 'warning';

  ngAfterContentInit() {
    this.validateProjectedContent();
  }

  ngOnDestroy() {
    // Clean up any references if needed
    this.specialButton = undefined;
  }

  private validateProjectedContent() {
    if (this.isValidButton(this.specialButton)) {
      this.hasValidButton = true;
      this.statusMessage = 'Valid button component found';
      this.statusClass = 'success';
    } else {
      this.hasValidButton = false;
      this.statusMessage = 'No valid button component found';
      this.statusClass = 'error';
    }
  }

  private isValidButton(button: any): button is SpecialButtonComponent {
    return button && 
           typeof button.sayHello === 'function' && 
           typeof button.getText === 'function';
  }

  safeInteraction() {
    // Always check before using
    if (this.specialButton) {
      try {
        this.specialButton.sayHello();
      } catch (error) {
        console.error('Error interacting with button:', error);
      }
    }
  }

  chainedOperations() {
    // Safe chaining with optional chaining operator
    const text = this.specialButton?.getText?.() ?? 'Default text';
    console.log('Button text:', text);

    // Multiple safe operations
    if (this.specialButton) {
      this.specialButton.sayHello();
      const buttonText = this.specialButton.getText();
      console.log('Retrieved text:', buttonText);
    }
  }
}
```

## Advanced Examples

### Using @ContentChildren for Multiple Elements

```typescript
// multi-button-card.component.ts
import { Component, ContentChildren, QueryList, AfterContentInit } from '@angular/core';
import { SpecialButtonComponent } from './special-button.component';

@Component({
  selector: 'app-multi-button-card',
  template: `
    <div class="multi-button-card">
      <h3>Multiple Buttons Card</h3>
      <div class="stats">
        <p>Total buttons found: {{ buttonCount }}</p>
        <p>Active buttons: {{ activeButtonCount }}</p>
      </div>
      <ng-content></ng-content>
      <div class="controls" *ngIf="buttonCount > 0">
        <button (click)="greetAllButtons()">Greet All</button>
        <button (click)="getButtonTexts()">Get All Texts</button>
      </div>
    </div>
  `,
  styles: [`
    .multi-button-card {
      border: 2px solid #6f42c1;
      border-radius: 8px;
      padding: 16px;
      margin: 16px;
    }
    .stats {
      background: #f8f9fa;
      padding: 8px;
      border-radius: 4px;
      margin-bottom: 16px;
    }
    .controls {
      margin-top: 16px;
      display: flex;
      gap: 8px;
    }
  `]
})
export class MultiButtonCardComponent implements AfterContentInit {
  @ContentChildren(SpecialButtonComponent) buttons!: QueryList<SpecialButtonComponent>;

  buttonCount = 0;
  activeButtonCount = 0;

  ngAfterContentInit() {
    this.updateButtonStats();
    
    // Listen for changes in projected content
    this.buttons.changes.subscribe(() => {
      this.updateButtonStats();
    });
  }

  private updateButtonStats() {
    this.buttonCount = this.buttons.length;
    this.activeButtonCount = this.buttons.filter(btn => btn).length;
  }

  greetAllButtons() {
    this.buttons.forEach((button, index) => {
      if (button) {
        console.log(`Button ${index + 1}:`);
        button.sayHello();
      }
    });
  }

  getButtonTexts() {
    const texts = this.buttons.map((button, index) => 
      button ? button.getText() : `Button ${index + 1} unavailable`
    );
    console.log('All button texts:', texts);
    return texts;
  }
}
```

### Usage with Multiple Buttons

```html
<app-multi-button-card>
  <app-special-button></app-special-button>
  <app-special-button></app-special-button>
  <app-special-button></app-special-button>
  <p>Some text between buttons</p>
  <app-special-button></app-special-button>
</app-multi-button-card>
```

## Best Practices

### 1. Always Use Lifecycle Hooks Correctly
```typescript
// ✅ Good - Use ngAfterContentInit
ngAfterContentInit() {
  if (this.projectedComponent) {
    this.projectedComponent.initialize();
  }
}

// ❌ Bad - Don't use ngOnInit
ngOnInit() {
  // Projected content not available yet!
  this.projectedComponent.initialize(); // Will fail
}
```

### 2. Implement Safe Guards
```typescript
// ✅ Good - Safe checking
if (this.projectedComponent && typeof this.projectedComponent.method === 'function') {
  this.projectedComponent.method();
}

// ✅ Good - Using optional chaining (Angular 12+)
this.projectedComponent?.method?.();

// ❌ Bad - No safety check
this.projectedComponent.method(); // Might throw error
```

### 3. Handle Dynamic Content Changes
```typescript
// ✅ Good - Listen for changes
ngAfterContentInit() {
  this.contentChildren.changes.subscribe(changes => {
    this.handleContentChanges(changes);
  });
}
```

### 4. Use Proper TypeScript Types
```typescript
// ✅ Good - Proper typing
@ContentChild(MyComponent) myComponent?: MyComponent;

// ✅ Good - Explicit undefined handling
@ContentChild(MyComponent) myComponent: MyComponent | undefined;

// ❌ Bad - Using non-null assertion without checking
@ContentChild(MyComponent) myComponent!: MyComponent;
```

### 5. Component Module Configuration

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { CommonModule } from '@angular/common';

import { AppComponent } from './app.component';
import { MyCardComponent } from './my-card.component';
import { SpecialButtonComponent } from './special-button.component';
import { SpecialButtonAComponent } from './special-button-a.component';
import { SpecialButtonBComponent } from './special-button-b.component';
import { EnhancedCardComponent } from './enhanced-card.component';
import { RefCardComponent } from './ref-card.component';
import { SafeCardComponent } from './safe-card.component';
import { MultiButtonCardComponent } from './multi-button-card.component';

@NgModule({
  declarations: [
    AppComponent,
    MyCardComponent,
    SpecialButtonComponent,
    SpecialButtonAComponent,
    SpecialButtonBComponent,
    EnhancedCardComponent,
    RefCardComponent,
    SafeCardComponent,
    MultiButtonCardComponent
  ],
  imports: [
    BrowserModule,
    CommonModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Summary

### Key Takeaways

| Concept | Usage | When to Use |
|---------|-------|-------------|
| `@ContentChild(ComponentType)` | Query single component by type | When you need one specific component |
| `@ContentChild('templateRef')` | Query by template reference | When you need to access DOM element |
| `@ContentChildren(ComponentType)` | Query multiple components | When you need to work with arrays of components |
| `ngAfterContentInit()` | Access projected content | Always use this lifecycle hook for content queries |
| Optional chaining (`?.`) | Safe property access | When component might not be projected |
| Type guards | Runtime type checking | When you need to ensure component exists and has expected methods |

### Common Pitfalls to Avoid

1. **Don't use `ngOnInit()`** - Projected content isn't available yet
2. **Don't assume content exists** - Always check before using
3. **Don't forget about dynamic changes** - Listen to `QueryList.changes` when needed
4. **Don't mix query strategies** - Be consistent with component type vs. template ref queries
5. **Don't ignore TypeScript warnings** - Use proper typing for better development experience

This guide covers the essential patterns for working with content projection in Angular 13. The examples progress from basic usage to advanced scenarios, ensuring you have a solid foundation for implementing content projection in your applications.