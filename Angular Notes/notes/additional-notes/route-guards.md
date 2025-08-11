# Angular Route Guards - Comprehensive Guide

## What are Route Guards?

Route Guards in Angular are interfaces that control navigation to and from routes. They act as security checkpoints that determine whether a user can:
- Navigate to a route (enter)
- Leave a route (exit)
- Load child routes
- Load route data

Think of route guards like bouncers at a club - they check if you're allowed to enter, leave, or access certain areas.

## Types of Route Guards

Angular provides several types of route guards, each serving different purposes:

### 1. CanActivate
**Purpose**: Controls if a route can be activated (entered)
**Use Cases**: 
- Authentication checks
- Authorization/permission checks
- Feature flags

### 2. CanActivateChild
**Purpose**: Controls if child routes can be activated
**Use Cases**:
- Protecting entire sections of the app
- Role-based access to admin panels

### 3. CanDeactivate
**Purpose**: Controls if user can leave the current route
**Use Cases**:
- Unsaved form data warnings
- Confirmation dialogs before navigation

### 4. CanLoad
**Purpose**: Controls if a module can be loaded (for lazy loading)
**Use Cases**:
- Preventing unauthorized users from downloading module code
- Feature-based module loading

### 5. Resolve
**Purpose**: Pre-fetches data before route activation
**Use Cases**:
- Loading user profile data
- Fetching required configuration

## Comparison Table

| Guard Type | When It Runs | Return Type | Primary Use Case | Module Loading |
|------------|--------------|-------------|------------------|----------------|
| CanActivate | Before route activation | boolean/Observable&lt;boolean&gt;/Promise&lt;boolean&gt; | Authentication/Authorization | Module already loaded |
| CanActivateChild | Before child route activation | boolean/Observable&lt;boolean&gt;/Promise&lt;boolean&gt; | Protecting child routes | Module already loaded |
| CanDeactivate | Before leaving route | boolean/Observable&lt;boolean&gt;/Promise&lt;boolean&gt; | Prevent accidental navigation | N/A |
| CanLoad | Before lazy loading module | boolean/Observable&lt;boolean&gt;/Promise&lt;boolean&gt; | Prevent module download | Prevents module loading |
| Resolve | Before route activation | any/Observable&lt;any&gt;/Promise&lt;any&gt; | Pre-fetch data | Module already loaded |

## Key Differences: CanActivate vs CanLoad

| Aspect | CanActivate | CanLoad |
|--------|-------------|---------|
| **Timing** | After module is loaded | Before module is loaded |
| **Performance** | Module code downloaded even if blocked | Prevents unnecessary downloads |
| **Use with** | All routes | Lazy-loaded routes only |
| **Security Level** | Route-level | Module-level |

## Implementation Methods

### Modern Approach (Functional Guards - Angular 14+)
```typescript
// Functional approach (recommended)
export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  return authService.isAuthenticated();
};
```

### Class-based Approach (Legacy)
```typescript
// Class-based approach (older versions)
@Injectable()
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService) {}
  
  canActivate(): boolean {
    return this.authService.isAuthenticated();
  }
}
```

## Simple Project Example: Library Management System

Let's create a simple library management system to demonstrate all guard types:

### Project Structure
```
src/
├── app/
│   ├── guards/
│   │   ├── auth.guard.ts
│   │   ├── admin.guard.ts
│   │   ├── unsaved-changes.guard.ts
│   │   ├── feature.guard.ts
│   │   └── book-resolver.ts
│   ├── services/
│   │   ├── auth.service.ts
│   │   └── book.service.ts
│   ├── components/
│   │   ├── login/
│   │   ├── dashboard/
│   │   ├── books/
│   │   ├── admin/
│   │   └── book-form/
│   └── app-routing.module.ts
```

### 1. Authentication Service
```typescript
// services/auth.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private isLoggedIn = new BehaviorSubject<boolean>(false);
  private userRole = new BehaviorSubject<string>('user');

  login(username: string, password: string): boolean {
    // Simple login logic
    if (username === 'admin' && password === 'admin123') {
      this.isLoggedIn.next(true);
      this.userRole.next('admin');
      localStorage.setItem('user', JSON.stringify({ role: 'admin' }));
      return true;
    } else if (username === 'user' && password === 'user123') {
      this.isLoggedIn.next(true);
      this.userRole.next('user');
      localStorage.setItem('user', JSON.stringify({ role: 'user' }));
      return true;
    }
    return false;
  }

  logout(): void {
    this.isLoggedIn.next(false);
    this.userRole.next('');
    localStorage.removeItem('user');
  }

  isAuthenticated(): boolean {
    const user = localStorage.getItem('user');
    const authenticated = !!user;
    this.isLoggedIn.next(authenticated);
    return authenticated;
  }

  hasRole(role: string): boolean {
    const user = JSON.parse(localStorage.getItem('user') || '{}');
    return user.role === role;
  }

  getCurrentRole(): string {
    const user = JSON.parse(localStorage.getItem('user') || '{}');
    return user.role || '';
  }
}
```

### 2. CanActivate Guard (Authentication)
```typescript
// guards/auth.guard.ts
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isAuthenticated()) {
    return true;
  }

  // Redirect to login if not authenticated
  router.navigate(['/login'], { 
    queryParams: { returnUrl: state.url } 
  });
  return false;
};
```

### 3. CanActivateChild Guard (Admin Access)
```typescript
// guards/admin.guard.ts
import { inject } from '@angular/core';
import { CanActivateChildFn, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

export const adminGuard: CanActivateChildFn = (childRoute, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isAuthenticated() && authService.hasRole('admin')) {
    return true;
  }

  // Redirect to dashboard if not admin
  router.navigate(['/dashboard']);
  return false;
};
```

### 4. CanDeactivate Guard (Unsaved Changes)
```typescript
// guards/unsaved-changes.guard.ts
import { CanDeactivateFn } from '@angular/router';

export interface CanComponentDeactivate {
  canDeactivate(): boolean;
}

export const unsavedChangesGuard: CanDeactivateFn<CanComponentDeactivate> = (component) => {
  if (component.canDeactivate) {
    return component.canDeactivate();
  }
  return true;
};
```

### 5. CanLoad Guard (Feature Access)
```typescript
// guards/feature.guard.ts
import { inject } from '@angular/core';
import { CanLoadFn, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

export const featureGuard: CanLoadFn = (route) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isAuthenticated()) {
    return true;
  }

  router.navigate(['/login']);
  return false;
};
```

### 6. Resolve Guard (Data Pre-loading)
```typescript
// guards/book-resolver.ts
import { inject } from '@angular/core';
import { ResolveFn } from '@angular/router';
import { Observable } from 'rxjs';
import { BookService } from '../services/book.service';

export interface Book {
  id: number;
  title: string;
  author: string;
  isbn: string;
}

export const bookResolver: ResolveFn<Book[]> = (route, state): Observable<Book[]> => {
  const bookService = inject(BookService);
  return bookService.getAllBooks();
};
```

### 7. Book Service
```typescript
// services/book.service.ts
import { Injectable } from '@angular/core';
import { Observable, of, delay } from 'rxjs';
import { Book } from '../guards/book-resolver';

@Injectable({
  providedIn: 'root'
})
export class BookService {
  private books: Book[] = [
    { id: 1, title: 'Angular Fundamentals', author: 'John Doe', isbn: '978-1234567890' },
    { id: 2, title: 'TypeScript Deep Dive', author: 'Jane Smith', isbn: '978-0987654321' },
    { id: 3, title: 'RxJS Patterns', author: 'Bob Johnson', isbn: '978-1122334455' }
  ];

  getAllBooks(): Observable<Book[]> {
    // Simulate API delay
    return of(this.books).pipe(delay(1000));
  }

  getBookById(id: number): Observable<Book | undefined> {
    const book = this.books.find(b => b.id === id);
    return of(book).pipe(delay(500));
  }
}
```

### 8. Book Form Component (with CanDeactivate)
```typescript
// components/book-form/book-form.component.ts
import { Component } from '@angular/core';
import { CanComponentDeactivate } from '../../guards/unsaved-changes.guard';

@Component({
  selector: 'app-book-form',
  template: `
    <div class="form-container">
      <h2>Add New Book</h2>
      <form (ngSubmit)="onSubmit()">
        <div>
          <label>Title:</label>
          <input [(ngModel)]="book.title" name="title" (input)="markAsChanged()">
        </div>
        <div>
          <label>Author:</label>
          <input [(ngModel)]="book.author" name="author" (input)="markAsChanged()">
        </div>
        <div>
          <label>ISBN:</label>
          <input [(ngModel)]="book.isbn" name="isbn" (input)="markAsChanged()">
        </div>
        <button type="submit">Save Book</button>
      </form>
      <p *ngIf="hasUnsavedChanges" style="color: red;">
        You have unsaved changes!
      </p>
    </div>
  `
})
export class BookFormComponent implements CanComponentDeactivate {
  book = { title: '', author: '', isbn: '' };
  hasUnsavedChanges = false;

  markAsChanged(): void {
    this.hasUnsavedChanges = true;
  }

  onSubmit(): void {
    // Save logic here
    this.hasUnsavedChanges = false;
    console.log('Book saved:', this.book);
  }

  canDeactivate(): boolean {
    if (this.hasUnsavedChanges) {
      return confirm('You have unsaved changes. Are you sure you want to leave?');
    }
    return true;
  }
}
```

### 9. App Routing Configuration
```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './components/login/login.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { BooksComponent } from './components/books/books.component';
import { BookFormComponent } from './components/book-form/book-form.component';
import { AdminComponent } from './components/admin/admin.component';

import { authGuard } from './guards/auth.guard';
import { adminGuard } from './guards/admin.guard';
import { unsavedChangesGuard } from './guards/unsaved-changes.guard';
import { featureGuard } from './guards/feature.guard';
import { bookResolver } from './guards/book-resolver';

const routes: Routes = [
  { path: '', redirectTo: '/login', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  
  // Protected routes with CanActivate
  {
    path: 'dashboard',
    component: DashboardComponent,
    canActivate: [authGuard]
  },
  
  // Route with Resolve guard (pre-load data)
  {
    path: 'books',
    component: BooksComponent,
    canActivate: [authGuard],
    resolve: { books: bookResolver }
  },
  
  // Route with CanDeactivate guard
  {
    path: 'add-book',
    component: BookFormComponent,
    canActivate: [authGuard],
    canDeactivate: [unsavedChangesGuard]
  },
  
  // Admin section with CanActivateChild
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [authGuard],
    canActivateChild: [adminGuard],
    children: [
      { path: 'users', component: AdminUsersComponent },
      { path: 'settings', component: AdminSettingsComponent },
      { path: 'reports', component: AdminReportsComponent }
    ]
  },
  
  // Lazy-loaded module with CanLoad
  {
    path: 'premium',
    loadChildren: () => import('./premium/premium.module').then(m => m.PremiumModule),
    canLoad: [featureGuard]
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Best Practices

### 1. Return Types
- **Synchronous**: Return `boolean` directly
- **Asynchronous**: Return `Observable<boolean>` or `Promise<boolean>`

### 2. Error Handling
```typescript
export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  return authService.checkAuth().pipe(
    map(isAuthenticated => {
      if (isAuthenticated) {
        return true;
      }
      router.navigate(['/login']);
      return false;
    }),
    catchError(error => {
      console.error('Auth check failed:', error);
      router.navigate(['/login']);
      return of(false);
    })
  );
};
```

### 3. Dependency Injection
Use the `inject()` function in functional guards:
```typescript
export const myGuard: CanActivateFn = (route, state) => {
  const service = inject(MyService);
  const router = inject(Router);
  // Guard logic here
};
```

### 4. Guard Composition
Combine multiple conditions:
```typescript
export const complexGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const featureService = inject(FeatureService);
  
  return authService.isAuthenticated() && 
         featureService.isFeatureEnabled('advanced-features');
};
```

## Testing Route Guards

### Unit Test Example
```typescript
// auth.guard.spec.ts
import { TestBed } from '@angular/core/testing';
import { Router } from '@angular/router';
import { authGuard } from './auth.guard';
import { AuthService } from '../services/auth.service';

describe('AuthGuard', () => {
  let authService: jasmine.SpyObj<AuthService>;
  let router: jasmine.SpyObj<Router>;

  beforeEach(() => {
    const authSpy = jasmine.createSpyObj('AuthService', ['isAuthenticated']);
    const routerSpy = jasmine.createSpyObj('Router', ['navigate']);

    TestBed.configureTestingModule({
      providers: [
        { provide: AuthService, useValue: authSpy },
        { provide: Router, useValue: routerSpy }
      ]
    });

    authService = TestBed.inject(AuthService) as jasmine.SpyObj<AuthService>;
    router = TestBed.inject(Router) as jasmine.SpyObj<Router>;
  });

  it('should allow access when authenticated', () => {
    authService.isAuthenticated.and.returnValue(true);
    
    const result = TestBed.runInInjectionContext(() => 
      authGuard({} as any, {} as any)
    );
    
    expect(result).toBe(true);
  });

  it('should redirect to login when not authenticated', () => {
    authService.isAuthenticated.and.returnValue(false);
    
    const result = TestBed.runInInjectionContext(() => 
      authGuard({} as any, { url: '/dashboard' } as any)
    );
    
    expect(result).toBe(false);
    expect(router.navigate).toHaveBeenCalledWith(['/login'], {
      queryParams: { returnUrl: '/dashboard' }
    });
  });
});
```

## Common Pitfalls and Solutions

### 1. Infinite Redirect Loops
**Problem**: Guard redirects to a route that has the same guard
**Solution**: Create specific public routes for redirects

### 2. Memory Leaks
**Problem**: Not unsubscribing from observables in guards
**Solution**: Use operators like `take(1)` or `first()`

### 3. Performance Issues
**Problem**: Heavy operations in guards blocking navigation
**Solution**: Use loading states and async operations

### 4. CanLoad vs CanActivate Confusion
**Problem**: Using wrong guard type for lazy-loaded routes
**Solution**: Use CanLoad to prevent module loading, CanActivate for route access

## Summary

Route Guards are essential for:
- **Security**: Control access to routes and features
- **User Experience**: Prevent data loss and provide smooth navigation
- **Performance**: Control when modules are loaded
- **Data Management**: Pre-load required data before route activation

Choose the right guard type based on your specific needs:
- **Authentication/Authorization**: CanActivate
- **Protecting sections**: CanActivateChild
- **Preventing data loss**: CanDeactivate
- **Lazy loading control**: CanLoad
- **Data pre-loading**: Resolve

The functional approach (Angular 14+) is recommended for new projects as it's more concise and easier to test.