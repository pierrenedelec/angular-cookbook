# Testing Angular Services

Test a service's behavior independently from the component or template that uses
it. Use Angular's `TestBed` when the service is obtained through Angular's
dependency injection system or when you need to replace one of its dependencies.
For logic that does not rely on Angular or injected dependencies, a direct unit
test can be simpler.

## Retrieve the service with `TestBed`

Configure the test providers before requesting the service. `TestBed.inject`
returns the instance created in that test environment:

```ts
import { TestBed } from '@angular/core/testing';
import { beforeEach, describe, expect, it } from 'vitest';
import { CartService } from './cart.service';

describe('CartService', () => {
  let service: CartService;

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [CartService],
    });
    service = TestBed.inject(CartService);
  });

  it('calculates the cart total', () => {
    expect(service.total([{ price: 12 }, { price: 8 }])).toBe(20);
  });
});
```

The service and method in this example represent the application's own API.
Replace the sample data and expected value with cases that exercise your service's
behavior, including boundary cases and error handling where relevant.

## Replace a dependency with a test value

When a service collaborates with another service, provide a controlled value for
that dependency. This keeps the test focused on the service under test and avoids
calling the collaborator's real implementation:

```ts
import { TestBed } from '@angular/core/testing';
import { beforeEach, describe, expect, it } from 'vitest';
import { OrderTotalService } from './order-total.service';
import { TaxCalculator } from './tax-calculator';

describe('OrderTotalService', () => {
  let service: OrderTotalService;

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [
        OrderTotalService,
        { provide: TaxCalculator, useValue: { calculate: () => 5 } },
      ],
    });
    service = TestBed.inject(OrderTotalService);
  });

  it('adds the calculated tax to the subtotal', () => {
    expect(service.total(100)).toBe(105);
  });
});
```

Keep the fake's shape compatible with the methods the service calls. If you need
to assert that a collaborator was called, use a spy from your test runner and
check the call separately from the result.

## Services that make HTTP requests

Do not let a unit test make a real network request. Angular's
[HTTP testing utilities](https://angular.dev/guide/http/testing) provide a
testing backend and `HttpTestingController` for asserting requests and supplying
responses. See that guide for the setup and request-matching examples.
