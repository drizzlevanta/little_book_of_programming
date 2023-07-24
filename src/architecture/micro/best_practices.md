# Best Practices

1. different domain can use different representations for the same thing. If we tried to create a single model for both of these subsystems, it would be unnecessarily complex. It would also become harder for the model to evolve over time, because any changes will need to satisfy multiple teams working on separate subsystems. Therefore, it's often better to design separate models that represent the same real-world entity (in this case, a drone) in two different contexts. Each model contains only the features and attributes that are relevant within its particular context.
1. a microservice should be no smaller than an aggregate, and no larger than a bounded context
1. an aggregate is a transactional boundary
1. the Scheduler service sends an asynchronous message to the Supervisor, so that the Supervisor can schedule compensating transactions
1. failure fallback during transaction
1. independent deployability of micro frontends is key
1. ? there could be a separate server responsible for rendering and serving each frontend
1. against publish each micro front-end as a package
1. Ensure that CSS is only applied to a specific front-end component via naming conventions
1. communications between micro-fronts: we recommend having them communicate as little as possible, as it often reintroduces the sort of inappropriate coupling that we're seeking to avoid in the first place.
1. Use custom event to communicate indirectly via micro front-ends.
1. Just like sharing a database across microservices, as soon as we share our data structures and domain models, we create massive amounts of coupling, and it becomes extremely difficult to make changes.
1. The redux docs even mention "isolating a Redux app as a component in a bigger application" as a valid reason to have multiple stores.
1. auth or other cross-cutting concerns should be owned by the container app.
1. each micro front-end should has its own source repo and own deployment pipeline
1. name collisions?
1. extract dependencies? One approach is to externalise common dependencies from our compiled bundles. If there is a breaking change in a dependency, we might end up needing a big coordinated upgrade effort and a one-off lockstep release event. This is everything we were trying to avoid with micro frontends in the first place!
1. define standards and conventions
1. use of SSI?
1. googd loading states for users?

## Sharing the same database?

Microservices can share a database, but it is generally not recommended as a best practice.

Data model complexities: Different microservices might have different data requirements and structures. Sharing a database can lead to complex data models that are hard to manage and evolve.

Performance bottlenecks: If multiple microservices are accessing the same database, it can create contention and performance issues, especially during high loads.

Lack of independence: Changes to one microservice may have unintended consequences on other microservices that share the same database, leading to versioning and deployment challenges.

Reduced fault isolation: A bug or issue in one microservice could affect others that rely on the shared database, making it harder to pinpoint the root cause of the problem.

## Sharing a state store between MFEs?

Redux is one of the most popular libraries for predictable state management. However, the general practice in using Redux is to have a single store, thereby having a single state object. This approach would mean that all the Micro Frontends would have a shared state. This is a violation of the Micro Frontend based architecture since each App is supposed to be a self-contained unit having its store.

In a Micro Frontend architecture, an individual application should not be able to modify the state of other apps. However, they should be able to see the state of other apps. Along the same line for enabling cross-application communication, they should also be able to send events/actions to other Stores and also get notified of changes in other apps' state. This library aims to attain that sweet spot between providing isolation and cross-application communication.

## Communication between MFEs

IF we add providerIn:root to shared service, shell and remote mfe’s will initiate two instances of the services. Lazy loaded modules have their own root scope
