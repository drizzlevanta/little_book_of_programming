# Best Practices

- Consistent folder strucutre: separate folders for actions, reducers, effects.
- Keep actions simple
- Reducers should be pure
- Maintain immutability
- Use effects for side effects
- Avoid excessive nesting of states
- Effects are for _orchestration_ only, make sure that the actual logic is in services to keep the effects clean.
- Keep effects small and modular as possible.
- Use @ngrx/schematics to auto generate feature
- The MOST GENIUS things about NgRx is that whole 80% of logic we have to write is just plain TypeScript functions which are NOT aware of Angular or RxJs. 80% are in the form of pure functions.
- When we need to access NgRx state slice from one lazy feature in another lazy feature of our Angular application. Importing stuff between sibling lazy features is forbidden because it would break all the benefits of such architecture and could in theory lead also to runtime errors
- componenet should only do two things in the context of ngrx:
  - retrieve and display state from the store using NgRx selectors
  - dispatch actions based on user interactions
- In effects, use `catchError` operator to handle error
- Avoid saving derived state in state object: A common problem with NgRx and other state management libraries is saving derived state in your state object. The derived state is often updated via the reducer as a result of a change in another part of the state. This can cause the derived state to get out of sync if any of the reducers forgets to also update the derived state. That is the reason why NgRx recommends always accessing the derived state via selectors instead of saving it in the state object. However, if performance is an issue, you can try to save a minimal derived state in store.
