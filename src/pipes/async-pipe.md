# Async Pipe

`AsyncPipe` unwraps the latest value emitted by an `Observable` or resolved by a
`Promise`. It marks the component for checking when a new value arrives and
unsubscribes when the component is destroyed. If the expression starts referring
to a different asynchronous source, the pipe unsubscribes from the old source and
subscribes to the new one.

```ts
import { AsyncPipe } from '@angular/common';
import { Component } from '@angular/core';
import { Observable, of } from 'rxjs';

interface Profile {
  name: string;
}

@Component({
  selector: 'app-profile',
  imports: [AsyncPipe],
  template: `
    @if (profile$ | async; as profile) {
      <p>Welcome, {{ profile.name }}!</p>
    } @else {
      <p>Loading profile…</p>
    }
  `,
})
export class ProfileComponent {
  profile$: Observable<Profile> = of({ name: 'Ada' });
}
```

The `@if` block handles the period before the first value arrives and binds the
emitted value to `profile`. The source is evaluated once in this template, avoiding
multiple `async` subscriptions to the same stream.

## Use with component state

The async pipe works well with observable state exposed by a component:

```ts
readonly profile$ = this.profileService.getProfile();
```

The pipe owns the template subscription, so there is no need to manually subscribe
just to render the result or remember to unsubscribe in a lifecycle hook. For a
stream that can fail, handle errors in the stream or present an explicit error
state; the pipe does not turn an error into a successful value.

Use `AsyncPipe` from `@angular/common` in the component's `imports`. For details,
see the official [`AsyncPipe` API](https://angular.dev/api/common/AsyncPipe) and
[pipes guide](https://angular.dev/guide/templates/pipes).
