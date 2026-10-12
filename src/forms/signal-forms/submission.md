# Submit a Signal Form

A Signal Form keeps the input model current; your component or application service is still responsible for deciding when and how to send that value. A native form `submit` handler is one straightforward option.

## Validate before saving

Use a submit button and prevent the browser's default navigation. Check the form's validity before passing the model value to the code that performs the save:

```typescript
onSubmit(event: SubmitEvent): void {
  event.preventDefault();

  if (this.profileForm().invalid()) {
    return;
  }

  const profile = this.profile();
  this.profileService.save(profile);
}
```

In this example, `profileForm` and `profile` are the form and model signals from [Create your first Signal Form](first-form.md); `profileService` represents the application's own persistence service. A form can also be submitted through another interaction, such as a wizard's Next button, after applying the same validity check.

## Handle an asynchronous save

If saving returns a promise, track the request state so users cannot submit the same data repeatedly and can see failures:

```typescript
import {signal} from '@angular/core';

// Component class fields
readonly saving = signal(false);
readonly saveError = signal<string | null>(null);

async onSubmit(event: SubmitEvent): Promise<void> {
  event.preventDefault();

  if (this.profileForm().invalid() || this.saving()) {
    return;
  }

  this.saving.set(true);
  this.saveError.set(null);

  try {
    await this.profileService.save(this.profile());
  } catch {
    this.saveError.set('The profile could not be saved. Please try again.');
  } finally {
    this.saving.set(false);
  }
}
```

Use that state in the template:

```html
<button type="submit" [disabled]="profileForm().invalid() || saving()">
  @if (saving()) {
    Saving...
  } @else {
    Save profile
  }
</button>

@if (saveError()) {
  <p role="alert">{{ saveError() }}</p>
}
```

The server remains the authority on whether data can be saved. If the request returns field-specific errors, map them into user-visible feedback using the validation and error-handling APIs available in your Angular version. See [Signal Form validation](validation.md).
