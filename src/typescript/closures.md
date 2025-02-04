# Closures

A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function's scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

A closure is a feature where a function retains access to its lexical scope, even when the function is executed outside of its original scope.

```typescript
function outerFunction(outerVariable: string) {
  return function innerFunction(innerVariable: string) {
    console.log(`Outer Variable: ${outerVariable}`);
    console.log(`Inner Variable: ${innerVariable}`);
  };
}

const closureFunction = outerFunction("outside");
closureFunction("inside");
// Output:
// Outer Variable: outside
// Inner Variable: inside
```

Another example:

```typescript
function createCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

Closures are a powerful and flexible feature, enabling patterns such as function factories, currying, and maintaining state.

## Currying

Currying is a functional programming technique where a function that takes multiple arguments is transformed into a sequence of functions, each taking a single argument.

In essence, instead of calling a function with all its arguments at once, you call it with one argument at a time, returning a new function for each subsequent argument until all arguments are provided.

Currying is a technique that promotes modular and reusable code, especially in functional programming paradigms. It simplifies the handling of multi-argument functions and allows for elegant function composition and partial application.

Why Use Currying?

- Reusability: It allows the creation of specialized functions by fixing some arguments.
- Composition: Curried functions are easier to compose with other functions.

```typescript
const multiply = (a: number) => (b: number) => a * b;

const triple = multiply(3);
console.log(triple(4)); // 12
```
