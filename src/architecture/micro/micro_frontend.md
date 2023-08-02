# Micro Frontend

## TL;DR

Micro Frontend is an architecture in which a monolithic front-end is decomposed into separate and _independent_ apps. Each app has an independent build process and deployment, and even tech stack. So this indicates a faster development cycle.

## Pros

Each MFE

- has a small bundle size
- can be built and deployed independently
- can be lazy loaded
- has minimum communication between other MFEs

## Cons

- Version mismatches where applications are deployed with different versions of shared libraries, which can lead to imcompatibility issues.

## Use Cases

We recommend MFE for teams that require applications to be deployed independently. It is important to consider the cost of MFEs and decide whether it makes sense for your own teams.

- Version mismatches where applications are deployed with different versions of shared libraries, which can lead to incompatibility issues.
- Independent deployments can lead to unexpected errors, such as any host-level changes to orchestration/coordination logic that breaks compatibility with remotes.

## Deployment

Since deployments with MFEs are not atomic, there is a chance that shared libraries -- both external (npm) and workspace -- between the host and remotes are mismatched. The default the Nx setup configures all libraries as singletons, which requires that all affected applications be deployed for any given changeset, and makes à la carte deployments riskier. Core libraries such as react, angular, redux, ngrx, etc. must be singletons. Otherwise the applications will not work together.

There are mitigation strategies that can minimize mismatch errors. One such strategy is to share as little as possible between applications.
