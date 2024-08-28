# Change Detection

In Angular, an OnPush component refers to a component that uses the OnPush change detection strategy. This strategy optimizes the performance of the component by reducing the number of change detection checks that Angular performs, leading to fewer re-renders and improved performance, especially in large applications.

## Default vs. OnPush Change Detection
### Default Change Detection
Strategy: The default change detection strategy in Angular is Default. With this strategy, Angular automatically checks every component in the application tree for changes during each change detection cycle. This can happen frequently, such as when an event is triggered, or an async operation completes.

Reactivity: If any bound data changes (including mutable objects), Angular re-renders the affected components.
### OnPush Change Detection
Strategy: With the OnPush strategy, Angular only checks a component for changes under specific conditions:
- Input Property Changes: If the component receives new input data through its @Input properties (the input reference must change, not just its internal state).
- Event Triggered within the Component: If an event originates from the component itself (like a button click within the component).
- Manually Triggered Change Detection: If you explicitly trigger change detection via methods like markForCheck() or detectChanges().
- Signal attached to it is changed

Reactivity: This strategy assumes that the component is mostly immutable or that changes to the component’s data are infrequent. Angular will skip checking this component unless one of the above conditions is met, significantly reducing the number of checks it performs.