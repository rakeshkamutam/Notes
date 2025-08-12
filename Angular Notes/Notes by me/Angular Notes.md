
# Angular Complete Notes

## Table of Contents

- [1. Introduction to Angular](#1-introduction-to-angular)
- [2. TypeScript Fundamentals](#2-typescript-fundamentals)
- [3. Angular CLI](#3-angular-cli)
- [4. Project Structure](#4-project-structure)
- [5. Components](#5-components)
- [6. Templates and Data Binding](#6-templates-and-data-binding)
- [7. Directives](#7-directives)
- [8. Pipes](#8-pipes)
- [9. Services and Dependency Injection](#9-services-and-dependency-injection)
- [10. Routing](#10-routing)
- [11. Forms](#11-forms)
- [12. HTTP Client and Observables](#12-http-client-and-observables)
- [13. Component Communication](#13-component-communication)
- [14. Lifecycle Hooks](#14-lifecycle-hooks)
- [15. Change Detection](#15-change-detection)
- [16. Modules](#16-modules)
- [17. State Management](#17-state-management)
- [18. Testing](#18-testing)
- [19. Performance Optimization](#19-performance-optimization)
- [20. Security](#20-security)
- [21. Animations](#21-animations)
- [22. Progressive Web Apps (PWA)](#22-progressive-web-apps-pwa)
- [23. Server-Side Rendering (SSR)](#23-server-side-rendering-ssr)
- [24. Deployment](#24-deployment)
- [25. Best Practices](#25-best-practices)

***

## 1. Introduction to Angular

### **Definition:**

Angular is a TypeScript-based, open-source web application framework developed and maintained by Google. It's designed for building scalable single-page applications (SPAs) and provides a comprehensive solution for frontend development with a component-based architecture.

### **Why We Need Angular:**

- **Rapid Development**: Provides pre-built components and tools for faster development
- **Scalability**: Component-based architecture makes it easy to scale large applications
- **Maintainability**: Clear structure and separation of concerns improve code maintainability
- **Enterprise-Ready**: Backed by Google with long-term support and regular updates
- **Full-Featured**: Includes routing, forms, HTTP client, testing tools out of the box
- **TypeScript Integration**: Static typing reduces runtime errors and improves development experience
- **Community Support**: Large ecosystem with extensive documentation and third-party libraries


### **Key Features:**

- **Component-based architecture** - Modular and reusable components
- **Two-way data binding** - Automatic synchronization between model and view
- **Dependency injection** - Built-in DI system for better modularity
- **Routing** - Client-side navigation
- **TypeScript support** - Static typing and modern JavaScript features
- **Testing utilities** - Comprehensive testing tools
- **Cross-platform** - Web, mobile, and desktop applications


### **Architecture Overview:**

```
Application
├── Modules
│   ├── Components
│   ├── Services
│   ├── Directives
│   └── Pipes
└── Router
```


### **Angular Versions:**

- **AngularJS (1.x)** - JavaScript-based framework
- **Angular 2+** - Complete rewrite with TypeScript
- **Latest Version** - Angular 16+ (with standalone components, signals)


### **Real-World Usage:**

Angular is used by major companies like Google, Microsoft, IBM, Samsung, and Deutsche Bank for building:

- Enterprise web applications
- E-commerce platforms
- Content management systems
- Progressive Web Apps (PWAs)
- Admin dashboards and business tools

[↑ Back to Top](#table-of-contents)

***

## 2. TypeScript Fundamentals

### **Definition:**

TypeScript is a superset of JavaScript that adds static typing, classes, interfaces, and other object-oriented programming features. It compiles to plain JavaScript and is essential for Angular development.

### **Why We Need TypeScript in Angular:**

- **Static Typing**: Catch errors at compile time rather than runtime
- **Better IDE Support**: Excellent autocomplete, refactoring, and navigation
- **Code Documentation**: Types serve as documentation for your code
- **Refactoring Safety**: Rename variables and methods across files with confidence
- **Object-Oriented Features**: Classes, interfaces, inheritance, and access modifiers
- **Modern JavaScript Features**: Support for latest ECMAScript features
- **Large Team Collaboration**: Type definitions help teams understand code contracts


### **Usage in Angular:**

TypeScript is mandatory in Angular. Every component, service, and module is written in TypeScript.

### **Basic Types:**

```typescript
// Primitive types
let message: string = "Hello Angular";
let count: number = 42;
let isActive: boolean = true;
let data: any = { id: 1, name: "John" };

// Arrays
let numbers: number[] = [1, 2, 3, 4, 5];
let names: Array<string> = ["John", "Jane", "Bob"];

// Tuple
let person: [string, number] = ["John", 30];

// Enum
enum Color {
  Red,
  Green,
  Blue
}
let selectedColor: Color = Color.Red;

// Union types
let id: string | number = 123;

// Null and undefined
let nullable: string | null = null;
let optional: string | undefined = undefined;
```


### **Interfaces:**

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  isActive?: boolean; // Optional property
  readonly createdAt: Date; // Read-only property
}

// Extending interfaces
interface Admin extends User {
  permissions: string[];
}

// Function interface
interface SearchFunction {
  (source: string, subString: string): boolean;
}
```


### **Classes:**

```typescript
class UserService {
  private users: User[] = [];
  
  constructor(private apiUrl: string) {}
  
  public addUser(user: User): void {
    this.users.push(user);
  }
  
  public getUsers(): User[] {
    return this.users;
  }
  
  protected validateUser(user: User): boolean {
    return user.email.includes('@');
  }
}

// Inheritance
class AdminService extends UserService {
  constructor(apiUrl: string) {
    super(apiUrl);
  }
  
  public promoteUser(userId: number): void {
    // Implementation
  }
}
```


### **Generics:**

```typescript
function identity<T>(arg: T): T {
  return arg;
}

class DataService<T> {
  private items: T[] = [];
  
  add(item: T): void {
    this.items.push(item);
  }
  
  getAll(): T[] {
    return this.items;
  }
}

const userService = new DataService<User>();
```


### **Decorators:**

```typescript
// Class decorator
@Component({
  selector: 'app-user',
  template: '<h1>User Component</h1>'
})
class UserComponent {}

// Property decorator
class MyClass {
  @Input() name: string = '';
  @Output() nameChange = new EventEmitter<string>();
}

// Method decorator
class ApiService {
  @Get('/users')
  getUsers() {
    // Implementation
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 3. Angular CLI

### **Definition:**

Angular CLI (Command Line Interface) is a powerful command-line tool that helps developers create, build, test, and deploy Angular applications. It automates many development tasks and follows Angular best practices.

### **Why We Need Angular CLI:**

- **Project Scaffolding**: Quickly creates new projects with proper structure
- **Code Generation**: Generates components, services, modules with proper boilerplate
- **Build Optimization**: Handles complex webpack configurations automatically
- **Development Server**: Provides hot reloading during development
- **Testing Integration**: Sets up unit and e2e testing environments
- **Best Practices**: Enforces Angular coding standards and conventions
- **Productivity**: Reduces setup time and eliminates repetitive tasks


### **Usage Scenarios:**

- Creating new Angular projects
- Generating application building blocks (components, services, etc.)
- Running development server with live reload
- Building production-ready applications
- Running tests and generating code coverage reports
- Adding third-party libraries with proper configuration


### **Installation:**

```bash
# Install globally
npm install -g @angular/cli

# Check version
ng version

# Update CLI
npm update -g @angular/cli
```


### **Creating a New Project:**

```bash
# Basic project
ng new my-app

# With routing
ng new my-app --routing

# With specific stylesheet format
ng new my-app --style=scss

# Skip npm install
ng new my-app --skip-install

# All options
ng new my-app --routing --style=scss --skip-tests --package-manager=yarn
```


### **Development Server:**

```bash
# Start development server
ng serve

# Custom port
ng serve --port 4200

# Open in browser
ng serve --open

# Production mode
ng serve --prod
```


### **Generating Code:**

```bash
# Components
ng generate component user-list
ng g c user-detail --skip-tests

# Services
ng generate service user
ng g s data --skip-tests

# Modules
ng generate module shared
ng g m feature --routing

# Directives
ng generate directive highlight
ng g d custom-validator

# Pipes
ng generate pipe currency
ng g p date-format

# Guards
ng generate guard auth
ng g g admin --implements CanActivate

# Interfaces
ng generate interface user
ng g i api-response

# Classes
ng generate class user-model
ng g class utilities
```


### **Building:**

```bash
# Development build
ng build

# Production build
ng build --prod

# Build with specific configuration
ng build --configuration=staging

# Analyze bundle size
ng build --stats-json
npx webpack-bundle-analyzer dist/my-app/stats.json
```


### **Testing:**

```bash
# Unit tests
ng test

# Run tests once
ng test --watch=false

# Code coverage
ng test --code-coverage

# E2E tests
ng e2e
```


### **Workspace Commands:**

```bash
# Add library to workspace
ng generate library my-lib

# Add application to workspace
ng generate application my-app

# Add PWA support
ng add @angular/pwa

# Add Angular Material
ng add @angular/material
```

[↑ Back to Top](#table-of-contents)

***

## 4. Project Structure

### **Definition:**

Angular project structure is the organized arrangement of files and folders in an Angular application. It follows a hierarchical, modular approach that separates concerns and promotes maintainability.

### **Why We Need Proper Project Structure:**

- **Maintainability**: Easy to locate and modify code
- **Scalability**: Structure grows logically with application size
- **Team Collaboration**: Consistent organization helps team members navigate codebase
- **Code Reusability**: Clear separation makes components and services reusable
- **Testing**: Well-organized structure makes testing easier
- **Build Optimization**: Proper structure enables better tree-shaking and lazy loading
- **Best Practices**: Follows Angular style guide and community standards


### **Usage Guidelines:**

- Place shared components in a shared module
- Keep feature-specific code in feature modules
- Use core module for singleton services
- Organize assets by type and feature
- Follow consistent naming conventions


### **Standard Project Structure:**

```
my-angular-app/
├── e2e/                          # End-to-end tests
├── node_modules/                 # Dependencies
├── src/                          # Source code
│   ├── app/                      # Application code
│   │   ├── components/           # Components
│   │   ├── services/            # Services
│   │   ├── models/              # TypeScript interfaces/classes
│   │   ├── guards/              # Route guards
│   │   ├── pipes/               # Custom pipes
│   │   ├── directives/          # Custom directives
│   │   ├── modules/             # Feature modules
│   │   ├── shared/              # Shared components/services
│   │   ├── core/                # Singleton services
│   │   ├── app.component.html   # Root component template
│   │   ├── app.component.scss   # Root component styles
│   │   ├── app.component.ts     # Root component class
│   │   ├── app.module.ts        # Root module
│   │   └── app-routing.module.ts # Routing configuration
│   ├── assets/                  # Static assets
│   ├── environments/            # Environment configurations
│   ├── styles.scss              # Global styles
│   ├── index.html              # Main HTML file
│   ├── main.ts                 # Application bootstrap
│   └── polyfills.ts            # Browser compatibility
├── angular.json                 # Angular CLI configuration
├── package.json                # Dependencies and scripts
├── tsconfig.json               # TypeScript configuration
└── README.md                   # Project documentation
```


### **App Module Structure:**

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { CoreModule } from './core/core.module';
import { SharedModule } from './shared/shared.module';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    FormsModule,
    ReactiveFormsModule,
    AppRoutingModule,
    CoreModule,
    SharedModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```


### **Feature Module Structure:**

```typescript
// user/user.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';

import { UserListComponent } from './user-list/user-list.component';
import { UserDetailComponent } from './user-detail/user-detail.component';
import { UserService } from './user.service';

const routes = [
  { path: '', component: UserListComponent },
  { path: ':id', component: UserDetailComponent }
];

@NgModule({
  declarations: [
    UserListComponent,
    UserDetailComponent
  ],
  imports: [
    CommonModule,
    RouterModule.forChild(routes)
  ],
  providers: [UserService]
})
export class UserModule { }
```


### **Core Module (Singleton Services):**

```typescript
// core/core.module.ts
import { NgModule, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';

import { AuthService } from './auth.service';
import { LoggerService } from './logger.service';
import { ApiService } from './api.service';

@NgModule({
  declarations: [],
  imports: [CommonModule],
  providers: [
    AuthService,
    LoggerService,
    ApiService
  ]
})
export class CoreModule {
  constructor(@Optional() @SkipSelf() parentModule: CoreModule) {
    if (parentModule) {
      throw new Error('CoreModule is already loaded. Import only once.');
    }
  }
}
```


### **Shared Module (Reusable Components):**

```typescript
// shared/shared.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { LoadingComponent } from './loading/loading.component';
import { ConfirmDialogComponent } from './confirm-dialog/confirm-dialog.component';
import { HighlightPipe } from './pipes/highlight.pipe';

@NgModule({
  declarations: [
    LoadingComponent,
    ConfirmDialogComponent,
    HighlightPipe
  ],
  imports: [
    CommonModule,
    FormsModule,
    ReactiveFormsModule
  ],
  exports: [
    CommonModule,
    FormsModule,
    ReactiveFormsModule,
    LoadingComponent,
    ConfirmDialogComponent,
    HighlightPipe
  ]
})
export class SharedModule { }
```


### **Environment Configuration:**

```typescript
// environments/environment.ts (development)
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000/api',
  appName: 'My App (Dev)',
  enableLogging: true
};

// environments/environment.prod.ts (production)
export const environment = {
  production: true,
  apiUrl: 'https://api.myapp.com',
  appName: 'My App',
  enableLogging: false
};
```

[↑ Back to Top](#table-of-contents)

***

## 5. Components

### **Definition:**

Components are the fundamental building blocks of Angular applications. Each component controls a patch of the screen called a view and consists of a TypeScript class, HTML template, and CSS styles. Components encapsulate data, HTML, and logic for a specific part of the user interface.

### **Why We Need Components:**

- **Reusability**: Write once, use multiple times across the application
- **Encapsulation**: Each component has its own scope and styling
- **Maintainability**: Easier to debug and maintain smaller, focused components
- **Testability**: Individual components can be unit tested in isolation
- **Organization**: Break complex UI into manageable pieces
- **Separation of Concerns**: Logic, template, and styles are clearly separated
- **Composition**: Complex UIs built by combining simple components


### **Usage Scenarios:**

- Creating reusable UI elements (buttons, cards, modals)
- Building page layouts and views
- Implementing business logic for specific features
- Creating form controls and input elements
- Displaying data lists and tables


### **Component Basics:**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-user',
  template: `
    <div class="user-card">
      <h2>{{user.name}}</h2>
      <p>{{user.email}}</p>
    </div>
  `,
  styles: [`
    .user-card {
      border: 1px solid #ccc;
      padding: 16px;
      margin: 8px;
    }
  `]
})
export class UserComponent {
  user = {
    name: 'John Doe',
    email: 'john@example.com'
  };
}
```


### **Component with External Files:**

```typescript
@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.scss']
})
export class UserComponent {
  // Component logic here
}
```


### **Component Properties and Methods:**

```typescript
export class UserComponent {
  // Properties
  title: string = 'User Management';
  users: User[] = [];
  selectedUser: User | null = null;
  isLoading: boolean = false;
  
  // Constructor
  constructor(private userService: UserService) {}
  
  // Methods
  loadUsers(): void {
    this.isLoading = true;
    this.userService.getUsers().subscribe(
      users => {
        this.users = users;
        this.isLoading = false;
      },
      error => {
        console.error('Error loading users:', error);
        this.isLoading = false;
      }
    );
  }
  
  selectUser(user: User): void {
    this.selectedUser = user;
  }
  
  deleteUser(id: number): void {
    this.userService.deleteUser(id).subscribe(
      () => {
        this.users = this.users.filter(u => u.id !== id);
      }
    );
  }
}
```


### **Input and Output Properties:**

```typescript
// Parent Component
@Component({
  selector: 'app-parent',
  template: `
    <app-child 
      [user]="selectedUser"
      [isEditable]="true"
      (userUpdated)="onUserUpdated($event)"
      (userDeleted)="onUserDeleted($event)">
    </app-child>
  `
})
export class ParentComponent {
  selectedUser: User = { id: 1, name: 'John', email: 'john@example.com' };
  
  onUserUpdated(user: User): void {
    console.log('User updated:', user);
  }
  
  onUserDeleted(userId: number): void {
    console.log('User deleted:', userId);
  }
}

// Child Component
@Component({
  selector: 'app-child',
  template: `
    <div *ngIf="user">
      <h3>{{user.name}}</h3>
      <button *ngIf="isEditable" (click)="editUser()">Edit</button>
      <button (click)="deleteUser()">Delete</button>
    </div>
  `
})
export class ChildComponent {
  @Input() user!: User;
  @Input() isEditable: boolean = false;
  
  @Output() userUpdated = new EventEmitter<User>();
  @Output() userDeleted = new EventEmitter<number>();
  
  editUser(): void {
    // Edit logic
    const updatedUser = { ...this.user, name: 'Updated Name' };
    this.userUpdated.emit(updatedUser);
  }
  
  deleteUser(): void {
    this.userDeleted.emit(this.user.id);
  }
}
```


### **ViewChild and ViewChildren:**

```typescript
@Component({
  template: `
    <input #nameInput type="text" placeholder="Name">
    <app-user-card #userCard [user]="user"></app-user-card>
    <app-user-card #userCard [user]="user2"></app-user-card>
  `
})
export class ParentComponent implements AfterViewInit {
  @ViewChild('nameInput') nameInput!: ElementRef;
  @ViewChild('userCard') userCard!: UserCardComponent;
  @ViewChildren('userCard') userCards!: QueryList<UserCardComponent>;
  
  ngAfterViewInit(): void {
    // Access child components after view initialization
    this.nameInput.nativeElement.focus();
    console.log('User cards:', this.userCards.toArray());
  }
}
```


### **Content Projection:**

```typescript
// Container Component
@Component({
  selector: 'app-card',
  template: `
    <div class="card">
      <div class="card-header">
        <ng-content select="[slot=header]"></ng-content>
      </div>
      <div class="card-body">
        <ng-content></ng-content>
      </div>
      <div class="card-footer">
        <ng-content select="[slot=footer]"></ng-content>
      </div>
    </div>
  `
})
export class CardComponent {}

// Usage
@Component({
  template: `
    <app-card>
      <h2 slot="header">Card Title</h2>
      <p>This is the main content of the card.</p>
      <button slot="footer">Action</button>
    </app-card>
  `
})
export class AppComponent {}
```


### **Dynamic Components:**

```typescript
@Component({
  template: `
    <div #dynamicComponent></div>
    <button (click)="loadComponent()">Load Component</button>
  `
})
export class DynamicHostComponent {
  @ViewChild('dynamicComponent', { read: ViewContainerRef }) 
  dynamicComponent!: ViewContainerRef;
  
  constructor(private componentFactoryResolver: ComponentFactoryResolver) {}
  
  loadComponent(): void {
    const componentFactory = this.componentFactoryResolver
      .resolveComponentFactory(UserComponent);
    
    this.dynamicComponent.clear();
    const componentRef = this.dynamicComponent.createComponent(componentFactory);
    componentRef.instance.user = { id: 1, name: 'Dynamic User' };
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 6. Templates and Data Binding

### **Definition:**

Angular templates are HTML files with Angular-specific markup that define how components are rendered. Data binding is the mechanism that connects the component's data with the template, enabling dynamic content and interactive user interfaces.

### **Why We Need Templates and Data Binding:**

- **Dynamic Content**: Display data that changes over time
- **User Interaction**: Respond to user events and input
- **Reactive UIs**: Automatically update views when data changes
- **Code Separation**: Keep HTML structure separate from business logic
- **Declarative Approach**: Describe what the UI should look like, not how to manipulate it
- **Two-Way Synchronization**: Keep model and view in sync automatically
- **Expression Evaluation**: Execute TypeScript expressions within templates


### **Usage Scenarios:**

- Displaying dynamic content from component properties
- Handling user events (clicks, input changes, form submissions)
- Conditional rendering based on component state
- Styling elements based on component data
- Creating interactive forms and controls


### **Types of Data Binding:**

#### **1. Interpolation (One-way: Component → Template):**

```html
<!-- String interpolation -->
<h1>{{title}}</h1>
<p>Welcome, {{user.name}}!</p>
<div>Total: {{price * quantity}}</div>
<span>{{getCurrentDate()}}</span>

<!-- With expressions -->
<p>{{user.name || 'Guest'}}</p>
<div>{{isLoggedIn ? 'Logout' : 'Login'}}</div>
```


#### **2. Property Binding (One-way: Component → Template):**

```html
<!-- Element properties -->
<img [src]="imageUrl" [alt]="imageAlt">
<button [disabled]="isLoading">Submit</button>
<div [textContent]="message"></div>
<input [value]="username">

<!-- Class binding -->
<div [class]="cssClass"></div>
<div [class.active]="isActive"></div>
<div [ngClass]="{'active': isActive, 'disabled': isDisabled}"></div>

<!-- Style binding -->
<div [style.color]="textColor"></div>
<div [style.font-size.px]="fontSize"></div>
<div [ngStyle]="{'color': textColor, 'font-size': fontSize + 'px'}"></div>

<!-- Attribute binding -->
<button [attr.aria-label]="buttonLabel">Click me</button>
<td [attr.colspan]="colSpanValue">Cell</td>
```


#### **3. Event Binding (One-way: Template → Component):**

```html
<!-- Basic event binding -->
<button (click)="onClick()">Click me</button>
<input (keyup)="onKeyUp($event)">
<form (submit)="onSubmit($event)">

<!-- Event with parameters -->
<button (click)="onEdit(user.id)">Edit</button>
<li *ngFor="let item of items" (click)="selectItem(item)">{{item.name}}</li>

<!-- Template reference variables -->
<input #nameInput (keyup)="onSearch(nameInput.value)">
<button (click)="focusInput(nameInput)">Focus Input</button>
```


#### **4. Two-Way Data Binding:**

```html
<!-- Using ngModel -->
<input [(ngModel)]="username" placeholder="Username">
<textarea [(ngModel)]="description"></textarea>
<select [(ngModel)]="selectedOption">
  <option *ngFor="let option of options" [value]="option.value">
    {{option.label}}
  </option>
</select>

<!-- Custom two-way binding -->
<app-custom-input [(value)]="inputValue"></app-custom-input>
```


### **Template Reference Variables:**

```html
<!-- Element reference -->
<input #phoneInput placeholder="Phone">
<button (click)="callPhone(phoneInput.value)">Call</button>

<!-- Component reference -->
<app-user-form #userForm></app-user-form>
<button (click)="userForm.save()">Save</button>

<!-- Directive reference -->
<form #form="ngForm">
  <input name="email" ngModel #email="ngModel" required>
  <div *ngIf="email.invalid && email.touched">Email is required</div>
</form>
```


### **Safe Navigation Operator:**

```html
<!-- Prevents errors when object is null/undefined -->
<p>{{user?.name}}</p>
<p>{{user?.address?.street}}</p>
<img [src]="user?.profile?.avatar">

<!-- Non-null assertion operator -->
<p>{{user!.name}}</p> <!-- Only use if you're sure user exists -->
```


### **Template Expressions:**

```typescript
// Component
export class TemplateComponent {
  title = 'My App';
  user = { name: 'John', age: 30 };
  items = [1, 2, 3, 4, 5];
  isVisible = true;
  
  getMessage(): string {
    return `Hello, ${this.user.name}!`;
  }
  
  getSum(): number {
    return this.items.reduce((sum, item) => sum + item, 0);
  }
}
```

```html
<!-- Template expressions -->
<h1>{{title.toUpperCase()}}</h1>
<p>Age next year: {{user.age + 1}}</p>
<div>{{getMessage()}}</div>
<span>Sum: {{getSum()}}</span>
<p>{{items.length > 0 ? 'Items available' : 'No items'}}</p>

<!-- Avoid complex expressions in templates -->
<!-- Good -->
<div>{{displayMessage}}</div>

<!-- Better to use methods or computed properties -->
<div>{{getDisplayMessage()}}</div>
```


### **Pipes in Templates:**

```html
<!-- Built-in pipes -->
<p>{{message | uppercase}}</p>
<p>{{price | currency:'USD':'symbol':'1.2-2'}}</p>
<p>{{today | date:'fullDate'}}</p>
<p>{{items | json}}</p>

<!-- Chaining pipes -->
<p>{{message | uppercase | slice:0:10}}</p>

<!-- Pipes with parameters -->
<p>{{longText | slice:0:100}}</p>
<p>{{users | slice:0:5}}</p>
```

[↑ Back to Top](#table-of-contents)

***

## 7. Directives

### **Definition:**

Directives are classes that add behavior to elements in Angular templates. They are instructions in the DOM that tell Angular how to transform or manipulate elements. There are three types: components (directives with templates), structural directives (change DOM layout), and attribute directives (change element appearance or behavior).

### **Why We Need Directives:**

- **DOM Manipulation**: Safely modify DOM elements without direct DOM access
- **Reusable Behavior**: Apply same functionality across multiple elements
- **Conditional Rendering**: Show/hide elements based on conditions
- **Dynamic Styling**: Change element appearance dynamically
- **Custom Functionality**: Extend HTML with custom behaviors
- **Template Logic**: Add logic to templates in a declarative way
- **Code Organization**: Keep template logic separate from component logic


### **Usage Scenarios:**

- Showing/hiding content based on conditions (*ngIf)
- Iterating over data collections (*ngFor)
- Conditional styling based on component state
- Creating reusable UI behaviors (tooltips, dropdowns)
- Form validation and input formatting
- Adding accessibility features to elements


### **Types of Directives:**

#### **1. Structural Directives (Change DOM Layout):**

**ngIf - Conditional Rendering:**

```html
<!-- Basic ngIf -->
<div *ngIf="isLoggedIn">Welcome back!</div>
<div *ngIf="user">Hello, {{user.name}}!</div>

<!-- ngIf with else -->
<div *ngIf="isLoading; else content">Loading...</div>
<ng-template #content>
  <div>Content loaded successfully!</div>
</ng-template>

<!-- ngIf with then/else -->
<div *ngIf="hasData; then dataTemplate else noDataTemplate"></div>
<ng-template #dataTemplate>
  <div>Data is available</div>
</ng-template>
<ng-template #noDataTemplate>
  <div>No data available</div>
</ng-template>

<!-- Complex conditions -->
<div *ngIf="user && user.isActive && user.hasPermission('read')">
  Authorized content
</div>
```

**ngFor - List Rendering:**

```html
<!-- Basic ngFor -->
<ul>
  <li *ngFor="let item of items">{{item}}</li>
</ul>

<!-- ngFor with index -->
<div *ngFor="let user of users; let i = index">
  {{i + 1}}. {{user.name}}
</div>

<!-- ngFor with tracking -->
<div *ngFor="let item of items; trackBy: trackByFn">
  {{item.name}}
</div>

<!-- ngFor with additional variables -->
<div *ngFor="let item of items; let first = first; let last = last; let even = even">
  <span [class.first]="first" [class.last]="last" [class.even]="even">
    {{item.name}}
  </span>
</div>

<!-- Nested ngFor -->
<div *ngFor="let category of categories">
  <h3>{{category.name}}</h3>
  <ul>
    <li *ngFor="let product of category.products">{{product.name}}</li>
  </ul>
</div>
```

```typescript
// TrackBy function for performance
trackByFn(index: number, item: any): any {
  return item.id; // Use unique identifier
}
```

**ngSwitch - Multiple Conditions:**

```html
<div [ngSwitch]="userRole">
  <div *ngSwitchCase="'admin'">Admin Panel</div>
  <div *ngSwitchCase="'user'">User Dashboard</div>
  <div *ngSwitchCase="'guest'">Guest Content</div>
  <div *ngSwitchDefault>Unknown Role</div>
</div>

<!-- With multiple cases -->
<div [ngSwitch]="status">
  <div *ngSwitchCase="'loading'">Loading...</div>
  <div *ngSwitchCase="'success'">Success!</div>
  <div *ngSwitchCase="'error'">Error occurred</div>
  <div *ngSwitchCase="'timeout'">Request timeout</div>
  <div *ngSwitchDefault>Unknown status</div>
</div>
```


#### **2. Attribute Directives (Change Appearance/Behavior):**

**ngClass - Dynamic CSS Classes:**

```html
<!-- Object syntax -->
<div [ngClass]="{'active': isActive, 'disabled': isDisabled}">Content</div>

<!-- Array syntax -->
<div [ngClass]="['class1', 'class2', isActive ? 'active' : '']">Content</div>

<!-- String syntax -->
<div [ngClass]="cssClasses">Content</div>

<!-- Method call -->
<div [ngClass]="getClasses()">Content</div>
```

**ngStyle - Dynamic Inline Styles:**

```html
<!-- Object syntax -->
<div [ngStyle]="{'color': textColor, 'font-size': fontSize + 'px'}">Styled text</div>

<!-- Method call -->
<div [ngStyle]="getStyles()">Content</div>

<!-- Conditional styles -->
<div [ngStyle]="{'background-color': isHighlighted ? 'yellow' : 'transparent'}">
  Conditional background
</div>
```


### **Custom Attribute Directive:**

```typescript
import { Directive, ElementRef, Input, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() appHighlight: string = 'yellow';
  @Input() defaultColor: string = 'transparent';
  
  constructor(private el: ElementRef) {}
  
  @HostListener('mouseenter') onMouseEnter(): void {
    this.highlight(this.appHighlight);
  }
  
  @HostListener('mouseleave') onMouseLeave(): void {
    this.highlight(this.defaultColor);
  }
  
  private highlight(color: string): void {
    this.el.nativeElement.style.backgroundColor = color;
  }
}

// Usage
// <p appHighlight="lightblue">Hover over me!</p>
// <p [appHighlight]="color" [defaultColor]="'white'">Custom colors</p>
```


### **Custom Structural Directive:**

```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {
  private hasView = false;
  
  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}
  
  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}

// Usage
// <div *appUnless="isHidden">This shows when isHidden is false</div>
```


### **Advanced Directive Features:**

```typescript
@Directive({
  selector: '[appAdvanced]',
  exportAs: 'advanced'  // Template reference variable
})
export class AdvancedDirective {
  @Input() config: any;
  @Output() statusChange = new EventEmitter<string>();
  
  // Host property binding
  @HostBinding('class.active') isActive = false;
  @HostBinding('attr.role') role = 'button';
  @HostBinding('style.cursor') cursor = 'pointer';
  
  // Host event listeners
  @HostListener('click', ['$event'])
  onClick(event: Event): void {
    this.isActive = !this.isActive;
    this.statusChange.emit(this.isActive ? 'active' : 'inactive');
  }
  
  @HostListener('document:keydown', ['$event'])
  onKeyDown(event: KeyboardEvent): void {
    if (event.key === 'Escape') {
      this.isActive = false;
    }
  }
  
  // Public methods for template reference
  public activate(): void {
    this.isActive = true;
  }
  
  public deactivate(): void {
    this.isActive = false;
  }
}
```

```html
<!-- Usage with template reference -->
<div appAdvanced #advanced="advanced" (statusChange)="onStatusChange($event)">
  Click me or use the buttons below
</div>
<button (click)="advanced.activate()">Activate</button>
<button (click)="advanced.deactivate()">Deactivate</button>
```

[↑ Back to Top](#table-of-contents)

***

## 8. Pipes

### **Definition:**

Pipes are simple functions that accept input values and return transformed output. They are used in Angular templates to transform data for display without changing the original data. Pipes can format strings, dates, numbers, and more.

### **Why We Need Pipes:**

- **Data Transformation**: Format data for display without modifying the source
- **Reusability**: Same formatting logic can be used across multiple components
- **Clean Templates**: Keep formatting logic out of component code
- **Performance**: Pure pipes are optimized and only run when inputs change
- **Composability**: Chain multiple pipes together for complex transformations
- **Internationalization**: Built-in pipes support localization
- **Readability**: Make templates more readable and declarative


### **Usage Scenarios:**

- Formatting dates and times for display
- Converting text to uppercase/lowercase
- Formatting currency and numbers
- Filtering and sorting arrays
- Transforming JSON objects for debugging
- Creating custom formatting for specific data types


### **Built-in Pipes:**

#### **String Pipes:**

```html
<!-- UpperCase and LowerCase -->
<p>{{ 'hello world' | uppercase }}</p>  <!-- HELLO WORLD -->
<p>{{ 'HELLO WORLD' | lowercase }}</p>  <!-- hello world -->

<!-- TitleCase -->
<p>{{ 'hello world' | titlecase }}</p>  <!-- Hello World -->

<!-- Slice -->
<p>{{ 'Hello Angular' | slice:0:5 }}</p>  <!-- Hello -->
<p>{{ 'Hello Angular' | slice:6 }}</p>    <!-- Angular -->
```


#### **Number Pipes:**

```html
<!-- Decimal -->
<p>{{ 3.14159 | number:'1.2-4' }}</p>  <!-- 3.1416 -->
<p>{{ 1234.56 | number:'1.0-0' }}</p>  <!-- 1,235 -->

<!-- Currency -->
<p>{{ 1234.56 | currency }}</p>                    <!-- $1,234.56 -->
<p>{{ 1234.56 | currency:'EUR' }}</p>              <!-- €1,234.56 -->
<p>{{ 1234.56 | currency:'USD':'symbol':'1.0-0' }}</p> <!-- $1,235 -->

<!-- Percent -->
<p>{{ 0.1234 | percent }}</p>           <!-- 12.34% -->
<p>{{ 0.1234 | percent:'1.1-1' }}</p>   <!-- 12.3% -->
```


#### **Date Pipe:**

```html
<!-- Basic date formatting -->
<p>{{ today | date }}</p>                    <!-- Dec 25, 2023 -->
<p>{{ today | date:'short' }}</p>            <!-- 12/25/23, 12:00 PM -->
<p>{{ today | date:'medium' }}</p>           <!-- Dec 25, 2023, 12:00:00 PM -->
<p>{{ today | date:'long' }}</p>             <!-- December 25, 2023 at 12:00:00 PM GMT -->
<p>{{ today | date:'full' }}</p>             <!-- Monday, December 25, 2023 at 12:00:00 PM GMT -->

<!-- Custom date formatting -->
<p>{{ today | date:'dd/MM/yyyy' }}</p>       <!-- 25/12/2023 -->
<p>{{ today | date:'MMM d, y, h:mm a' }}</p> <!-- Dec 25, 2023, 12:00 PM -->
<p>{{ today | date:'EEEE, MMMM d, y' }}</p>  <!-- Monday, December 25, 2023 -->

<!-- Timezone -->
<p>{{ today | date:'short':'UTC' }}</p>      <!-- UTC time -->
<p>{{ today | date:'short':'GMT-5' }}</p>    <!-- EST time -->
```


#### **Collection Pipes:**

```html
<!-- JSON -->
<pre>{{ user | json }}</pre>

<!-- KeyValue (for objects) -->
<div *ngFor="let item of object | keyvalue">
  {{ item.key }}: {{ item.value }}
</div>
```


### **Pipe Chaining:**

```html
<!-- Multiple pipes -->
<p>{{ message | uppercase | slice:0:10 }}</p>
<p>{{ price | currency:'USD' | uppercase }}</p>
<p>{{ users | slice:0:5 | json }}</p>
```


### **Custom Pipes:**

#### **Simple Custom Pipe:**

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'reverse'
})
export class ReversePipe implements PipeTransform {
  transform(value: string): string {
    if (!value) return value;
    return value.split('').reverse().join('');
  }
}

// Usage: {{ 'Hello' | reverse }} --> olleH
```


#### **Pipe with Parameters:**

```typescript
@Pipe({
  name: 'truncate'
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit: number = 10, trail: string = '...'): string {
    if (!value) return value;
    
    return value.length > limit ? 
           value.substring(0, limit) + trail : 
           value;
  }
}

// Usage: 
// {{ longText | truncate:20:'...' }}
// {{ longText | truncate:50 }}
```


#### **Pure vs Impure Pipes:**

```typescript
// Pure pipe (default) - only executes when input changes
@Pipe({
  name: 'purePipe',
  pure: true  // This is default
})
export class PurePipe implements PipeTransform {
  transform(value: any[]): any[] {
    console.log('Pure pipe executed');
    return value.filter(item => item.active);
  }
}

// Impure pipe - executes on every change detection cycle
@Pipe({
  name: 'impurePipe',
  pure: false
})
export class ImpurePipe implements PipeTransform {
  transform(value: any[]): any[] {
    console.log('Impure pipe executed');
    return value.filter(item => item.active);
  }
}
```


#### **Advanced Custom Pipes:**

```typescript
// Highlight pipe
@Pipe({
  name: 'highlight'
})
export class HighlightPipe implements PipeTransform {
  transform(text: string, search: string, className: string = 'highlight'): string {
    if (!search) return text;
    
    const regex = new RegExp(`(${search})`, 'gi');
    return text.replace(regex, `<span class="${className}">$1</span>`);
  }
}

// Filter pipe
@Pipe({
  name: 'filter'
})
export class FilterPipe implements PipeTransform {
  transform(items: any[], searchTerm: string, property?: string): any[] {
    if (!items || !searchTerm) return items;
    
    return items.filter(item => {
      const value = property ? item[property] : item;
      return value.toString().toLowerCase().includes(searchTerm.toLowerCase());
    });
  }
}

// Sort pipe
@Pipe({
  name: 'sort'
})
export class SortPipe implements PipeTransform {
  transform(array: any[], field: string, direction: 'asc' | 'desc' = 'asc'): any[] {
    if (!array || !field) return array;
    
    const sorted = array.sort((a, b) => {
      const aValue = a[field];
      const bValue = b[field];
      
      if (aValue < bValue) return direction === 'asc' ? -1 : 1;
      if (aValue > bValue) return direction === 'asc' ? 1 : -1;
      return 0;
    });
    
    return [...sorted]; // Return new array to trigger change detection
  }
}
```


### **Using Custom Pipes:**

```html
<!-- Highlight pipe -->
<div [innerHTML]="searchText | highlight:query:'search-highlight'"></div>

<!-- Filter pipe -->
<ul>
  <li *ngFor="let user of users | filter:searchTerm:'name'">
    {{ user.name }}
  </li>
</ul>

<!-- Sort pipe -->
<div *ngFor="let item of items | sort:'name':'asc'">
  {{ item.name }}
</div>

<!-- Combining custom pipes -->
<div *ngFor="let user of users | filter:searchTerm:'name' | sort:'name':'asc' | slice:0:10">
  {{ user.name }}
</div>
```


### **Async Pipe:**

```html
<!-- Observable -->
<div *ngIf="user$ | async as user">
  <h2>{{ user.name }}</h2>
  <p>{{ user.email }}</p>
</div>

<!-- Promise -->
<div>{{ dataPromise | async }}</div>

<!-- Multiple async pipes (avoid multiple subscriptions) -->
<!-- Bad -->
<div>{{ user$ | async }}?.name</div>
<div>{{ user$ | async }}?.email</div>

<!-- Good -->
<div *ngIf="user$ | async as user">
  <div>{{ user.name }}</div>
  <div>{{ user.email }}</div>
</div>
```

[↑ Back to Top](#table-of-contents)

***

## 9. Services and Dependency Injection

### **Definition:**

Services are classes that provide functionality independent of any particular component. Dependency Injection (DI) is Angular's design pattern that supplies a class with its dependencies rather than creating them internally. Services handle business logic, data management, and shared functionality across components.

### **Why We Need Services and Dependency Injection:**

- **Code Reusability**: Share functionality across multiple components
- **Separation of Concerns**: Keep components focused on UI logic only
- **Testability**: Easy to mock dependencies during testing
- **Maintainability**: Centralize business logic in dedicated services
- **Loose Coupling**: Components don't need to know how dependencies are created
- **Scalability**: Easy to swap implementations without changing components
- **Single Responsibility**: Each service handles one specific concern


### **Usage Scenarios:**

- API communication and data fetching
- Business logic and data processing
- Shared state management across components
- Utility functions and helper methods
- Authentication and authorization
- Caching and data persistence
- Logging and error handling


### **Creating a Service:**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'  // Available app-wide as singleton
})
export class UserService {
  private apiUrl = 'https://api.example.com/users';
  private usersSubject = new BehaviorSubject<User[]>([]);
  public users$ = this.usersSubject.asObservable();
  
  constructor(private http: HttpClient) {}
  
  // Get all users
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }
  
  // Get user by ID
  getUser(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`);
  }
  
  // Create user
  createUser(user: User): Observable<User> {
    return this.http.post<User>(this.apiUrl, user);
  }
  
  // Update user
  updateUser(user: User): Observable<User> {
    return this.http.put<User>(`${this.apiUrl}/${user.id}`, user);
  }
  
  // Delete user
  deleteUser(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }
  
  // Update local state
  updateUsersState(users: User[]): void {
    this.usersSubject.next(users);
  }
  
  // Get current users value
  getCurrentUsers(): User[] {
    return this.usersSubject.value;
  }
}
```


### **Service with Different Scopes:**

```typescript
// App-wide singleton
@Injectable({
  providedIn: 'root'
})
export class GlobalService {}

// Module-specific
@Injectable({
  providedIn: UserModule
})
export class UserModuleService {}

// Component-specific (new instance for each component)
@Component({
  providers: [ComponentSpecificService]
})
export class MyComponent {
  constructor(private service: ComponentSpecificService) {}
}
```


### **Dependency Injection Patterns:**

#### **Constructor Injection:**

```typescript
@Component({
  selector: 'app-user-list'
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  
  constructor(
    private userService: UserService,
    private router: Router,
    private dialog: MatDialog
  ) {}
  
  ngOnInit(): void {
    this.loadUsers();
  }
  
  private loadUsers(): void {
    this.userService.getUsers().subscribe(
      users => this.users = users,
      error => console.error('Error loading users:', error)
    );
  }
}
```


#### **Service-Based Communication:**

```typescript
@Injectable({
  providedIn: 'root'
})
export class NotificationService {
  private notificationSubject = new Subject<Notification>();
  public notifications$ = this.notificationSubject.asObservable();
  
  showNotification(message: string, type: 'success' | 'error' | 'info' = 'info'): void {
    this.notificationSubject.next({ message, type, timestamp: new Date() });
  }
}

// Usage in component
@Component({})
export class AppComponent implements OnInit {
  constructor(private notificationService: NotificationService) {}
  
  ngOnInit(): void {
    this.notificationService.notifications$.subscribe(
      notification => {
        console.log(`${notification.type}: ${notification.message}`);
      }
    );
  }
  
  showSuccess(): void {
    this.notificationService.showNotification('Operation successful!', 'success');
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 10. Routing

### **Definition:**

Angular Router enables navigation between different views or components in a single-page application (SPA). It maps URL paths to components and manages the browser's history, allowing users to navigate through different application states using URLs.

### **Why We Need Routing:**

- **Navigation**: Enable users to move between different views/pages
- **URL Management**: Provide meaningful URLs for different application states
- **Browser History**: Support browser back/forward buttons
- **Bookmarking**: Allow users to bookmark specific application states
- **SEO Benefits**: Search engines can index different routes
- **Lazy Loading**: Load features on demand to improve performance
- **Route Guards**: Control access to different parts of the application
- **Parameter Passing**: Pass data between routes via URL parameters


### **Usage Scenarios:**

- Creating multi-page SPAs with different views
- Implementing user authentication flows
- Building admin panels with different sections
- E-commerce sites with product catalogs and details
- Dashboard applications with various modules
- Blog or content management systems


### **Basic Routing Setup:**

#### **App Routing Module:**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';
import { ContactComponent } from './contact/contact.component';
import { NotFoundComponent } from './not-found/not-found.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'contact', component: ContactComponent },
  { path: '**', component: NotFoundComponent } // Wildcard route - must be last
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```


### **Route Parameters:**

```typescript
const routes: Routes = [
  { path: 'user/:id', component: UserDetailComponent },
  { path: 'user/:id/edit', component: UserEditComponent },
  { path: 'category/:categoryId/product/:productId', component: ProductDetailComponent }
];

// Accessing Route Parameters
@Component({})
export class UserDetailComponent implements OnInit {
  user: User | undefined;
  
  constructor(
    private route: ActivatedRoute,
    private userService: UserService,
    private router: Router
  ) {}
  
  ngOnInit(): void {
    // Method 1: Snapshot (for parameters that don't change)
    const id = this.route.snapshot.paramMap.get('id');
    if (id) {
      this.loadUser(+id);
    }
    
    // Method 2: Observable (for parameters that can change)
    this.route.paramMap.pipe(
      switchMap((params: ParamMap) => {
        const userId = params.get('id');
        return userId ? this.userService.getUser(+userId) : [];
      })
    ).subscribe(user => {
      this.user = user;
    });
  }
}
```


### **Route Guards:**

```typescript
// CanActivate Guard
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}
  
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    } else {
      this.router.navigate(['/login'], {
        queryParams: { returnUrl: state.url }
      });
      return false;
    }
  }
}
```


### **Lazy Loading:**

```typescript
const routes: Routes = [
  {
    path: 'features',
    loadChildren: () => import('./features/features.module').then(m => m.FeaturesModule)
  },
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule),
    canLoad: [AuthGuard]
  }
];
```

[↑ Back to Top](#table-of-contents)

***

## 11. Forms

### **Definition:**

Angular Forms provide a structured way to handle user input, validation, and form submission. Angular offers two approaches: Template-driven forms (simpler, suitable for basic forms) and Reactive forms (more powerful, suitable for complex forms with dynamic validation).

### **Why We Need Angular Forms:**

- **Data Collection**: Gather user input systematically
- **Validation**: Ensure data integrity before submission
- **User Experience**: Provide immediate feedback on input errors
- **State Management**: Track form and field states (touched, dirty, valid)
- **Dynamic Forms**: Create forms that change based on user input
- **Type Safety**: TypeScript integration for better development experience
- **Accessibility**: Built-in support for form accessibility features
- **Testing**: Easy to test form behavior and validation logic


### **Usage Scenarios:**

- User registration and login forms
- Contact forms and surveys
- Dynamic configuration forms
- Multi-step wizards
- Data entry applications
- Search filters and advanced queries


### **Reactive Forms (Recommended):**

```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  template: `
    <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" formControlName="name">
        <div *ngIf="name?.invalid && name?.touched" class="error">
          <div *ngIf="name?.errors?.['required']">Name is required</div>
          <div *ngIf="name?.errors?.['minlength']">Name must be at least 2 characters</div>
        </div>
      </div>
      
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" formControlName="email">
        <div *ngIf="email?.invalid && email?.touched" class="error">
          <div *ngIf="email?.errors?.['required']">Email is required</div>
          <div *ngIf="email?.errors?.['email']">Invalid email format</div>
        </div>
      </div>
      
      <button type="submit" [disabled]="userForm.invalid">Submit</button>
    </form>
  `
})
export class ReactiveFormComponent implements OnInit {
  userForm!: FormGroup;
  
  constructor(private fb: FormBuilder) {}
  
  ngOnInit(): void {
    this.createForm();
  }
  
  private createForm(): void {
    this.userForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(2)]],
      email: ['', [Validators.required, Validators.email]]
    });
  }
  
  // Getter methods for easier access in template
  get name() { return this.userForm.get('name'); }
  get email() { return this.userForm.get('email'); }
  
  onSubmit(): void {
    if (this.userForm.valid) {
      console.log('Form submitted:', this.userForm.value);
    }
  }
}
```


### **Custom Validators:**

```typescript
// Custom validator function
export function forbiddenNameValidator(forbiddenName: RegExp): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const forbidden = forbiddenName.test(control.value);
    return forbidden ? { forbiddenName: { value: control.value } } : null;
  };
}

// Cross-field validator
export function passwordMatchValidator(): ValidatorFn {
  return (form: AbstractControl): ValidationErrors | null => {
    const password = form.get('password');
    const confirmPassword = form.get('confirmPassword');
    
    if (!password || !confirmPassword) {
      return null;
    }
    
    return password.value === confirmPassword.value ? null : { passwordMismatch: true };
  };
}
```

[↑ Back to Top](#table-of-contents)

***

## 12. HTTP Client and Observables

### **Definition:**

Angular's HttpClient provides a simplified API for HTTP functionality based on XMLHttpRequest interface. It works seamlessly with RxJS Observables to handle asynchronous operations, providing powerful operators for data transformation and error handling.

### **Why We Need HTTP Client and Observables:**

- **API Communication**: Interface with backend services and REST APIs
- **Asynchronous Operations**: Handle time-based operations without blocking the UI
- **Data Transformation**: Process and transform data streams
- **Error Handling**: Manage and recover from HTTP errors gracefully
- **Caching**: Implement client-side caching strategies
- **Request Cancellation**: Cancel ongoing requests when needed
- **Retry Logic**: Automatically retry failed requests
- **Progressive Loading**: Load data incrementally for better user experience


### **Usage Scenarios:**

- Fetching data from REST APIs
- Submitting forms to backend services
- Uploading and downloading files
- Real-time data streaming
- Implementing search with debouncing
- Managing authentication tokens
- Handling offline scenarios


### **Basic HTTP Operations:**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable } from 'rxjs';
import { map, catchError, retry } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private apiUrl = 'https://api.example.com';
  
  constructor(private http: HttpClient) {}
  
  // GET with parameters
  searchUsers(searchTerm: string, page: number = 1): Observable<UserResponse> {
    const params = new HttpParams()
      .set('q', searchTerm)
      .set('page', page.toString())
      .set('limit', '10');
    
    return this.http.get<UserResponse>(`${this.apiUrl}/users/search`, { params });
  }
  
  // POST request
  createUser(user: User): Observable<User> {
    return this.http.post<User>(`${this.apiUrl}/users`, user);
  }
  
  // Error handling and retry
  getUsersWithRetry(): Observable<User[]> {
    return this.http.get<User[]>(`${this.apiUrl}/users`).pipe(
      retry(3), // Retry up to 3 times
      catchError(error => {
        console.error('Failed after retries:', error);
        return of([]); // Return empty array as fallback
      })
    );
  }
}
```


### **HTTP Interceptors:**

```typescript
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const authToken = localStorage.getItem('auth-token');
    
    if (authToken) {
      const authReq = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${authToken}`)
      });
      return next.handle(authReq);
    }
    
    return next.handle(req);
  }
}
```


### **Advanced Observable Patterns:**

```typescript
@Component({})
export class SearchComponent implements OnInit {
  searchControl = new FormControl('');
  searchResults$ = new Observable<User[]>();
  
  ngOnInit(): void {
    this.searchResults$ = this.searchControl.valueChanges.pipe(
      startWith(''),
      debounceTime(300), // Wait 300ms after user stops typing
      distinctUntilChanged(), // Only trigger if search term changed
      switchMap(term => 
        term.length > 2 
          ? this.userService.searchUsers(term)
          : of([])
      ),
      catchError(error => {
        console.error('Search error:', error);
        return of([]);
      })
    );
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 13. Component Communication

### **Definition:**

Component communication refers to the various methods by which Angular components can share data and interact with each other. This includes parent-child communication, sibling communication, and global state sharing across the application.

### **Why We Need Component Communication:**

- **Data Sharing**: Pass data between related components
- **Event Propagation**: Notify parent components of child component actions
- **State Synchronization**: Keep multiple components in sync
- **Loose Coupling**: Maintain component independence while enabling interaction
- **Reusability**: Create generic components that can work in different contexts
- **User Experience**: Coordinate UI updates across multiple components
- **Business Logic**: Implement complex workflows involving multiple components


### **Usage Scenarios:**

- Parent component passing configuration to child components
- Child components notifying parents of user actions
- Sibling components sharing data through services
- Global notifications and alerts
- Shopping cart updates across multiple components
- User authentication state management


### **Parent to Child Communication (@Input):**

```typescript
// Parent Component
@Component({
  selector: 'app-parent',
  template: `
    <app-child 
      [user]="selectedUser"
      [isEditable]="true"
      [config]="appConfig">
    </app-child>
  `
})
export class ParentComponent {
  selectedUser: User = { id: 1, name: 'John', email: 'john@example.com' };
  appConfig = { theme: 'dark', language: 'en' };
}

// Child Component
@Component({
  selector: 'app-child',
  template: `
    <div *ngIf="user">
      <h3>{{ user.name }}</h3>
      <p>{{ user.email }}</p>
      <button *ngIf="isEditable">Edit</button>
    </div>
  `
})
export class ChildComponent {
  @Input() user!: User;
  @Input() isEditable: boolean = false;
  @Input() config: any;
}
```


### **Child to Parent Communication (@Output):**

```typescript
// Child Component
@Component({
  selector: 'app-child',
  template: `
    <button (click)="sendMessage()">Send Message</button>
    <button (click)="sendUser()">Send User</button>
  `
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>();
  @Output() userEvent = new EventEmitter<User>();
  
  sendMessage(): void {
    this.messageEvent.emit('Hello from child!');
  }
  
  sendUser(): void {
    const user: User = { id: 1, name: 'John', email: 'john@example.com' };
    this.userEvent.emit(user);
  }
}

// Parent Component
@Component({
  template: `
    <app-child 
      (messageEvent)="onMessageReceived($event)"
      (userEvent)="onUserReceived($event)">
    </app-child>
  `
})
export class ParentComponent {
  onMessageReceived(message: string): void {
    console.log('Message received:', message);
  }
  
  onUserReceived(user: User): void {
    console.log('User received:', user);
  }
}
```


### **Service-Based Communication:**

```typescript
// Shared Service
@Injectable({
  providedIn: 'root'
})
export class CommunicationService {
  private messageSource = new BehaviorSubject<string>('Initial message');
  currentMessage = this.messageSource.asObservable();
  
  changeMessage(message: string): void {
    this.messageSource.next(message);
  }
}

// Component A (Sender)
@Component({
  template: `<button (click)="sendMessage()">Send Message</button>`
})
export class ComponentAComponent {
  constructor(private communicationService: CommunicationService) {}
  
  sendMessage(): void {
    this.communicationService.changeMessage('Message from Component A');
  }
}

// Component B (Receiver)
@Component({
  template: `<p>Received message: {{ message }}</p>`
})
export class ComponentBComponent implements OnInit {
  message: string = '';
  
  constructor(private communicationService: CommunicationService) {}
  
  ngOnInit(): void {
    this.communicationService.currentMessage.subscribe(
      message => this.message = message
    );
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 14. Lifecycle Hooks

### **Definition:**

Lifecycle hooks are methods that Angular calls at specific moments in a component's or directive's lifecycle. They provide visibility into key moments and the ability to act when they occur, from creation through destruction of components and directives.

### **Why We Need Lifecycle Hooks:**

- **Initialization Logic**: Perform setup tasks when component is created
- **Data Fetching**: Load data at the appropriate time in component lifecycle
- **Change Detection**: Respond to input property changes
- **Cleanup**: Prevent memory leaks by cleaning up subscriptions and timers
- **Performance Optimization**: Optimize rendering and update cycles
- **DOM Manipulation**: Access and manipulate DOM elements safely
- **Resource Management**: Manage component resources efficiently
- **Integration**: Integrate with third-party libraries at the right time


### **Usage Scenarios:**

- Loading data from APIs when component initializes
- Setting up event listeners and timers
- Cleaning up subscriptions to prevent memory leaks
- Responding to user input changes
- Integrating with jQuery plugins or other libraries
- Implementing complex animation sequences
- Managing component state transitions


### **Component Lifecycle Hooks:**

#### **ngOnInit - Initialization:**

```typescript
@Component({
  template: `
    <div>
      <h2>{{ title }}</h2>
      <ul>
        <li *ngFor="let user of users">{{ user.name }}</li>
      </ul>
    </div>
  `
})
export class UserListComponent implements OnInit {
  title = 'User Management';
  users: User[] = [];
  
  constructor(private userService: UserService) {
    // Constructor is for dependency injection only
    console.log('Constructor called');
  }
  
  ngOnInit(): void {
    // Component initialization logic
    console.log('ngOnInit called');
    this.loadUsers();
    this.setupEventListeners();
  }
  
  private loadUsers(): void {
    this.userService.getUsers().subscribe(
      users => this.users = users
    );
  }
  
  private setupEventListeners(): void {
    // Setup any event listeners here
  }
}
```


#### **ngOnChanges - Input Changes:**

```typescript
@Component({
  selector: 'app-user-detail',
  template: `
    <div *ngIf="user">
      <h3>{{ user.name }}</h3>
      <p>{{ user.email }}</p>
      <p>Profile updated {{ updateCount }} times</p>
    </div>
  `
})
export class UserDetailComponent implements OnChanges {
  @Input() user!: User;
  @Input() theme: string = 'light';
  
  updateCount = 0;
  
  ngOnChanges(changes: SimpleChanges): void {
    console.log('ngOnChanges called', changes);
    
    // Check specific property changes
    if (changes['user']) {
      const userChange = changes['user'];
      console.log('User changed:', {
        previousValue: userChange.previousValue,
        currentValue: userChange.currentValue,
        firstChange: userChange.firstChange
      });
      
      if (!userChange.firstChange) {
        this.updateCount++;
        this.onUserUpdate();
      }
    }
    
    if (changes['theme']) {
      this.applyTheme(changes['theme'].currentValue);
    }
  }
  
  private onUserUpdate(): void {
    // Handle user updates
    console.log('User profile updated');
  }
  
  private applyTheme(theme: string): void {
    // Apply theme changes
    console.log('Theme applied:', theme);
  }
}
```


#### **ngAfterViewInit - View Initialization:**

```typescript
@Component({
  template: `
    <input #searchInput type="text" placeholder="Search">
    <app-child-component #childComp></app-child-component>
  `
})
export class ParentComponent implements AfterViewInit {
  @ViewChild('searchInput') searchInput!: ElementRef;
  @ViewChild('childComp') childComponent!: ChildComponent;
  
  ngAfterViewInit(): void {
    // View and child components are now initialized
    console.log('ngAfterViewInit called');
    
    // Safe to access ViewChild references
    this.searchInput.nativeElement.focus();
    
    // Safe to call child component methods
    this.childComponent.initialize();
    
    // Setup third-party plugins that need DOM access
    this.initializeThirdPartyPlugins();
  }
  
  private initializeThirdPartyPlugins(): void {
    // Initialize jQuery plugins, charts, etc.
  }
}
```


#### **ngOnDestroy - Cleanup:**

```typescript
@Component({
  template: `<div>Component with subscriptions and timers</div>`
})
export class MyComponent implements OnInit, OnDestroy {
  private subscriptions = new Subscription();
  private timer: any;
  
  constructor(
    private dataService: DataService,
    private notificationService: NotificationService
  ) {}
  
  ngOnInit(): void {
    // Setup subscriptions
    this.subscriptions.add(
      this.dataService.getData().subscribe(data => {
        console.log('Data received:', data);
      })
    );
    
    this.subscriptions.add(
      this.notificationService.notifications$.subscribe(notification => {
        console.log('Notification:', notification);
      })
    );
    
    // Setup timer
    this.timer = setInterval(() => {
      console.log('Timer tick');
    }, 1000);
    
    // Setup event listeners
    document.addEventListener('keydown', this.handleKeyDown);
  }
  
  ngOnDestroy(): void {
    console.log('ngOnDestroy called');
    
    // Cleanup subscriptions
    this.subscriptions.unsubscribe();
    
    // Cleanup timer
    if (this.timer) {
      clearInterval(this.timer);
    }
    
    // Cleanup event listeners
    document.removeEventListener('keydown', this.handleKeyDown);
    
    // Cleanup third-party libraries
    this.cleanupThirdPartyLibraries();
  }
  
  private handleKeyDown = (event: KeyboardEvent) => {
    console.log('Key pressed:', event.key);
  }
  
  private cleanupThirdPartyLibraries(): void {
    // Cleanup jQuery plugins, destroy charts, etc.
  }
}
```


### **Complete Lifecycle Hook Example:**

```typescript
@Component({
  selector: 'app-lifecycle-demo',
  template: `
    <div>
      <h3>{{ title }}</h3>
      <p>Count: {{ count }}</p>
      <button (click)="increment()">Increment</button>
    </div>
  `
})
export class LifecycleDemoComponent implements 
  OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked,
  AfterViewInit, AfterViewChecked, OnDestroy {
  
  @Input() title: string = 'Default Title';
  count = 0;
  
  constructor() {
    console.log('Constructor');
  }
  
  ngOnChanges(changes: SimpleChanges): void {
    console.log('ngOnChanges', changes);
  }
  
  ngOnInit(): void {
    console.log('ngOnInit');
  }
  
  ngDoCheck(): void {
    console.log('ngDoCheck');
  }
  
  ngAfterContentInit(): void {
    console.log('ngAfterContentInit');
  }
  
  ngAfterContentChecked(): void {
    console.log('ngAfterContentChecked');
  }
  
  ngAfterViewInit(): void {
    console.log('ngAfterViewInit');
  }
  
  ngAfterViewChecked(): void {
    console.log('ngAfterViewChecked');
  }
  
  ngOnDestroy(): void {
    console.log('ngOnDestroy');
  }
  
  increment(): void {
    this.count++;
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 15. Change Detection

### **Definition:**

Change Detection is Angular's mechanism for keeping the application UI in sync with the application state. It's the process by which Angular checks for changes in data-bound properties and updates the DOM accordingly.[^1]

### **Why We Need Change Detection:**

- **UI Synchronization**: Keep the view updated when data changes
- **Performance**: Only update DOM elements that actually changed
- **Automatic Updates**: No manual DOM manipulation required
- **Data Binding**: Support for two-way data binding and reactive programming
- **Consistency**: Ensure UI always reflects current application state
- **Developer Experience**: Reduces boilerplate code for UI updates
- **Zone.js Integration**: Handles asynchronous operations automatically


### **Usage Scenarios:**

- Updating lists when items are added or removed
- Reflecting form input changes in real-time
- Updating counters and progress indicators
- Handling API response updates
- Managing component state changes
- Optimizing performance in large applications


### **Change Detection Strategies:**

#### **Default Strategy:**

```typescript
@Component({
  selector: 'app-default-detection',
  template: `
    <div>
      <h3>{{ title }}</h3>
      <p>Count: {{ count }}</p>
      <button (click)="increment()">Increment</button>
      <app-child [data]="childData"></app-child>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.Default // This is default
})
export class DefaultDetectionComponent {
  title = 'Default Change Detection';
  count = 0;
  childData = { value: 'initial' };
  
  increment(): void {
    this.count++;
    // Angular automatically detects this change
  }
  
  updateChildData(): void {
    this.childData.value = 'updated'; // This will trigger change detection
  }
}
```


#### **OnPush Strategy:**

```typescript
@Component({
  selector: 'app-onpush-detection',
  template: `
    <div>
      <h3>{{ title }}</h3>
      <p>Count: {{ count }}</p>
      <button (click)="increment()">Increment</button>
      <p>Last update: {{ lastUpdate | date:'medium' }}</p>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OnPushDetectionComponent {
  @Input() count: number = 0;
  
  title = 'OnPush Change Detection';
  lastUpdate = new Date();
  
  constructor(private cdr: ChangeDetectorRef) {}
  
  increment(): void {
    this.count++;
    this.lastUpdate = new Date();
    
    // Manually trigger change detection with OnPush strategy
    this.cdr.detectChanges();
  }
  
  // This method won't trigger change detection automatically with OnPush
  updateTitle(): void {
    this.title = 'Updated Title';
    this.cdr.markForCheck(); // Mark component for check in next cycle
  }
}
```


### **Manual Change Detection Control:**

```typescript
@Component({
  template: `
    <div>
      <p>{{ heavyComputationResult }}</p>
      <button (click)="performHeavyComputation()">Compute</button>
      <button (click)="detachChangeDetection()">Detach</button>
      <button (click)="attachChangeDetection()">Attach</button>
    </div>
  `
})
export class ManualDetectionComponent implements OnInit {
  heavyComputationResult = '';
  
  constructor(private cdr: ChangeDetectorRef) {}
  
  ngOnInit(): void {
    // Detach from change detection for performance
    this.cdr.detach();
  }
  
  performHeavyComputation(): void {
    // Simulate heavy computation
    this.heavyComputationResult = 'Computing...';
    
    setTimeout(() => {
      this.heavyComputationResult = 'Computation complete!';
      this.cdr.detectChanges(); // Manually trigger detection
    }, 2000);
  }
  
  detachChangeDetection(): void {
    this.cdr.detach();
    console.log('Change detection detached');
  }
  
  attachChangeDetection(): void {
    this.cdr.reattach();
    console.log('Change detection reattached');
  }
}
```


### **Immutable Data Patterns:**

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

@Component({
  selector: 'app-immutable-data',
  template: `
    <div *ngFor="let user of users; trackBy: trackByUserId">
      {{ user.name }} - {{ user.email }}
    </div>
    <button (click)="addUser()">Add User</button>
    <button (click)="updateUser(0)">Update First User</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ImmutableDataComponent {
  users: User[] = [
    { id: 1, name: 'John', email: 'john@example.com' },
    { id: 2, name: 'Jane', email: 'jane@example.com' }
  ];
  
  // TrackBy function for performance
  trackByUserId(index: number, user: User): number {
    return user.id;
  }
  
  addUser(): void {
    const newUser: User = {
      id: this.users.length + 1,
      name: `User ${this.users.length + 1}`,
      email: `user${this.users.length + 1}@example.com`
    };
    
    // Create new array (immutable update)
    this.users = [...this.users, newUser];
  }
  
  updateUser(index: number): void {
    const updatedUser: User = {
      ...this.users[index],
      name: `Updated ${this.users[index].name}`,
      email: `updated.${this.users[index].email}`
    };
    
    // Create new array with updated user (immutable update)
    this.users = [
      ...this.users.slice(0, index),
      updatedUser,
      ...this.users.slice(index + 1)
    ];
  }
}
```


### **Async Operations and Change Detection:**

```typescript
@Component({
  template: `
    <div>
      <p>{{ message$ | async }}</p>
      <p>User: {{ (user$ | async)?.name }}</p>
      <button (click)="loadData()">Load Data</button>
    </div>
  `
})
export class AsyncDetectionComponent implements OnInit {
  message$ = new BehaviorSubject<string>('Initial message');
  user$: Observable<User>;
  
  constructor(
    private userService: UserService,
    private ngZone: NgZone
  ) {
    this.user$ = this.userService.getCurrentUser();
  }
  
  ngOnInit(): void {
    // Operations outside Angular's zone won't trigger change detection
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => {
        console.log('This runs outside Angular zone');
      }, 1000);
    });
  }
  
  loadData(): void {
    // This will trigger change detection via async pipe
    this.message$.next('Loading data...');
    
    setTimeout(() => {
      this.ngZone.run(() => {
        // Ensure this runs inside Angular zone for change detection
        this.message$.next('Data loaded successfully!');
      });
    }, 2000);
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 16. Modules

### **Definition:**

Angular Modules (NgModules) are classes decorated with `@NgModule` that organize an application into cohesive blocks of functionality. They provide a compilation context for components, services, and other code files, and define how the application should be compiled and run.

### **Why We Need Modules:**

- **Organization**: Group related functionality together
- **Lazy Loading**: Load features on demand for better performance
- **Encapsulation**: Control what is available to other parts of the application
- **Dependency Management**: Manage dependencies and provider scope
- **Feature Separation**: Separate concerns into distinct, manageable units
- **Code Splitting**: Enable webpack to split code into separate bundles
- **Reusability**: Create reusable modules across different applications
- **Testing**: Isolate functionality for easier testing


### **Usage Scenarios:**

- Organizing application into feature modules
- Creating shared functionality across features
- Implementing lazy-loaded routes
- Building reusable component libraries
- Managing third-party library integrations
- Separating core application services


### **Root Module (AppModule):**

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { CoreModule } from './core/core.module';
import { SharedModule } from './shared/shared.module';

@NgModule({
  declarations: [
    AppComponent  // Root component
  ],
  imports: [
    BrowserModule,        // Required for running in browser
    HttpClientModule,     // HTTP client for API calls
    FormsModule,          // Template-driven forms
    ReactiveFormsModule,  // Reactive forms
    AppRoutingModule,     // Application routing
    CoreModule,           // Singleton services
    SharedModule          // Shared components and utilities
  ],
  providers: [
    // Global providers go here
  ],
  bootstrap: [AppComponent] // Component to start the application
})
export class AppModule { }
```


### **Feature Module:**

```typescript
// user/user.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule, Routes } from '@angular/router';
import { ReactiveFormsModule } from '@angular/forms';

// Components
import { UserListComponent } from './components/user-list/user-list.component';
import { UserDetailComponent } from './components/user-detail/user-detail.component';
import { UserFormComponent } from './components/user-form/user-form.component';

// Services
import { UserService } from './services/user.service';
import { UserResolver } from './resolvers/user.resolver';

// Guards
import { UserGuard } from './guards/user.guard';

const routes: Routes = [
  { path: '', component: UserListComponent },
  { 
    path: ':id', 
    component: UserDetailComponent,
    resolve: { user: UserResolver },
    canActivate: [UserGuard]
  },
  { path: 'new', component: UserFormComponent }
];

@NgModule({
  declarations: [
    UserListComponent,
    UserDetailComponent,
    UserFormComponent
  ],
  imports: [
    CommonModule,
    ReactiveFormsModule,
    RouterModule.forChild(routes)  // Child routing
  ],
  providers: [
    UserService,      // Feature-specific service
    UserResolver,     // Route resolver
    UserGuard        // Route guard
  ]
})
export class UserModule { }
```


### **Shared Module:**

```typescript
// shared/shared.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { RouterModule } from '@angular/router';

// Shared Components
import { LoadingComponent } from './components/loading/loading.component';
import { ConfirmDialogComponent } from './components/confirm-dialog/confirm-dialog.component';
import { PageNotFoundComponent } from './components/page-not-found/page-not-found.component';

// Shared Directives
import { HighlightDirective } from './directives/highlight.directive';
import { AutofocusDirective } from './directives/autofocus.directive';

// Shared Pipes
import { TruncatePipe } from './pipes/truncate.pipe';
import { SafeHtmlPipe } from './pipes/safe-html.pipe';

// Material Design Components (if using Angular Material)
import { MatButtonModule } from '@angular/material/button';
import { MatCardModule } from '@angular/material/card';
import { MatInputModule } from '@angular/material/input';

const SHARED_COMPONENTS = [
  LoadingComponent,
  ConfirmDialogComponent,
  PageNotFoundComponent
];

const SHARED_DIRECTIVES = [
  HighlightDirective,
  AutofocusDirective
];

const SHARED_PIPES = [
  TruncatePipe,
  SafeHtmlPipe
];

const MATERIAL_MODULES = [
  MatButtonModule,
  MatCardModule,
  MatInputModule
];

@NgModule({
  declarations: [
    ...SHARED_COMPONENTS,
    ...SHARED_DIRECTIVES,
    ...SHARED_PIPES
  ],
  imports: [
    CommonModule,
    FormsModule,
    ReactiveFormsModule,
    RouterModule,
    ...MATERIAL_MODULES
  ],
  exports: [
    // Re-export common modules
    CommonModule,
    FormsModule,
    ReactiveFormsModule,
    RouterModule,
    
    // Export shared components, directives, and pipes
    ...SHARED_COMPONENTS,
    ...SHARED_DIRECTIVES,
    ...SHARED_PIPES,
    
    // Re-export Material modules
    ...MATERIAL_MODULES
  ]
})
export class SharedModule { }
```


### **Core Module (Singleton Services):**

```typescript
// core/core.module.ts
import { NgModule, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HTTP_INTERCEPTORS } from '@angular/common/http';

// Core Services (Singletons)
import { AuthService } from './services/auth.service';
import { LoggerService } from './services/logger.service';
import { ErrorHandlerService } from './services/error-handler.service';
import { NotificationService } from './services/notification.service';

// HTTP Interceptors
import { AuthInterceptor } from './interceptors/auth.interceptor';
import { ErrorInterceptor } from './interceptors/error.interceptor';
import { LoggingInterceptor } from './interceptors/logging.interceptor';

// Guards
import { AuthGuard } from './guards/auth.guard';
import { RoleGuard } from './guards/role.guard';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ],
  providers: [
    // Singleton Services
    AuthService,
    LoggerService,
    ErrorHandlerService,
    NotificationService,
    
    // Guards
    AuthGuard,
    RoleGuard,
    
    // HTTP Interceptors
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true
    },
    {
      provide: HTTP_INTERCEPTORS,
      useClass: ErrorInterceptor,
      multi: true
    },
    {
      provide: HTTP_INTERCEPTORS,
      useClass: LoggingInterceptor,
      multi: true
    }
  ]
})
export class CoreModule {
  // Prevent multiple imports of CoreModule
  constructor(@Optional() @SkipSelf() parentModule: CoreModule) {
    if (parentModule) {
      throw new Error(
        'CoreModule is already loaded. Import it in the AppModule only'
      );
    }
  }
}
```


### **Lazy Loading Configuration:**

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AuthGuard } from './core/guards/auth.guard';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { 
    path: 'home', 
    loadChildren: () => import('./features/home/home.module').then(m => m.HomeModule)
  },
  {
    path: 'users',
    loadChildren: () => import('./features/user/user.module').then(m => m.UserModule),
    canLoad: [AuthGuard]  // Guard for lazy-loaded module
  },
  {
    path: 'admin',
    loadChildren: () => import('./features/admin/admin.module').then(m => m.AdminModule),
    canLoad: [AuthGuard],
    data: { roles: ['admin'] }  // Additional data for guards
  },
  { path: '**', redirectTo: '/home' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes, {
    enableTracing: false,  // Set to true for debugging
    preloadingStrategy: PreloadAllModules  // Preload lazy modules
  })],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```


### **Standalone Components (Angular 14+):**

```typescript
// Modern approach without NgModules
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterOutlet } from '@angular/router';
import { MatToolbarModule } from '@angular/material/toolbar';

@Component({
  selector: 'app-root',
  standalone: true,  // This component doesn't need a module
  imports: [
    CommonModule,
    RouterOutlet,
    MatToolbarModule
  ],
  template: `
    <mat-toolbar color="primary">
      <span>My App</span>
    </mat-toolbar>
    <router-outlet></router-outlet>
  `
})
export class AppComponent {
  title = 'My Standalone App';
}

// Bootstrap standalone component
// main.ts
import { bootstrapApplication } from '@angular/platform-browser';
import { importProvidersFrom } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent, {
  providers: [
    importProvidersFrom(HttpClientModule),
    // Other providers...
  ]
});
```

[↑ Back to Top](#table-of-contents)

***

## 17. State Management

### **Definition:**

State management in Angular refers to the practice of managing and synchronizing application state across components and services. Modern Angular state management leverages Signals and libraries like NgRx SignalStore to create reactive, predictable state containers.[^2][^3][^4]

### **Why We Need State Management:**

- **Centralized State**: Single source of truth for application data
- **Predictable Updates**: Consistent patterns for state modifications
- **Component Communication**: Share state between unrelated components
- **Debugging**: Track state changes and debug application behavior
- **Scalability**: Manage complex application state as apps grow
- **Performance**: Optimize rendering with fine-grained reactivity
- **Time Travel**: Ability to replay and debug state changes
- **Persistence**: Save and restore application state


### **Usage Scenarios:**

- User authentication and authorization state
- Shopping cart management in e-commerce apps
- Form state in multi-step wizards
- Global UI state (themes, language preferences)
- Real-time data synchronization
- Offline data management
- Complex business workflows


### **Modern State Management with Signals (2025):**

#### **Signal-Based State Service:**

```typescript
import { Injectable, signal, computed } from '@angular/core';
import { HttpClient } from '@angular/common/http';

interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'guest';
}

interface AppState {
  user: User | null;
  isLoading: boolean;
  error: string | null;
  theme: 'light' | 'dark';
}

@Injectable({
  providedIn: 'root'
})
export class AppStateService {
  // Private signals for internal state management
  private userSignal = signal<User | null>(null);
  private isLoadingSignal = signal<boolean>(false);
  private errorSignal = signal<string | null>(null);
  private themeSignal = signal<'light' | 'dark'>('light');
  
  // Public readonly signals for components
  readonly user = this.userSignal.asReadonly();
  readonly isLoading = this.isLoadingSignal.asReadonly();
  readonly error = this.errorSignal.asReadonly();
  readonly theme = this.themeSignal.asReadonly();
  
  // Computed signals for derived state
  readonly isAuthenticated = computed(() => this.user() !== null);
  readonly isAdmin = computed(() => this.user()?.role === 'admin');
  readonly userName = computed(() => this.user()?.name ?? 'Guest');
  
  constructor(private http: HttpClient) {}
  
  // State update methods
  setUser(user: User | null): void {
    this.userSignal.set(user);
    this.errorSignal.set(null);
  }
  
  setLoading(loading: boolean): void {
    this.isLoadingSignal.set(loading);
  }
  
  setError(error: string | null): void {
    this.errorSignal.set(error);
  }
  
  toggleTheme(): void {
    this.themeSignal.update(current => current === 'light' ? 'dark' : 'light');
  }
  
  // Async operations
  async login(credentials: { email: string; password: string }): Promise<void> {
    this.setLoading(true);
    this.setError(null);
    
    try {
      const user = await this.http.post<User>('/api/login', credentials).toPromise();
      this.setUser(user);
    } catch (error) {
      this.setError('Login failed. Please try again.');
    } finally {
      this.setLoading(false);
    }
  }
  
  logout(): void {
    this.setUser(null);
    this.setError(null);
  }
}
```


### **NgRx SignalStore (Recommended for 2025):**

```typescript
import { Injectable } from '@angular/core';
import { signalStore, withState, withMethods, withComputed, patchState } from '@ngrx/signals';
import { rxMethod } from '@ngrx/signals/rxjs-interop';
import { inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { pipe, switchMap, tap, catchError } from 'rxjs';
import { of } from 'rxjs';

// State interface
interface TodoState {
  todos: Todo[];
  loading: boolean;
  error: string | null;
  filter: 'all' | 'active' | 'completed';
}

// Initial state
const initialState: TodoState = {
  todos: [],
  loading: false,
  error: null,
  filter: 'all'
};

// SignalStore definition
export const TodoStore = signalStore(
  { providedIn: 'root' },
  
  // Define state
  withState(initialState),
  
  // Computed signals (derived state)
  withComputed((store) => ({
    filteredTodos: computed(() => {
      const todos = store.todos();
      const filter = store.filter();
      
      switch (filter) {
        case 'active':
          return todos.filter(todo => !todo.completed);
        case 'completed':
          return todos.filter(todo => todo.completed);
        default:
          return todos;
      }
    }),
    
    totalCount: computed(() => store.todos().length),
    activeCount: computed(() => store.todos().filter(t => !t.completed).length),
    completedCount: computed(() => store.todos().filter(t => t.completed).length)
  })),
  
  // Methods for state updates and side effects
  withMethods((store, http = inject(HttpClient)) => ({
    // Synchronous state updates
    setFilter(filter: 'all' | 'active' | 'completed'): void {
      patchState(store, { filter });
    },
    
    addTodo(text: string): void {
      const newTodo: Todo = {
        id: Date.now(),
        text,
        completed: false,
        createdAt: new Date()
      };
      
      patchState(store, (state) => ({
        todos: [...state.todos, newTodo]
      }));
    },
    
    toggleTodo(id: number): void {
      patchState(store, (state) => ({
        todos: state.todos.map(todo =>
          todo.id === id ? { ...todo, completed: !todo.completed } : todo
        )
      }));
    },
    
    removeTodo(id: number): void {
      patchState(store, (state) => ({
        todos: state.todos.filter(todo => todo.id !== id)
      }));
    },
    
    clearCompleted(): void {
      patchState(store, (state) => ({
        todos: state.todos.filter(todo => !todo.completed)
      }));
    },
    
    // Async operations using rxMethod
    loadTodos: rxMethod<void>(
      pipe(
        tap(() => patchState(store, { loading: true, error: null })),
        switchMap(() =>
          http.get<Todo[]>('/api/todos').pipe(
            tap((todos) => patchState(store, { todos, loading: false })),
            catchError((error) => {
              patchState(store, { loading: false, error: error.message });
              return of([]);
            })
          )
        )
      )
    ),
    
    saveTodo: rxMethod<Todo>(
      pipe(
        switchMap((todo) =>
          http.post<Todo>('/api/todos', todo).pipe(
            tap((savedTodo) => {
              patchState(store, (state) => ({
                todos: state.todos.map(t => 
                  t.id === todo.id ? savedTodo : t
                )
              }));
            }),
            catchError((error) => {
              patchState(store, { error: error.message });
              return of(todo);
            })
          )
        )
      )
    )
  }))
);
```


### **Using SignalStore in Components:**

```typescript
@Component({
  selector: 'app-todo-list',
  template: `
    <div class="todo-app">
      <header>
        <h1>Todo List</h1>
        <p>{{ todoStore.activeCount() }} of {{ todoStore.totalCount() }} remaining</p>
      </header>
      
      <form (ngSubmit)="addTodo()" #form="ngForm">
        <input [(ngModel)]="newTodoText" name="todo" placeholder="Add todo...">
        <button type="submit">Add</button>
      </form>
      
      <div class="filters">
        <button 
          *ngFor="let filter of filters"
          (click)="todoStore.setFilter(filter.value)"
          [class.active]="todoStore.filter() === filter.value">
          {{ filter.label }}
        </button>
      </div>
      
      @if (todoStore.loading()) {
        <div class="loading">Loading todos...</div>
      }
      
      @if (todoStore.error()) {
        <div class="error">{{ todoStore.error() }}</div>
      }
      
      <ul class="todo-list">
        @for (todo of todoStore.filteredTodos(); track todo.id) {
          <li class="todo-item" [class.completed]="todo.completed">
            <input 
              type="checkbox" 
              [checked]="todo.completed"
              (change)="todoStore.toggleTodo(todo.id)">
            <span>{{ todo.text }}</span>
            <button (click)="todoStore.removeTodo(todo.id)">Delete</button>
          </li>
        }
      </ul>
      
      <footer>
        <button 
          (click)="todoStore.clearCompleted()"
          [disabled]="todoStore.completedCount() === 0">
          Clear Completed
        </button>
      </footer>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class TodoListComponent implements OnInit {
  todoStore = inject(TodoStore);
  
  newTodoText = '';
  
  filters = [
    { label: 'All', value: 'all' as const },
    { label: 'Active', value: 'active' as const },
    { label: 'Completed', value: 'completed' as const }
  ];
  
  ngOnInit(): void {
    // Load todos when component initializes
    this.todoStore.loadTodos();
  }
  
  addTodo(): void {
    if (this.newTodoText.trim()) {
      this.todoStore.addTodo(this.newTodoText.trim());
      this.newTodoText = '';
    }
  }
}
```


### **Global State with Multiple Stores:**

```typescript
// User Store
export const UserStore = signalStore(
  { providedIn: 'root' },
  withState<UserState>(initialUserState),
  withComputed((store) => ({
    isAuthenticated: computed(() => store.user() !== null),
    fullName: computed(() => {
      const user = store.user();
      return user ? `${user.firstName} ${user.lastName}` : '';
    })
  })),
  withMethods((store) => ({
    login: rxMethod<LoginCredentials>(/* ... */),
    logout(): void {
      patchState(store, { user: null, token: null });
    }
  }))
);

// Settings Store
export const SettingsStore = signalStore(
  { providedIn: 'root' },
  withState<SettingsState>(initialSettingsState),
  withMethods((store) => ({
    updateTheme(theme: Theme): void {
      patchState(store, { theme });
    },
    updateLanguage(language: string): void {
      patchState(store, { language });
    }
  }))
);

// App Component using multiple stores
@Component({
  template: `
    <div [attr.data-theme]="settingsStore.theme()">
      @if (userStore.isAuthenticated()) {
        <p>Welcome, {{ userStore.fullName() }}!</p>
        <button (click)="userStore.logout()">Logout</button>
      } @else {
        <button (click)="showLogin()">Login</button>
      }
    </div>
  `
})
export class AppComponent {
  userStore = inject(UserStore);
  settingsStore = inject(SettingsStore);
  
  showLogin(): void {
    // Show login dialog
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 18. Testing

### **Definition:**

Testing in Angular involves verifying that your application components, services, and other parts work correctly. Angular provides robust testing utilities including TestBed for configuring test modules, fixtures for component testing, and integration with popular testing frameworks like Jasmine and Jest.

### **Why We Need Testing:**

- **Code Quality**: Ensure code works as expected and catch bugs early
- **Regression Prevention**: Prevent breaking existing functionality when adding new features
- **Refactoring Confidence**: Safely refactor code knowing tests will catch issues
- **Documentation**: Tests serve as living documentation of how code should behave
- **Team Collaboration**: Enable multiple developers to work on code with confidence
- **Continuous Integration**: Automate quality checks in deployment pipelines
- **User Experience**: Ensure application behavior meets user expectations
- **Performance Monitoring**: Track performance regressions over time


### **Usage Scenarios:**

- Unit testing individual components and services
- Integration testing component interactions
- End-to-end testing complete user workflows
- Testing form validation and user input
- Testing HTTP requests and API interactions
- Testing routing and navigation
- Performance and accessibility testing


### **Unit Testing Components:**

```typescript
// user.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';
import { DebugElement } from '@angular/core';
import { UserComponent } from './user.component';
import { UserService } from './user.service';

describe('UserComponent', () => {
  let component: UserComponent;
  let fixture: ComponentFixture<UserComponent>;
  let userService: jasmine.SpyObj<UserService>;
  let debugElement: DebugElement;

  beforeEach(async () => {
    // Create spy object for UserService
    const userServiceSpy = jasmine.createSpyObj('UserService', ['getUsers', 'deleteUser']);

    await TestBed.configureTestingModule({
      declarations: [UserComponent],
      providers: [
        { provide: UserService, useValue: userServiceSpy }
      ]
    }).compileComponents();

    fixture = TestBed.createComponent(UserComponent);
    component = fixture.componentInstance;
    userService = TestBed.inject(UserService) as jasmine.SpyObj<UserService>;
    debugElement = fixture.debugElement;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should display user name', () => {
    // Arrange
    component.user = { id: 1, name: 'John Doe', email: 'john@example.com' };
    
    // Act
    fixture.detectChanges();
    
    // Assert
    const nameElement = debugElement.query(By.css('.user-name'));
    expect(nameElement.nativeElement.textContent).toContain('John Doe');
  });

  it('should call deleteUser when delete button is clicked', () => {
    // Arrange
    component.user = { id: 1, name: 'John', email: 'john@example.com' };
    userService.deleteUser.and.returnValue(of({}));
    fixture.detectChanges();
    
    // Act
    const deleteButton = debugElement.query(By.css('.delete-btn'));
    deleteButton.nativeElement.click();
    
    // Assert
    expect(userService.deleteUser).toHaveBeenCalledWith(1);
  });

  it('should emit userDeleted event when user is deleted', () => {
    // Arrange
    component.user = { id: 1, name: 'John', email: 'john@example.com' };
    userService.deleteUser.and.returnValue(of({}));
    spyOn(component.userDeleted, 'emit');
    
    // Act
    component.deleteUser();
    
    // Assert
    expect(component.userDeleted.emit).toHaveBeenCalledWith(1);
  });

  it('should handle loading state', () => {
    // Arrange
    component.isLoading = true;
    
    // Act
    fixture.detectChanges();
    
    // Assert
    const loadingElement = debugElement.query(By.css('.loading'));
    expect(loadingElement).toBeTruthy();
    expect(loadingElement.nativeElement.textContent).toContain('Loading...');
  });
});
```


### **Testing Services:**

```typescript
// user.service.spec.ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [UserService]
    });
    
    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    // Verify that no unmatched requests are outstanding
    httpMock.verify();
  });

  it('should be created', () => {
    expect(service).toBeTruthy();
  });

  it('should get users', () => {
    // Arrange
    const mockUsers: User[] = [
      { id: 1, name: 'John', email: 'john@example.com' },
      { id: 2, name: 'Jane', email: 'jane@example.com' }
    ];

    // Act
    service.getUsers().subscribe(users => {
      // Assert
      expect(users).toEqual(mockUsers);
      expect(users.length).toBe(2);
    });

    // Assert HTTP request
    const req = httpMock.expectOne('/api/users');
    expect(req.request.method).toBe('GET');
    req.flush(mockUsers);
  });

  it('should create user', () => {
    // Arrange
    const newUser: User = { id: 3, name: 'Bob', email: 'bob@example.com' };

    // Act
    service.createUser(newUser).subscribe(user => {
      // Assert
      expect(user).toEqual(newUser);
    });

    // Assert HTTP request
    const req = httpMock.expectOne('/api/users');
    expect(req.request.method).toBe('POST');
    expect(req.request.body).toEqual(newUser);
    req.flush(newUser);
  });

  it('should handle error when getting users fails', () => {
    // Act
    service.getUsers().subscribe(
      users => fail('should have failed with 500 error'),
      error => {
        // Assert
        expect(error.status).toBe(500);
        expect(error.error).toContain('Server error');
      }
    );

    // Assert HTTP request
    const req = httpMock.expectOne('/api/users');
    req.flush('Server error', { status: 500, statusText: 'Internal Server Error' });
  });
});
```


### **Testing Forms:**

```typescript
// user-form.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { ReactiveFormsModule } from '@angular/forms';
import { UserFormComponent } from './user-form.component';

describe('UserFormComponent', () => {
  let component: UserFormComponent;
  let fixture: ComponentFixture<UserFormComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [UserFormComponent],
      imports: [ReactiveFormsModule]
    }).compileComponents();

    fixture = TestBed.createComponent(UserFormComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should create form with default values', () => {
    expect(component.userForm.get('name')?.value).toBe('');
    expect(component.userForm.get('email')?.value).toBe('');
  });

  it('should validate required fields', () => {
    // Act
    component.userForm.patchValue({ name: '', email: '' });
    
    // Assert
    expect(component.userForm.valid).toBeFalsy();
    expect(component.userForm.get('name')?.errors?.['required']).toBeTruthy();
    expect(component.userForm.get('email')?.errors?.['required']).toBeTruthy();
  });

  it('should validate email format', () => {
    // Act
    component.userForm.patchValue({ email: 'invalid-email' });
    
    // Assert
    expect(component.userForm.get('email')?.errors?.['email']).toBeTruthy();
  });

  it('should submit valid form', () => {
    // Arrange
    spyOn(component.formSubmitted, 'emit');
    component.userForm.patchValue({
      name: 'John Doe',
      email: 'john@example.com'
    });
    
    // Act
    component.onSubmit();
    
    // Assert
    expect(component.formSubmitted.emit).toHaveBeenCalledWith({
      name: 'John Doe',
      email: 'john@example.com'
    });
  });

  it('should not submit invalid form', () => {
    // Arrange
    spyOn(component.formSubmitted, 'emit');
    component.userForm.patchValue({ name: '', email: 'invalid' });
    
    // Act
    component.onSubmit();
    
    // Assert
    expect(component.formSubmitted.emit).not.toHaveBeenCalled();
  });
});
```


### **Testing with Signals (Modern Angular):**

```typescript
// signal-component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { signal } from '@angular/core';
import { SignalComponent } from './signal-component';

describe('SignalComponent', () => {
  let component: SignalComponent;
  let fixture: ComponentFixture<SignalComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [SignalComponent] // For standalone components
    }).compileComponents();

    fixture = TestBed.createComponent(SignalComponent);
    component = fixture.componentInstance;
  });

  it('should update signal value', () => {
    // Arrange
    const initialValue = component.count();
    
    // Act
    component.increment();
    
    // Assert
    expect(component.count()).toBe(initialValue + 1);
  });

  it('should update computed signal when dependency changes', () => {
    // Arrange
    component.count.set(5);
    
    // Act
    fixture.detectChanges();
    
    // Assert
    expect(component.doubleCount()).toBe(10);
  });

  it('should render signal values in template', () => {
    // Arrange
    component.count.set(42);
    
    // Act
    fixture.detectChanges();
    
    // Assert
    const countElement = fixture.nativeElement.querySelector('.count');
    expect(countElement.textContent).toContain('42');
  });
});
```


### **End-to-End Testing:**

```typescript
// app.e2e-spec.ts
import { browser, by, element } from 'protractor';

describe('User Management App', () => {
  beforeEach(() => {
    browser.get('/users');
  });

  it('should display users list', async () => {
    const userItems = element.all(by.css('.user-item'));
    expect(await userItems.count()).toBeGreaterThan(0);
  });

  it('should add new user', async () => {
    const nameInput = element(by.css('input[name="name"]'));
    const emailInput = element(by.css('input[name="email"]'));
    const submitButton = element(by.css('button[type="submit"]'));

    await nameInput.sendKeys('John Doe');
    await emailInput.sendKeys('john@example.com');
    await submitButton.click();

    const successMessage = element(by.css('.success-message'));
    expect(await successMessage.getText()).toContain('User created successfully');
  });

  it('should delete user', async () => {
    const firstDeleteButton = element.all(by.css('.delete-btn')).first();
    const initialCount = await element.all(by.css('.user-item')).count();

    await firstDeleteButton.click();
    
    // Confirm deletion
    const confirmButton = element(by.css('.confirm-delete'));
    await confirmButton.click();

    const finalCount = await element.all(by.css('.user-item')).count();
    expect(finalCount).toBe(initialCount - 1);
  });
});
```

[↑ Back to Top](#table-of-contents)

***

## 19. Performance Optimization

### **Definition:**

Performance optimization in Angular involves techniques and strategies to improve application speed, reduce bundle size, optimize rendering, and enhance user experience. This includes lazy loading, change detection optimization, tree shaking, and efficient resource management.

### **Why We Need Performance Optimization:**

- **User Experience**: Faster loading and responsive applications improve user satisfaction
- **SEO Benefits**: Search engines favor fast-loading websites
- **Mobile Performance**: Essential for good performance on slower devices and networks
- **Cost Efficiency**: Reduced server costs and bandwidth usage
- **Competitive Advantage**: Users prefer faster applications over slower alternatives
- **Accessibility**: Better performance benefits users with disabilities and assistive technologies
- **Business Impact**: Faster applications lead to higher conversion rates and user retention


### **Usage Scenarios:**

- Large-scale enterprise applications
- Mobile-first responsive applications
- E-commerce platforms with heavy content
- Real-time applications with frequent updates
- Applications with complex forms and data visualization
- Progressive Web Apps (PWAs)
- Applications serving users with slow internet connections


### **Bundle Optimization:**

```typescript
// Lazy loading modules
const routes: Routes = [
  {
    path: 'users',
    loadChildren: () => import('./features/users/users.module').then(m => m.UsersModule)
  },
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule)
  }
];

// Preloading strategy
@NgModule({
  imports: [RouterModule.forRoot(routes, {
    preloadingStrategy: PreloadAllModules // or custom strategy
  })]
})
export class AppRoutingModule {}

// Custom preloading strategy
export class CustomPreloadingStrategy implements PreloadingStrategy {
  preload(route: Route, load: () => Observable<any>): Observable<any> {
    // Only preload routes marked for preloading
    return route.data?.['preload'] ? load() : of(null);
  }
}
```


### **OnPush Change Detection:**

```typescript
@Component({
  selector: 'app-optimized-component',
  template: `
    <div>
      <h3>{{ title }}</h3>
      <p>Count: {{ count }}</p>
      <button (click)="increment()">Increment</button>
      
      <!-- Use trackBy for lists -->
      <ul>
        <li *ngFor="let item of items; trackBy: trackByFn">
          {{ item.name }} - {{ item.value }}
        </li>
      </ul>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  @Input() title: string = '';
  count = 0;
  items: Item[] = [];

  constructor(private cdr: ChangeDetectorRef) {}

  increment(): void {
    this.count++;
    this.cdr.markForCheck(); // Trigger change detection
  }

  // TrackBy function for optimal list rendering
  trackByFn(index: number, item: Item): number {
    return item.id; // Use unique identifier
  }

  updateItems(newItems: Item[]): void {
    // Use immutable updates for OnPush optimization
    this.items = [...newItems];
    this.cdr.markForCheck();
  }
}
```


### **Virtual Scrolling:**

```typescript
// Install CDK: npm install @angular/cdk
import { ScrollingModule } from '@angular/cdk/scrolling';

@Component({
  selector: 'app-virtual-scroll',
  template: `
    <cdk-virtual-scroll-viewport itemSize="50" class="viewport">
      <div *cdkVirtualFor="let item of items; trackBy: trackByFn" class="item">
        {{ item.name }} - {{ item.description }}
      </div>
    </cdk-virtual-scroll-viewport>
  `,
  styles: [`
    .viewport {
      height: 400px;
      width: 100%;
    }
    .item {
      height: 50px;
      padding: 10px;
      border-bottom: 1px solid #ccc;
    }
  `]
})
export class VirtualScrollComponent {
  items: LargeDataItem[] = this.generateLargeDataset();

  trackByFn(index: number, item: LargeDataItem): number {
    return item.id;
  }

  private generateLargeDataset(): LargeDataItem[] {
    return Array.from({ length: 10000 }, (_, i) => ({
      id: i,
      name: `Item ${i}`,
      description: `Description for item ${i}`
    }));
  }
}
```


### **Async Data Loading and Caching:**

```typescript
@Injectable({
  providedIn: 'root'
})
export class CachedDataService {
  private cache = new Map<string, { data: any; timestamp: number }>();
  private readonly CACHE_DURATION = 5 * 60 * 1000; // 5 minutes

  constructor(private http: HttpClient) {}

  getData(url: string, useCache: boolean = true): Observable<any> {
    if (useCache && this.isCacheValid(url)) {
      return of(this.cache.get(url)!.data);
    }

    return this.http.get(url).pipe(
      tap(data => this.setCache(url, data)),
      shareReplay(1) // Share the observable to prevent multiple HTTP calls
    );
  }

  private isCacheValid(key: string): boolean {
    const cached = this.cache.get(key);
    if (!cached) return false;
    
    return Date.now() - cached.timestamp < this.CACHE_DURATION;
  }

  private setCache(key: string, data: any): void {
    this.cache.set(key, {
      data,
      timestamp: Date.now()
    });
  }

  clearCache(): void {
    this.cache.clear();
  }
}
```


### **Image Optimization:**

```typescript
@Component({
  selector: 'app-optimized-images',
  template: `
    <!-- Modern image optimization -->
    <img 
      [src]="imageSrc" 
      [alt]="imageAlt"
      loading="lazy"
      [width]="imageWidth"
      [height]="imageHeight"
      (load)="onImageLoad()"
      (error)="onImageError()">
    
    <!-- Progressive image loading -->
    <div class="image-container">
      <img 
        *ngIf="!imageLoaded" 
        [src]="placeholderSrc" 
        class="placeholder">
      <img 
        [src]="highResSrc" 
        [style.opacity]="imageLoaded ? 1 : 0"
        (load)="imageLoaded = true"
        class="main-image">
    </div>
    
    <!-- Responsive images -->
    <picture>
      <source media="(max-width: 768px)" [srcset]="mobileSrc">
      <source media="(max-width: 1024px)" [srcset]="tabletSrc">
      <img [src]="desktopSrc" [alt]="imageAlt">
    </picture>
  `,
  styles: [`
    .image-container {
      position: relative;
    }
    .main-image {
      transition: opacity 0.3s ease;
    }
    .placeholder {
      filter: blur(5px);
    }
  `]
})
export class OptimizedImagesComponent {
  imageSrc = '';
  imageAlt = '';
  imageWidth = 300;
  imageHeight = 200;
  imageLoaded = false;
  
  placeholderSrc = 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzAwIiBoZWlnaHQ9IjIwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cmVjdCB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiBmaWxsPSIjY2NjIi8+PC9zdmc+';
  highResSrc = '';
  mobileSrc = '';
  tabletSrc = '';
  desktopSrc = '';

  onImageLoad(): void {
    console.log('Image loaded successfully');
  }

  onImageError(): void {
    console.error('Failed to load image');
    // Fallback to placeholder or default image
  }
}
```


### **Memory Management:**

```typescript
@Component({
  selector: 'app-memory-efficient',
  template: `<div>Memory efficient component</div>`
})
export class MemoryEfficientComponent implements OnInit, OnDestroy {
  private subscriptions = new Subscription();
  private intervalId?: number;
  private observers: ResizeObserver[] = [];

  constructor(
    private dataService: DataService,
    private destroyRef: DestroyRef // Angular 16+ for automatic cleanup
  ) {
    // Modern approach with takeUntilDestroyed
    this.dataService.getData()
      .pipe(takeUntilDestroyed(this.destroyRef))
      .subscribe(data => {
        // Handle data
      });
  }

  ngOnInit(): void {
    // Subscription management
    this.subscriptions.add(
      this.dataService.getLiveData().subscribe(data => {
        // Handle live data
      })
    );

    // Timer management
    this.intervalId = window.setInterval(() => {
      // Periodic task
    }, 1000);

    // DOM observer management
    const observer = new ResizeObserver(entries => {
      // Handle resize
    });
    this.observers.push(observer);
  }

  ngOnDestroy(): void {
    // Cleanup subscriptions
    this.subscriptions.unsubscribe();

    // Cleanup timers
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }

    // Cleanup observers
    this.observers.forEach(observer => observer.disconnect());
  }
}
```


### **Build Optimization (angular.json):**

```json
{
  "projects": {
    "my-app": {
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/my-app",
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "tsconfig.app.json",
            "optimization": true,
            "outputHashing": "all",
            "sourceMap": false,
            "extractCss": true,
            "namedChunks": false,
            "aot": true,
            "extractLicenses": true,
            "vendorChunk": false,
            "buildOptimizer": true,
            "budgets": [
              {
                "type": "initial",
                "maximumWarning": "2mb",
                "maximumError": "5mb"
              },
              {
                "type": "anyComponentStyle",
                "maximumWarning": "6kb",
                "maximumError": "10kb"
              }
            ]
          }
        }
      }
    }
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 20. Security

### **Definition:**

Security in Angular involves protecting applications from common web vulnerabilities and attacks. Angular provides built-in security features and best practices to help developers create secure applications that protect user data and prevent malicious attacks.

### **Why We Need Security:**

- **Data Protection**: Safeguard sensitive user information and business data
- **Trust Building**: Maintain user confidence and brand reputation
- **Compliance**: Meet regulatory requirements (GDPR, HIPAA, SOX, etc.)
- **Financial Protection**: Prevent financial losses from security breaches
- **Legal Requirements**: Avoid legal consequences of data breaches
- **Business Continuity**: Prevent disruptions from security incidents
- **Competitive Advantage**: Security-conscious users prefer secure applications


### **Usage Scenarios:**

- User authentication and authorization systems
- Financial and e-commerce applications
- Healthcare and personal data management
- Enterprise applications with sensitive business data
- API integration and data transmission
- File upload and content management systems
- Social applications with user-generated content


### **XSS (Cross-Site Scripting) Prevention:**

```typescript
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';

@Component({
  selector: 'app-safe-content',
  template: `
    <!-- Angular automatically sanitizes this -->
    <div>{{ userInput }}</div>
    
    <!-- For trusted HTML content -->
    <div [innerHTML]="sanitizedHtml"></div>
    
    <!-- NEVER do this - bypasses sanitization -->
    <!-- <div [innerHTML]="userInput"></div> -->
    
    <!-- Safe pipe for trusted content -->
    <div [innerHTML]="trustedContent | safeHtml"></div>
  `
})
export class SafeContentComponent {
  userInput = '<script>alert("XSS")</script>Safe content';
  sanitizedHtml: SafeHtml;
  trustedContent = '<p>This is <strong>trusted</strong> HTML content</p>';

  constructor(private sanitizer: DomSanitizer) {
    // Manually sanitize when needed
    this.sanitizedHtml = this.sanitizer.sanitize(
      SecurityContext.HTML, 
      this.userInput
    ) || '';
  }

  // Method to sanitize user input
  sanitizeInput(input: string): SafeHtml {
    return this.sanitizer.sanitize(SecurityContext.HTML, input) || '';
  }

  // Only use for truly trusted content
  bypassSanitization(trustedHtml: string): SafeHtml {
    return this.sanitizer.bypassSecurityTrustHtml(trustedHtml);
  }
}

// Safe HTML Pipe
@Pipe({ name: 'safeHtml' })
export class SafeHtmlPipe implements PipeTransform {
  constructor(private sanitizer: DomSanitizer) {}

  transform(value: string): SafeHtml {
    return this.sanitizer.bypassSecurityTrustHtml(value);
  }
}
```


### **Authentication and Authorization:**

```typescript
// JWT Token Service
@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private readonly TOKEN_KEY = 'auth_token';
  private readonly REFRESH_TOKEN_KEY = 'refresh_token';
  private tokenSubject = new BehaviorSubject<string | null>(null);
  
  public token$ = this.tokenSubject.asObservable();

  constructor(
    private http: HttpClient,
    private router: Router
  ) {
    // Initialize token from storage
    const token = localStorage.getItem(this.TOKEN_KEY);
    if (token && !this.isTokenExpired(token)) {
      this.tokenSubject.next(token);
    }
  }

  login(credentials: LoginCredentials): Observable<AuthResponse> {
    return this.http.post<AuthResponse>('/api/auth/login', credentials).pipe(
      tap(response => {
        this.setTokens(response.token, response.refreshToken);
        this.tokenSubject.next(response.token);
      })
    );
  }

  logout(): void {
    localStorage.removeItem(this.TOKEN_KEY);
    localStorage.removeItem(this.REFRESH_TOKEN_KEY);
    this.tokenSubject.next(null);
    this.router.navigate(['/login']);
  }

  refreshToken(): Observable<AuthResponse> {
    const refreshToken = localStorage.getItem(this.REFRESH_TOKEN_KEY);
    if (!refreshToken) {
      throw new Error('No refresh token available');
    }

    return this.http.post<AuthResponse>('/api/auth/refresh', { refreshToken }).pipe(
      tap(response => {
        this.setTokens(response.token, response.refreshToken);
        this.tokenSubject.next(response.token);
      }),
      catchError(error => {
        this.logout(); // Force logout on refresh failure
        return throwError(error);
      })
    );
  }

  isAuthenticated(): boolean {
    const token = this.tokenSubject.value;
    return token !== null && !this.isTokenExpired(token);
  }

  hasRole(role: string): boolean {
    const token = this.tokenSubject.value;
    if (!token) return false;

    try {
      const payload = JSON.parse(atob(token.split('.')[1]));
      return payload.roles?.includes(role) || false;
    } catch {
      return false;
    }
  }

  private setTokens(token: string, refreshToken: string): void {
    localStorage.setItem(this.TOKEN_KEY, token);
    localStorage.setItem(this.REFRESH_TOKEN_KEY, refreshToken);
  }

  private isTokenExpired(token: string): boolean {
    try {
      const payload = JSON.parse(atob(token.split('.')[1]));
      const expiry = payload.exp * 1000; // Convert to milliseconds
      return Date.now() > expiry;
    } catch {
      return true; // Consider invalid tokens as expired
    }
  }
}

// Auth Guard
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): boolean {
    if (this.authService.isAuthenticated()) {
      // Check for required roles
      const requiredRoles = route.data?.['roles'] as string[];
      if (requiredRoles) {
        const hasRequiredRole = requiredRoles.some(role => 
          this.authService.hasRole(role)
        );
        
        if (!hasRequiredRole) {
          this.router.navigate(['/forbidden']);
          return false;
        }
      }
      
      return true;
    }

    // Store attempted URL for redirecting after login
    this.router.navigate(['/login'], {
      queryParams: { returnUrl: state.url }
    });
    return false;
  }
}
```


### **HTTP Security:**

```typescript
// Security Headers Interceptor
@Injectable()
export class SecurityInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Add security headers
    const secureReq = req.clone({
      setHeaders: {
        'X-Content-Type-Options': 'nosniff',
        'X-Frame-Options': 'DENY',
        'X-XSS-Protection': '1; mode=block',
        'Strict-Transport-Security': 'max-age=31536000; includeSubDomains',
        'Content-Security-Policy': "default-src 'self'; script-src 'self' 'unsafe-inline'"
      }
    });

    return next.handle(secureReq);
  }
}

// CSRF Protection
@Injectable()
export class CsrfInterceptor implements HttpInterceptor {
  constructor(private cookieService: CookieService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Only add CSRF token for state-changing operations
    if (req.method === 'GET' || req.method === 'HEAD') {
      return next.handle(req);
    }

    const csrfToken = this.cookieService.get('XSRF-TOKEN');
    if (csrfToken) {
      const csrfReq = req.clone({
        setHeaders: {
          'X-XSRF-TOKEN': csrfToken
        }
      });
      return next.handle(csrfReq);
    }

    return next.handle(req);
  }
}
```


### **Secure Data Handling:**

```typescript
// Input Validation Service
@Injectable({
  providedIn: 'root'
})
export class ValidationService {
  // Email validation
  isValidEmail(email: string): boolean {
    const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    return emailRegex.test(email) && email.length <= 254;
  }

  // Password strength validation
  isStrongPassword(password: string): boolean {
    const minLength = 8;
    const hasUpperCase = /[A-Z]/.test(password);
    const hasLowerCase = /[a-z]/.test(password);
    const hasNumbers = /\d/.test(password);
    const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password);

    return password.length >= minLength && 
           hasUpperCase && 
           hasLowerCase && 
           hasNumbers && 
           hasSpecialChar;
  }

  // Sanitize user input
  sanitizeInput(input: string): string {
    return input
      .replace(/[<>]/g, '') // Remove angle brackets
      .replace(/javascript:/gi, '') // Remove javascript: protocol
      .replace(/on\w+=/gi, '') // Remove event handlers
      .trim();
  }

  // File upload validation
  isValidFileType(file: File, allowedTypes: string[]): boolean {
    return allowedTypes.includes(file.type);
  }

  isValidFileSize(file: File, maxSizeInMB: number): boolean {
    const maxSizeInBytes = maxSizeInMB * 1024 * 1024;
    return file.size <= maxSizeInBytes;
  }
}

// Secure File Upload Component
@Component({
  selector: 'app-secure-upload',
  template: `
    <div>
      <input 
        type="file" 
        (change)="onFileSelect($event)"
        accept=".jpg,.jpeg,.png,.pdf"
        #fileInput>
      <button (click)="upload()" [disabled]="!selectedFile || uploading">
        {{ uploading ? 'Uploading...' : 'Upload' }}
      </button>
      
      <div *ngIf="errorMessage" class="error">
        {{ errorMessage }}
      </div>
    </div>
  `
})
export class SecureUploadComponent {
  selectedFile: File | null = null;
  uploading = false;
  errorMessage = '';

  private readonly MAX_FILE_SIZE_MB = 5;
  private readonly ALLOWED_TYPES = [
    'image/jpeg',
    'image/png',
    'application/pdf'
  ];

  constructor(
    private http: HttpClient,
    private validationService: ValidationService
  ) {}

  onFileSelect(event: any): void {
    const file = event.target.files[0];
    if (!file) return;

    this.errorMessage = '';

    // Validate file type
    if (!this.validationService.isValidFileType(file, this.ALLOWED_TYPES)) {
      this.errorMessage = 'Invalid file type. Only JPG, PNG, and PDF files are allowed.';
      return;
    }

    // Validate file size
    if (!this.validationService.isValidFileSize(file, this.MAX_FILE_SIZE_MB)) {
      this.errorMessage = `File size must be less than ${this.MAX_FILE_SIZE_MB}MB.`;
      return;
    }

    this.selectedFile = file;
  }

  upload(): void {
    if (!this.selectedFile) return;

    this.uploading = true;
    const formData = new FormData();
    formData.append('file', this.selectedFile);

    this.http.post('/api/upload', formData, {
      reportProgress: true,
      observe: 'events'
    }).subscribe(
      event => {
        if (event.type === HttpEventType.UploadProgress && event.total) {
          const progress = Math.round(100 * event.loaded / event.total);
          console.log(`Upload progress: ${progress}%`);
        } else if (event.type === HttpEventType.Response) {
          console.log('Upload complete');
          this.uploading = false;
          this.selectedFile = null;
        }
      },
      error => {
        console.error('Upload failed:', error);
        this.errorMessage = 'Upload failed. Please try again.';
        this.uploading = false;
      }
    );
  }
}
```


### **Environment Security:**

```typescript
// environment.prod.ts
export const environment = {
  production: true,
  apiUrl: 'https://secure-api.example.com',
  enableLogging: false,
  
  // Don't store sensitive data in environment files
  // Use server-side configuration or environment variables instead
  features: {
    analytics: true,
    debugging: false
  }
};

// Secure Configuration Service
@Injectable({
  providedIn: 'root'
})
export class ConfigService {
  private config: AppConfig | null = null;

  constructor(private http: HttpClient) {}

  // Load configuration from server
  loadConfig(): Promise<AppConfig> {
    return this.http.get<AppConfig>('/api/config').toPromise().then(
      config => {
        this.config = config;
        return config;
      }
    );
  }

  get apiUrl(): string {
    return this.config?.apiUrl || environment.apiUrl;
  }

  get features(): FeatureFlags {
    return this.config?.features || environment.features;
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 21. Animations

### **Definition:**

Angular Animations provide a way to create smooth, engaging transitions and effects in your application. Built on the Web Animations API, Angular's animation system allows you to define complex animation sequences using a declarative syntax that integrates seamlessly with component lifecycle and state changes.

### **Why We Need Animations:**

- **User Experience**: Provide visual feedback and guide user attention
- **Professional Polish**: Make applications feel more refined and modern
- **State Communication**: Visually communicate state changes and transitions
- **Engagement**: Keep users engaged with smooth, interactive interfaces
- **Accessibility**: Help users understand interface changes and navigation
- **Brand Identity**: Create distinctive visual experiences that reinforce branding
- **Performance**: Hardware-accelerated animations provide smooth 60fps experiences


### **Usage Scenarios:**

- Page transitions and route changes
- Form validation feedback and state changes
- Loading states and progress indicators
- Modal dialogs and overlay animations
- List item additions, removals, and reordering
- Hover effects and interactive elements
- Data visualization and chart animations


### **Basic Animation Setup:**

```typescript
// app.module.ts
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

@NgModule({
  imports: [
    BrowserModule,
    BrowserAnimationsModule // Required for animations
  ]
})
export class AppModule { }

// Basic animation component
import { Component } from '@angular/core';
import { trigger, state, style, transition, animate } from '@angular/animations';

@Component({
  selector: 'app-basic-animation',
  template: `
    <div [@slideIn]="'in'" class="container">
      <h2>Animated Content</h2>
      <button (click)="toggle()" [@fadeInOut]="isVisible ? 'in' : 'out'">
        {{ isVisible ? 'Hide' : 'Show' }}
      </button>
      
      <div *ngIf="isVisible" [@slideUp]="'in'" class="content">
        <p>This content slides up when shown!</p>
      </div>
    </div>
  `,
  animations: [
    // Slide in from left
    trigger('slideIn', [
      transition(':enter', [
        style({ transform: 'translateX(-100%)', opacity: 0 }),
        animate('300ms ease-in', style({ transform: 'translateX(0%)', opacity: 1 }))
      ])
    ]),
    
    // Fade in/out
    trigger('fadeInOut', [
      state('in', style({ opacity: 1 })),
      state('out', style({ opacity: 0 })),
      transition('in <=> out', animate('300ms ease-in-out'))
    ]),
    
    // Slide up
    trigger('slideUp', [
      transition(':enter', [
        style({ transform: 'translateY(100%)', opacity: 0 }),
        animate('400ms cubic-bezier(0.25, 0.8, 0.25, 1)', 
          style({ transform: 'translateY(0%)', opacity: 1 }))
      ]),
      transition(':leave', [
        animate('300ms ease-in', 
          style({ transform: 'translateY(-100%)', opacity: 0 }))
      ])
    ])
  ]
})
export class BasicAnimationComponent {
  isVisible = true;
  
  toggle(): void {
    this.isVisible = !this.isVisible;
  }
}
```


### **Advanced Animations:**

```typescript
@Component({
  selector: 'app-advanced-animations',
  template: `
    <div class="animation-container">
      <!-- Stagger animation for lists -->
      <ul>
        <li *ngFor="let item of items; let i = index" 
            [@staggerIn]="i"
            (click)="removeItem(i)">
          {{ item.name }}
        </li>
      </ul>
      
      <!-- Complex state-based animation -->
      <div [@cardFlip]="cardState" 
           (click)="flipCard()"
           class="card">
        <div class="front">Front</div>
        <div class="back">Back</div>
      </div>
      
      <!-- Route transition outlet -->
      <div [@routeAnimation]="prepareRoute(outlet)">
        <router-outlet #outlet="outlet"></router-outlet>
      </div>
    </div>
  `,
  animations: [
    // Stagger animation for list items
    trigger('staggerIn', [
      transition('* => *', [
        query(':enter', [
          style({ opacity: 0, transform: 'translateX(-100px)' }),
          stagger(100, [
            animate('300ms ease-out', 
              style({ opacity: 1, transform: 'translateX(0px)' }))
          ])
        ], { optional: true })
      ])
    ]),
    
    // Card flip animation
    trigger('cardFlip', [
      state('front', style({ transform: 'rotateY(0deg)' })),
      state('back', style({ transform: 'rotateY(180deg)' })),
      transition('front <=> back', [
        animate('600ms ease-in-out')
      ])
    ]),
    
    // Route transition animation
    trigger('routeAnimation', [
      transition('* <=> *', [
        query(':enter, :leave', [
          style({
            position: 'absolute',
            top: 0,
            left: 0,
            width: '100%'
          })
        ], { optional: true }),
        
        query(':enter', [
          style({ transform: 'translateX(100%)' })
        ], { optional: true }),
        
        query(':leave', [
          animate('300ms ease-out', style({ transform: 'translateX(-100%)' }))
        ], { optional: true }),
        
        query(':enter', [
          animate('300ms ease-out', style({ transform: 'translateX(0%)' }))
        ], { optional: true })
      ])
    ])
  ]
})
export class AdvancedAnimationsComponent {
  items = [
    { name: 'Item 1' },
    { name: 'Item 2' },
    { name: 'Item 3' }
  ];
  
  cardState = 'front';
  
  removeItem(index: number): void {
    this.items.splice(index, 1);
  }
  
  flipCard(): void {
    this.cardState = this.cardState === 'front' ? 'back' : 'front';
  }
  
  prepareRoute(outlet: RouterOutlet): string {
    return outlet?.activatedRouteData?.['animation'] || '';
  }
}
```


### **Programmatic Animations:**

```typescript
import { AnimationBuilder, AnimationFactory } from '@angular/animations';

@Component({
  selector: 'app-programmatic-animation',
  template: `
    <div #animatedElement class="animated-box">
      Animated Element
    </div>
    <button (click)="animateElement()">Animate</button>
  `
})
export class ProgrammaticAnimationComponent {
  @ViewChild('animatedElement', { static: true }) element!: ElementRef;
  
  constructor(private animationBuilder: AnimationBuilder) {}
  
  animateElement(): void {
    const factory: AnimationFactory = this.animationBuilder.build([
      style({ transform: 'scale(1) rotate(0deg)' }),
      animate('1000ms cubic-bezier(0.25, 0.8, 0.25, 1)', 
        style({ transform: 'scale(1.2) rotate(360deg)' })),
      animate('500ms ease-out', 
        style({ transform: 'scale(1) rotate(360deg)' }))
    ]);
    
    const player = factory.create(this.element.nativeElement);
    player.play();
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 22. Progressive Web Apps (PWA)

### **Definition:**

Progressive Web Apps (PWAs) are web applications that use modern web capabilities to deliver app-like experiences to users. They combine the best of web and mobile apps, providing reliable, fast, and engaging experiences that work offline and can be installed on devices.

### **Why We Need PWAs:**

- **Offline Functionality**: Work without internet connection using service workers
- **App-like Experience**: Native app feel with web technology flexibility
- **Installation**: Can be installed on devices without app stores
- **Performance**: Fast loading and smooth interactions
- **Push Notifications**: Re-engage users with notifications
- **Cross-Platform**: Single codebase works on all platforms
- **Cost Effective**: Reduced development and maintenance costs compared to native apps
- **SEO Benefits**: Discoverable by search engines unlike native apps


### **Usage Scenarios:**

- E-commerce applications for shopping offline
- News and content apps for offline reading
- Social media platforms for consistent engagement
- Business applications for field workers
- Educational apps for offline learning
- Gaming applications with offline capabilities
- Travel apps for offline maps and information


### **PWA Setup with Angular:**

```bash
# Add PWA support to existing Angular app
ng add @angular/pwa

# This automatically adds:
# - Service worker configuration
# - Web app manifest
# - Icon files
# - PWA-specific configurations
```


### **Service Worker Configuration:**

```typescript
// app.module.ts
import { ServiceWorkerModule } from '@angular/service-worker';

@NgModule({
  imports: [
    BrowserModule,
    ServiceWorkerModule.register('ngsw-worker.js', {
      enabled: environment.production,
      registrationStrategy: 'registerWhenStable:30000'
    })
  ]
})
export class AppModule { }

// PWA Service
@Injectable({
  providedIn: 'root'
})
export class PwaService {
  private promptEvent: any;
  
  constructor(private swUpdate: SwUpdate) {
    if (swUpdate.isEnabled) {
      // Check for updates
      swUpdate.versionUpdates.subscribe(event => {
        if (event.type === 'VERSION_READY') {
          this.showUpdateNotification();
        }
      });
      
      // Listen for installation prompt
      window.addEventListener('beforeinstallprompt', event => {
        event.preventDefault();
        this.promptEvent = event;
      });
    }
  }
  
  // Check for app updates
  checkForUpdate(): void {
    if (this.swUpdate.isEnabled) {
      this.swUpdate.checkForUpdate().then(() => {
        console.log('Checking for updates...');
      });
    }
  }
  
  // Install app
  installApp(): Promise<void> {
    return new Promise((resolve, reject) => {
      if (this.promptEvent) {
        this.promptEvent.prompt();
        this.promptEvent.userChoice.then((choiceResult: any) => {
          if (choiceResult.outcome === 'accepted') {
            console.log('App installed');
            resolve();
          } else {
            console.log('Installation dismissed');
            reject('Installation dismissed');
          }
          this.promptEvent = null;
        });
      } else {
        reject('Installation not available');
      }
    });
  }
  
  // Show update notification
  private showUpdateNotification(): void {
    const updateConfirm = confirm('New version available. Update now?');
    if (updateConfirm) {
      window.location.reload();
    }
  }
  
  // Check if app is installed
  isInstalled(): boolean {
    return window.matchMedia('(display-mode: standalone)').matches;
  }
}
```


### **Offline Data Management:**

```typescript
// Offline Service
@Injectable({
  providedIn: 'root'
})
export class OfflineService {
  private readonly CACHE_KEY = 'offline-data';
  
  constructor(private http: HttpClient) {}
  
  // Get data with offline fallback
  getData(url: string): Observable<any> {
    return this.http.get(url).pipe(
      tap(data => this.cacheData(url, data)),
      catchError(error => {
        console.warn('Online request failed, trying cache:', error);
        return this.getCachedData(url);
      })
    );
  }
  
  // Cache data locally
  private cacheData(key: string, data: any): void {
    const cached = this.getAllCachedData();
    cached[key] = {
      data,
      timestamp: Date.now()
    };
    localStorage.setItem(this.CACHE_KEY, JSON.stringify(cached));
  }
  
  // Retrieve cached data
  private getCachedData(key: string): Observable<any> {
    const cached = this.getAllCachedData();
    const item = cached[key];
    
    if (item) {
      return of(item.data);
    } else {
      return throwError('No cached data available');
    }
  }
  
  // Get all cached data
  private getAllCachedData(): any {
    const cached = localStorage.getItem(this.CACHE_KEY);
    return cached ? JSON.parse(cached) : {};
  }
  
  // Clear old cache
  clearOldCache(maxAge: number = 24 * 60 * 60 * 1000): void {
    const cached = this.getAllCachedData();
    const now = Date.now();
    
    Object.keys(cached).forEach(key => {
      if (now - cached[key].timestamp > maxAge) {
        delete cached[key];
      }
    });
    
    localStorage.setItem(this.CACHE_KEY, JSON.stringify(cached));
  }
}
```


### **Push Notifications:**

```typescript
// Push Notification Service
@Injectable({
  providedIn: 'root'
})
export class PushNotificationService {
  constructor(private swPush: SwPush, private http: HttpClient) {}
  
  // Subscribe to push notifications
  subscribeToNotifications(): Promise<PushSubscription | null> {
    if (!this.swPush.isEnabled) {
      console.warn('Push notifications not supported');
      return Promise.resolve(null);
    }
    
    return this.swPush.requestSubscription({
      serverPublicKey: environment.vapidPublicKey
    }).then(subscription => {
      // Send subscription to server
      this.sendSubscriptionToServer(subscription);
      return subscription;
    }).catch(error => {
      console.error('Push notification subscription failed:', error);
      return null;
    });
  }
  
  // Listen for push messages
  listenForMessages(): void {
    if (this.swPush.isEnabled) {
      this.swPush.messages.subscribe(message => {
        console.log('Push message received:', message);
        this.showNotification(message);
      });
      
      this.swPush.notificationClicks.subscribe(event => {
        console.log('Notification clicked:', event);
        // Handle notification click
      });
    }
  }
  
  // Send subscription to server
  private sendSubscriptionToServer(subscription: PushSubscription): void {
    this.http.post('/api/push/subscribe', {
      subscription: subscription.toJSON()
    }).subscribe(
      response => console.log('Subscription sent to server:', response),
      error => console.error('Failed to send subscription:', error)
    );
  }
  
  // Show local notification
  private showNotification(message: any): void {
    if ('Notification' in window && Notification.permission === 'granted') {
      new Notification(message.title, {
        body: message.body,
        icon: message.icon || '/assets/icons/icon-72x72.png',
        badge: '/assets/icons/icon-72x72.png',
        vibrate: [200, 100, 200]
      });
    }
  }
}
```


### **PWA Component:**

```typescript
@Component({
  selector: 'app-pwa-controls',
  template: `
    <div class="pwa-controls">
      <div *ngIf="!isInstalled && canInstall" class="install-prompt">
        <p>Install this app for a better experience!</p>
        <button (click)="installApp()" class="install-btn">
          Install App
        </button>
      </div>
      
      <div class="pwa-status">
        <p>Connection: {{ isOnline ? 'Online' : 'Offline' }}</p>
        <p>App Status: {{ isInstalled ? 'Installed' : 'Browser' }}</p>
        
        <button (click)="checkForUpdates()">Check for Updates</button>
        <button (click)="enableNotifications()" 
                [disabled]="notificationsEnabled">
          {{ notificationsEnabled ? 'Notifications Enabled' : 'Enable Notifications' }}
        </button>
      </div>
      
      <div *ngIf="updateAvailable" class="update-prompt">
        <p>New version available!</p>
        <button (click)="updateApp()">Update Now</button>
      </div>
    </div>
  `,
  styles: [`
    .pwa-controls {
      padding: 16px;
      border: 1px solid #ddd;
      border-radius: 8px;
      margin: 16px 0;
    }
    
    .install-prompt, .update-prompt {
      background: #e3f2fd;
      padding: 12px;
      border-radius: 4px;
      margin-bottom: 16px;
    }
    
    .install-btn {
      background: #2196f3;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class PwaControlsComponent implements OnInit {
  isOnline = navigator.onLine;
  isInstalled = false;
  canInstall = false;
  updateAvailable = false;
  notificationsEnabled = false;
  
  constructor(
    private pwaService: PwaService,
    private pushService: PushNotificationService,
    private swUpdate: SwUpdate
  ) {}
  
  ngOnInit(): void {
    // Monitor online status
    window.addEventListener('online', () => this.isOnline = true);
    window.addEventListener('offline', () => this.isOnline = false);
    
    // Check installation status
    this.isInstalled = this.pwaService.isInstalled();
    
    // Listen for installation availability
    window.addEventListener('beforeinstallprompt', () => {
      this.canInstall = true;
    });
    
    // Check for updates
    if (this.swUpdate.isEnabled) {
      this.swUpdate.versionUpdates.subscribe(event => {
        if (event.type === 'VERSION_READY') {
          this.updateAvailable = true;
        }
      });
    }
    
    // Setup push notifications
    this.pushService.listenForMessages();
  }
  
  installApp(): void {
    this.pwaService.installApp().then(() => {
      this.canInstall = false;
      this.isInstalled = true;
    }).catch(error => {
      console.error('Installation failed:', error);
    });
  }
  
  checkForUpdates(): void {
    this.pwaService.checkForUpdate();
  }
  
  updateApp(): void {
    window.location.reload();
  }
  
  enableNotifications(): void {
    this.pushService.subscribeToNotifications().then(subscription => {
      if (subscription) {
        this.notificationsEnabled = true;
        console.log('Notifications enabled');
      }
    });
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 23. Server-Side Rendering (SSR)

### **Definition:**

Server-Side Rendering (SSR) in Angular is the process of rendering Angular applications on the server before sending them to the client. This technique improves initial page load performance, SEO, and provides better user experience, especially for users with slower devices or connections.

### **Why We Need SSR:**

- **SEO Optimization**: Search engines can crawl and index pre-rendered content
- **Performance**: Faster initial page load and Time to First Contentful Paint (FCP)
- **Social Media Sharing**: Proper meta tags and Open Graph data for social platforms
- **Accessibility**: Better experience for screen readers and assistive technologies
- **Mobile Performance**: Improved performance on slower mobile devices
- **Core Web Vitals**: Better Largest Contentful Paint (LCP) and Cumulative Layout Shift (CLS)
- **User Experience**: Instant content visibility without JavaScript loading delays


### **Usage Scenarios:**

- E-commerce product pages for SEO
- Blog and news websites for content indexing
- Marketing and landing pages
- Corporate websites with static content
- Applications requiring social media sharing
- Mobile-first applications
- Applications targeting users with slow connections


### **Angular Universal Setup:**

```bash
# Add SSR to existing Angular application
ng add @nguniversal/express-engine

# This adds:
# - Server-side rendering configuration
# - Express server setup
# - Build scripts for SSR
# - Pre-rendering capabilities
```


### **SSR Configuration:**

```typescript
// app.server.module.ts
import { NgModule } from '@angular/core';
import { ServerModule } from '@angular/platform-server';
import { AppModule } from './app.module';
import { AppComponent } from './app.component';

@NgModule({
  imports: [
    AppModule,
    ServerModule
  ],
  bootstrap: [AppComponent]
})
export class AppServerModule {}

// server.ts
import 'zone.js/dist/zone-node';
import { ngExpressEngine } from '@nguniversal/express-engine';
import { existsSync } from 'fs';
import express from 'express';
import { APP_BASE_HREF } from '@angular/common';

import { AppServerModule } from './src/main.server';

const app = express();
const PORT = process.env['PORT'] || 4000;
const DIST_FOLDER = join(process.cwd(), 'dist');

// Set the engine
app.engine('html', ngExpressEngine({
  bootstrap: AppServerModule,
}));

app.set('view engine', 'html');
app.set('views', DIST_FOLDER);

// Serve static files
app.get('*.*', express.static(DIST_FOLDER));

// All regular routes use Universal
app.get('*', (req, res) => {
  res.render('index', { req, providers: [{ provide: APP_BASE_HREF, useValue: req.baseUrl }] });
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```


### **Platform-Specific Code:**

```typescript
import { isPlatformBrowser, isPlatformServer } from '@angular/common';
import { Inject, PLATFORM_ID } from '@angular/core';

@Component({
  selector: 'app-platform-aware',
  template: `
    <div>
      <h3>Platform-Aware Component</h3>
      <p *ngIf="isBrowser">This renders only in browser</p>
      <p *ngIf="isServer">This renders only on server</p>
      
      <div #clientOnlyContent>
        <!-- This content is hidden during SSR -->
        <canvas #myCanvas width="400" height="200"></canvas>
      </div>
    </div>
  `
})
export class PlatformAwareComponent implements OnInit, AfterViewInit {
  isBrowser: boolean;
  isServer: boolean;
  
  @ViewChild('myCanvas') canvas?: ElementRef<HTMLCanvasElement>;
  
  constructor(@Inject(PLATFORM_ID) private platformId: Object) {
    this.isBrowser = isPlatformBrowser(this.platformId);
    this.isServer = isPlatformServer(this.platformId);
  }
  
  ngOnInit(): void {
    if (this.isBrowser) {
      // Browser-only initialization
      this.initializeBrowserFeatures();
    }
    
    if (this.isServer) {
      // Server-only initialization
      this.initializeServerFeatures();
    }
  }
  
  ngAfterViewInit(): void {
    if (this.isBrowser && this.canvas) {
      // Canvas operations only in browser
      const ctx = this.canvas.nativeElement.getContext('2d');
      if (ctx) {
        ctx.fillStyle = 'blue';
        ctx.fillRect(10, 10, 100, 100);
      }
    }
  }
  
  private initializeBrowserFeatures(): void {
    // localStorage, sessionStorage, window objects
    const theme = localStorage.getItem('theme') || 'light';
    console.log('Browser theme:', theme);
  }
  
  private initializeServerFeatures(): void {
    // Server-side data fetching, file system access
    console.log('Server-side initialization');
  }
}
```


### **SEO and Meta Tags:**

```typescript
import { Injectable } from '@angular/core';
import { Meta, Title } from '@angular/platform-browser';

@Injectable({
  providedIn: 'root'
})
export class SeoService {
  constructor(
    private meta: Meta,
    private title: Title
  ) {}
  
  updatePageMetadata(data: {
    title?: string;
    description?: string;
    keywords?: string;
    image?: string;
    url?: string;
    type?: string;
  }): void {
    // Update page title
    if (data.title) {
      this.title.setTitle(data.title);
    }
    
    // Update meta tags
    const metaTags = [
      { name: 'description', content: data.description || '' },
      { name: 'keywords', content: data.keywords || '' },
      { name: 'robots', content: 'index,follow' },
      
      // Open Graph tags for social media
      { property: 'og:title', content: data.title || '' },
      { property: 'og:description', content: data.description || '' },
      { property: 'og:image', content: data.image || '' },
      { property: 'og:url', content: data.url || '' },
      { property: 'og:type', content: data.type || 'website' },
      
      // Twitter Card tags
      { name: 'twitter:card', content: 'summary_large_image' },
      { name: 'twitter:title', content: data.title || '' },
      { name: 'twitter:description', content: data.description || '' },
      { name: 'twitter:image', content: data.image || '' }
    ];
    
    metaTags.forEach(tag => {
      if (tag.content) {
        if (tag.name) {
          this.meta.updateTag({ name: tag.name, content: tag.content });
        } else if (tag.property) {
          this.meta.updateTag({ property: tag.property, content: tag.content });
        }
      }
    });
  }
  
  // Remove meta tags
  removeMetaTags(): void {
    this.meta.removeTag('name="description"');
    this.meta.removeTag('name="keywords"');
    this.meta.removeTag('property="og:title"');
    this.meta.removeTag('property="og:description"');
    // ... remove other tags as needed
  }
}

// Usage in component
@Component({
  selector: 'app-product-detail',
  template: `
    <div *ngIf="product">
      <h1>{{ product.name }}</h1>
      <p>{{ product.description }}</p>
      <img [src]="product.image" [alt]="product.name">
    </div>
  `
})
export class ProductDetailComponent implements OnInit {
  product?: Product;
  
  constructor(
    private route: ActivatedRoute,
    private productService: ProductService,
    private seoService: SeoService
  ) {}
  
  ngOnInit(): void {
    const productId = this.route.snapshot.paramMap.get('id');
    if (productId) {
      this.productService.getProduct(+productId).subscribe(product => {
        this.product = product;
        this.updateSEO(product);
      });
    }
  }
  
  private updateSEO(product: Product): void {
    this.seoService.updatePageMetadata({
      title: `${product.name} - Our Store`,
      description: product.description,
      keywords: product.tags?.join(', '),
      image: product.image,
      url: `https://ourstore.com/products/${product.id}`,
      type: 'product'
    });
  }
}
```


### **Data Transfer State:**

```typescript
// Prevent duplicate HTTP calls between server and client
import { BrowserTransferStateModule, TransferState, makeStateKey } from '@angular/platform-browser';
import { ServerTransferStateModule } from '@angular/platform-server';

// app.module.ts
@NgModule({
  imports: [
    BrowserModule.withServerTransition({ appId: 'my-app' }),
    BrowserTransferStateModule,
    // other imports
  ]
})
export class AppModule {}

// app.server.module.ts
@NgModule({
  imports: [
    AppModule,
    ServerModule,
    ServerTransferStateModule
  ]
})
export class AppServerModule {}

// Data service with transfer state
@Injectable({
  providedIn: 'root'
})
export class DataService {
  constructor(
    private http: HttpClient,
    private transferState: TransferState,
    @Inject(PLATFORM_ID) private platformId: Object
  ) {}
  
  getUsers(): Observable<User[]> {
    const USERS_KEY = makeStateKey<User[]>('users');
    
    // Check if data exists in transfer state
    if (this.transferState.hasKey(USERS_KEY)) {
      const users = this.transferState.get(USERS_KEY, []);
      this.transferState.remove(USERS_KEY); // Clean up
      return of(users);
    }
    
    // Fetch from API
    return this.http.get<User[]>('/api/users').pipe(
      tap(users => {
        // Store in transfer state if on server
        if (isPlatformServer(this.platformId)) {
          this.transferState.set(USERS_KEY, users);
        }
      })
    );
  }
}
```


### **Pre-rendering for Static Content:**

```bash
# Build and pre-render static routes
npm run build:ssr
npm run prerender

# Or build specific routes
ng run my-app:prerender --routes /home /about /contact
```

```typescript
// angular.json - prerender configuration
{
  "projects": {
    "my-app": {
      "architect": {
        "prerender": {
          "builder": "@nguniversal/builders:prerender",
          "options": {
            "routes": [
              "/",
              "/home",
              "/about",
              "/contact",
              "/products/1",
              "/products/2"
            ]
          }
        }
      }
    }
  }
}

// Dynamic route discovery for prerendering
export async function getRoutes(): Promise<string[]> {
  // Fetch dynamic routes from API or database
  const products = await fetch('https://api.example.com/products').then(r => r.json());
  const productRoutes = products.map((p: any) => `/products/${p.id}`);
  
  return [
    '/',
    '/home',
    '/about',
    '/contact',
    ...productRoutes
  ];
}
```

[↑ Back to Top](#table-of-contents)

***

## 24. Deployment

### **Definition:**

Deployment in Angular refers to the process of building, optimizing, and hosting your Angular application for production use. This involves creating optimized bundles, configuring servers, setting up continuous integration/deployment pipelines, and ensuring optimal performance and security in production environments.

### **Why We Need Proper Deployment:**

- **Performance**: Optimized builds provide faster loading and better user experience
- **Security**: Proper deployment ensures secure hosting and data protection
- **Scalability**: Deploy to infrastructure that can handle user load
- **Reliability**: Ensure high availability and minimal downtime
- **Cost Efficiency**: Optimize hosting costs through efficient deployment strategies
- **Monitoring**: Track application performance and errors in production
- **Continuous Delivery**: Enable rapid and reliable feature releases
- **Global Reach**: Deploy to CDNs for worldwide content delivery


### **Usage Scenarios:**

- Production web application hosting
- Staging environments for testing
- Multi-environment deployments (dev, test, prod)
- Mobile app hybrid deployment
- Enterprise internal applications
- Static site hosting for marketing pages
- Microservice architecture deployment


### **Production Build:**

```bash
# Production build with optimization
ng build --configuration=production

# Or using the shorthand
ng build --prod

# Build with specific configuration
ng build --configuration=staging

# Analyze bundle size
ng build --stats-json
npx webpack-bundle-analyzer dist/my-app/stats.json
```


### **Build Configurations:**

```json
// angular.json - build configurations
{
  "projects": {
    "my-app": {
      "architect": {
        "build": {
          "configurations": {
            "production": {
              "budgets": [
                {
                  "type": "initial",
                  "maximumWarning": "2mb",
                  "maximumError": "5mb"
                }
              ],
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.prod.ts"
                }
              ],
              "outputHashing": "all",
              "optimization": true,
              "sourceMap": false,
              "namedChunks": false,
              "extractLicenses": true,
              "vendorChunk": false,
              "buildOptimizer": true
            },
            "staging": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.staging.ts"
                }
              ],
              "optimization": true,
              "sourceMap": true,
              "budgets": [
                {
                  "type": "anyComponentStyle",
                  "maximumWarning": "6kb"
                }
              ]
            }
          }
        }
      }
    }
  }
}
```


### **Environment Configuration:**

```typescript
// environment.prod.ts
export const environment = {
  production: true,
  apiUrl: 'https://api.myapp.com',
  enableAnalytics: true,
  logLevel: 'error',
  features: {
    enablePayments: true,
    enableNotifications: true
  }
};

// environment.staging.ts
export const environment = {
  production: false,
  apiUrl: 'https://staging-api.myapp.com',
  enableAnalytics: false,
  logLevel: 'debug',
  features: {
    enablePayments: false,
    enableNotifications: true
  }
};

// environment.ts (development)
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000/api',
  enableAnalytics: false,
  logLevel: 'debug',
  features: {
    enablePayments: false,
    enableNotifications: false
  }
};
```


### **Static Hosting (Netlify/Vercel):**

```json
// netlify.toml
[build]
  command = "npm run build:prod"
  publish = "dist/my-app"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[build.environment]
  NODE_VERSION = "18"

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    X-Content-Type-Options = "nosniff"

[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000"
```

```json
// vercel.json
{
  "buildCommand": "ng build --configuration=production",
  "outputDirectory": "dist/my-app",
  "rewrites": [
    {
      "source": "/**",
      "destination": "/index.html"
    }
  ],
  "headers": [
    {
      "source": "/assets/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ]
}
```


### **Docker Deployment:**

```dockerfile
# Dockerfile for Angular app
# Build stage
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build:prod

# Production stage
FROM nginx:alpine

# Copy built app
COPY --from=builder /app/dist/my-app /usr/share/nginx/html

# Copy nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

```nginx
# nginx.conf
events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1000;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    
    server {
        listen 80;
        server_name localhost;
        root /usr/share/nginx/html;
        index index.html;
        
        # Security headers
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-XSS-Protection "1; mode=block" always;
        add_header X-Content-Type-Options "nosniff" always;
        
        # Cache static assets
        location /assets/ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }
        
        # Handle Angular routing
        location / {
            try_files $uri $uri/ /index.html;
        }
        
        # Health check endpoint
        location /health {
            access_log off;
            return 200 "healthy\n";
            add_header Content-Type text/plain;
        }
    }
}
```


### **CI/CD Pipeline (GitHub Actions):**

```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm run test:ci
    
    - name: Run e2e tests
      run: npm run e2e:ci
    
    - name: Lint
      run: npm run lint

  build-and-deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build application
      run: npm run build:prod
      env:
        NODE_ENV: production
    
    - name: Deploy to Netlify
      uses: nwtgck/actions-netlify@v1.2
      with:
        publish-dir: './dist/my-app'
        production-branch: main
        github-token: ${{ secrets.GITHUB_TOKEN }}
        deploy-message: "Deploy from GitHub Actions"
      env:
        NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
        NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
```


### **Monitoring and Error Tracking:**

```typescript
// Error tracking service
@Injectable({
  providedIn: 'root'
})
export class ErrorTrackingService {
  constructor() {
    if (environment.production) {
      // Initialize error tracking (Sentry, LogRocket, etc.)
      this.initializeSentry();
    }
  }
  
  private initializeSentry(): void {
    // Sentry configuration
    import('@sentry/angular').then(Sentry => {
      Sentry.init({
        dsn: environment.sentryDsn,
        environment: environment.production ? 'production' : 'development',
        tracesSampleRate: 0.1
      });
    });
  }
  
  logError(error: any, context?: any): void {
    if (environment.production) {
      // Send to error tracking service
      console.error('Production error:', error, context);
    } else {
      console.error('Development error:', error, context);
    }
  }
  
  logEvent(event: string, data?: any): void {
    if (environment.enableAnalytics) {
      // Send to analytics service
      console.log('Analytics event:', event, data);
    }
  }
}

// Global error handler
@Injectable()
export class GlobalErrorHandler implements ErrorHandler {
  constructor(private errorTracking: ErrorTrackingService) {}
  
  handleError(error: any): void {
    this.errorTracking.logError(error);
    console.error('Global error:', error);
  }
}

// app.module.ts
@NgModule({
  providers: [
    { provide: ErrorHandler, useClass: GlobalErrorHandler }
  ]
})
export class AppModule {}
```


### **Performance Monitoring:**

```typescript
// Performance monitoring service
@Injectable({
  providedIn: 'root'
})
export class PerformanceService {
  constructor() {
    if (environment.production) {
      this.initializePerformanceMonitoring();
    }
  }
  
  private initializePerformanceMonitoring(): void {
    // Monitor Core Web Vitals
    this.measureWebVitals();
    
    // Monitor route changes
    this.monitorRoutePerformance();
  }
  
  private measureWebVitals(): void {
    if ('web-vitals' in window) {
      import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
        getCLS(this.sendToAnalytics);
        getFID(this.sendToAnalytics);
        getFCP(this.sendToAnalytics);
        getLCP(this.sendToAnalytics);
        getTTFB(this.sendToAnalytics);
      });
    }
  }
  
  private sendToAnalytics = (metric: any): void => {
    console.log('Performance metric:', metric);
    // Send to analytics service
  }
  
  private monitorRoutePerformance(): void {
    // Monitor navigation timing
    const observer = new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        if (entry.entryType === 'navigation') {
          this.logNavigationTiming(entry as PerformanceNavigationTiming);
        }
      }
    });
    
    observer.observe({ entryTypes: ['navigation'] });
  }
  
  private logNavigationTiming(timing: PerformanceNavigationTiming): void {
    const metrics = {
      domContentLoaded: timing.domContentLoadedEventEnd - timing.navigationStart,
      firstByte: timing.responseStart - timing.requestStart,
      domComplete: timing.domComplete - timing.navigationStart
    };
    
    console.log('Navigation timing:', metrics);
  }
}
```

[↑ Back to Top](#table-of-contents)

***

## 25. Best Practices

### **Definition:**

Best practices in Angular are established guidelines, patterns, and conventions that help developers write maintainable, scalable, and performant applications. These practices are derived from community experience, Angular team recommendations, and proven patterns that improve code quality and development efficiency.

### **Why We Need Best Practices:**

- **Code Quality**: Maintain high standards and consistency across the codebase
- **Team Collaboration**: Enable multiple developers to work efficiently together
- **Maintainability**: Make code easier to understand, modify, and extend
- **Performance**: Ensure applications run efficiently and scale well
- **Security**: Follow secure coding practices to protect applications
- **Debugging**: Make applications easier to troubleshoot and debug
- **Future-Proofing**: Write code that adapts well to Angular updates and changes
- **Developer Experience**: Improve productivity and reduce development time


### **Usage Scenarios:**

- Large team development projects
- Enterprise application development
- Long-term maintenance projects
- Performance-critical applications
- Security-sensitive applications
- Applications requiring high code quality standards


### **1. Project Structure and Organization:**

```typescript
// Recommended folder structure
src/
├── app/
│   ├── core/                 # Singleton services, guards, interceptors
│   │   ├── guards/
│   │   ├── interceptors/
│   │   ├── services/
│   │   └── core.module.ts
│   ├── shared/               # Shared components, directives, pipes
│   │   ├── components/
│   │   ├── directives/
│   │   ├── pipes/
│   │   └── shared.module.ts
│   ├── features/             # Feature modules
│   │   ├── user/
│   │   │   ├── components/
│   │   │   ├── services/
│   │   │   ├── models/
│   │   │   └── user.module.ts
│   │   └── product/
│   ├── layout/               # Layout components
│   └── app.component.ts
├── assets/                   # Static assets
├── environments/             # Environment configurations
└── styles/                   # Global styles

// Feature module organization
user/
├── components/
│   ├── user-list/
│   │   ├── user-list.component.ts
│   │   ├── user-list.component.html
│   │   ├── user-list.component.scss
│   │   └── user-list.component.spec.ts
│   └── user-detail/
├── services/
│   ├── user.service.ts
│   └── user.service.spec.ts
├── models/
│   └── user.interface.ts
├── guards/
│   └── user.guard.ts
└── user-routing.module.ts
```


### **2. Component Best Practices:**

```typescript
// ✅ Good: OnPush strategy for performance
@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent implements OnInit, OnDestroy {
  // ✅ Good: Use readonly for inputs
  @Input() readonly users: User[] = [];
  @Input() readonly loading = false;
  
  // ✅ Good: Use typed EventEmitter
  @Output() userSelected = new EventEmitter<User>();
  @Output() userDeleted = new EventEmitter<number>();
  
  // ✅ Good: Use trackBy for ngFor
  trackByUserId = (index: number, user: User): number => user.id;
  
  private destroy$ = new Subject<void>();
  
  constructor(
    private userService: UserService,
    private cdr: ChangeDetectorRef
  ) {}
  
  ngOnInit(): void {
    // ✅ Good: Use takeUntil for subscription management
    this.userService.getUsers()
      .pipe(takeUntil(this.destroy$))
      .subscribe(users => {
        // Handle users
        this.cdr.markForCheck(); // For OnPush
      });
  }
  
  ngOnDestroy(): void {
    // ✅ Good: Clean up subscriptions
    this.destroy$.next();
    this.destroy$.complete();
  }
  
  // ✅ Good: Use readonly and pure functions
  selectUser(user: User): void {
    this.userSelected.emit(user);
  }
  
  deleteUser(userId: number): void {
    this.userDeleted.emit(userId);
  }
}

// ❌ Bad practices to avoid:
// - Using any type
// - Direct DOM manipulation
// - Subscriptions without unsubscription
// - Complex logic in templates
// - Mutating input properties
```


### **3. Service Best Practices:**

```typescript
// ✅ Good: Injectable with providedIn
@Injectable({
  providedIn: 'root'
})
export class UserService {
  private readonly apiUrl = '/api/users';
  private usersSubject = new BehaviorSubject<User[]>([]);
  
  // ✅ Good: Expose readonly observables
  readonly users$ = this.usersSubject.asObservable();
  
  constructor(private http: HttpClient) {}
  
  // ✅ Good: Typed return types and error handling
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl).pipe(
      tap(users => this.usersSubject.next(users)),
      catchError(this.handleError<User[]>('getUsers', []))
    );
  }
  
  getUserById(id: number): Observable<User | undefined> {
    return this.http.get<User>(`${this.apiUrl}/${id}`).pipe(
      catchError(this.handleError<User>('getUserById'))
    );
  }
  
  createUser(user: Omit<User, 'id'>): Observable<User> {
    return this.http.post<User>(this.apiUrl, user).pipe(
      tap(newUser => {
        const currentUsers = this.usersSubject.value;
        this.usersSubject.next([...currentUsers, newUser]);
      }),
      catchError(this.handleError<User>('createUser'))
    );
  }
  
  // ✅ Good: Generic error handler
  private handleError<T>(operation = 'operation', result?: T) {
    return (error: any): Observable<T> => {
      console.error(`${operation} failed:`, error);
      
      // Let the app keep running by returning an empty result
      return of(result as T);
    };
  }
  
  // ✅ Good: Cleanup method for testing
  resetState(): void {
    this.usersSubject.next([]);
  }
}
```


### **4. Template Best Practices:**

```html
<!-- ✅ Good: Use trackBy for performance -->
<ul>
  <li *ngFor="let user of users; trackBy: trackByUserId">
    {{ user.name }}
  </li>
</ul>

<!-- ✅ Good: Use async pipe to manage subscriptions -->
<div *ngIf="users$ | async as users">
  <app-user-list [users]="users"></app-user-list>
</div>

<!-- ✅ Good: Use safe navigation operator -->
<p>{{ user?.profile?.bio }}</p>

<!-- ✅ Good: Separate presentation and logic -->
<button 
  [disabled]="isFormInvalid()" 
  (click)="onSubmit()"
  class="submit-btn">
  {{ getSubmitButtonText() }}
</button>

<!-- ✅ Good: Use semantic HTML -->
<section aria-labelledby="user-list-heading">
  <h2 id="user-list-heading">User List</h2>
  <!-- content -->
</section>

<!-- ❌ Bad: Complex expressions in templates -->
<!-- <div>{{ users.filter(u => u.active).map(u => u.name).join(', ') }}</div> -->

<!-- ❌ Bad: Function calls that return new objects -->
<!-- <div [ngClass]="getClasses()"></div> -->
```


### **5. RxJS Best Practices:**

```typescript
// ✅ Good: Proper observable patterns
@Component({})
export class SearchComponent implements OnInit, OnDestroy {
  searchControl = new FormControl('');
  
  // ✅ Good: Use proper operators
  searchResults$ = this.searchControl.valueChanges.pipe(
    startWith(''),
    debounceTime(300),
    distinctUntilChanged(),
    switchMap(term => 
      term.length > 2 
        ? this.searchService.search(term)
        : of([])
    ),
    catchError(error => {
      console.error('Search error:', error);
      return of([]);
    }),
    shareReplay(1)
  );
  
  private destroy$ = new Subject<void>();
  
  ngOnInit(): void {
    // ✅ Good: Use takeUntil for subscription management
    this.searchResults$
      .pipe(takeUntil(this.destroy$))
      .subscribe(results => {
        // Handle results
      });
  }
  
  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}

// ✅ Good: Service with proper RxJS patterns
@Injectable()
export class DataService {
  private cache = new Map<string, Observable<any>>();
  
  getData(id: string): Observable<any> {
    // ✅ Good: Implement caching
    if (!this.cache.has(id)) {
      const request$ = this.http.get(`/api/data/${id}`).pipe(
        shareReplay(1),
        catchError(error => {
          this.cache.delete(id); // Remove failed request from cache
          return throwError(error);
        })
      );
      this.cache.set(id, request$);
    }
    
    return this.cache.get(id)!;
  }
  
  // ✅ Good: Use operators for transformation
  getProcessedData(): Observable<ProcessedData[]> {
    return this.getData('all').pipe(
      map(data => this.processData(data)),
      filter(processed => processed.length > 0),
      tap(processed => console.log('Processed data:', processed))
    );
  }
}
```


### **6. Performance Best Practices:**

```typescript
// ✅ Good: OnPush strategy with immutable data
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  @Input() set data(value: DataItem[]) {
    // ✅ Good: Use immutable updates
    this._data = [...value];
  }
  get data(): DataItem[] {
    return this._data;
  }
  private _data: DataItem[] = [];
  
  // ✅ Good: Use trackBy functions
  trackById = (index: number, item: DataItem): number => item.id;
  
  // ✅ Good: Lazy load heavy components
  @ViewChild('heavyComponent', { static: false }) heavyComponent?: ElementRef;
  
  constructor(private cdr: ChangeDetectorRef) {}
  
  // ✅ Good: Debounce expensive operations
  @HostListener('window:resize', ['$event'])
  onResize = debounce(() => {
    this.calculateLayout();
    this.cdr.markForCheck();
  }, 250);
  
  private calculateLayout(): void {
    // Expensive layout calculation
  }
}

// ✅ Good: Lazy loading modules
const routes: Routes = [
  {
    path: 'feature',
    loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule)
  }
];
```


### **7. Security Best Practices:**

```typescript
// ✅ Good: Input sanitization
@Component({})
export class SecureComponent {
  @Input() set userInput(value: string) {
    // ✅ Good: Sanitize input
    this._userInput = this.sanitizer.sanitize(SecurityContext.HTML, value) || '';
  }
  get userInput(): string {
    return this._userInput;
  }
  private _userInput = '';
  
  constructor(private sanitizer: DomSanitizer) {}
  
  // ✅ Good: Validate before processing
  processUserData(data: any): void {
    if (this.isValidData(data)) {
      // Process data
    } else {
      console.error('Invalid data received');
    }
  }
  
  private isValidData(data: any): boolean {
    // Implement validation logic
    return data && typeof data === 'object';
  }
}

// ✅ Good: HTTP interceptor for security headers
@Injectable()
export class SecurityInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const secureReq = req.clone({
      setHeaders: {
        'X-Content-Type-Options': 'nosniff',
        'X-Frame-Options': 'DENY'
      }
    });
    
    return next.handle(secureReq);
  }
}
```


### **8. Testing Best Practices:**

```typescript
// ✅ Good: Comprehensive component testing
describe('UserComponent', () => {
  let component: UserComponent;
  let fixture: ComponentFixture<UserComponent>;
  let userService: jasmine.SpyObj<UserService>;
  
  beforeEach(() => {
    const spy = jasmine.createSpyObj('UserService', ['getUsers', 'deleteUser']);
    
    TestBed.configureTestingModule({
      declarations: [UserComponent],
      providers: [{ provide: UserService, useValue: spy }]
    });
    
    fixture = TestBed.createComponent(UserComponent);
    component = fixture.componentInstance;
    userService = TestBed.inject(UserService) as jasmine.SpyObj<UserService>;
  });
  
  // ✅ Good: Test behavior, not implementation
  it('should display users when data is loaded', () => {
    const mockUsers: User[] = [{ id: 1, name: 'John' }];
    userService.getUsers.and.returnValue(of(mockUsers));
    
    component.ngOnInit();
    fixture.detectChanges();
    
    const userElements = fixture.debugElement.queryAll(By.css('.user-item'));
    expect(userElements.length).toBe(1);
    expect(userElements[0].nativeElement.textContent).toContain('John');
  });
  
  // ✅ Good: Test error scenarios
  it('should handle service errors gracefully', () => {
    userService.getUsers.and.returnValue(throwError('Service error'));
    
    component.ngOnInit();
    
    expect(component.errorMessage).toBe('Failed to load users');
  });
});
```


### **9. Code Style and Conventions:**

```typescript
// ✅ Good: Follow Angular style guide
export class UserManagementComponent implements OnInit, OnDestroy {
  // ✅ Good: Use PascalCase for class names
  // ✅ Good: Use camelCase for properties and methods
  // ✅ Good: Use meaningful names
  
  readonly maxUsersPerPage = 10;
  isLoading = false;
  selectedUsers: User[] = [];
  
  // ✅ Good: Group related properties
  private readonly destroy$ = new Subject<void>();
  private subscription?: Subscription;
  
  constructor(
    private userService: UserService,
    private router: Router,
    private fb: FormBuilder
  ) {}
  
  ngOnInit(): void {
    this.loadUsers();
  }
  
  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
  
  // ✅ Good: Use descriptive method names
  loadUsers(): void {
    this.isLoading = true;
    // Implementation
  }
  
  navigateToUserDetail(userId: number): void {
    this.router.navigate(['/users', userId]);
  }
  
  // ✅ Good: Use private methods for internal logic
  private handleLoadingError(error: any): void {
    console.error('Error loading users:', error);
    this.isLoading = false;
  }
}
```


### **10. Documentation Best Practices:**

```typescript
/**
 * Service responsible for managing user data and operations.
 * 
 * @example
 * ```
 * constructor(private userService: UserService) {}
 * 
 * ngOnInit() {
 *   this.userService.getUsers().subscribe(users => {
 *     console.log('Users loaded:', users);
 *   });
 * }
 * ```
 */
@Injectable({
  providedIn: 'root'
})
export class UserService {
  /**
   * Retrieves all users from the API.
   * 
   * @returns Observable that emits an array of users
   * @throws Error when the API request fails
   */
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>('/api/users');
  }
  
  /**
   * Creates a new user.
   * 
   * @param userData - The user data (without ID)
   * @returns Observable that emits the created user with assigned ID
   */
  createUser(userData: Omit<User, 'id'>): Observable<User> {
    return this.http.post<User>('/api/users', userData);
  }
}

// ✅ Good: README.md structure
/*
# Project Name

## Description
Brief description of the project.

## Installation
```

npm install

```

## Development
```

ng serve

```

## Testing
```

npm run test
npm run e2e

```

## Building
```

npm run build

```

## Contributing
Guidelines for contributing to the project.
*/
```

[↑ Back to Top](#table-of-contents)

***

