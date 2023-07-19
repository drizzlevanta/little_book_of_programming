# Best Practices

- Follow the Single Responsibility Principle (SRP): Keep your components focused on a single responsibility. Split complex components into smaller, reusable components to improve code maintainability and reusability.
- Leverage reactive programming using RxJS to handle async operations, manage state, and handle event streams.
- Use modules
- Minimize DOM Manipulation: Directly manipulating the DOM can be costly in terms of performance. Utilize Angular's data binding and structural directives (e.g., ngFor, ngIf) to update the DOM efficiently.
- Optimize Change Detection: Angular's change detection is powerful but can impact performance. Use ChangeDetectionStrategy.OnPush where possible to enable change detection optimizations. This strategy helps reduce the number of checks performed by the Angular change detection mechanism.
- Use simple scalable architecture:
  - core: eager layout, logic, app wide state
  - features: lazy features
  - shared: reusable components, directives, pipes
