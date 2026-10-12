
# Percent Pipe

```admonish info
`PercentPipe` formats a ratio as a percentage string using locale-specific
separators and decimal digits. Pass the ratio, not a value that has already been
multiplied by 100.
```

For example, `0.259` becomes `26%` with the default formatting:

```html
<p>Completed: {{ completion | percent }}</p>
```

```ts
import { PercentPipe } from '@angular/common';
import { Component } from '@angular/core';

@Component({
  selector: 'app-progress',
  imports: [PercentPipe],
  template: `<p>Completed: {{ completion | percent:'1.0-1' }}</p>`,
})
export class ProgressComponent {
  completion = 0.259;
}
```

The example renders `Completed: 25.9%`. The optional `digitsInfo` argument uses
the format `minIntegerDigits.minFractionDigits-maxFractionDigits`:

- `'1.0-0'` rounds to a whole percentage, such as `26%`.
- `'1.0-1'` shows up to one fractional digit, such as `25.9%`.
- `'1.2-2'` always shows two fractional digits, such as `25.90%`.

The optional final argument is a locale. If omitted, the pipe uses the configured
`LOCALE_ID` (which defaults to `en-US`). Angular ships only `en-US` locale data;
register additional locale data before formatting for other locales.

`PercentPipe` is pure. If its input is an object-backed value, replace the
reference when the value changes rather than mutating it in place.

See the [`PercentPipe` API](https://angular.dev/api/common/PercentPipe) and
[locale formatting guide](https://angular.dev/guide/i18n/format-data-locale).
