# Webpack

Webpack is a JavaScript module bundler. It is used in web development to bundle and package assets. Assets such as JS files, CSS files, images and such. It bundles these assets into a single optmized bundle that can be served in the web browser.

The main purpose of Webpack is to take a dependency graph of modules (files) and generate a single bundle that contains all the necessary code for the application to run. This process includes resolving dependencies between modules, handling different types of assets, and applying various optimizations to reduce the size and improve the performance of the final bundle.

It simplifies the management of dependencies and improves the performance of web applications by reducing file sizes and enabling advanced features like code splitting and lazy loading.

## Features

- Entry Point: Webpack starts bundling from one or more entry points, which are the entry files of your application. It analyzes these entry points and builds a dependency graph by following import statements.
- It's easier to handle and faster to serve because everything is neatly packed together.
- Webpack is smart too! It knows when to break the big cookie into smaller ones (code splitting) so that you can serve only what your visitors need at a particular moment. This makes your website faster and saves bandwidth.
- Code Splitting: Webpack allows you to split your code into multiple chunks, which can be loaded asynchronously, improving the initial loading time of your application.

## Reduce bundle size

Ways to reduce bundle size:

- Use Angular CLI Builders: The Angular CLI has built-in builders that can help optimize your bundles, such as the @angular-devkit/build-optimizer. This can further reduce the size of your code.
- Lazy Load Third-Party Libraries: If you're using third-party libraries, load them lazily only when needed, rather than including them in the main bundle.
- Optimize CSS: Minify and optimize your CSS. You can use tools like PostCSS to remove unused styles and reduce the size of your stylesheets.
- Code splitting: lazy-load modules. This means that a module and its dependencies will only be loaded when the user navigates to a route that uses that module.
- Tree shaking: use a bundler like Webpack that supports tree shaking.
- Avoid using wildcard imports: Wildcard imports, such as import \* as myModule from './my-module', can prevent tree shaking from working effectively. Instead of using wildcard imports, consider using named imports, such as import { myFunction } from './my-module'.
- Use tools like webpack-bundle-analyzer to analyze your bundle and identify large chunks that could be optimized.
