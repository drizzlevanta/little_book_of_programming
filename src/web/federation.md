# Federation

In web development, federation refers to the practice of distributing and sharing responsibilities across multiple independent systems or services. The concept of federation is particularly common in the context of microservices architecture, where different services work together to build a larger, more complex application.

In a federated system, each service is autonomous and responsible for a specific set of functionalities. These services can communicate with each other through well-defined interfaces, enabling them to collaborate and provide a cohesive experience to users. The main idea behind federation is to promote loose coupling and scalability, as individual services can be developed, deployed, and maintained independently.

## Module Federation

Module Federation is a feature provided by Webpack, the popular JavaScript module bundler. It allows multiple independent frontend applications to dynamically load and use code from each other at runtime. In simpler terms, it enables sharing of JavaScript modules (components, libraries, etc.) across different frontend applications.

Module Federation provides a solution to the scaling problem by allowing a Single Page Application (SPA) to be sliced into multiple smaller remote applications that are built independently.

With Module Federation, a large application is split into:

1.  A single Host application that references external...
2.  Remote applications, which handle a single domain or feature.

Downsides:

- Developers need to think about which remotes they are working on, since it is a waste of CPU and memory to run all remotes in development mode. In practice this may not be a problem if the teams are already divided by domain or feature.
- Increased orchestration since remotes are independent of each other, shared state may require the host application to coordinate it between remotes. For example, sharing Redux state across remotes is more complicated versus a SPA.
- Version-Mismatch-Hell where different applications are deployed with different versions of shared libraries can lead to unexpected errors.

## Module Federation vs Micro Frontend

While Module Federation enables faster builds by vertically slicing your application into smaller ones, the MFE architecture layers independent deployments on top of federation. The Micro Frontend (MFE) architecture builds on top of Module Federation by providing independent deployability. Teams should only choose MFEs if they want to deploy their host and remotes independently.
