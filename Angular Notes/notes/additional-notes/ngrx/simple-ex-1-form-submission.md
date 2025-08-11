Great! Below is the **complete working structure and code** based on your request:

---

## ✅ Folder Structure

```
src/
├── app/
│   ├── reducers/
│   │   └── user.reducer.ts
│   ├── form/
│   │   └── form.component.ts
│   │   └── form.component.html
│   ├── display/
│   │   └── display.component.ts
│   │   └── display.component.html
│   ├── app.component.html
│   ├── app.module.ts
```

---

## 📁 `reducers/user.reducer.ts`

```ts
export const initialState = {
  name: '',
  email: '',
  mobile: ''
};

export function userReducer(state = initialState, action: any) {
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

## 🛠️ `app.module.ts`

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { StoreModule } from '@ngrx/store';
import { AppComponent } from './app.component';
import { userReducer } from './reducers/user.reducer';
import { FormComponent } from './form/form.component';
import { DisplayComponent } from './display/display.component';

@NgModule({
  declarations: [AppComponent, FormComponent, DisplayComponent],
  imports: [
    BrowserModule,
    FormsModule,
    StoreModule.forRoot({ user: userReducer })
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

---

## 🧠 `form.component.ts`

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';

@Component({
  selector: 'app-form',
  templateUrl: './form.component.html'
})
export class FormComponent {
  constructor(private store: Store<any>) {}

  updateName(input: HTMLInputElement) {
    this.store.dispatch({ type: 'name-edit', payload: input.value });
    input.value = '';
  }

  updateEmail(input: HTMLInputElement) {
    this.store.dispatch({ type: 'email-edit', payload: input.value });
    input.value = '';
  }

  updateMobile(input: HTMLInputElement) {
    this.store.dispatch({ type: 'mobile-edit', payload: input.value });
    input.value = '';
  }
}
```

---

## 🧾 `form.component.html`

```html
<!-- Name -->
<input #nameInput placeholder="Enter name" />
<button (click)="updateName(nameInput)">Submit Name</button>

<!-- Email -->
<input #emailInput placeholder="Enter email" />
<button (click)="updateEmail(emailInput)">Submit Email</button>

<!-- Mobile -->
<input #mobileInput placeholder="Enter mobile" />
<button (click)="updateMobile(mobileInput)">Submit Mobile</button>
```

---

## 🧠 `display.component.ts`

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';

@Component({
  selector: 'app-display',
  templateUrl: './display.component.html'
})
export class DisplayComponent {
  user: any = {};

  constructor(private store: Store<any>) {
    this.store.select('user').subscribe(data => {
      this.user = data;
      console.log('User from store:', this.user);
    });
  }
}
```

---

## 🎨 `display.component.html`

```html
<h3>Display User Info</h3>
<p><strong>Name:</strong> {{ user.name }}</p>
<p><strong>Email:</strong> {{ user.email }}</p>
<p><strong>Mobile:</strong> {{ user.mobile }}</p>
```

---

## 🌐 `app.component.html`

```html
<h1>NgRx Simple Input Example</h1>
<app-form></app-form>
<hr />
<app-display></app-display>
```

---

## ✅ OUTPUT:

* Enter name/email/mobile in input box → Click button → Updates the store.
* `DisplayComponent` listens to the store and prints values + logs them.

---

Let me know if you want:

* Reactive Forms version
* Validation
* Saving data in `localStorage`
* Zip or StackBlitz version

Want to add reset, edit, or delete actions?
