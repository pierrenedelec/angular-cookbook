# Create your first Signal Form

This example creates a small profile form. The model is a writable signal, `form()` derives a typed field tree from it, and `FormField` binds each input to a field.

## Component

```typescript
import {Component, signal} from '@angular/core';
import {FormField, email, form, required} from '@angular/forms/signals';

@Component({
  selector: 'app-profile-form',
  imports: [FormField],
  template: `
    <form (submit)="onSubmit($event)">
      <label for="name">Name</label>
      <input id="name" name="name" [formField]="profileForm.name" />

      <label for="email">Email</label>
      <input id="email" name="email" type="email" [formField]="profileForm.email" />

      <button type="submit" [disabled]="profileForm().invalid()">Save profile</button>
    </form>
  `,
})
export class ProfileFormComponent {
  readonly profile = signal({
    name: '',
    email: '',
  });

  readonly profileForm = form(this.profile, (path) => {
    required(path.name);
    required(path.email);
    email(path.email);
  });

  onSubmit(event: SubmitEvent): void {
    event.preventDefault();

    if (this.profileForm().invalid()) {
      return;
    }

    // The signal contains the latest values entered in the form.
    console.log(this.profile());
  }
}
```

`FormField` is a directive, so it must be included in the component's `imports`. The schema paths are inferred from the model; misspelled or nonexistent paths are caught by TypeScript.

## Read and update the model

Read the complete value by calling the model signal:

```typescript
const profileData = this.profile();
```

You can also update the model from application code with the signal's `set()` or `update()` methods. Bound fields reflect the updated value:

```typescript
this.profile.update((current) => ({
  ...current,
  name: 'Ada Lovelace',
}));
```

For validation feedback and custom rules, see [Validate a Signal Form](validation.md). For handling a real save operation, see [Submit a Signal Form](submission.md).
