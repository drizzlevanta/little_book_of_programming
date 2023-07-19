# Event Loop

- JS runtime engine (like v8) contains the heap (where memory allocatin happens), and the stack (which includes the call stack).
- WebApis are provided by browser, not by JS runtime
- WebApis includes DOM, setTimeout
- event loop, call back
- JS is single threaded, a single callstack, it can do one thing at a time
- a callstack is a data structure records where we are in the program, sort like the to-do list
- we can't block the stack because it's in the browser and we want to have nice fluidUI, the solution is async callback
- JS can have callback and other thing to run things async is because browser has other things like Web Apis
- Any webapis (like callbacks in settimeout), when they're done, they are pushed to the task queue
- event loop's job is to look at the stack and look at the task queue, if the stack is empty, it takes the first thing on the queue, and pushes it on to the stack.
- if you want to do settimeout 0, is when you want to defer something until the stack is clear
- the browser has a render queue, which gives a higher priority than the callback queue (microtask queue).
- don't block the event loop - don't put slow code on the stack, it will block the render queue
- Microtasks have a higher priority than macrotasks, meaning that they are executed before the event loop moves to the next macrotask.
  The microtask queue is processed completely before any macrotask is executed.
- microtask vs macrotask
