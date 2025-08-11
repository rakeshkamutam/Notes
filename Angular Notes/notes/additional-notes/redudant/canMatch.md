
# 🛡️ Angular `CanMatch` Guard – Beginner-Friendly Notes

## 🔰 What is `CanMatch`?

* `CanMatch` is a **route guard introduced in Angular 14**.
* It is used to **conditionally match and activate a route** based on logic (e.g., user roles, authentication).
* Combines the power of both `canActivate` and `canLoad`.
* It is **ideal for route-based decision-making**, such as loading different modules for different users.

---

## 📦 Why Was It Introduced?

Before Angular 14:

* Developers used:

  * `canActivate` → Controls *access to routes*
  * `canLoad` → Controls *lazy loading of modules*
* 🛑 Problem:

  * Could **not use the same path (`home`) for multiple routes**.
  * Angular would **only load the first matched route**.

With `CanMatch`:

* You can **define multiple routes with the same path**.
* Angular will **evaluate each route using `canMatch`**, and choose the one that returns `true`.

---

## ⚙️ Real-Life Scenario

🧩 Use Case:
You have **two modules** with the same route path `/home`, but:

* `AdminHomeModule` should load for admins.
* `UserHomeModule` should load for regular users.


---

## 💡 Important Tips

| 🔍 Concern                | ✅ Best Practice                             |
| ------------------------- | ------------------------------------------- |
| Order of Routes           | Put `canMatch` routes **first**             |
| Multiple Same Paths       | Yes, allowed with `canMatch`                |
| Role-based Routing        | Use localStorage, service, or auth state    |
| Fallback Routing          | Always include a wildcard (`**`) route      |
| Lazy Loading Optimization | `CanMatch` helps load only relevant modules |

---

## ✅ Benefits of `CanMatch`

* 💡 Cleaner and more flexible route logic
* 🛡️ Secures lazy-loaded routes based on conditions
* ⚡ Better performance – avoids unnecessary module loading
* 🔄 Can reuse the same URL path (`/home`) with multiple outcomes

---

## 🧠 Summary

| Feature                              | Before Angular 14         | After Angular 14 (`canMatch`) |
| ------------------------------------ | ------------------------- | ----------------------------- |
| Match multiple routes with same path | ❌ Not possible            | ✅ Possible                    |
| Role-based navigation                | Manual, complex setup     | Simple with `canMatch`        |
| Route control                        | `canActivate` / `canLoad` | ✅ Unified with `canMatch`     |

---

## 🧱 Folder Structure Overview

```
src/
├── app/
│   ├── admin-home/
│   │   ├── admin-home.component.ts
│   │   └── admin-home.module.ts
│   ├── user-home/
│   │   ├── user-home.component.ts
│   │   └── user-home.module.ts
│   ├── forget/
│   │   └── forget.component.ts
│   ├── guards/
│   │   └── role.guard.ts
│   └── app-routing.module.ts
│   └── app.component.ts
```

---

## 1️⃣ Generate Components & Modules

```bash
ng generate module user-home --routing
ng generate module admin-home --routing
ng generate component user-home
ng generate component admin-home
ng generate component forget
ng generate guard guards/role --skip-tests
```

---

## 2️⃣ `UserHome` Routing (`user-home/user-home-routing.module.ts`)

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { UserHomeComponent } from './user-home.component';

const routes: Routes = [
  { path: '', component: UserHomeComponent }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class UserHomeRoutingModule {}
```

---

## 3️⃣ `AdminHome` Routing (`admin-home/admin-home-routing.module.ts`)

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AdminHomeComponent } from './admin-home.component';

const routes: Routes = [
  { path: '', component: AdminHomeComponent }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class AdminHomeRoutingModule {}
```

---

## 4️⃣ Lazy Load Modules with `canMatch` (`app-routing.module.ts`)

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ForgetComponent } from './forget/forget.component';
import { roleGuard } from './guards/role.guard';

const routes: Routes = [
  {
    path: 'home',
    loadChildren: () =>
      import('./admin-home/admin-home.module').then(m => m.AdminHomeModule),
    canMatch: [() => roleGuard('admin')]
  },
  {
    path: 'home',
    loadChildren: () =>
      import('./user-home/user-home.module').then(m => m.UserHomeModule),
    canMatch: [() => roleGuard('user')]
  },
  { path: '**', component: ForgetComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

---

## 5️⃣ Functional `role.guard.ts` (`guards/role.guard.ts`)

```ts
import { CanMatchFn } from '@angular/router';

export function roleGuard(expectedRole: 'admin' | 'user'): CanMatchFn {
  return () => {
    const role = localStorage.getItem('role');
    console.log('Current Role:', role);
    return role === expectedRole;
  };
}
```

---

## 6️⃣ Add Dummy Content

### `user-home.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-user-home',
  template: `<h2>User Home</h2>`,
})
export class UserHomeComponent {}
```

### `admin-home.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-admin-home',
  template: `<h2>Admin Home</h2>`,
})
export class AdminHomeComponent {}
```

### `forget.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-forget',
  template: `<h2>Page not found or no role assigned</h2>`,
})
export class ForgetComponent {}
```

---

## 🧪 Test It Out

1. Open browser dev tools → `Application` → `Local Storage`.
2. Add key:

   ```js
   localStorage.setItem('role', 'admin')   // or 'user'
   ```
3. Navigate to: `http://localhost:4200/home`

📍 Behavior:

* `role = 'admin'` → Loads `AdminHomeComponent`
* `role = 'user'` → Loads `UserHomeComponent`
* No role or invalid → Loads `ForgetComponent`

---

## ✅ Summary

| Feature             | Purpose                                    |
| ------------------- | ------------------------------------------ |
| `canMatch`          | Conditionally matches a route              |
| Same path (`/home`) | Loads different modules for admin vs user  |
| Lazy Loading        | Only loads necessary module based on guard |
| `localStorage`      | Mocked login/role state                    |
| Wildcard `**` route | Fallback in case nothing matches           |

---


## 🛑 The Problem: Duplicate Route Paths Without `CanMatch`

### 🧠 The Rule in Angular (before v14):

> Angular's router matches **only the first route** with a matching path.
> It **ignores any later routes** with the same path.

---

## 🔍 Example Before Angular 14 (without `canMatch`)

```ts
const routes: Routes = [
  {
    path: 'home',
    loadChildren: () =>
      import('./admin-home/admin-home.module').then(m => m.AdminHomeModule),
  },
  {
    path: 'home', // 👈 same path again!
    loadChildren: () =>
      import('./user-home/user-home.module').then(m => m.UserHomeModule),
  },
];
```

### ❌ What Happens:

* You visit: `http://localhost:4200/home`
* Angular sees the first matching `path: 'home'` → **loads only `admin-home`**
* The second `'home'` route is **ignored** — even if the user is *not* an admin!

So:

* **You can’t say “If admin, load this module; if user, load another”** with the same URL.
* The second route never gets a chance.

---

## ✅ The Solution: Use `CanMatch` (Angular 14+)

Now you can write **multiple routes with the same path**, but add a condition:

```ts
const routes: Routes = [
  {
    path: 'home',
    loadChildren: () =>
      import('./admin-home/admin-home.module').then(m => m.AdminHomeModule),
    canMatch: [() => roleGuard('admin')] // ✅ Only matches if admin
  },
  {
    path: 'home',
    loadChildren: () =>
      import('./user-home/user-home.module').then(m => m.UserHomeModule),
    canMatch: [() => roleGuard('user')]  // ✅ Only matches if user
  }
];
```

### ✔️ Now what happens:

* Angular checks:

  1. Does the first route **match path** *and* `canMatch` returns `true`? If yes → use it.
  2. If not, check the next route, and so on.
* Each route is **conditionally matched** — so only the right module loads based on logic (like user role).

---

## 🔁 Analogy: Think of Route Matching Like a Queue

| Before `canMatch`                      | With `canMatch`                            |
| -------------------------------------- | ------------------------------------------ |
| Only first door opens. Others ignored. | Each door has a guard checking permission. |

---

## 🔧 Summary

| Concept             | Without `canMatch`      | With `canMatch`                   |
| ------------------- | ----------------------- | --------------------------------- |
| Multiple same paths | ❌ Not allowed           | ✅ Allowed with guards             |
| Role-based modules  | ❌ Not possible directly | ✅ Easy using `canMatch`           |
| Match logic         | Static                  | Dynamic (role, permissions, etc.) |



