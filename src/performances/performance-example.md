# Live Task List Example

This standalone component uses a computed signal to filter the list and a stable `id` key to keep each rendered row associated with its task.

```ts,angular
import { ChangeDetectionStrategy, Component, computed, signal } from '@angular/core';

@Component({
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <section>
      <h2>Tasks</h2>
      <p>Filter the list to see stable, keyed rows update in place.</p>
      <nav aria-label="Filter tasks">
        @for (category of categories; track category) {
          <button
            type="button"
            (click)="selectedCategory.set(category)"
            [attr.aria-pressed]="selectedCategory() === category">
            {{ category }}
          </button>
        }
      </nav>
      <ul>
        @for (task of visibleTasks(); track task.id) {
          <li>{{ task.title }} — {{ task.status }}</li>
        } @empty {
          <li>No tasks in this category.</li>
        }
      </ul>
    </section>
  `,
  styles: `
    section { font-family: sans-serif; }
    nav { display: flex; gap: 0.5rem; margin-block: 1rem; }
    button[aria-pressed="true"] { font-weight: bold; }
  `,
})
export class TaskListComponent {
  categories = ['all', 'open', 'done'];
  selectedCategory = signal('all');
  tasks = [
    { id: 1, title: 'Measure the initial bundle', status: 'open' },
    { id: 2, title: 'Ship the image optimization', status: 'done' },
    { id: 3, title: 'Profile the task list', status: 'open' },
  ];

  visibleTasks = computed(() => {
    const category = this.selectedCategory();
    return category === 'all'
      ? this.tasks
      : this.tasks.filter((task) => task.status === category);
  });
}
```
