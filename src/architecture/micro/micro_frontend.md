# Micro Frontend

## TL;DR

Micro Frontend is an architecture in which a monolithic front-end is decomposed into separate and _independent_ apps. Each app has an independent build process and deployment, and even tech stack. So this indicates a faster development cycle.

## Pros

Each MFE

- has a small bundle size
- can be built and deployed independently
- can be lazy loaded
- has minimum communication between other MFEs

## Use Cases

We recommend MFE for teams that require applications to be deployed independently. It is important to consider the cost of MFEs and decide whether it makes sense for your own teams.

- Version mismatches where applications are deployed with different versions of shared libraries, which can lead to incompatibility issues.
- Independent deployments can lead to unexpected errors, such as any host-level changes to orchestration/coordination logic that breaks compatibility with remotes.
