# Signals
- A signal is a wrapper around a value that can notify interested consumers when that value changes. Signals can contain any value, from simple primitives to complex data structures.
- Primary usage scenario of Signals: binding values reactively to the view. 
- A signal's value is always read through a getter function, which allows Angular to track where the signal is used.
- Signals may be either writable or read-only. Computed signals are not writable.
- when working with signals that contain objects, sometime it's useful to mutate object directly?
- computed signals are both **lazily evaluated and memorized**. In the example below: doubleCount's derivation function does not run to calculate its value until the first time doubleCount is read. Once calculated, this value is cached, and future reads of doubleCount will return the cached value without recalculating. When count changes, it tells doubleCount that its cached value is no longer valid, and the value is only recalculated on the next read of doubleCount.

  ```typescript
  const count: WritableSignal<number> = signal(0);
  const doubleCount: Signal<number> = computed(() => count() * 2);
  ```
- Therefore, it is safe to perform computationally expensive derivations in computed signals, such as filtering arrays. 
- Computed signal dependencies are dynamic.
- An effect is an operation that runs whenever one or more signal values change. Effects always run at least once. When an effect runs, it tracks any signal value reads. Whenever any of these signal values change, the effect runs again.
- Effects are rarely needed, but can be a good solution to logging, custom DOM behavior.
- Avoid using effects for propagation of state changes.
- The computed signal will run as many times as it was read. If the involved signals were not modified between the reads, computation will not be recomputed. 
- Will effect() run if a signal is updated but not changed? No, effect() is a consumer, and memoization works here as well.
- When using signals in template, even if a computation itself will return the same value, it will still notify the consumer (in this case — the template), and it will still be recomputed (simply because we can not know the new value in advance). But if all the dependencies of this computed signal will return the same values, it will not notify the template, and, therefore, will not be recomputed.
- Signals are glitch-free. If you change a signal several times in a row within a stack frame, only the last change will be seen by the consumer. This shows that Signals are not intended for modelling events but for data we want to bind to the view. In the cases we want to express events, observables are the way to go. 
- Signals are less suitable for asynchronous tasks and for representing events: Firstly, they do not offer an easy way to deal with overlapping asynchronous requests and the resulting race conditions. In addition, they cannot directly represent error states. Secondly, Signals ignore the resulting intermediate states when value changes occur in direct succession.

## Lazy
Unlike Angular's traditional change detection mechanism, signals do not automatically re-run whenever something in the component changes. They only react when their dependencies explicitly change, and even then, only when the signal's value is accessed.

will only emit the value after the signal stabilizes?
```typescript
const obs$ = toObservable(mySignal);
obs$.subscribe(value => console.log(value));
mySignal.set(1);
mySignal.set(2);
mySignal.set(3);
```

## Change detection
With default change detection, there is no way for Angular to know exactly what has changed on the page, so that is why we cannot make any assumptions about what happened, and we need to check everything!

Because we have no guarantees of what could or could not have changed, we need to scan the whole component tree and all the expressions on every component.

## Mutable Objects with Signals
Pitfall: Signals in Angular rely on reference checks to detect changes. If you use mutable objects within signals, updating the contents of these objects won't trigger a reactivity change unless the reference itself changes.
Solution: Favor immutability when working with signals. Instead of modifying an object in place, create a new object and set it to the signal.
```typescript
const user = signal({ name: 'Alice', age: 30 });

// This won't trigger reactivity because the reference hasn't changed
user().age = 31;

// Correct approach
user.set({ ...user(), age: 31 });

```

## Rules about `computed()`
Do not modify things in computed(). It should compute a new result, that’s it. Do not modify the DOM, do not mutate variables using this, and do not call functions that might do that. Do not push values to Observables — it will cause unintentional reactive context propagation (explained below for effect()). computed() should not have side effects, it should be a **pure function**.

Do not make asynchronous calls in computed(). This function does not allow modification of Signals (and it is amazingly helpful), but it can not track asynchronous code. Moreover, Angular Signals are strictly synchronous, so if you want to use asynchronous code in computed(), you are doing something wrong. So, no setTimeout(), no Promises, no other asynchronous things.

## Rules about `effect()`
The function you provide to effect() should be as small as possible. This way it will be easier to read and spot erroneous behavior.

A best practice is to read signals first, then wrap the rest of the effect into untracked():

Effects needs an injection context to work. The technical reason is that effects use inject to get hold of the current `DestroyRef`. Typicallly setup your effects in the constructor.

Effects are not allowed to write signals.

Effects will execute minimal number of times: if an effect depends on multiple signals and several of them change at once, only one effect execution will be scheduled.

- 
- Will effect() run if a signal is updated but not changed?

two copies of everything...??

## Computed Signals and Effects as a Replacement for Life Cycle Hooks
Life cycle hooks like ngOnInit and ngOnChanges can now be replaced with computed and effect:
```typescript
markDownTitle = computed(() => '# ' + this.label())

constructor() {
  effect(() => {
    console.log('label updated', this.label());
    console.log('markdown', this.markDownTitle());
  });
}
```
using inputs within computed or effect is always safe, as they are only first triggered when the component has been initialized. Example: 
```typescript
@Component([...])
export class OptionComponent implements OnInit, OnChanges {
  label = input.required<string>();

  // safe
  markDownTitle = computed(() => '# ' + this.label())

  constructor() {
    // this would cause an exception,
    // as data hasn't been bound so far
    console.log('label', this.label);

    effect(() => {
        // safe
        console.log('label', this.label);
    })
  }

  ngOnInit() {
    // safe
    console.log('label', this.label);
  }

  ngOnChanges() {
    // safe
    console.log('label', this.label);
  }
}
```

## Transform input
IMPORTANT: Do not use transforms if they change the meaning of the input, or if they are impure. Instead, use computed for transformations with different meaning, or an effect for impure code that should run whenever the input changes.

Benefits of signal inputs over `@Input`:
- Signal inputs are more type safe.
- Signal inputs, when used in templates, will automatically mark OnPush components as dirty.
- Values can be easily derived whenever an input changes using computed.
- Easier and more local monitoring of inputs using effect instead of ngOnChanges or setters.

## Signals are Glitch-free
When writing code like in the previous section, we need to be aware that Signals are glitch-free. That means that if you change a signal several times in a row (within a stack frame), only the last change will be seen by the consumer, e.g. the effect:

```typescript
@Component([...])
export class AboutComponent {

  constructor() {
    const signal1 = signal('A');
    const signal2 = signal('B');

    effect(() => {
      console.log('signal1', signal1());
      console.log('signal2', signal2());
    });

    signal1.set('C');
    signal1.set('D');

    signal1.set('E');

    signal2.set('F');
  }
}
```
In this case, we will only see the values E and F on the console. Indemediate values are skipped.

In cases where we want to express events, Observables are the way to go, as they don't have this glitch-free guarantee by design.

## Signals and Observables
- Every variable (that might change) in your new templates should be a Signal.
- If the role of the variable can be described as conditions, then use Signal; if the description of the variable includes time, you need an observable. Signals has no time axis. 
- If you need some value from an Observable in your computed(), create a Signal using toSignal() (outside of computed()).
- If you need to read a signal in Observables's pipe():
  - If you need to react to changes in that signals in your observable, convert the signal into observable and add it using some join operator
  - If you just need to current value of the signal, read a signal directly in your operators. Observable is not a reactive context, no need to `untracked()`.  
  - However, when discussing reading signals inside the observable in general, there's one case where we should use `untracked()`. For this case to occur, two conditions must be met: 
    - We are in a reactive context before the `subscribe()` call (the template calls our function, or our function is inside `computed()` or `effect()`).
    - The observable emits a value synchronously (for example, if `getData()` decides to return cached data).
- Signals have no “complete” state
- One of the important differences between signals and observables is that signals do not send their new value to their consumers when their value is modified. Instead, they simply notify their consumers that the value has been modified. To fetch the new value of a signal, the consumer should “read” that signal. Derived signals (created with computed()) produce new values by executing their computations. And only the consumer decides when to do this.


## Reactivity Context
The body of the function passed to computed(), effect() or template is called “reactivity context”. Observable is not a reactive context. 
### Reading a signal outside the watching function
```typescript
const title = message.$title();

watcher(() => {
  console.log(title);
});

message.updateTitle('Bar');
```
Here, title is not a signal. It’s the value we’ve read outside of the watching function, so watcher() will not be notified when we update the signal.

### Changing a non-observable reference
```typescript
watcher(() => {
  console.log(message.$title());
});

message = new Message('Bar', 'Martijn', ['Felicia', 'Marcus']);
```
Here, we replace the message, but watcher() uses the reference to another variable, and it will not be notified that we’ve replaced the reference.


## Dependency Tracking
Dependency Tracking in Angular Signals is recursive for synchronous function calls. Producers, consumed asynchronously, will not be registered.

