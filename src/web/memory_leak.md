# Memory Leak

Common memory leaks scenarios:

## `setTimeout` and `setInterval`

When using `setTimeout` and `setInterval`, we need to clear them, otherwise this will cause memory leaks. Best practice is to provide a reference, and clear that reference on destroy. For example:

```typescript
const timer=setInterval(
  doSomething();
, 1000); //provide a reference to the timer
clearInterval(timer); // clear the timer
```

## `console.log({object})`

Retention of Object References: When `console.log` logs an object, it logs a reference to the object rather than its immediate value. This means: If the object is still in memory and logged to the console, the console holds a reference to it, preventing it from being garbage collected. This behavior is especially problematic if you're logging large objects or objects that are updated frequently.

Deferred Evaluation in Developer Tools: When you log an object or array, the actual evaluation and display of the logged value are deferred until you expand the log in the console. By that time, the object might have already changed due to subsequent operations in the code.

For example:

```typescript
const largeObject = { data: new Array(1000000).fill("large") };
console.log(largeObject); // Large object retained in memory
```
