# Angular NgRx Components and Data Flow

## Overview
NgRx is a global state management library for Angular applications based on the Redux pattern. It provides a predictable state container that helps manage application state in a reactive way.

## Core Components

### 1. Store
The single source of truth that holds the entire application state.

```typescript
// Store Structure (based on your data)
interface AppState {
  products: Product[];
  orders: Order[];
  cart: CartItem[];
  userDetails: UserDetails;
  user: {
    name: string;
    age: number;
  };
}
```

### 2. Actions
Actions describe what happened in the application. They are dispatched to trigger state changes.

```typescript
// Actions (corrected from your example)
import { createAction, props } from '@ngrx/store';

// User Actions
export const editUsername = createAction(
  '[User] Edit Username',
  props<{ name: string }>()
);

export const editUserAge = createAction(
  '[User] Edit User Age',
  props<{ age: number }>()
);

// Product Actions
export const loadProducts = createAction('[Product] Load Products');
export const loadProductsSuccess = createAction(
  '[Product] Load Products Success',
  props<{ products: Product[] }>()
);

// Cart Actions
export const addToCart = createAction(
  '[Cart] Add Item',
  props<{ item: CartItem }>()
);
```

### 3. Reducers
Pure functions that specify how the state changes in response to actions.

```typescript
// User Reducer (corrected from your example)
import { createReducer, on } from '@ngrx/store';

interface UserState {
  name: string;
  age: number;
}

const initialState: UserState = {
  name: "isJSDocLinkLike",
  age: 25
};

export const userReducer = createReducer(
  initialState,
  on(editUsername, (state, { name }) => ({
    ...state,
    name
  })),
  on(editUserAge, (state, { age }) => ({
    ...state,
    age
  }))
);

// Products Reducer
export const productsReducer = createReducer(
  [],
  on(loadProductsSuccess, (state, { products }) => products)
);

// Cart Reducer
export const cartReducer = createReducer(
  [],
  on(addToCart, (state, { item }) => [...state, item])
);
```

### 4. Effects
Handle side effects like API calls, timeouts, and async operations.

```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { of } from 'rxjs';
import { map, mergeMap, catchError, delay } from 'rxjs/operators';

@Injectable()
export class ProductEffects {
  
  loadProducts$ = createEffect(() =>
    this.actions$.pipe(
      ofType(loadProducts),
      delay(2000), // Simulate network delay
      mergeMap(() =>
        this.productService.getProducts().pipe(
          map(products => loadProductsSuccess({ products })),
          catchError(() => of({ type: '[Product] Load Products Failure' }))
        )
      )
    )
  );

  constructor(
    private actions$: Actions,
    private productService: ProductService
  ) {}
}
```

### 5. Selectors
Pure functions used to select, derive and compose pieces of state.

```typescript
import { createSelector, createFeatureSelector } from '@ngrx/store';

// Feature selectors
export const selectUserState = createFeatureSelector<UserState>('user');
export const selectProducts = createFeatureSelector<Product[]>('products');
export const selectCart = createFeatureSelector<CartItem[]>('cart');

// Memoized selectors
export const selectUserName = createSelector(
  selectUserState,
  (state: UserState) => state.name
);

export const selectUserAge = createSelector(
  selectUserState,
  (state: UserState) => state.age
);

export const selectCartTotal = createSelector(
  selectCart,
  (cart: CartItem[]) => cart.reduce((total, item) => total + item.price * item.quantity, 0)
);

export const selectCartItemCount = createSelector(
  selectCart,
  (cart: CartItem[]) => cart.reduce((count, item) => count + item.quantity, 0)
);
```

## Data Propagation in Components

### Direct Store Subscription (Not Recommended)
```typescript
// Component directly subscribing to store
export class UserComponent implements OnInit {
  user$ = this.store.select(selectUserState);
  
  constructor(private store: Store) {}
  
  ngOnInit() {
    this.user$.subscribe(user => {
      console.log('User updated:', user);
    });
  }
}
```

### Using Selectors (Recommended)
```typescript
// Component using selectors
export class UserProfileComponent {
  userName$ = this.store.select(selectUserName);
  userAge$ = this.store.select(selectUserAge);
  
  constructor(private store: Store) {}
  
  updateName(newName: string) {
    this.store.dispatch(editUsername({ name: newName }));
  }
  
  updateAge(newAge: number) {
    this.store.dispatch(editUserAge({ age: newAge }));
  }
}
```

### Service Layer for Complex Operations
```typescript
@Injectable()
export class UserFacade {
  userName$ = this.store.select(selectUserName);
  userAge$ = this.store.select(selectUserAge);
  
  constructor(private store: Store) {}
  
  updateUserProfile(name: string, age: number) {
    this.store.dispatch(editUsername({ name }));
    this.store.dispatch(editUserAge({ age }));
  }
  
  loadUserData() {
    // Could trigger effects for API calls
    this.store.dispatch(loadUserData());
  }
}
```

## Method Inputs and Outputs

### Actions
- **Input**: Type (string) + Payload (optional data)
- **Output**: Dispatched to store, picked up by reducers and effects

### Reducers
- **Input**: Current state + Action
- **Output**: New state (immutable)

### Effects
- **Input**: Action stream
- **Output**: New actions or side effects

### Selectors
- **Input**: Application state
- **Output**: Selected/computed data

## When to Use Effects

Effects are used for:
1. **API Calls**: HTTP requests to backend services
2. **Async Operations**: Timeouts, intervals, delays
3. **Side Effects**: Navigation, notifications, logging
4. **Complex Business Logic**: Multi-step operations

```typescript
// Example: Auto-save user profile after changes
autoSaveProfile$ = createEffect(() =>
  this.actions$.pipe(
    ofType(editUsername, editUserAge),
    debounceTime(3000), // Wait 3 seconds after last change
    withLatestFrom(this.store.select(selectUserState)),
    mergeMap(([action, userState]) =>
      this.userService.saveProfile(userState).pipe(
        map(() => ({ type: '[User] Save Profile Success' })),
        catchError(() => of({ type: '[User] Save Profile Failure' }))
      )
    )
  )
);
```

## Data Flow Summary

1. **Component** dispatches an **Action**
2. **Action** is received by **Reducers** and **Effects**
3. **Reducer** updates the **Store** with new state
4. **Effects** handle side effects and may dispatch new actions
5. **Selectors** retrieve data from store
6. **Components** receive updated data through **Selectors**
7. **UI** re-renders with new data

This creates a unidirectional data flow that makes the application predictable and easier to debug.