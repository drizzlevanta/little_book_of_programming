# Operators
## `!!dark`
In TypeScript (or JavaScript), the !! operator is a way to convert a value to a boolean. Here's how it works:

The first ! negates the value, converting it to its opposite boolean value.
The second ! negates the result of the first !, effectively converting the original value to a boolean.

Example:
```typescript
let dark = "some truthy value";
console.log(!!dark); // true

dark = null;
console.log(!!dark); // false

```
