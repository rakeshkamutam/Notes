Excellent! You're now moving from using `createAction()` with `props` to understanding **manual action creators** using plain functions like:

```ts
export function editEmail(email) {
  return {
    type: 'email-edit',
    payload: email
  };
}
```

This is a valid and older pattern (pre-`createAction`), and useful for learning how actions work under the hood. Let's now **build the full NgRx project** using this **manual action creator pattern**, while keeping the structure clean and readable.

---

## ✅ Project Overview

* 👨‍💻 Dispatch actions using manual creator functions (e.g., `editEmail(email)`).
* 💾 Use plain JS object actions with `type` and `payload`.
* 🧠 Reducer handles them using `action.payload`.

---

## 📁 Folder Structure

```
src/
└── app/
    ├── store/
    │   ├── user.actions.ts
    │   ├── user.reducer.ts
    │   ├── user.state.ts
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

## 1️⃣ `store/user.state.ts`

```ts
// Interface for typed state
export interface UserState {
  name: string;
  email: string;
  mobile: string;
}

// Initial state
export const initialState: UserState = {
  name: '',
  email: '',
  mobile: ''
};
```

---

## 2️⃣ `store/user.actions.ts`

```ts
// 👇 Manual action creator functions (no createAction)

export function editName(name: string) {
  return {
    type: 'name-edit',
    payload: name
  };
}

export function editEmail(email: string) {
  return {
    type: 'email-edit',
    payload: email
  };
}

export function editMobile(mobile: string) {
  return {
    type: 'mobile-edit',
    payload: mobile
  };
}
```

---

## 3️⃣ `store/user.reducer.ts`

```ts
import { initialState, UserState } from './user.state';

export function userReducer(state: UserState = initialState, action: any): UserState {
  switch (action.type) {
    case 'name-edit':
      return { ...state, name: action.payload };

    case 'email-edit':
      return { ...state, email: action.payload };

    case 'mobile-edit':
      return { ...state, mobile: action.payload };

    default:
      return state;
  }
}
```

---

## 4️⃣ `form/form.component.ts`

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

  editName(input: HTMLInputElement) {
    this.store.dispatch(editName(input.value));
    input.value = '';
  }

  editEmail(input: HTMLInputElement) {
    this.store.dispatch(editEmail(input.value));
    input.value = '';
  }

  editMobile(input: HTMLInputElement) {
    this.store.dispatch(editMobile(input.value));
    input.value = '';
  }
}
```

---

## 5️⃣ `form/form.component.html`

```html
<input #nameInput placeholder="Enter name" />
<button (click)="editName(nameInput)">Submit Name</button>

<input #emailInput placeholder="Enter email" />
<button (click)="editEmail(emailInput)">Submit Email</button>

<input #mobileInput placeholder="Enter mobile" />
<button (click)="editMobile(mobileInput)">Submit Mobile</button>
```

---

## 6️⃣ `display/display.component.ts`

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
    this.store.select('user').subscribe(data => {
      this.user = data;
      console.log('User from store:', this.user);
    });
  }
}
```

---

## 7️⃣ `display/display.component.html`

```html
<h3>User Details</h3>
<p><strong>Name:</strong> {{ user?.name }}</p>
<p><strong>Email:</strong> {{ user?.email }}</p>
<p><strong>Mobile:</strong> {{ user?.mobile }}</p>
```

---

## 8️⃣ `app.module.ts`

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
    StoreModule.forRoot({ user: userReducer }) // Register reducer
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

---

## 9️⃣ `app.component.html`

```html
<h1>NgRx - Manual Action Creator Example</h1>
<app-form></app-form>
<hr />
<app-display></app-display>
```

---

## 🧪 Sample Dispatch (manually)

```ts
store.dispatch(editEmail('anil@example.com'))

// result:
{
  type: 'email-edit',
  payload: 'anil@example.com'
}
```

---

## ✅ Summary

| Part                   | Status                                            |
| ---------------------- | ------------------------------------------------- |
| Manual actions         | ✅ `function() { return {type, payload} }` pattern |
| Reducer uses `payload` | ✅                                                 |
| Component dispatch     | ✅                                                 |
| Folder structure       | ✅                                                 |
| State tracking         | ✅                                                 |

---

Want this as a **downloadable project** or **upgrade to async effects next**?
I can also show you how to switch between `createAction` and manual style as a practice!

Let me know!
