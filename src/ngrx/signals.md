# NgRx + Signals
## Signal Store
A SignalStore is created using the signalStore function. This function accepts a sequence of store features.

Signals are not meant to have a concept of time. Also, the effect is somewhat tied to Angular change detection, so you can't observe every action that would be dispatched over time through some sort of Signal API. The global NgRx Store is still the best mechanism to dispatch action(s) over time and react to them across multiple features.

When a reactive method is called with a signal, the reactive chain is executed every time the signal value changes. Example:
```typescript
import { Component, OnInit, signal } from '@angular/core';
import { map, pipe, tap } from 'rxjs';
import { rxMethod } from '@ngrx/signals/rxjs-interop';

@Component({ /* ... */ })
export class NumbersComponent implements OnInit {
  readonly logDoubledNumber = rxMethod<number>(
    pipe(
      map((num) => num * 2),
      tap(console.log)
    )
  );

  ngOnInit(): void {
    const num = signal(10);
    this.logDoubledNumber(num);
    // console output: 20
    
    num.set(20);
    // console output: 40
  }
}
```
