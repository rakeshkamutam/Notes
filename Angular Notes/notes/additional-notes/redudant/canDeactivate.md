Certainly! Below is a **beginner-friendly, clean Angular 13+ example** demonstrating both **functional** and **class-based `CanDeactivate` guards** with routing, form, and a real-world scenario.

---

## 🧪 Scenario

We have a form in `FormComponent`. If the user edits it and tries to navigate away (e.g., to `HomeComponent`), we want to **warn them with a confirmation prompt**.

---

## 🗂️ Project Setup

### ✅ Generate Components

```bash
ng generate component home --skip-tests
ng generate component form --skip-tests
```

---

## 🧭 App Routing Module (`app-routing.module.ts`)

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { FormComponent } from './form/form.component';
import { formDeactivateGuard } from './form-deactivate.guard'; // functional
// import { FormDeactivateClassGuard } from './form-deactivate-class.guard'; // class version

const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  {
    path: 'form',
    component: FormComponent,
    canDeactivate: [formDeactivateGuard], // ✅ Use functional here
    // canDeactivate: [FormDeactivateClassGuard], // ⛔️ Use this for class version
  },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

---

## 🧾 FormComponent

### `form.component.ts`

```ts
import { Component } from '@angular/core';
import { FormControl, Validators } from '@angular/forms';

@Component({
  selector: 'app-form',
  templateUrl: './form.component.html',
})
export class FormComponent {
  username = new FormControl('', Validators.required);
}
```

### `form.component.html`

```html
<h2>Form Page</h2>
<input placeholder="Username" [formControl]="username" />
<br /><br />
<a routerLink="/home">Go to Home</a>
```

---

## 🧷 Functional CanDeactivate Guard

### ✅ Generate

```bash
ng generate guard form-deactivate --skip-tests --functional
```

### `form-deactivate.guard.ts`

```ts
import { CanDeactivateFn } from '@angular/router';
import { FormComponent } from './form/form.component';

export const formDeactivateGuard: CanDeactivateFn<FormComponent> = (
  component
) => {
  if (component.username?.dirty) {
    return confirm('You have unsaved changes. Are you sure you want to leave?');
  }
  return true;
};
```

---

## 🧱 Class-Based CanDeactivate Guard

### ✅ Generate

```bash
ng generate guard form-deactivate-class --skip-tests
```

### `form-deactivate-class.guard.ts`

```ts
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { FormComponent } from './form/form.component';

@Injectable({ providedIn: 'root' })
export class FormDeactivateClassGuard implements CanDeactivate<FormComponent> {
  canDeactivate(component: FormComponent): boolean {
    if (component.username?.dirty) {
      return confirm('You have unsaved changes (from class guard). Proceed?');
    }
    return true;
  }
}
```

---

## 🏠 HomeComponent (Simple navigation UI)

### `home.component.html`

```html
<h2>Home Page</h2>
<a routerLink="/form">Go to Form</a>
```

---

## 💡 Notes

| Feature                  | Functional Guard       | Class-Based Guard            |
| ------------------------ | ---------------------- | ---------------------------- |
| Simpler syntax           | ✅                      | ❌ Requires `@Injectable`     |
| Preferred in Angular 16+ | ✅                      | ❌ Deprecated warning appears |
| Testability              | Good                   | Good                         |
| Performance              | Faster (tree-shakable) | Slightly slower              |

---

## ✅ Optional: Destroy Lifecycle Check

Add to `FormComponent`:

```ts
ngOnDestroy() {
  console.log('FormComponent destroyed');
}
```

This helps verify that **`CanDeactivate` runs before `ngOnDestroy()`**.

---

Let me know if you'd like a downloadable `.zip` or PDF of this example.

Learn more on Glasp: [https://glasp.co/reader?url=https://www.youtube.com/watch?v=atWo8aj\_Bq8](https://glasp.co/reader?url=https://www.youtube.com/watch?v=atWo8aj_Bq8)
