# Best Practices

## General

- Avoid nested subscriptions: Nested subscriptions can lead to memory leaks and make your code harder to understand and maintain. Instead, use higher-order operators like switchMap, mergeMap, or concatMap to flatten and manage inner observables.
- Use Subjects judiciously: Subjects can be powerful, but they should be used carefully. Avoid using Subjects as a default solution for sharing data between components. Consider using BehaviorSubject or ReplaySubject when you need to share state or emit initial values.
- Leverage error handling: RxJS provides operators like catchError and retry to handle errors gracefully. Use these operators to handle errors at the appropriate level in your observable chain and provide meaningful error messages to users.
- Use AsyncPipe when possible: In Angular templates, prefer using the AsyncPipe to subscribe to observables and handle the subscriptions automatically. This helps manage subscriptions and simplifies the template code.


## Avoid Race Conditions
Overlapping asynchronous operations usually lead to undesirable race conditions. For example, if the user searches for two different desserts in quick succession, both results might be displayed one after the other. One of the two only flashes briefly before the other replaces it. Due to the asynchronous nature, the order of results does not need to match the order of requests.

To prevent this confusing behavior, RxJS provides various flattening operators:
- switchMap
- mergeMap
- concatMap
- exhaustMap