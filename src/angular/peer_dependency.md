# Peer Dependency

Peer dependencies is the dependencies of an npm package. Peer dependencies are provided by the consuming app.

A library is more like a pure function that gives you an output with a set input. A library is not instantiated, nor can it stand on its own, this is also due to that library bundles do not include any of its dependencies. Instances of the dependencies will need to be provided by the consuming apps.

With peerDependencies you are required to download those packages yourself (the user consuming your library needs to download those packages, it doesn't come bundled with your library).

Npm will not attempt to install peer dependencies when doing `npm install`.

## Peer Dependency vs Dependency

In a library `package.json`, if a package is specified as a dependency instead of as a peer dependency, in the consuming app, npm will attempt to install it. This could easily end up with several verisons.

For example, a lib has a dependency of `rxjs ^6.0.0`, and consuming app has a dependency of `rxjs ^7.0.0`. Both versions will be installed, with v7 listed at the root of the `node_module`, and v6 listed inside of the `node_module` of the lib.

If however, the `rxjs ^6.0.0` is listed as a peer dependency of the lib. Npm will throw an peer dependency error while running `npm install`, because the consuming app provides v7, not v6.

## Recommendation from `ng-packagr`

Use `peerDependencies` whenever possible. As a rule of thumb, consider that your library's dependencies are declared as peerDependencies. In most cases, this is the recommended solution for library dependencies.

Why? Publishing an npm packages with a dependencies section in package.json easily leads to installing multiple versions of a dependency to an application's node_modules folder. While this is a desirable solution on server-side or standalone programs, it's a source for bugs on front-end build stacks and UI technologies – you don't want to install two different versions of Angular or RxJS.

## Recommendation from Angular official doc

Angular libraries should list any `@angular/*` dependencies the library depends on as peer dependencies. This ensures that when modules ask for Angular, they all get the exact same module. If a library lists `@angular/core` in dependencies instead of peerDependencies, it might get a different Angular module instead, which would cause your application to break.

## Two `package.json` files

When you create a library inside an Angular workspace. There will be two `package.json` files, one for the workspace root, one for the library itself.

- `my-workspace/package.json`
- `my-workspace/projects/my-lib/package.json`

When you specify dependencies for your library, you should specify peer dependencies in the library `package.json`.

When developping a library, you should install all peer dependencies of the library, inside the workspace `package.json`. Otherwise the library would not compile.

## A note on dev dependency

You should not refer to any dev dependency inside your source code without allowing for your code to safely and gracefully fallback to alternatives since the package will be removed on pruning.
