Sure! Below is a **clear, simple example** of the `CanActivate` guard in both **functional** and **class-based** ways, side by side, with everything separated for easy understanding.

---

## ✅ Use Case:

Protect the `/dashboard` route. If the user is **not logged in** (no token in `localStorage`), redirect to `/auth/login`.

---

## 🧱 Folder Structure (Minimal)

```
src/
├── app/
│   ├── guards/
│   │   ├── auth.guard.ts               ← functional
│   │   └── auth-class.guard.ts         ← class-based
│   ├── auth/
│   │   └── login.component.ts
│   ├── dashboard.component.ts
│   ├── app-routing.module.ts
│   └── app.module.ts
```

---

# 🔹 Functional CanActivate Guard

### ✅ 1. `auth.guard.ts`

```ts
// src/app/guards/auth.guard.ts
import { CanActivateFn } from '@angular/router';
import { inject } from '@angular/core';
import { Router } from '@angular/router';

export const authGuard: CanActivateFn = (route, state) => {
  const router = inject(Router);
  const token = localStorage.getItem('token');

  console.log('Token:', token);

  if (token) {
    return true;
  } else {
    router.navigate(['/auth/login']);
    return false;
  }
};
```

### ✅ 2. Routing: use Functional Guard

```ts
// src/app/app-routing.module.ts
import { authGuard } from './guards/auth.guard';

const routes: Routes = [
  { path: 'auth/login', component: LoginComponent },
  { path: 'dashboard', component: DashboardComponent, canActivate: [authGuard] },
  { path: '', redirectTo: 'dashboard', pathMatch: 'full' }
];
```

---

# 🔸 Class-Based CanActivate Guard

### ✅ 1. `auth-class.guard.ts`

```ts
// src/app/guards/auth-class.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

@Injectable({ providedIn: 'root' })
export class AuthClassGuard implements CanActivate {
  constructor(private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    const token = localStorage.getItem('token');
    console.log('Token:', token);

    if (token) {
      return true;
    } else {
      this.router.navigate(['/auth/login']);
      return false;
    }
  }
}
```

### ✅ 2. Routing: use Class-Based Guard

```ts
// src/app/app-routing.module.ts
import { AuthClassGuard } from './guards/auth-class.guard';

const routes: Routes = [
  { path: 'auth/login', component: LoginComponent },
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthClassGuard] },
  { path: '', redirectTo: 'dashboard', pathMatch: 'full' }
];
```

---

## ✅ Components for Testing

### `login.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-login',
  template: `
    <h3>Login Page</h3>
    <button (click)="login()">Login</button>
  `
})
export class LoginComponent {
  login() {
    localStorage.setItem('token', 'demo-token');
  }
}
```

### `dashboard.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-dashboard',
  template: `<h2>Dashboard - Protected Route</h2>`
})
export class DashboardComponent {}
```

---

## 🧪 Test Flow

1. Start app → it tries to load `/dashboard`.
2. If no token → redirects to `/auth/login`.
3. Click “Login” → token is set.
4. Go back to `/dashboard` → now allowed.
5. Remove token → try `/dashboard` → again blocked.

---

## 🟨 Summary Table

| Feature           | Functional (`auth.guard.ts`)     | Class-Based (`auth-class.guard.ts`)       |
| ----------------- | -------------------------------- | ----------------------------------------- |
| Syntax Style      | Function                         | Class with `@Injectable()`                |
| Token Check Logic | Inside function using `inject()` | Inside class method using `constructor()` |
| Usage in Routes   | `canActivate: [authGuard]`       | `canActivate: [AuthClassGuard]`           |
| Simplicity        | ✅ Shorter                        | 🧱 More scalable and configurable         |

---

Would you like this as a downloadable code zip or PDF with both versions side-by-side?
