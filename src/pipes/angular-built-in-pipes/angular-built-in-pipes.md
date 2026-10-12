# Angular Built-in Pipes

Angular's built-in pipes are exported by `@angular/common`. In a standalone
component, import the pipes used by its template.

| Pipe name | What it does | Example |
| --- | --- | --- |
| `date` | Formats a date using locale rules | `{{ birthday \| date:'mediumDate' }}` |
| `currency` | Formats a number as currency | `{{ total \| currency:'EUR' }}` |
| `number` | Formats a number with locale-specific separators and precision | `{{ score \| number:'1.0-2' }}` |
| `percent` | Formats a ratio as a percentage | `{{ completion \| percent:'1.0-0' }}` |
| `uppercase`, `lowercase`, `titlecase` | Changes the letter case of text | `{{ name \| titlecase }}` |
| `json` | Serializes a value as JSON, useful for debugging | `{{ model \| json }}` |
| `slice` | Returns a portion of a string or array | `{{ items \| slice:0:3 }}` |
| `keyvalue` | Converts object or `Map` entries into key/value pairs | `@for (entry of settings \| keyvalue; track entry.key) { ... }` |
| `i18nSelect`, `i18nPlural` | Selects text for message variants | `{{ count \| i18nPlural:messages }}` |
| `async` | Emits the latest value from an `Observable` or `Promise` | `{{ profile$ \| async \| json }}` |

Text and number formatting pipes are pure. The `async` pipe is impure because it
must react to emissions even when the source reference stays the same. Read
[pure and impure pipes](../pure-and-impure-pipes.md) before writing an impure pipe
of your own.

## Locale-aware formatting

`DatePipe`, `CurrencyPipe`, `DecimalPipe` (the `number` pipe), and `PercentPipe`
format according to locale rules. Angular includes `en-US` locale data by default.
To use another locale, register its data and configure `LOCALE_ID`; merely passing
a different locale name to a pipe does not register that locale's data.

For example, the final argument can select French formatting after its locale data
has been registered:

```html
{{ total | currency:'EUR':'symbol':'1.2-2':'fr' }}
```

See [Format data based on locale](https://angular.dev/guide/i18n/format-data-locale)
for setup details.

## Choose the right pipe

- Use `date`, `currency`, `number`, and `percent` for localized display rather than
  manually assembling separators or symbols.
- Use `async` to display asynchronous values and clean up the subscription
  automatically.
- Use `json` for inspecting values while developing, not as a production data
  serialization strategy.
- Import only the pipes that a standalone component uses.

See the Angular [template pipes guide](https://angular.dev/guide/templates/pipes)
and the [`@angular/common` API](https://angular.dev/api/common) for current
signatures and options.