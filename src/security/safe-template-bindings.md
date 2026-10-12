# Safely Render Untrusted Values

Use Angular template bindings to render untrusted values. Interpolation displays markup as text, while an `[innerHTML]` binding renders allowed HTML after Angular sanitizes it for the HTML context.

```ts,angular
import {Component} from '@angular/core';

@Component({
  standalone: true,
  template: `
    <h3>Interpolated as text</h3>
    <p>{{ untrustedHtml }}</p>

    <h3>Bound as HTML</h3>
    <div [innerHTML]="untrustedHtml"></div>
  `,
})
export class SafeRenderingExampleComponent {
  readonly untrustedHtml =
    '<strong>Safe formatting remains</strong><img src="x" onerror="alert(1)">';
}
```

The interpolated value appears as text. In the HTML binding, Angular preserves the safe formatting and removes the unsafe event handler. This example demonstrates Angular's protection for a template binding; it does not make arbitrary HTML safe for other contexts or direct DOM APIs.
