# Little Book of Web Programming

Notes and best practices of various web technologies, design patterns, and architecture.

Built with [mdBook](https://rust-lang.github.io/mdBook/).

## Topics

- **Architecture** — Micro Services & Micro Frontend
- **Angular** — Signals, Dependency Injection, SSR, Content Projection, and more
- **RxJS** — Foundations, Operators, Pure Functions
- **NgRx** — Actions, Reducers, Selectors, Effects, Entity, Signals
- **Nx** — Module Federation
- **TypeScript** — Generics, Closures, Immutability, Operators
- **Web** — Event Loop, Memory Leaks, Webpack, Security, CORS, Tree Shaking
- **API Design** — GraphQL
- **OOP** — State Pattern, OOP Features

## Prerequisites

- [Rust](https://www.rust-lang.org/tools/install)
- [mdBook](https://github.com/rust-lang/mdBook)

  ```sh
  cargo install mdbook
  ```

## Usage

Serve locally with live reload:

```sh
mdbook serve --open
```

Build the static site:

```sh
mdbook build
```

The output is generated in the `book/` directory.
