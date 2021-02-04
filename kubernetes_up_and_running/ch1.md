# Chapter 1 - Introduction

Benefits of using containers & container APIs:

1. Velocity
2. Scaling (of both software and teams)
3. Abstracting your infrastructure
4. Efficiency

## Velocity

Immutability - allows you to re-deploy a completely new image that replaces the old one

- You don't have to mess with an existing image, engaging in mutable infrastructure
- There is an audit of how an image was created

Declarative configuration

- Represents the desired state of the system
- k8s' job is to ensure that the actual state of the world matches the desired state
- aka "Infrastructure as code"

Online self-healing systems

- Continuously takes actions to ensure that the current state matches the desired state
- Requies less/no human intervention

## Scaling Your Service and Your Teams

Decoupling

- Each component is separated from others by defined APIs and service load balancers
- Load balancers make it easy to scale
- APIs allow teams to focus on less surface area via microservices

Easy Scaling for Applications and Clusters

- Immuatble containers make it easy to create new instances

Scaling Development Teams with Microservices

- k8s provides abstractions and APIs that make it easier to build these decoupled microservice architectures:
    - *Pods* - groups of containers - can be grouped into a single deployable unit
    - *Services* - provide load balancing, naming, and discovery to isolate microservices
    - *Namespaces* - provide isolation and access contiol
    - *Ingress* objects provide an easy-to-use frontend to combine multiple microservices into a single externalized API surface area

Separation of Concerns for Consistency and Scaling

## Abstracting Your Infrastructure

Separates developers from machines

k8s has plugins that can abstract you from a particular cloud

## Efficiency

Allows for higher levels of utilization for CPU -> no idle CPU time

Reducing overall number of machines drives up efficiency of each system


