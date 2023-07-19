# NgRx

NgRx is a reactive state management library for Angular applications inspired by Redux. It leverages RxJS to handle state changes and provides a predictable state container. NgRx follows a unidirectional data flow, where actions trigger state changes through reducers, and components subscribe to the state to update their views.

In NgRx, you typically define a separate reducer function for each slice of state in the store, rather than for every individual field. Each reducer is responsible for managing a specific portion of the overall state.

NgRx isolates side effects to promote a cleaner component architecture.
