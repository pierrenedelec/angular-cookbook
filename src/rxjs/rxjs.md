# RxJS

RxJS represents values that arrive over time as **Observables**. An Observable
can emit no values, one value, or many values, then either complete or fail.
This makes it useful for events, timers, and asynchronous work such as HTTP
requests.

## Create and transform a stream

An Observable describes a source of values. Operators let you transform those
values and compose asynchronous work without changing the original source:

```ts
import { filter, from, map } from 'rxjs';

const names = ['Ada', 'Grace', 'Linus'];
const longNames$ = from(names).pipe(
  filter((name) => name.length > 4),
  map((name) => name.toUpperCase()),
);
```

`from` creates a stream from an iterable. `pipe` applies operators in order:
here, `filter` keeps names longer than four characters, then `map` transforms
each remaining value. The `$` suffix is a common naming convention for
Observable-valued properties; it has no special meaning to TypeScript or RxJS.

An Observable does not deliver values to a consumer until it is subscribed to.
Subscribing returns a `Subscription`, which can be unsubscribed to stop receiving
values and release resources. Sources differ in how they behave: for example,
subscribing to an `interval` starts a timer, while each subscription to an
`HttpClient` Observable makes a new request.

## Choose operators by intent

Use operators to describe each step of an asynchronous workflow:

- `map` transforms each emitted value.
- `filter` lets only matching values through.
- `debounceTime` waits for a pause in emissions, often useful for text input.
- `switchMap` starts work for each value and switches to the latest inner
  Observable, which is useful for searches where older results are no longer
  needed.
- `catchError` handles an error by returning a replacement Observable.

Operators such as `map`, `filter`, and `switchMap` return a new Observable;
they do not mutate the source. Keep the pipeline focused on transforming values
and coordinating asynchronous work. Handle errors deliberately: choose whether
to show an error state, retry, or provide a fallback rather than silently
turning every failure into success.

## Display a stream in an Angular template

For values that a template displays, expose an Observable and use Angular's
`AsyncPipe`. The pipe subscribes, updates the view when values arrive, and
unsubscribes when the component is destroyed.

```ts,angular
import { AsyncPipe } from '@angular/common';
import { Component } from '@angular/core';
import { interval, map } from 'rxjs';

@Component({
  imports: [AsyncPipe],
  template: `
    @if (elapsedSeconds$ | async; as elapsedSeconds) {
      <p>Elapsed seconds: {{ elapsedSeconds }}</p>
    } @else {
      <p>Starting timer…</p>
    }
  `,
})
export class TimerComponent {
  readonly elapsedSeconds$ = interval(1000).pipe(
    map((tick) => tick + 1),
  );
}
```

`interval` emits an increasing number once per second, starting with `0` after
the first second. `map` turns that into elapsed seconds starting at `1`. The
`AsyncPipe` owns the template subscription, so the component does not need to
subscribe manually just to render the timer.

When imperative code must subscribe—for example, to perform an action that is
not part of rendering—ensure that the subscription ends when its owner is no
longer needed. In Angular, `takeUntilDestroyed` from
`@angular/core/rxjs-interop` can bind an Observable subscription to a component
or service's destruction context.

For more details, see the RxJS [Observable guide](https://rxjs.dev/guide/observable)
and [operators guide](https://rxjs.dev/guide/operators), Angular's
[HTTP guide](https://angular.dev/guide/http/making-requests),
[AsyncPipe API](https://angular.dev/api/common/AsyncPipe), and
[`takeUntilDestroyed` API](https://angular.dev/api/core/rxjs-interop/takeUntilDestroyed).
