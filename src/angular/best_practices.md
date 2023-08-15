# Best Practices

## General

- Follow the Single Responsibility Principle (SRP): Keep your components focused on a single responsibility. Split complex components into smaller, reusable components to improve code maintainability and reusability.
- Leverage reactive programming using RxJS to handle async operations, manage state, and handle event streams.
- Use modules
- Minimize DOM Manipulation: Directly manipulating the DOM can be costly in terms of performance. Utilize Angular's data binding and structural directives (e.g., ngFor, ngIf) to update the DOM efficiently.
- Optimize Change Detection: Angular's change detection is powerful but can impact performance. Use ChangeDetectionStrategy.OnPush where possible to enable change detection optimizations. This strategy helps reduce the number of checks performed by the Angular change detection mechanism.
- Use simple scalable architecture:
  - core: eager layout, logic, app wide state
  - features: lazy features
  - shared: reusable components, directives, pipes

## Libraries

- Declarations such as components and pipes should be designed as stateless, meaning they don't rely on or alter external variables.
- Use peerDependencies whenever possible. Publishing an npm packages with a dependencies section in package.json easily leads to installing multiple versions of a dependency to an application's node_modules folder. While this is a desirable solution on server-side or standalone programs, it's a source for bugs on front-end build stacks and UI technologies – you don't want to install two different versions of Angular or RxJS.
- Angular libraries should list any @angular/\* dependencies the library depends on as peer dependencies. This ensures that when modules ask for Angular, they all get the exact same module. If a library lists @angular/core in dependencies instead of peerDependencies, it might get a different Angular module instead, which would cause your application to break.
- Peer dependency requirements, unlike those for regular dependencies, should be lenient. You should not lock your peer dependencies down to specific patch versions.

### Lightweight Injection Tokens
