# Strategies for Breaking the Monolith

## Working Effectively with Legacy Code
*by Michael Feathers*
#### Highly recommended

## Agenda
* Monoliths
* Why?
* Strategies that may work, but often don't
* Evolution not revolution
* An example

## Monoliths

| Advantages | Disadvantages|
|---|---|
| Simple to develop and maintain | Massive scale, tends towards massive dollars|
| Tend to scale better vertically than horizontally | Tightly coupled design|

Monoliths aren't necessarily the problem. It's usually bad coding practices and architecture that creates the pain.

Microservices give you the flexibility to handle more scale at lower cost. They are not good by default.

## Why break the monolith?

* Business needs evolve
* Production software needs to serve the business

## Strategies that don't usually work

### Rewrite the software with a new team

| Advantages | Disadvantages |
|---|---|
| Won't make the same mistakes as the old team | Will make all new mistakes |
| Can take a fresh look at an od problem | Lack of skills |

#### Why does this fail?

* Scope creep
* Lack of expertise in the business
* Relationships
* Culture 
* Morale

### Rewrite the software with the same team

| Advantages | Disadvantages |
|---|---|
| Won't make the same mistakes as the old team | Will make all new mistakes |
| Allows for parallel development | Features will diverge |
| | Scope |
| | Teams skills don't level up together |

#### Why does it fail?

* Scope creep
* Business pressure
* Morale

## Evolution, not Revolution

* Keep the band together
* Level up together
* Keep the business happy
* Production application can evolve

### Anti-Corruption Layer

#### Domain Driven Design
*by Eric Evans*

* Create an isolated layer between monolith and microservices that provides clients with functionality in terms of their own domain model.
* The layer talks to the other systems through its existing interface, requiring little or no modification to the other system.
* Internally, the layer translates in both directions as necessary between the two models.

*Anti-Corruption Layer could be an API Gateway, or an actual application if a more complicated model transformation needs to be implemented.*

## Demo 

### Goals

* Allow new systems to integrate with the old
* Allow the monolith to be the monolith for as long as it has to be
* Replace monolith functionality a little at a time
* Allow new and old systems to work together

### Implementation

1. Set up tests around system you want to change
1. Build replacemet API
1. Build anti-corruption layer

