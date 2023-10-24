# Unit Test
## Mocking
- Use mocks for all dependencies, instead of the real dependencies. 
- Recommend to use [`ng-mocks`](https://ng-mocks.sudo.eu/) to quickly mock up dependencies.
- Use `ng-mocks` together with `Jest` to customize mocking and spying for more complicated scenarios. 

## Anti-pattern
- Avoid testing the round-trip. Test only the isolated piece. 

## Scenarios
- Overwrite default mocking implementation within test: `jest.spyon(myObject, 'someMethod').mockImplementation()`
- Use `fakeAsync` and `tick` from Angular testing utilities to deal with asynchronous code.
- To test multiple subscriptions within `ngOnInit`, for each subscription, conditionally return a specific observable. 
  ```typescript
        jest.spyOn(dataService, 'sub').mockImplementationOnce((s, e) => {
        if (e === Events.ON_SUBMIT) {
          return of([1]); // Return an observable on Events.ON_SUBMIT event stream
        }
        return EMPTY; // Return an EMPTY on all other event streams
      });

  ```

