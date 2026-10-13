# Using Pipes in Templates

Angular pipes are template operators that transform a value for display. The value
before the pipe operator (`|`) is the input; the pipe name and optional arguments
follow it.

```html
<p>{{ birthday | date }}</p>
<p>{{ price | currency:'EUR' }}</p>
```

The `date` and `currency` pipes format their inputs without changing `birthday` or
`price` in the component.

## Import the pipe

Pipes from `@angular/common` are standalone and can be imported directly by a
standalone component:

```ts
import { CurrencyPipe, DatePipe } from '@angular/common';
import { Component } from '@angular/core';

@Component({
  selector: 'app-receipt',
  imports: [CurrencyPipe, DatePipe],
  template: `
    <p>Purchased {{ purchasedOn | date:'mediumDate' }}</p>
    <p>Total: {{ total | currency:'EUR' }}</p>
  `,
})
export class ReceiptComponent {
  purchasedOn = new Date(2026, 0, 15);
  total = 42.5;
}
```

For an NgModule-based application, import the module or declare/import the pipe
according to the Angular version and whether the pipe is standalone. New Angular
components are standalone by default.

## Pass arguments

Use a colon to pass an argument. Separate multiple arguments with additional
colons; their order follows the pipe's `transform` method signature.

```ts,angular
{{ 3*2 }}
```

```html
{{ birthday | date:'yyyy-MM-dd':'UTC' }}
{{ amount | number:'1.2-2' }}
{{ amount | currency:'USD':'symbol':'1.2-2' }}
```

Here, `date` receives a format and time zone. `number` and `currency` use
`digitsInfo`, written as `minIntegerDigits.minFractionDigits-maxFractionDigits`.
For example, `'1.2-2'` displays at least two and at most two fraction digits.

An argument can be an expression as well as a literal:

```html
{{ amount | currency:currencyCode }}
```

## Chain pipes

Pipe expressions can be chained. Angular applies them from left to right, passing
each result to the next pipe:

```html
{{ eventDate | date:'longDate' | uppercase }}
```

Use parentheses when mixing pipes with conditional expressions so the intended
value is unambiguous:

```html
{{ (isReady ? 'Ready' : 'Loading') | uppercase }}
```

The pipe operator has lower precedence than arithmetic, logical, and nullish
coalescing operators, and higher precedence than the conditional operator.

## Keep formatting in the view

Pipes are best for values presented in templates. Do not use a formatted string as
the canonical value for calculations or API requests; keep the original typed value
in the component and format it only where it is displayed. For formatting outside
templates, use a regular function or an appropriate formatting API.

See the official [Pipes guide](https://angular.dev/guide/templates/pipes) for more
details.
