# Reducers

Reducers are pure functions in that they produce the same output for a given input. They are without side effects and handle each state transition synchronously.

- "I need to Remove a state value if another state part has changed. How should I access to other store parts from a reducer in NgRx?" in this case, recommended to dispatch a new action and handle via effects
- Use meta-reducers as middleware to log or debug. Example:

  ```typescript
  // console.log all actions
  export function debug(reducer: ActionReducer<any>): ActionReducer<any> {
    return function (state, action) {
      console.log("state", state);
      console.log("action", action);
      return reducer(state, action);
    };
  }

  export const metaReducers: MetaReducer<any>[] = [debug];

  @NgModule({
    imports: [StoreModule.forRoot(reducers, { metaReducers })],
  })
  export class AppModule {}
  ```
