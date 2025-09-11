# State Pattern

The State Pattern is a behavioral design pattern used in object-oriented programming. It allows an object to change its behavior when its internal state changes, making the object appear to change its class. It is generally used when an object must behave differently based on its internal state.

## Structure

The pattern typically includes:

- Context – the main object that holds a _reference_ to a state object.
- State Interface – defines the behavior expected of all states.
- Concrete States – implement specific behaviors and can change the context’s current state.

Example in TypeScript

```typescript
// State interface
interface State {
  handle(context: Context): void;
}

// Concrete States
class StateA implements State {
  handle(context: Context): void {
    console.log("Handling in State A");
    context.setState(new StateB());
  }
}

class StateB implements State {
  handle(context: Context): void {
    console.log("Handling in State B");
    context.setState(new StateA());
  }
}

// Context
class Context {
  private state: State;

  constructor(state: State) {
    this.state = state;
  }

  setState(state: State): void {
    this.state = state;
  }

  request(): void {
    this.state.handle(this);
  }
}

// Usage
const context = new Context(new StateA());
context.request(); // Handling in State A
context.request(); // Handling in State B
```

## Pros

- Makes state-specific behavior easier to manage and extend.
- Encourages encapsulation and separation of concerns.

## Cons

- Increases the number of classes
- Can be an overkill if only a few states exists
