# Module Federation

## `withModuleFederation` function

`withModuleFederation` is used in the webpack config. This function is an abstraction on top of webpack's ModuleFederationPlugin with some Nx-specific behavior.

- All libraries (npm and workspace) are shared singletons by default, so you don't manually configure them.
- Remotes are referenced by name only, since Nx knows which ports each remote is running on (in development mode).
