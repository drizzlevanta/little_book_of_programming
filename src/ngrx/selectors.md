# Selectors

- @ngrx/store keeps track of the latest arguments in which your selector function was invoked. _Because selectors are pure functions, the last result can be returned when the arguments match without reinvoking your selector function_. This can provide performance benefits, particularly with selectors that perform expensive computation. This practice is known as memoization.
- An advanced technique is to combine selectors with RxJS pipeable operators. The select method can be used inside the pipe operator. Example:

  ```typescript
  import { select } from "@ngrx/store";
  import { map, filter } from "rxjs/operators";

  store
    .pipe(
      select(selectValues),
      filter((val) => val !== undefined)
    )
    .subscribe(/* .. */);
  ```

  -To make the select() and filter() behaviour a reusable piece of code, we extract a pipeable operator using the RxJS pipe() utility function:

  ```typescript
  import { select } from "@ngrx/store";
  import { pipe } from "rxjs";
  import { filter } from "rxjs/operators";

  export const selectFilteredValues = pipe(
    select(selectValues),
    filter((val) => val !== undefined)
  );

  store.pipe(selectFilteredValues).subscribe(/* .. */);
  ```

- Advanced exmaple: select the last {n} state transitions, a combination of NgRx and RxJS operators:

  - selector function from the state

    ```typescript
    export const selectProjectedValues = createSelector(
      selectFoo,
      selectBar,
      (foo, bar) => {
        if (foo && bar) {
          return { foo, bar };
        }

        return undefined;
      }
    );
    ```

  - component should visualize the history of state transitions. We are not only interested in the current state but rather like to display the last n pieces of state. Meaning that we will map a stream of state values (1, 2, 3) to an array of state values ([1, 2, 3]).

    ```typescript
    // The number of state transitions is given by the user (subscriber)
    export const selectLastStateTransitions = (count: number) => {
      return pipe(
        // Thanks to `createSelector` the operator will have memoization "for free"
        select(selectProjectedValues),
        // Combines the last `count` state values in array
        scan((acc, curr) => {
          return [curr, ...acc].filter(
            (val, index) => index < count && val !== undefined
          );
        }, [] as { foo: number; bar: string }[]) // XX: Explicit type hint for the array.
        // Equivalent to what is emitted by the selector
      );
    };
    ```

  - subscribe to in component and provide the number n

    ```typescript
    // Subscribe to the store using the custom pipeable operator
    store.pipe(selectLastStateTransitions(3)).subscribe(/* .. */);
    ```

- The key difference between a selector and a feature selector is that selectors are used to retrieve and transform state from the store in a general sense, whereas feature selectors are specifically used to work with feature state, which is state associated with a particular feature module in your application.
- By using feature selectors, you can access and manipulate the state specific to a feature module without worrying about other parts of the application state, providing a more modular and focused approach to state management in NgRx.
- Using feature selector vs state.feature, it allows you to compose other more complicated selectors using a feature selector.
