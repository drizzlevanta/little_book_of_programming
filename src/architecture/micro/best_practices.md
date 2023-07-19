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
