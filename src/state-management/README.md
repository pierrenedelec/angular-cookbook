# State Management

State is the data that influences what an application displays or how it behaves. Good state management starts with deciding who owns each piece of data, which parts of the UI need it, and how long it should live. Use the smallest scope that fits; a state library is not a prerequisite for building an Angular application.

## Start with component state

Keep state in a component when it is only needed by that component, such as a selected tab, an expanded panel, or a small form interaction. Angular signals are a convenient way to represent synchronous state in a component. Try the [live task-counter example](live-example.md) to see writable and derived state together.

Use component inputs and outputs when a parent and child need to coordinate. For state shared by multiple components, move ownership to a service or to a common ancestor rather than maintaining separate, potentially conflicting copies.

## Share state with a service

An injectable service is a good fit for state shared across a feature or application, especially when it also owns the rules for changing that state or coordinates it with an API. A service provided at the application root is shared by consumers in that application injector; providing it at a component or route scope can give a feature its own instance and lifetime.

A signal-based service can keep its writable signals private, expose read-only signals and `computed` values, and offer methods for updates. Components then consume the same source of truth without depending on one another. Keep transient presentation details local unless other parts of the application genuinely need them.

## When to use RxJS or a store library

Signals work well for synchronous state and derived values. RxJS remains useful for asynchronous streams, such as composing HTTP requests, user events, cancellation, or time-based operations. Use the tools that fit the data flow; avoid maintaining the same value independently in both a signal and an observable.

A larger store library such as [NgRx Store](https://ngrx.io/guide/store) may be worthwhile when the application benefits from explicit actions and reducers, standardized effects, centralized state transitions, or supporting tooling across a large team. Compare those benefits with the additional concepts and maintenance cost. For a small feature, a component signal or a focused service is often easier to understand and test.

## A practical checklist

- Keep each value close to the component or feature that owns it.
- Derive values with `computed` instead of storing duplicate values that can get out of sync.
- Encapsulate shared updates behind service methods, and expose only the state consumers need.
- Keep server persistence, caching, and UI-only state distinct; a client-side store does not replace a backend.
- Test state transitions and important derived values, then test the components or services that use them.

For details on signal behavior, see the [Angular Signals guide](https://angular.dev/guide/signals). For service scopes and dependency injection, see the [Angular dependency injection guide](https://angular.dev/guide/di).
