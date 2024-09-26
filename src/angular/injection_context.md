# Injection Context
## Injection token `@Inject`
When and Why to Use `@Inject` Custom Tokens: 
- If you're using a string or symbol as a token (as shown in the example), you need to tell Angular what specific provider to inject. This is often used for injecting values or services that aren't classes.
- Multiple Implementations: If you have multiple providers that implement the same interface or provide the same class, you can use @Inject with different tokens to differentiate between them.
- Abstract Classes or Interfaces: When using abstract classes or interfaces, which don't exist at runtime, @Inject can be used with a custom token to specify the implementation to inject.`


```typescript

@Injectable({ providedIn: 'root' })
class EventBusService1 implements EventBusService {
  // Implementation for EventBusService1
}

@Injectable({ providedIn: 'root' })
class EventBusService2 implements EventBusService {
  // Implementation for EventBusService2
}

// In your module or component providers
providers: [
  { provide: 'EVENT_BUS_SERVICE', useClass: EventBusService1 },
  { provide: 'ALTERNATIVE_EVENT_BUS_SERVICE', useClass: EventBusService2 }
]

// In your component
constructor(@Inject('EVENT_BUS_SERVICE') private eventBus: EventBusService) {
  // eventBus will be an instance of EventBusService1
}

constructor(@Inject('ALTERNATIVE_EVENT_BUS_SERVICE') private eventBus: EventBusService) {
  // eventBus will be an instance of EventBusService2
}
```
