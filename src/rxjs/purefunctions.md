# Pure Functions

## Characteristics of a Pure function:

- Referential Transparency: A pure function always produces the same output for the same input, regardless of when or where it is called in a program. In other words, the output of a pure function depends solely on its input and has no dependency on any external state or side effects.
- Lack of Side Effects: A pure function does not modify or affect the state of variables outside its local scope. It does not have any observable side effects, such as modifying global variables, writing to a file, or making network requests. The only result of calling a pure function is the computed return value.

## Purity in RxJS:

Normally you would create an impure function, where other pieces of your code can mess up your state.

```javascript
let count = 0;
document.addEventListener("click", () =>
  console.log(`Clicked ${++count} times`)
);
```

Using RxJS you isolate the state.

```javascript
import { fromEvent, scan } from "rxjs";

fromEvent(document, "click")
  .pipe(scan((count) => count + 1, 0))
  .subscribe((count) => console.log(`Clicked ${count} times`));
```
