Awesome! You're progressing well with Angular NgRx! Now let’s **refactor your existing NgRx reducer and actions** using `createAction`, `createReducer`, and `props` properly.

---

## ✅ GOAL

We’ll do the following:

1. Use `createAction` with `props` for strong typing.
2. Refactor `userReducer` using `createReducer` and `on(...)`.
3. Create dispatch-friendly component methods like `editName(input)`.
4. Organize everything properly under a **folder structure**.
5. Show sample state + payload for better clarity.
6. Add code comments for learning.

---

## 📁 Folder Structure (Best Practice)

```
src/
└── app/
    ├── store/
    │   ├── user.actions.ts       👈 Define action creators
    │   ├── user.reducer.ts       👈 Create reducer function
    │   ├── user.state.ts         👈 Define interface and initialState
    ├── form/
    │   ├── form.component.ts
    │   └── form.component.html
    ├── display/
    │   ├── display.component.ts
    │   └── display.component.html
    ├── app.module.ts
    ├── app.component.html
```

---

## 🔹 Step-by-Step Code

### 📁 `store/user.state.ts`

```ts
// State interface and initial values
export interface UserState {
  name: string;
  email: string;
  mobile: string;
}

export const initialState: UserState = {
  name: '',
  email: '',
  mobile: ''
};
```

---

### 📁 `store/user.actions.ts`

```ts
import { createAction, props } from '@ngrx/store';

// 👇 Actions with props
export const editName = createAction(
  'name-edit',
  props<{ name: string }>()
);

export const editEmail = createAction(
  'email-edit',
  props<{ email: string }>()
);

export const editMobile = createAction(
  'mobile-edit',
  props<{ mobile: string }>()
);
```

---

### 📁 `store/user.reducer.ts`

```ts
import { createReducer, on } from '@ngrx/store';
import { initialState, UserState } from './user.state';
import { editName, editEmail, editMobile } from './user.actions';

// 👇 Reducer with strongly typed data (action payloads)
export const userReducer = createReducer(
  initialState,

  // Sample data for this reducer:
  // state: { name: '', email: '', mobile: '' }
  // data: { name: 'John' }
  on(editName, (state, data) => ({
    ...state,
    name: data.name
  })),

  on(editEmail, (state, data) => ({
    ...state,
    email: data.email
  })),

  on(editMobile, (state, data) => ({
    ...state,
    mobile: data.mobile
  }))
);
```

---

### 🛠️ `app.module.ts`

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { StoreModule } from '@ngrx/store';

import { AppComponent } from './app.component';
import { FormComponent } from './form/form.component';
import { DisplayComponent } from './display/display.component';
import { userReducer } from './store/user.reducer';

@NgModule({
  declarations: [AppComponent, FormComponent, DisplayComponent],
  imports: [
    BrowserModule,
    FormsModule,
    StoreModule.forRoot({ user: userReducer }) // Register reducer slice as 'user'
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

---

### 👨‍💻 `form.component.ts`

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { editName, editEmail, editMobile } from '../store/user.actions';

@Component({
  selector: 'app-form',
  templateUrl: './form.component.html'
})
export class FormComponent {
  constructor(private store: Store) {}

  // Dispatch the editName action
  editName(input: HTMLInputElement) {
    this.store.dispatch(editName({ name: input.value }));
    input.value = '';
  }

  editEmail(input: HTMLInputElement) {
    this.store.dispatch(editEmail({ email: input.value }));
    input.value = '';
  }

  editMobile(input: HTMLInputElement) {
    this.store.dispatch(editMobile({ mobile: input.value }));
    input.value = '';
  }
}
```

---

### 🧾 `form.component.html`

```html
<input #nameInput placeholder="Enter name" />
<button (click)="editName(nameInput)">Submit Name</button>

<input #emailInput placeholder="Enter email" />
<button (click)="editEmail(emailInput)">Submit Email</button>

<input #mobileInput placeholder="Enter mobile" />
<button (click)="editMobile(mobileInput)">Submit Mobile</button>
```

---

### 👁️ `display.component.ts`

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';

@Component({
  selector: 'app-display',
  templateUrl: './display.component.html'
})
export class DisplayComponent {
  user: any;

  constructor(private store: Store<any>) {
    // Subscribe to 'user' slice of store
    store.select('user').subscribe(data => {
      this.user = data;
      console.log('User from store:', this.user);
    });
  }
}
```

---

### 🖼️ `display.component.html`

```html
<h3>User Info</h3>
<p><strong>Name:</strong> {{ user?.name }}</p>
<p><strong>Email:</strong> {{ user?.email }}</p>
<p><strong>Mobile:</strong> {{ user?.mobile }}</p>
```

---

## 🧪 Sample State and Action Payload

**Initial `state`:**

```ts
{
  name: '',
  email: '',
  mobile: ''
}
```

**Action dispatched:**

```ts
editName({ name: 'John' })
```

**`data` inside reducer:**

```ts
{ name: 'John' }
```

**New state returned:**

```ts
{
  name: 'John',
  email: '',
  mobile: ''
}
```

---

## ✅ Summary

| Concept            | Used & Explained      |
| ------------------ | --------------------- |
| `createAction()`   | ✅ With `props<{...}>` |
| `createReducer()`  | ✅ With `on(...)`      |
| Dispatch in method | ✅ From component      |
| Folder structure   | ✅ Cleaned             |
| Typing             | ✅ For payload/state   |

---

Let me know if you'd like:

* Full StackBlitz setup
* Add selectors
* Async data (effect)
* Use `createFeature` or `inject(Store)`

Shall we move on to selectors or effects next?
