# Foundations

- "Calling" or "subscribing" is an isolated operation: two function calls trigger two separate side effects, and two Observable subscribes trigger two separate side effects. As opposed to EventEmitters which share the side effects and have eager execution regardless of the existence of subscribers, Observables have no shared execution and are lazy.
- Observables are lazy Push collections of multiple values.
- Subscribing to an Observable is analogous to calling a Function.
- the subscription of observables was entirely synchronous, just like a function
- Observables are able to deliver values either synchronously or asynchronously
- Observables can "return" multiple values over time, something which functions cannot.
- Observables can be created with new Observable. Most commonly, observables are created using creation functions, like of, from, interval, etc.
- When you subscribe, you get back a Subscription, which represents the ongoing execution. Just call unsubscribe() to cancel the execution.
- RxJS is mostly useful for its operators, even though the Observable is the foundation.
- Observers are simply a set of callbacks, one for each type of notification delivered by the Observable: next, error, and complete.
- A Pipeable Operator is essentially a pure function which takes one Observable as input and generates another Observable as output. Subscribing to the output Observable will also subscribe to the input Observable.
- Observables most commonly emit ordinary values like strings and numbers, but surprisingly often, it is necessary to handle Observables of Observables, so-called higher-order Observables
- how do you work with a higher-order Observable? Typically, by flattening: by (somehow) converting a higher-order Observable into an ordinary Observable.

  ```typescript
  const fileObservable = urlObservable.pipe(
    map((url) => http.get(url)),
    concatAll()
  );
  ```

- An RxJS Subject is a special type of Observable that allows values to be multicasted to many Observers. While plain Observables are unicast (each subscribed Observer owns an independent execution of the Observable), Subjects are multicast.
- A Subject is like an Observable, but can multicast to many Observers. Subjects are like EventEmitters: they maintain a registry of many listeners.
- In RxJS, "single cast" and "multicast" refer to two different ways of handling the emission of values from an Observable to its subscribers.
- Single cast: When an Observable is single-cast, it means that each subscription to that Observable triggers a separate execution of its underlying logic. In other words, each subscriber gets its own independent stream of values.
- Multicast: When an Observable is multicast, it means that it shares a single execution of its underlying logic among multiple subscribers. This is achieved using a special type of Observable called a "Subject" or a "Subject-like" entity.
- refCount makes the multicasted Observable automatically start executing when the first subscriber arrives, and stop executing when the last subscriber leaves.
- scheduler: In RxJS, a scheduler is an abstraction that provides a way to control the execution and timing of Observable streams. It allows you to specify when and how the values emitted by an Observable are delivered to the subscribers.
