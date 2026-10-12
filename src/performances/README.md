# Performance Optimization

Performance work starts with measurement, not guesswork. A fast application loads quickly, responds smoothly to interaction, and avoids unnecessary work as its data grows. Optimize the bottleneck users can observe, then measure again.

## Find the bottleneck first

Reproduce a real user journey with representative data and a production build. Use the browser's Network and Performance panels to inspect resource loading, long tasks, layout, and rendering. Use [Angular DevTools](https://angular.dev/tools/devtools) to profile change-detection cycles and identify components with slow templates or lifecycle hooks.

Track user-visible metrics such as Largest Contentful Paint (LCP), Interaction to Next Paint (INP), and Cumulative Layout Shift (CLS). Compare measurements on the same device and under the same network and CPU conditions; development builds and tiny sample datasets can hide production bottlenecks.

## Improve loading performance

- Keep the initial route focused. Lazy-load routes and standalone features that users do not need immediately; see [lazy loading standalone components](../lazy-loading/lazy_loading_standalone_component.md).
- Defer non-critical sections with Angular's [`@defer`](https://angular.dev/guide/templates/defer) blocks. Put optional, expensive UI behind a trigger such as viewport visibility or user interaction, and provide a useful placeholder.
- Review the production bundle and its dependencies. Remove unused code and avoid importing a large library for a small feature.
- Size images for their rendered dimensions, use responsive sources where appropriate, and include width and height to prevent layout shifts. Lazy-load below-the-fold images, but do not delay the page's main above-the-fold image.

## Reduce unnecessary runtime work

Angular evaluates template expressions during change detection. Keep expressions cheap: avoid calling methods that perform filtering, sorting, or other repeated work from a template. For derived state, a [`computed`](https://angular.dev/api/core/computed) signal memoizes its result until one of its dependencies changes.

For large component trees, consider explicitly setting `ChangeDetectionStrategy.OnPush` on components whose inputs and state are updated through Angular's supported bindings and reactive APIs. Signals read in an `OnPush` template mark that component for refresh when they change. Keep input data immutable when relying on reference changes, and profile before and after: `OnPush` does not make expensive template expressions cheap.

Use stable keys when rendering collections. A stable key lets Angular associate each item with its existing DOM node instead of recreating rows when the collection changes. See the [live task-list example](performance-example.md) for a signal-based filter that tracks each row by its stable `id`.

For very large collections, first check whether pagination, server-side filtering, or virtual scrolling is more appropriate than rendering every row. If a template still has a slow calculation after profiling, improve its algorithm; memoization or a pure pipe can help when their caching behavior fits the data.

## Re-measure after each change

Change one thing at a time and repeat the same user journey. Keep the optimization only if it improves the relevant metric without making the interface harder to use. Performance is a balance: defer optional work, but keep essential content and feedback immediate.
