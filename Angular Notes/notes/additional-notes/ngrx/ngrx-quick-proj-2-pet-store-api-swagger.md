Certainly! Here's the **complete folder-wise Angular + NgRx project structure** using the [Swagger Petstore API](https://petstore.swagger.io/v2/pet/{id}):

> https://petstore.swagger.io/

---

## ✅ Folder Structure

```
src/
└── app/
    ├── pet/
    │   ├── actions/
    │   │   └── pet.actions.ts
    │   ├── reducers/
    │   │   └── pet.reducer.ts
    │   ├── effects/
    │   │   └── pet.effects.ts
    │   ├── services/
    │   │   └── pet.service.ts
    │   ├── models/
    │   │   └── pet.model.ts
    │   ├── components/
    │   │   ├── pet.component.ts
    │   │   └── pet.component.html
    │   └── pet.module.ts
    └── app.module.ts
```

---

## 📁 `models/pet.model.ts`

```ts
export interface Category {
  id: number;
  name: string;
}

export interface Tag {
  id: number;
  name: string;
}

export interface Pet {
  id: number;
  category: Category;
  name: string;
  photoUrls: string[];
  tags: Tag[];
  status: string;
}

export interface ErrorResponse {
  code: number;
  type: string;
  message: string;
}
```

---

## 📁 `actions/pet.actions.ts`

```ts
import { createAction, props } from '@ngrx/store';
import { Pet, ErrorResponse } from '../models/pet.model';

export const loadPet = createAction('[Pet] Load Pet', props<{ id: number }>());
export const loadPetSuccess = createAction('[Pet] Load Success', props<{ pet: Pet; status: number }>());
export const loadPetFailure = createAction('[Pet] Load Failure', props<{ error: ErrorResponse; status: number }>());
```

---

## 📁 `reducers/pet.reducer.ts`

```ts
import { createReducer, on } from '@ngrx/store';
import * as PetActions from '../actions/pet.actions';
import { Pet, ErrorResponse } from '../models/pet.model';

export interface PetState {
  pet: Pet | null;
  error: ErrorResponse | null;
  status: number | null;
}

const initialState: PetState = {
  pet: null,
  error: null,
  status: null,
};

export const petReducer = createReducer(
  initialState,
  on(PetActions.loadPetSuccess, (state, { pet, status }) => ({ ...state, pet, status, error: null })),
  on(PetActions.loadPetFailure, (state, { error, status }) => ({ ...state, error, status, pet: null }))
);
```

---

## 📁 `effects/pet.effects.ts`

```ts
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { catchError, map, mergeMap, of } from 'rxjs';
import * as PetActions from '../actions/pet.actions';
import { PetService } from '../services/pet.service';

@Injectable()
export class PetEffects {
  constructor(private actions$: Actions, private petService: PetService) {}

  loadPet$ = createEffect(() =>
    this.actions$.pipe(
      ofType(PetActions.loadPet),
      mergeMap(({ id }) =>
        this.petService.getPet(id).pipe(
          map(response => PetActions.loadPetSuccess({ pet: response.body!, status: response.status })),
          catchError(error =>
            of(PetActions.loadPetFailure({ error: error.error, status: error.status }))
          )
        )
      )
    )
  );
}
```

---

## 📁 `services/pet.service.ts`

```ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpResponse } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Pet } from '../models/pet.model';

@Injectable({ providedIn: 'root' })
export class PetService {
  constructor(private http: HttpClient) {}

  getPet(id: number): Observable<HttpResponse<Pet>> {
    return this.http.get<Pet>(`https://petstore.swagger.io/v2/pet/${id}`, {
      observe: 'response',
    });
  }
}
```

---

## 📁 `components/pet.component.ts`

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { loadPet } from '../actions/pet.actions';
import { Observable } from 'rxjs';
import { PetState } from '../reducers/pet.reducer';
import { Pet, ErrorResponse } from '../models/pet.model';

@Component({
  selector: 'app-pet',
  templateUrl: './pet.component.html',
})
export class PetComponent {
  pet$: Observable<Pet | null>;
  error$: Observable<ErrorResponse | null>;
  status$: Observable<number | null>;

  constructor(private store: Store<{ petState: PetState }>) {
    this.pet$ = this.store.select(state => state.petState.pet);
    this.error$ = this.store.select(state => state.petState.error);
    this.status$ = this.store.select(state => state.petState.status);
  }

  fetchPet(id: number) {
    this.store.dispatch(loadPet({ id: +id }));

    // Log to console
    this.status$.subscribe(status => console.log('Status Code:', status));
    this.pet$.subscribe(pet => pet && console.log('Pet:', pet));
    this.error$.subscribe(err => err && console.log('Error:', err));
  }
}
```

---

## 📁 `components/pet.component.html`

```html
<input type="number" #petId placeholder="Enter Pet ID" />
<button (click)="fetchPet(petId.value)">Fetch Pet</button>

<hr />

<div *ngIf="(pet$ | async) as pet">
  <h2>🐾 Pet Details</h2>
  <p><strong>ID:</strong> {{ pet.id }}</p>
  <p><strong>Name:</strong> {{ pet.name }}</p>
  <p><strong>Status:</strong> {{ pet.status }}</p>

  <div *ngIf="pet.category">
    <h4>📁 Category</h4>
    <p>ID: {{ pet.category.id }}</p>
    <p>Name: {{ pet.category.name }}</p>
  </div>

  <div *ngIf="pet.tags?.length">
    <h4>🏷️ Tags</h4>
    <ul>
      <li *ngFor="let tag of pet.tags">{{ tag.id }} - {{ tag.name }}</li>
    </ul>
  </div>

  <div *ngIf="pet.photoUrls?.length">
    <h4>🖼️ Photos</h4>
    <ul>
      <li *ngFor="let url of pet.photoUrls">{{ url }}</li>
    </ul>
  </div>
</div>

<div *ngIf="(error$ | async) as err">
  <h2>❌ Error</h2>
  <pre>{{ err | json }}</pre>
</div>
```

---

## 📁 `pet.module.ts`

```ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { PetComponent } from './components/pet.component';
import { HttpClientModule } from '@angular/common/http';
import { StoreModule } from '@ngrx/store';
import { petReducer } from './reducers/pet.reducer';
import { EffectsModule } from '@ngrx/effects';
import { PetEffects } from './effects/pet.effects';

@NgModule({
  declarations: [PetComponent],
  imports: [
    CommonModule,
    HttpClientModule,
    StoreModule.forFeature('petState', petReducer),
    EffectsModule.forFeature([PetEffects]),
  ],
  exports: [PetComponent],
})
export class PetModule {}
```

---

## 📁 `app.module.ts`

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { PetModule } from './pet/pet.module';
import { StoreModule } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    PetModule,
    StoreModule.forRoot({}),
    EffectsModule.forRoot([]),
  ],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

---

✅ **This is a complete functional NgRx setup** using Swagger Petstore API.

Would you like me to generate this into a GitHub repo or downloadable zip?
