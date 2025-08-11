Great! You're now ready to learn **NgRx Effects** — a major step in mastering Angular's reactive state management.

---

## 🧠 What Are NgRx Effects?

NgRx **Effects** help you handle **side effects** like HTTP calls, local storage, router navigation, etc., outside of components or reducers.
They listen to dispatched actions, perform async work (like HTTP), and then dispatch **new actions with results**.

---

## 🎯 Goal of This Mini Project

> 🔹 A button click → triggers an NgRx action
> 🔹 NgRx **Effect** calls an HTTP API → `https://fakestoreapi.in/api/products`
> 🔹 Result is dispatched and shown in the UI
> 🔹 Follow clean architecture with folders, naming, and best practices

---

## ✅ Folder Structure (Clean & Scalable)

```
src/
└── app/
    ├── products/
    │   ├── models/
    │   │   └── product.model.ts
    │   ├── store/
    │   │   ├── product.actions.ts
    │   │   ├── product.reducer.ts
    │   │   ├── product.effects.ts
    │   │   ├── product.selectors.ts
    │   │   └── product.state.ts
    │   ├── services/
    │   │   └── product-api.service.ts
    │   ├── components/
    │   │   ├── product-list.component.ts
    │   │   └── product-list.component.html
    ├── app.module.ts
    ├── app.component.html
```

---

## 🔧 Step-by-Step Implementation

---

### 1️⃣ models/product.model.ts

```ts
export interface Product {
  id: number;
  title: string;
  price: number;
  category: string;
  description: string;
  image: string;
}
```

---

### 2️⃣ store/product.state.ts

```ts
import { Product } from '../models/product.model';

export interface ProductState {
  products: Product[];
  loading: boolean;
  error: string | null;
}

export const initialProductState: ProductState = {
  products: [],
  loading: false,
  error: null
};
```

---

### 3️⃣ store/product.actions.ts

```ts
import { createAction, props } from '@ngrx/store';
import { Product } from '../models/product.model';

export const loadProducts = createAction('[Product] Load Products');

export const loadProductsSuccess = createAction(
  '[Product] Load Products Success',
  props<{ products: Product[] }>()
);

export const loadProductsFailure = createAction(
  '[Product] Load Products Failure',
  props<{ error: string }>()
);
```

---

### 4️⃣ store/product.reducer.ts

```ts
import { createReducer, on } from '@ngrx/store';
import { initialProductState } from './product.state';
import * as ProductActions from './product.actions';

export const productReducer = createReducer(
  initialProductState,

  on(ProductActions.loadProducts, state => ({
    ...state,
    loading: true,
    error: null
  })),

  on(ProductActions.loadProductsSuccess, (state, { products }) => ({
    ...state,
    products,
    loading: false
  })),

  on(ProductActions.loadProductsFailure, (state, { error }) => ({
    ...state,
    error,
    loading: false
  }))
);
```

---

### 5️⃣ store/product.selectors.ts

```ts
import { createFeatureSelector, createSelector } from '@ngrx/store';
import { ProductState } from './product.state';

export const selectProductState = createFeatureSelector<ProductState>('products');

export const selectAllProducts = createSelector(
  selectProductState,
  state => state.products
);

export const selectLoading = createSelector(
  selectProductState,
  state => state.loading
);
```

---

### 6️⃣ services/product-api.service.ts

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Product } from '../models/product.model';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class ProductApiService {
  private baseUrl = 'https://fakestoreapi.in/api/products';

  constructor(private http: HttpClient) {}

  getAllProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.baseUrl);
  }
}
```

---

### 7️⃣ store/product.effects.ts

```ts
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { ProductApiService } from '../services/product-api.service';
import * as ProductActions from './product.actions';
import { catchError, map, mergeMap, of } from 'rxjs';

@Injectable()
export class ProductEffects {
  constructor(
    private actions$: Actions,
    private productApi: ProductApiService
  ) {}

  loadProducts$ = createEffect(() =>
    this.actions$.pipe(
      ofType(ProductActions.loadProducts),
      mergeMap(() =>
        this.productApi.getAllProducts().pipe(
          map(products => ProductActions.loadProductsSuccess({ products })),
          catchError(error =>
            of(ProductActions.loadProductsFailure({ error: error.message }))
          )
        )
      )
    )
  );
}
```

---

### 8️⃣ components/product-list.component.ts

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { loadProducts } from '../../store/product.actions';
import { selectAllProducts, selectLoading } from '../../store/product.selectors';
import { Observable } from 'rxjs';
import { Product } from '../../models/product.model';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html'
})
export class ProductListComponent {
  products$: Observable<Product[]>;
  loading$: Observable<boolean>;

  constructor(private store: Store) {
    this.products$ = this.store.select(selectAllProducts);
    this.loading$ = this.store.select(selectLoading);
  }

  fetchProducts() {
    this.store.dispatch(loadProducts());
  }
}
```

---

### 9️⃣ components/product-list.component.html

```html
<button (click)="fetchProducts()">Fetch Products</button>

<div *ngIf="loading$ | async">Loading...</div>

<ul *ngIf="(products$ | async)?.length">
  <li *ngFor="let product of products$ | async">
    <strong>{{ product.title }}</strong> - ₹{{ product.price }}
  </li>
</ul>
```

---

### 🔟 app.module.ts

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { StoreModule } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { ProductListComponent } from './products/components/product-list.component';
import { productReducer } from './products/store/product.reducer';
import { ProductEffects } from './products/store/product.effects';

@NgModule({
  declarations: [AppComponent, ProductListComponent],
  imports: [
    BrowserModule,
    HttpClientModule,
    StoreModule.forRoot({ products: productReducer }),
    EffectsModule.forRoot([ProductEffects])
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

---

### 🔚 app.component.html

```html
<h1>Product Store</h1>
<app-product-list></app-product-list>
```

---

## ✅ Summary

| Feature                | Implemented ✅                      |
| ---------------------- | ---------------------------------- |
| Folder structure       | Yes                                |
| Manual API via service | Yes (`HttpClient`)                 |
| NgRx actions           | Yes (`load`, `success`, `failure`) |
| NgRx effect            | Yes (API call via `mergeMap`)      |
| State + selectors      | Yes                                |
| Loading + display UI   | Yes                                |

---

Would you like:

* To save this as a zip or StackBlitz project?
* To add retry or pagination in effects?
* A second API for deeper learning?

Let me know when you're ready for advanced effects like debouncing, switchMap, etc.
