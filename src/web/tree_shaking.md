# Tree Shaking

Tree shaking is a technique used by modern JavaScript bundlers, like Webpack or Rollup, to eliminate unused code (dead code) from the final bundle. The goal of tree shaking is to optimize the bundle size by removing any code that is not actually used in the application.

The term "tree shaking" comes from the idea of shaking a tree and letting the dead leaves fall off while keeping the healthy ones. In the context of JavaScript bundling, the "tree" refers to the dependency tree of the application, which represents the relationships between different modules and their dependencies.

This process is particularly beneficial for applications that rely on large libraries or frameworks, where not all features or modules are used.

## Tree shaking in Angular

### Register your service

Registering the provider in the @Injectable() metadata also allows Angular to optimize an app by removing the service from the compiled application if it isn't used (tree-shaking).

### Ensure your lib is tree-shakable

Minimize Side Effects: Avoid relying on global variables or causing unintended side effects during module initialization. Side effects can prevent the tree shaking process from removing unused code.

Pure Functions: Write pure functions whenever possible. Pure functions are more likely to be optimized and removed if they are not used.

Avoid Circular Dependencies: Circular dependencies can complicate the tree shaking process and may result in unused code not being properly eliminated. Keep your dependencies well-structured and avoid circular references.

Modular Code Organization: Organize your library code into small, focused modules. Each module should have a clear purpose and expose only the necessary APIs. This allows the tree shaking process to identify and remove unused code more effectively.

#### Export with caution

Adhere to the principle of encapsulation. Export Only What's Needed: Export only the classes, components, services, and functions that are part of your library's public API. Avoid exporting internal implementation details.

When internal details are exported, it can create complex interdependencies between different parts of the library. This complexity makes it harder for the tree shaker to accurately analyze and optimize the code, potentially resulting in unused code being retained.

The tree shaker relies on static analysis to determine which code paths are reachable and which are not. Exporting internal details can introduce dynamic or indirect dependencies that are difficult for the tree shaker to trace accurately.

Reduces Unused Code: When you export only the specific classes, components, services, and functions that are part of your library's public API, the tree shaker can easily determine which parts of your library are being used and which are not. This allows it to strip away the unused code from the final bundle.

Optimizes Dependencies: By exporting only what's needed, you prevent unnecessary dependencies from being included in the bundle. For example, if a component relies on a service, and that service is not used in the consuming application, the tree shaker can exclude both the component and the service from the bundle.

Simplifies Analysis: A smaller set of exports makes it easier for the tree shaker to analyze the relationships between different parts of your library. This streamlined analysis improves the accuracy of identifying and removing unused code.

### `@ViewChild` and `@ContentChild` with caution

Using @ViewChild and @ContentChild decorators in an Angular library can potentially impact tree shaking if they are not used carefully.

Retaining Unused Components: If you use @ViewChild or @ContentChild to reference components, directives, or elements that are not used by the consuming application, the tree shaker may not be able to eliminate them from the bundle.

Complex Template Dependencies: When you use @ViewChild or @ContentChild, it creates dependencies between the parent component and the child component or element being referenced. This can make it more challenging for the tree shaker to accurately determine which parts of the code are actually used.

Indirect Dependencies: The dependencies introduced by @ViewChild or @ContentChild might not be straightforward or immediately obvious. This can lead to indirect dependencies being retained in the bundle, even if the referenced elements or components are not used.

Dynamic Template Changes: @ViewChild and @ContentChild bindings can sometimes involve dynamic template changes, which can make it harder for the tree shaker to perform static analysis and determine if a particular component or element is used.

### Side effects

Side effects (such as direct dom manupulation, network requests) can negatively impact various aspects of application development and maintenance, including tree shaking, bundle size, performance, and behavior consistency.