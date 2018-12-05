# Toward Modular Monoliths Throught the Social View of Code

Pros and cons of rewrites

Alternatives to traditional layered architectures

## Dodge the Silver Bullet

### The Trade-Off Between Architectural Refinements and Replacement Systems

Rewrites pose problems:
- Immediate legacy code - current system is in use
- Playing the catch-up game - new system needs to catch up to current
- Division of efforts - few people working on existing system
- Motivation loss - maintaining a legacy app isn't motivating

**Consider**
- Do we need to port every single feature to the new system?

### Layered Architectures and the Cost of Consistency

Layered architectures -> increased coordination needs

### Change Patterns In Layered Architectures

![components](ch8_components.png "components")

![coupling](ch8_coupling.png "coupling")

### A Separation of Concerns That Concern

Complexity enforces more layers than just view/model/controller

With hierarchical layers, it's hard to define clear areas of responsibility for different teams

## Monolithic Alternatives: Use Case and Feature-Centric

Vast amount of space between monolith and microservice

You don't need to go full microservice to rescue a legacy codebase

### Package By Components and Features

*Package by component*
- Slice intertwined layers into coarse-grained components
- Make components an architectural building block that contains application logic and data-access logic, if needed
- Presentation layers and APIs are then built on top of the components

![Package by Component](ch8_pbc.png "Package by Component")

*Package by feature*
- Domain-oriented approach
- Each user-facing feature becomes a high-level building block
- UI is part of each feature, unlike in *Package by component*

![Package by Feature](ch8_pbf.png "Package by Feature")

*Data, context, and interaction*
- Clear separation between data/domain model (what the system is) and its features (what the system does)
- Separates data objects from feature-specific behaviors
    -Expressed in *object roles*
    - Differnt use cases express their context by combining specific object roles

![Data, context, and interaction](ch8_dci.png "Data, context, and interaction")

## Discover Bounded Contexts Through Change Patterns
