# Pure and Impure Pipes

Pipes are pure by default. Angular reevaluates a pure pipe when a primitive input
changes or when the reference of an object input changes. This lets Angular skip
work when the inputs are unchanged.

```ts
// For a pure pipe that receives a Task[]:
tasks.push(newTask); // keeps the same array reference
tasks = [...tasks, newTask]; // creates a new array reference
```

If a pure pipe receives `tasks`, it will not run again after the first mutation,
because the array reference is unchanged. Replacing the array reference makes the
change observable to the pipe.

This behavior also applies to `Date` objects. Mutating a date in place does not
cause `DatePipe` to rerun; assign a new `Date` instance instead.

## Why impure pipes are expensive

An impure pipe declares `pure: false`:

```ts
import { Pipe, PipeTransform } from '@angular/core';

interface Task {
  completed: boolean;
}

@Pipe({
  name: 'visibleTasks',
  pure: false,
})
export class VisibleTasksPipe implements PipeTransform {
  transform(tasks: Task[], completed: boolean): Task[] {
    return tasks.filter((task) => task.completed === completed);
  }
}
```

Angular may call an impure pipe during every change-detection cycle, even if its
inputs appear unchanged. An expensive transformation in a frequently checked
template can therefore slow down the whole view.

Prefer immutable updates and a pure pipe. If data must be updated in place, consider
changing how the state is updated or computing the derived value in a more
appropriate place before making a pipe impure. If an impure pipe is unavoidable,
keep its work small and measure its effect.

`AsyncPipe` is a built-in impure pipe because it must respond to asynchronous
emissions while its source object remains the same. It is purpose-built for that
job; this is not a reason to mark ordinary formatting pipes impure.

Read Angular's [pipes guide](https://angular.dev/guide/templates/pipes) for the
change-detection rules.
