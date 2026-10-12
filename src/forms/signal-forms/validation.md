# Validate a Signal Form

Signal Forms define validation rules in the schema passed to `form()`. The rules are associated with typed model paths and run as the user changes field values.

## Built-in rules

Import the validation rules you need from `@angular/forms/signals`. For example:

```typescript
import {signal} from '@angular/core';
import {email, form, maxLength, minLength, required} from '@angular/forms/signals';

// Component class fields
readonly registration = signal({
  username: '',
  email: '',
  password: '',
});

readonly registrationForm = form(this.registration, (path) => {
  required(path.username);
  minLength(path.username, 3);
  maxLength(path.username, 30);
  required(path.email);
  email(path.email);
  required(path.password);
  minLength(path.password, 12);
});
```

Use the field state's `invalid()` and `errors()` signals to show validation feedback. `touched()` is useful for avoiding messages before a user has interacted with a field:

```html
@if (registrationForm.email().touched() && registrationForm.email().invalid()) {
  @for (error of registrationForm.email().errors(); track $index) {
    <p role="alert">{{ error.message }}</p>
  }
}
```

The schema owns the rules; the template only reads the resulting state. A form-level check is available through `registrationForm().invalid()`.

## Custom rules

Use `validate()` for a rule that is specific to your application. Return no error when the value is valid, or return an error with a `kind` and human-readable `message`:

```typescript
import {signal} from '@angular/core';
import {form, required, validate} from '@angular/forms/signals';

// Component class fields
readonly account = signal({username: ''});

readonly accountForm = form(this.account, (path) => {
  required(path.username);
  validate(path.username, ({value}) => {
    if (value().includes(' ')) {
      return {
        kind: 'usernameContainsSpaces',
        message: 'Username cannot contain spaces.',
      };
    }

    return null;
  });
});
```

Keep custom rules deterministic and free of side effects. For checks that depend on a server, use an async validation API supported by the Angular version in your project rather than starting requests from a synchronous validator.

## Validation is not a security boundary

Client-side validation improves feedback, but it can be bypassed. Validate submitted data on the server as well, and never rely on a client-side rule to authorize an operation or protect sensitive data.

For an end-to-end form example, see [Create your first Signal Form](first-form.md).
