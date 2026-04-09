---
name: Angular (Latest Version)
description: A comprehensive guide to developing modern Angular applications using the latest features and best practices (v21+).
---

# Modern Angular Development Guide (v21+)

This skill provides guidelines and best practices for building robust, scalable, and high-performance applications using the latest features of Angular v21 (released late 2025).

## 🚀 Key Features of Angular v21

1.  **Zoneless by Default**: New applications are configured without `Zone.js` by default, unlocking better performance, smaller bundles, and better async/await debugging.
2.  **Signal Forms (Experimental)**: A new, strictly typed, signal-based forms API that eventually replaces `ReactiveForms` and `ControlValueAccessor`.
3.  **Angular Aria**: A built-in library of unstyled, accessible primitives (headless UI) for building custom component libraries.
4.  **Vitest**: The default unit testing runner, replacing Karma/Jasmine.
5.  **Built-in Control Flow**: Stable and preferred over `*ngIf`/`*ngFor`.
6.  **Signals Architecture**: The standard for state management.
7.  **AI-Powered DX**: Enhanced CLI integration with AI tools.

## 🛠️ Project Setup (v21)

To create a new modern Angular v21 project in the current directory:

```bash
npx -p @angular/cli@latest ng new my-app --directory ./ --standalone --strict --ssr --style=scss --skip-git --skip-install --defaults
```

*   **Note**: In v21, `Zone.js` is opt-out or removed by default in `angular.json` / `app.config.ts`.
*   **Testing**: Vitest is now configured automatically.

## 🏗️ Architecture & Best Practices

### 1. Zoneless & Signals
Angular v21 apps should rely entirely on Signals for change detection.
-   **Change Detection**: `provideExperimentalZonelessChangeDetection()` is now standard (or simply the default behavior).
-   **Async Pipe**: Use less often; prefer `toSignal()` to read Observables synchronously in templates.

```typescript
// app.config.ts
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    // In v21, you might not even need provideZoneChangeDetection for new apps
    // if using the zoneless provider:
    // provideExperimentalZonelessChangeDetection(), 
    provideRouter(routes),
    // HttpClient is provided by default in v21!
  ]
};
```

### 2. Signal Forms (New in v21)
Use the new `input` based forms where possible (check stability status as it evolves).

```typescript
import { Component, signal } from '@angular/core';
import { Form, FormField } from '@angular/forms-signals'; // Pseudo-code for new API

@Component({
  ...
})
export class LoginWithSignals {
  // The API focuses on direct signal binding
  username = signal('');
  password = signal('');
  
  // derived state
  isValid = computed(() => this.username().length > 0 && this.password().length > 5);
}
```

*Note: Since Signal Forms is experimental, stick to `ReactiveFormsModule` for mission-critical enterprise forms until officially stable.*

### 3. Standalone Components
Continue to use `standalone: true`.
-   **Inputs**: Use `input<T>()` and `input.required<T>()`.
-   **Outputs**: Use `output<T>()`.
-   **ViewChild**: Use `viewChild<T>()` which returns a Signal.

```typescript
import { Component, input, output, viewChild, ElementRef } from '@angular/core';

@Component({
  standalone: true,
  template: `<input #inputEl (input)="onInput($event)" />`
})
export class CustomInput {
  label = input.required<string>();
  valueChanged = output<string>();
  
  // Signal-based ViewChild
  inputElement = viewChild.required<ElementRef>('inputEl');

  onInput(e: Event) {
    // ...
  }
}
```

### 4. Data Fetching (Resources)
Angular v19/v20 introduced `resource` and `rxResource` to handle async data loading with Signals elegantly.

```typescript
import { Component, resource } from '@angular/core';

@Component({ ... })
export class UserProfile {
  userId = input.required<string>();

  // defined resource that automatically re-fetches when userId changes
  userResource = resource({
    request: this.userId, 
    loader: ({ request }) => fetch(`/api/users/${request}`).then(r => r.json())
  });

  // Template usage:
  // @if (userResource.isLoading()) { ... }
  // @if (userResource.error()) { ... }
  // {{ userResource.value()?.name }}
}
```

## 📂 Project Structure Guide

Organize by **Features**, not by Type.

```
src/
  app/
    core/             # Singleton services, interceptors, guards
    shared/           # UI Kit (consider using Angular Aria primitives here)
    features/         # Smart components and routes
      home/
      settings/
    app.component.ts
    app.config.ts
    app.routes.ts
    main.ts
```

## 📦 Recommended Libraries (v21)
-   **UI Primitives**: Angular Aria (built-in).
-   **Testing**: Vitest (built-in support).
-   **Forms**: Reactive Forms (Legacy/Stable) or Signal Forms (Experimental).
-   **State**: Signals (Local), NgRx Signals Store (Global).

## ✨ Clean Code Guidelines
-   **No Zone**: Avoid any code that relies on `Zone.current`.
-   **Strict Types**: `SimpleChanges` is generic now. Use `SimpleChanges<MyComponent>`.
-   **Inject**: Use `inject()` for everything.
-   **Resource API**: Prefer `resource()` over manual `useEffect` + `fetch` or complex RxJS for simple data loading.

---
Using this skill, you can confidently architect and build Angular v21+ applications.
