# Module Federation

## `withModuleFederation` function

`withModuleFederation` is used in the webpack config. This function is an abstraction on top of webpack's ModuleFederationPlugin with some Nx-specific behavior.

- All libraries (npm and workspace) are shared singletons by default, so you don't manually configure them.
- Remotes are referenced by name only, since Nx knows which ports each remote is running on (in development mode).

## `nx serve`

When a developer runs say `nx serve host --devRemotes=cart`, they still run the whole application, but shop and about are served statically, from cache. As a result, the serve time and the time it takes to see the changes on the screen go down, often by an order of magnitude.

## MFE

Nx provides linting rules. Once in place, they give us errors when we directly reference code belonging to another Micro Frontend.
