# Signals

- A signal is a wrapper around a value that can notify interested consumers when that value changes. Signals can contain any value, from simple primitives to complex data structures.
- A signal's value is always read through a getter function, which allows Angular to track where the signal is used.
- Signals may be either writable or read-only. Computed signals are not writable.
- when working with signals that contain objects, sometime it's useful to mutate object directly?
- computed signals are both lazily evaluated and memorized. doubleCount's derivation function does not run to alculate its value until the first time doubleCount is read. Once calculated, this value is cached, and future eads of doubleCount will return the cached value without recalculating. When count changes, it tells doubleCount hat its cached value is no longer valid, and the value is only recalculated on the next read of doubleCount.

  ```typescript
  const count: WritableSignal<number> = signal(0);
  const doubleCount: Signal<number> = computed(() => count() * 2);
  ```

- An effect is an operation that runs whenever one or more signal values change. Effects always run at least once. When an effect runs, it tracks any signal value reads. Whenever any of these signal values change, the effect runs again.
- Effects are rarely needed, but can be a good solution to logging, custom DOM behavior.
- Avoid using effects for propagation of state changes.
