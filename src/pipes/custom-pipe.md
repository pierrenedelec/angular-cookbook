# Custom Pipe

Create a custom pipe when a transformation belongs in a template and no built-in
pipe already provides it. A pipe has a template name and a `transform` method.
Implementing `PipeTransform` makes the method signature explicit.

This example shortens long text and accepts an optional limit and suffix:

```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'truncate',
})
export class TruncatePipe implements PipeTransform {
  transform(
    value: string | null | undefined,
    limit = 30,
    suffix = '…',
  ): string {
    if (value == null) {
      return '';
    }

    const safeLimit = Math.max(0, limit);
    return value.length > safeLimit
      ? `${value.slice(0, safeLimit)}${suffix}`
      : value;
  }
}
```

The pipe is standalone by default in current Angular versions. Import it into the
standalone component that uses it:

```ts
import { Component } from '@angular/core';
import { TruncatePipe } from './truncate.pipe';

@Component({
  selector: 'app-description',
  imports: [TruncatePipe],
  template: `<p>{{ description | truncate:40:'...' }}</p>`,
})
export class DescriptionComponent {
  description = 'A longer description that does not fit in the summary.';
}
```

The pipe name is written in lower camel case and has no hyphens; the class name is
UpperCamelCase with the `Pipe` suffix. The pipe receives its input first, followed
by the arguments supplied after colons.

## Keep the transformation focused

Pipes should be predictable and focused on presentation. Keep I/O, component state,
and unrelated business rules out of `transform`. If the same transformation is
needed outside templates, put its core logic in an ordinary TypeScript function
and call that function from the pipe and from other code.

Pipes are pure by default. Keep that default whenever possible; see
[Pure and Impure Pipes](./pure-and-impure-pipes.md).

For the full API and naming recommendations, see Angular's
[Pipes guide](https://angular.dev/guide/templates/pipes) and
[style guide](https://angular.dev/style-guide#style-02-09).
