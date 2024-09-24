# Pipes
## Standalone Pipes
1. Import Directly
When using a standalone pipe in a component or module, you should import it directly into the imports array of the component or module. This is similar to how you handle standalone components.
2. Why Not Providers?
Adding a pipe to the providers array is typically used for services, not for pipes. Pipes are used in templates and don’t require the providers array, since they don’t maintain any internal state or depend on Angular’s dependency injection.
