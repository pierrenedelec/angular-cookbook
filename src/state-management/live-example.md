# Live Component State Example

This standalone component keeps its task list local. The `completedCount` computed signal derives a value from that list, and each button updates the writable signal.

```ts,angular
import { Component, computed, signal } from '@angular/core';

@Component({
  selector: 'app-task-counter',
  standalone: true,
  template: `
    <h2>Task counter</h2>
    <p>{{ completedCount() }} of {{ tasks().length }} tasks completed</p>
    <button type="button" (click)="toggleFirstTask()">Toggle first task</button>
    <button type="button" (click)="addTask()">Add task</button>
  `,
})
export class TaskCounterComponent {
  readonly tasks = signal([
    { title: 'Choose the state owner', completed: true },
    { title: 'Keep the example small', completed: false },
  ]);

  readonly completedCount = computed(
    () => this.tasks().filter((task) => task.completed).length,
  );

  private nextTaskNumber = 1;

  toggleFirstTask(): void {
    this.tasks.update((tasks) =>
      tasks.map((task, index) =>
        index === 0 ? { ...task, completed: !task.completed } : task,
      ),
    );
  }

  addTask(): void {
    const number = this.nextTaskNumber++;
    this.tasks.update((tasks) => [
      ...tasks,
      { title: `New task ${number}`, completed: false },
    ]);
  }
}
```
