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
