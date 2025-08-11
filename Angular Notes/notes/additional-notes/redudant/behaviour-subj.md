Here's a **beginner-friendly Angular mini project** with:

* `AddTodoComponent` – to add new todos
* `ShowTodoComponent` – to display todos
* `TodoService` – shared service using RxJS `BehaviorSubject` to manage todo list

---

## 🧠 Goal

Learn **Services**, **RxJS**, and **Component Communication** in Angular by building a basic Todo app.

---

## 🗂 Project Structure

```
src/
├── app/
│   ├── add-todo/
│   │   └── add-todo.component.ts / html / css
│   ├── show-todo/
│   │   └── show-todo.component.ts / html / css
│   ├── todo.service.ts
│   ├── app.component.ts / html
```

---

## ✅ Step-by-step Implementation

---

### 1. Generate Components and Service

```bash
ng g c add-todo
ng g c show-todo
ng g s todo
```

---

### 2. `TodoService` – handles the todo list using RxJS

```ts
// todo.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class TodoService {
  private todosSubject = new BehaviorSubject<string[]>([]);
  todos$ = this.todosSubject.asObservable();

  addTodo(todo: string) {
    const current = this.todosSubject.value;
    this.todosSubject.next([...current, todo]);
  }

    /*
  if usign the subj

  private todosSubject = new BehaviorSubject<string[]>([]);
  todosSubject = new Subject<string[]>(); // ❌ No initial value
  // On late subscription
  todoService.todos$.subscribe(...) // gets nothing unless something is emitted again

  sendTodos() {
    this.todosSubject.next(this.todos);
  }

    */

}
```

---

### 3. `AddTodoComponent` – input box to add new todo

```ts
// add-todo.component.ts
import { Component } from '@angular/core';
import { TodoService } from '../todo.service';

@Component({
  selector: 'app-add-todo',
  templateUrl: './add-todo.component.html',
})
export class AddTodoComponent {
  newTodo = '';

  constructor(private todoService: TodoService) {}

  addTodo() {
    if (this.newTodo.trim()) {
      this.todoService.addTodo(this.newTodo);
      this.newTodo = '';
    }
  }
}
```

```html
<!-- add-todo.component.html -->
<input [(ngModel)]="newTodo" placeholder="Add new todo" />
<button (click)="addTodo()">Add</button>
```

---

### 4. `ShowTodoComponent` – subscribes to todo list from service

```ts
// show-todo.component.ts
import { Component, OnInit } from '@angular/core';
import { TodoService } from '../todo.service';

@Component({
  selector: 'app-show-todo',
  templateUrl: './show-todo.component.html',
})
export class ShowTodoComponent implements OnInit {
  todos: string[] = [];

  constructor(private todoService: TodoService) {}

  ngOnInit(): void {
    this.todoService.todos$.subscribe((list) => {
      this.todos = list;
    });
  }
}
```

```html
<!-- show-todo.component.html -->
<ul>
  <li *ngFor="let todo of todos">{{ todo }}</li>
</ul>
```

---

### 5. Use both components in `AppComponent`

```html
<!-- app.component.html -->
<h1>📝 Todo List</h1>
<app-add-todo></app-add-todo>
<app-show-todo></app-show-todo>
```

---

### 6. Don’t forget to import `FormsModule`

In `app.module.ts`:

```ts
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule
  ],
  ...
})
```

---

## 🧪 Test It!

1. Run the app: `ng serve`
2. Add a todo from input
3. It should appear immediately below

---

## 🧠 What You Learned

| Concept              | How it's used                         |
| -------------------- | ------------------------------------- |
| `Service`            | Shared state for todos                |
| `BehaviorSubject`    | Emits and stores the latest todo list |
| `subscribe()`        | `ShowTodo` reacts to todo changes     |
| `providedIn: 'root'` | Makes the service a singleton         |
| `FormsModule`        | Used for `[(ngModel)]` binding        |

---

Would you like this as a downloadable PDF or need enhancements (delete, edit todos, etc)?



### ✅ 3. Use `Subject` + `getTodos()` function

Maintain state internally and expose it only on demand:

```ts
private todos: string[] = [];
private todosSubject = new Subject<string[]>();
todos$ = this.todosSubject.asObservable();

addTodo(todo: string) {
  this.todos.push(todo);
  this.todosSubject.next(this.todos); // push latest
}

getTodos() {
  // explicitly called by component on init
  this.todosSubject.next(this.todos);
}
```

```ts
// in ShowTodoComponent
ngOnInit() {
  this.todoService.getTodos(); // manually fetch list
  this.todoService.todos$.subscribe(todos => this.todos = todos);
}
```
