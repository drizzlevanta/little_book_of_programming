# Considerations

1. only one database? based on microservices, it should have several?It's much easier to perform schema updates, because only a single microservice is affected.
1. decentralize everything. avoid sharing code or data schemas. Data storage should be private to the service that owns the data. Use the best storage for each service and data type. Avoid coupling between services. Causes of coupling include shared database schemas and rigid communication protocols.
1. not working as team based on each microservice?
1. micro-frontend: store is per microservice?
1. how to define bounded context
1. shell manages the messaging/routing?
1. cross micro-front end communications?
1. how can microservices be deployed with non-microfrontend
1. Is each microservice/micro-frontend a separate codebase?
1. Polyglot programming. Can some part of the code be written in Rust?
1. Bounded context: it's often better to design separate models that represent the same real-world identity in two different contexts.
1. As a general principle, a microservice should be no smaller than an aggregate, and no larger than a bounded context.
1. inter-service communication: sync API call or async messaging?
1. is micro-front-end and microservice 1-on-1 relationship? for each front-end, the backend might be an aggregation of services
1. team structure with micro-frontend and micro services
1. what are the business domains?
