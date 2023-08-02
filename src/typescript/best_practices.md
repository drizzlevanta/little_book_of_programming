# Best Practices

## Types

- Don't ever have a generic type which doesn’t use its type parameter.
- Don't use any as a type unless you are in the process of migrating a JavaScript project to TypeScript. In cases where you don’t know what type you want to accept, or when you want to accept anything because you will be blindly passing it through without interacting with it, you can use [`unknown`](https://www.typescriptlang.org/play#example/unknown-and-never)
- Don't use the return type any for callbacks whose value will be ignored:
- Using void as a return type
- Use strict comparisons, we should make sure that we use ` ===`` and  `!==` for equality comparisons.
- Use Strict String Expressions: `foo ${bar}` for string concatenation.
- Add default for `Switch`
- No Unnecessary Constructor. We shoudn't have constructors that are redundant. JavaScript will add them for us without it.
- Default Type Parameter. We can add a default type value to the generic type parameter.
  ```typescript
  function foo<N = number, S = string>() {}
  ```
- Explicitly writing void as the return type is optional, but it can be beneficial for clarity and when you want to explicitly state that a function does not return any meaningful value. Additionally, it helps prevent potential issues when using strict mode in TypeScript.
- Don’t use optional parameters in callbacks unless you really mean it
- It’s always legal for a callback to disregard a parameter, so there’s no need for the shorter overload. Here the done param can be discarded.
  ```typescript
  /* OK */
  declare function beforeAll(
    action: (done: DoneFn) => void,
    timeout?: number
  ): void;
  ```

## Function Overloads

### Ordering

TypeScript chooses the first matching overload when resolving function calls. When an earlier overload is "more general" than a later one, the later one is effectively hidden and cannot be called.

- Don't put more general overloads before more specific overloads like this:

  ```typescript
  /* WRONG */
  declare function fn(x: unknown): unknown;
  declare function fn(x: HTMLElement): number;
  declare function fn(x: HTMLDivElement): string;
  var myElem: HTMLDivElement;
  var x = fn(myElem); // x: unknown, wat?
  ```

- Do sort overloads by putting the more general signatures after more specific signatures:
  ```typescript
  declare function fn(x: HTMLDivElement): string;
  declare function fn(x: HTMLElement): number;
  declare function fn(x: unknown): unknown;
  var myElem: HTMLDivElement;
  var x = fn(myElem); // x: string, :)
  ```

### Use Optional Parameters

Use optional parameters instead of several overloads:

```typescript
interface Example {
  diff(one: string, two?: string, three?: boolean): number;
}
```
