# Pipes

Pipes transform values for display in Angular templates. They keep formatting close
to the view without changing the underlying component data:

```html
<p>{{ total | currency:'EUR' }}</p>
```

Angular ships pipes for common formatting tasks, including dates, numbers, text,
and asynchronous values. You can also create a custom pipe when a transformation
is specific to your application.

## In this chapter

- [Using pipes in templates](./using-pipes.md) covers pipe expressions, arguments,
  chaining, and standalone imports.
- [Angular built-in pipes](./angular-built-in-pipes/angular-built-in-pipes.md)
  surveys the pipes available from `@angular/common`.
- [The percent pipe](./angular-built-in-pipes/percent-pipe.md) formats ratios as
  localized percentages.
- [The async pipe](./async-pipe.md) unwraps `Observable` and `Promise` values and
  manages subscriptions.
- [Pure and impure pipes](./pure-and-impure-pipes.md) explains when Angular
  reevaluates a pipe and why purity matters for performance.
- [Custom pipes](./custom-pipe.md) shows how to implement a reusable transformation.
- [Testing pipes](./testing-pipe.md) covers unit tests for the transformation and
  its use in a template.

## Quick rules

```admonish note
Prefer a built-in pipe when it already provides the formatting you need.
Pipes are pure by default; keep them pure unless there is a compelling reason not to.
In standalone components, import each pipe used by the template.
```

Angular recommends naming the pipe class in `UpperCamelCase` and its template name
in `lowerCamelCase`. The template name cannot contain hyphens. See the
[Angular style guide](https://angular.dev/style-guide#style-02-09).

For the official guide, see [Pipes](https://angular.dev/guide/templates/pipes).
