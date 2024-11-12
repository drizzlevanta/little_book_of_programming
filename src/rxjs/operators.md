# Useful Operators

## Useful RxJS Operators:

- scan: With each emitted value, the accumulator function is applied, and the accumulated result is emitted instantaneously. You can remember this by the phrase "accumulate and emit on-the-go."
- reduce: However, be cautious when using scan for cases where the only the final accumulated result is crucial. In those situations, the reduce operator may be more appropriate, as it emits only the final value after the source completes.
- trottleTime: Emit first value then ignore for specified duration
- sampleTime: sampleTime periodically captures the most recent value at regular intervals, regardless of whether there is a new emission or not.
- debounceTime: debounceTime delays the emission of values until a pause of specified time occurs, discarding intermediate values
- exhaustAll subscribes to an Observable that emits Observables, also known as a higher-order Observable. Each time it observes one of these emitted inner Observables, the output Observable begins emitting the items emitted by that inner Observable.
- switchMap: Maps each value to an Observable, then flattens all of these inner Observables using switchAll. The main difference between switchMap and other flattening operators is the cancelling effect. On each emission the previous inner observable (the result of the function you supplied) is cancelled and the new observable is subscribed. You can remember this by the phrase switch to a new observable.
- switchMap is generally considered a safer default to mergeMap. Be careful though, you probably want to avoid switchMap in scenarios where every request needs to complete, think writes to a database. switchMap could cancel a request if the source emits quickly enough. In these scenarios mergeMap is the correct option.
- concatMap Warning: if source values arrive endlessly and faster than their corresponding inner Observables can complete, it will result in memory issues as inner Observables amass in an unbounded buffer waiting for their turn to be subscribed to. Note: concatMap is equivalent to mergeMap with concurrency parameter set to 1.
- mergeAll: Converts a higher-order Observable into a first-order Observable which concurrently delivers all values that are emitted on the inner Observables.
- If you need to merge multiple observables that rely on each other for calculations or decisions, combineLatest may be more suitable.
- If you're working with observables that only emit one value or you only require the last value of each before completion, forkJoin is likely a better choice.
- If you need to merge observables that produce values independently and are short-lived, mergeAll is the operator to reach for.
- One common use case for this is if you wish to issue multiple requests on page load (or some other event) and only want to take action when a response has been received for all. In this way it is similar to how you might use Promise.all.
- iif: Checks a boolean at subscription time, and chooses between one of two observable sources
- groupBy: Group objects by id and return as array.

  ```javascript
  import { of, groupBy, mergeMap, reduce } from "rxjs";

  of(
    { id: 1, name: "JavaScript" },
    { id: 2, name: "Parcel" },
    { id: 2, name: "webpack" },
    { id: 1, name: "TypeScript" },
    { id: 3, name: "TSLint" }
  )
    .pipe(
      groupBy((p) => p.id),
      mergeMap((group$) => group$.pipe(reduce((acc, cur) => [...acc, cur], [])))
    )
    .subscribe((p) => console.log(p));

  // displays:
  // [{ id: 1, name: 'JavaScript' }, { id: 1, name: 'TypeScript'}]
  // [{ id: 2, name: 'Parcel' }, { id: 2, name: 'webpack'}]
  // [{ id: 3, name: 'TSLint' }]
  ```

- distinctUntilChanged
  Returns a Observable that emits all values pushed by the source observable if they are distinct in comparison to the last value the result observable emitted.

  ```javascript
  of(1, 1, 1, 2, 2, 2, 1, 1, 3, 3)
    .pipe(distinctUntilChanged())
    .subscribe(console.log);
  // Logs: 1, 2, 1, 3
  ```

- catchError: Catches errors on the observable to be handled by returning a new observable or throwing an error.
- retry
  ```javascript
  const result = source.pipe(
    mergeMap((val) => (val > 5 ? throwError(() => "Error!") : of(val))),
    retry(2) // retry 2 times on error
  );
  ```
- shareReplay: You generally want to use shareReplay when you have side-effects or taxing computations that you do not wish to be executed amongst multiple subscribers. It may also be valuable in situations where you know you will have late subscribers to a stream that need access to previously emitted values. This ability to replay values on subscription is what differentiates share and shareReplay.

```typeScript
import { timer } from 'rxjs';
import { tap, mapTo, share } from 'rxjs/operators';

//emit value in 1s
const source = timer(1000);
//log side effect, emit result
const example = source.pipe(
  tap(() => console.log('***SIDE EFFECT***')),
  mapTo('***RESULT***')
);

/*
  ***NOT SHARED, SIDE EFFECT WILL BE EXECUTED TWICE***
  output:
  "***SIDE EFFECT***"
  "***RESULT***"
  "***SIDE EFFECT***"
  "***RESULT***"
*/
const subscribe = example.subscribe(val => console.log(val));
const subscribeTwo = example.subscribe(val => console.log(val));

//share observable among subscribers
const sharedExample = example.pipe(share());
/*
  ***SHARED, SIDE EFFECT EXECUTED ONCE***
  output:
  "***SIDE EFFECT***"
  "***RESULT***"
  "***RESULT***"
*/
const subscribeThree = sharedExample.subscribe(val => console.log(val));
const subscribeFour = sharedExample.subscribe(val => console.log(val));
```

## Comparisons

### `forkJoin` vs `combineLatest`

`forkJoin` require all input observables to be completed, but it also returns an observable that produces a single value that is an array of the last values produced by the input observables. In other words, it waits until the last input observable completes, and then produces a single value and completes.

In contrast, `combineLatest` returns an observable that produces a new value every time the input observables do, once all input observables have produced at least one value. This means it could have infinite values and may not complete. It also means that the input observables don't have to complete before producing a value.
