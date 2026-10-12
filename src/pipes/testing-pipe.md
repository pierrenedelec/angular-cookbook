# Testing Pipes

A pipe's `transform` method is usually a pure function, so test it directly without
rendering a component or creating Angular's testing module. This keeps the test
small and makes edge cases easy to cover.

For the [`TruncatePipe`](./custom-pipe.md):

```ts
import { TruncatePipe } from './truncate.pipe';

describe('TruncatePipe', () => {
  const pipe = new TruncatePipe();

  it('leaves short values unchanged', () => {
    expect(pipe.transform('Angular', 10)).toBe('Angular');
  });

  it('truncates long values and appends the suffix', () => {
    expect(pipe.transform('Angular Cookbook', 7, '...')).toBe('Angular...');
  });

  it('returns an empty string for a missing value', () => {
    expect(pipe.transform(null)).toBe('');
  });
});
```

Test the behavior that matters to the application: ordinary inputs, parameter
variations, empty or missing inputs, and boundary values. If the pipe depends on an
injected service or other state, use Angular's testing utilities to provide that
dependency rather than hiding it in the test.

## Test the template separately

An isolated test proves the transformation works; it does not prove a component
imports or uses the pipe correctly. Add a component test when that wiring or the
resulting DOM is important. Render a representative input, query the displayed
element, and assert on its text. Keep this as a separate test from the
transformation's edge-case tests.

Angular's [pipe testing guide](https://angular.dev/guide/testing/pipes) provides a
walkthrough of both isolated pipe tests and component-level tests.