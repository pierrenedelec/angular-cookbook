# Signal Forms

Signal Forms provide a signal-based way to model and validate form data. A form is built from a writable signal that holds the data, and a schema that describes rules for its fields. Angular keeps the form fields and model in sync.

Signal Forms are available from `@angular/forms/signals` in Angular 21 and later:

```typescript
import {FormField, email, form, required} from '@angular/forms/signals';
```

Import `FormField` into the component that binds the form to native inputs or custom controls. The form API is part of `@angular/forms`; no separate package is needed.

## How the pieces fit together

1. **Model** — A writable signal stores the application data.
2. **Form tree** — `form(model, schema)` creates a typed tree of fields and their state.
3. **Schema** — The callback passed to `form` associates validation rules with model paths.
4. **Controls** — Bind an input to a field with `[formField]`. Angular synchronizes changes with the model.
5. **Field state** — Call signals such as `invalid()`, `touched()`, and `errors()` to render feedback.

```typescript
readonly profile = signal({name: '', email: ''});

readonly profileForm = form(this.profile, (path) => {
  required(path.name);
  required(path.email);
  email(path.email);
});
```

Continue with [creating a first Signal Form](first-form.md), [validation](validation.md), and [submission](submission.md).

## When to use Signal Forms

Signal Forms are a good fit when building a new form around a signal-backed data model, especially when you want field-level state and validation rules expressed in one schema. Existing template-driven and reactive forms remain supported; there is no need to migrate a working form just to use signals elsewhere in an application.

Signal Forms are a newer API. Check the [official Signal Forms guide](https://angular.dev/guide/forms/signals) for APIs and examples that match the Angular version used by your application.
