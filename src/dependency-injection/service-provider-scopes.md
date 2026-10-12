# Understanding Angular service provider scopes

Where you provide a service determines which parts of an Angular application can inject it, which instance they receive, and how long that instance can live. Angular resolves a dependency from the nearest injector that provides its token, so a child scope can use its own instance instead of an ancestor's.

An instance is created when its provider is first requested and is then reused by that injector. In other words, a provider's scope is also the boundary for sharing and caching that instance.

## Application-wide services with the root injector

For a service shared across an application, the usual choice is `providedIn: 'root'`:

```ts
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class PreferencesService {
  compactMode = false;
}
```

Angular registers this provider in the application's root `EnvironmentInjector`. Components and other services in that application receive the same instance unless a nearer injector provides the same token. The instance is created on first use and remains available for the lifetime of the application.

Application-level providers configured in `ApplicationConfig` (for example, the `providers` passed to `bootstrapApplication`) also belong to the root environment scope. Use that option when a provider needs explicit application configuration. Prefer one registration for a token at a given scope rather than registering the same service in multiple ways.

## Component-scoped instances

Add a service to a component's `providers` when each component instance needs isolated state:

```ts
import { Component, Injectable, inject } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class DraftService {
  text = '';
}

@Component({
  selector: 'app-draft-editor',
  providers: [DraftService],
  template: `Draft: {{ draft.text }}`,
})
export class DraftEditorComponent {
  readonly draft = inject(DraftService);
}
```

Although `DraftService` has a root provider, `DraftEditorComponent` provides the same token locally. That nearer provider takes precedence: each editor component instance gets its own `DraftService`, and components below that editor can resolve that instance. Other parts of the application that are outside the editor's component subtree continue to use the root instance.

The component's injector owns its local service instance. When Angular destroys that component, it also destroys the injector and calls the service's `ngOnDestroy` hook, if it has one. This makes component providers useful for state that should be shared within a component subtree but not between separate instances of that subtree.

## Feature-scoped providers with routes

For a service shared by a routed feature and its descendants, register it in the route's `providers` array:

```ts
import { Routes } from '@angular/router';
import { DraftService } from './draft.service';

export const routes: Routes = [
  {
    path: 'drafts',
    providers: [DraftService],
    loadComponent: () =>
      import('./drafts-page.component').then((m) => m.DraftsPageComponent),
  },
];
```

The Router creates an `EnvironmentInjector` for a route with providers. The route's component and its descendants can resolve those providers, while unrelated routes do not inherit them. If the same token is provided at the root and at the route, the route provider is nearer and takes precedence within that route's scope.

Route providers are not component providers: their lifetime follows the route's environment injector, which is managed by the Router. Do not assume that navigating away and back creates a fresh service instance. If state must reset for each component instance, provide the service on that component instead.

## Choosing a scope

| Provider location | Typical sharing boundary | Lifetime |
| --- | --- | --- |
| `providedIn: 'root'` or application `providers` | One application | The application's root environment injector |
| Route `providers` | A route and its descendants | The route environment injector managed by the Router |
| Component `providers` | One component subtree | The component's injector |

Choose the narrowest scope that matches the intended sharing boundary. Use the root for application-wide state, route providers for feature-level dependencies, and component providers for state that belongs to a particular component instance.

For the full injector search order and advanced resolution options, see [Angular's hierarchical dependency injection guide](https://angular.dev/guide/di/hierarchical-dependency-injection).
