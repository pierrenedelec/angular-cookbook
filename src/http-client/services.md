# Creating and Using Angular Services

Services are classes that hold reusable functionality or data that components
can share. They are useful for keeping component code focused on the view while
placing shared logic in one place.

## Create a service

Use the Angular CLI to generate a service:

```sh
ng generate service greeting
```

You can also create the class yourself. Decorate it with `@Service()` so Angular
can provide it through dependency injection:

```ts
import { Service } from '@angular/core';

@Service()
export class GreetingService {
  greet(name: string): string {
    return `Hello, ${name}!`;
  }
}
```

## Inject and use the service in a component

Inject the service into a component with Angular's `inject()` function. The
component can then call the service method and display its result:

```ts
import { Component, inject } from '@angular/core';
import { GreetingService } from './greeting.service';

@Component({
  selector: 'app-greeting',
  template: `<p>{{ greeting }}</p>`,
})
export class GreetingComponent {
  private readonly greetingService = inject(GreetingService);
  readonly greeting = this.greetingService.greet('Angular');
}
```

In this example, the component delegates the greeting logic to the service and
uses the returned string in its template. The `@Service()` decorator makes the
service available for injection using Angular's default application-level
provisioning.

For more detail, see Angular's guide to
[creating and using services](https://angular.dev/guide/di/creating-and-using-services)
and the [`Service` API](https://angular.dev/api/core/Service).
