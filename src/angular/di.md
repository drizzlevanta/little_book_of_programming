# Dependency Injection

## Providers

A provider is an instruction to Angular's dependency injection system on how to create or obtain a dependency (typically a service or value) that a class or component needs. Essentially, a provider tells Angular:

- What to provide (the service, token, or value).
- How to provide it (how to create or retrieve the dependency).

Providers in Angular form a hierarchical structure. When a component requests a dependency, Angular starts at the closest injector (typically the component itself) and works its way up through the hierarchy to find the right provider.

## Injection Token

Previously, Angular only supported constructor-based dependency injection. However, things have changed with the inject function and we can now inject tokens and services outside of class constructors.

### Injection Token vs Constants

Injection Token: Used to provide values or services that don’t have a class type.

- Dynamic injection: Injection tokens are tied to Angular’s DI system, which means they can be injected into components, services, or other classes at runtime. This allows you to dynamically control or override the injected value depending on the context (e.g., by module, environment, testing).
- Scoped to modules: With injection tokens, you can define values that can vary per module or per lazy-loaded part of the application. Different modules can provide different values for the same token.
- Single value across the app: Constants are not tied to Angular’s DI scope, so their value is global and can’t be dynamically changed based on different modules or contexts.
- Static and global: Constants are static values and don’t involve Angular’s DI system. Once defined, their value remains fixed throughout the app. They cannot be easily modified or replaced at runtime.

## `forRoot()`

The forRoot() pattern helps to avoid a common problem in Angular: multiple instances of the same service being created across different modules. Here’s why it’s needed:

When a module is imported in multiple places (especially in lazy-loaded modules), Angular creates a new instance of the services declared in the module's providers array.
forRoot() allows you to centralize the service provisioning in the root module and only import the non-service parts (like components or directives) in feature modules, without duplicating the service.

## Using Factory Methods for Injection Token

- Dynamic Value Creation: The factory method allows the injected value to be created dynamically, based on runtime conditions.
- Lazy Initialization: The value is **only created when it's first injected, not at the module's startup**.
- Dependency Injection: The factory method itself can have dependencies, allowing for complex initialization logic that relies on other services or configuration values.
- Conditional Logic: You can introduce conditional logic (such as different environments, user roles, or feature flags) to control what value is provided by the token.

Example:

```typescript
export interface AppConfig {
  apiEndpoint: string;
  timeout: number;
}

// Define the factory method
export function configFactory(isProduction: boolean): AppConfig {
  return {
    apiEndpoint: isProduction ? "https://api.prod.com" : "https://api.dev.com",
    timeout: 5000,
  };
}

//Injection token
@NgModule({
  providers: [
    {
      provide: APP_CONFIG,
      useFactory: configFactory,
      deps: ["IS_PRODUCTION"], // Inject IS_PRODUCTION into the factory
    },
    { provide: "IS_PRODUCTION", useValue: true }, // Provide a value for IS_PRODUCTION
  ],
})
export class AppModule {}

//Use of Injection token
import { Inject, Component } from "@angular/core";

@Component({
  selector: "app-root",
  template: "<h1>{{ config.apiEndpoint }}</h1>",
})
export class AppComponent {
  constructor(@Inject(APP_CONFIG) public config: AppConfig) {
    console.log(this.config.apiEndpoint); // Outputs 'https://api.prod.com' if IS_PRODUCTION is true
  }
}
```

## Keep Providers from Ending Up in the Wrong Injector

For example, if you use `provideSvgIconsConfig` in a component or a directive injector, you'll get a compile error.

```typescript
import { makeEnvironmentProviders } from "@angular/core";

export function provideSvgIconsConfig(config: Partial<SVG_CONFIG>) {
  return makeEnvironmentProviders([
    {
      provide: SVG_ICONS_CONFIG,
      useValue: config,
    },
  ]);
}
```
