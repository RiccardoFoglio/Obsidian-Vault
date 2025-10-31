Requirements = what the system should do
Architecture = how the system should be built

- architecture = high level, decide major components
- design = lower level, decide internals of each component

most defects come from requirements and design
essential to define analyze and evaluate design choices early
- if no design is defined, but code is developed immediately, design choices are made implicitly and evaluated late
- doing design allows to make design choices explicit, document and evaluate them

Requirements to Design : given a set of requirements, many designs are possible but not all equal
![[Screenshot 2025-05-07 at 5.29.11 PM.png|500]]
![[Screenshot 2025-05-07 at 5.29.29 PM.png|500]]

Software process includes also a part about hardware components and software – hardware allocation (UML Deployment diagram)

## Process
- analysis
- formalization
- verification

Input : requirement document
Output : design document --> components + connections capable of satisfying functional and NF requirements

1. Architecture : define high level components and their interactions, select communication and coordination model, Use architectural style(s) / pattern(s)
2. Design
	1. High Level --> many classes, use design patterns
	2. Low Level --> single class

## Properties

Functional properties : does the design support functional requirements?
NF Properties : does the design support non-functional requirements?

NF properties specific to design:
- Testability
	- observability
	- controllability
- Monitorability
- Interoperability
- Scalability
- Deployability
- Mobility

- Complexity : number of components, of interactions
- Coupling : degree of dependence between two components
- Cohesion : degree of consistence of functions of a component

![[Screenshot 2025-05-07 at 5.42.10 PM.png|500]]

- Cost
- Schedule
- Staff Skills

what to do:
- Performance : localize critical operations and minimize communications. Use large rather than fine-grain components
- Security : use layered architecture with critical components in inner layers
- Safety : Localize safety-critical features in a small number of components
- Availability : Include redundant components and mechanisms for fault tolerance
- Maintainability : use fine-grain replaceable components

tradeoffs

## Formalization of Architecture

informal = box and lines
![[Screenshot 2025-05-07 at 5.50.05 PM.png|300]]

semiformal = UML diagrams
![[Screenshot 2025-05-07 at 5.51.06 PM.png|300]]
each package looks like this inside:
![[Screenshot 2025-05-07 at 5.51.46 PM.png|300]]

formal ADL (architecture description languages)

UML helps in presenting structure in an organized way
- packages in system
- classes in package
- attributes and methods in class
Presentation is sequential, but definition of structure requires several iterations:

Sequence
![[Screenshot 2025-05-07 at 5.54.53 PM.png|500]]

## Patterns

reusable solutions to recurring problems in a defined context

- architectural pattens : address system wide structures
- design patterns : leverage higher level mechanisms
- idioms : leverage language specific features

### Architectural Patterns

Layers, Pipes and Filters, Repository, Client server, Broker, MVC, Microkernel, Microservices
#### Repository Style
subsystems exchange data
- shared data held in central database or repo and may be accessed by all sub-systems
- each subsystem maintains its own database and passes data explicitly to other sub-systems

![[Screenshot 2025-05-07 at 6.05.43 PM.png|600]]

Pros:
- efficient way to share large amounts of data
- sub-systems not concerned with how data is produced
- centralized management
- sharing model is published as repository schema

Cons: 
- subsystems must agree on a repo data model --> compromise
- data evolution is difficult and expensive
- no scope for specific management policies
- difficult to distribute efficiently

#### Client Server

set of stand-alone servers which provide specific services such as printing, data management etc...
Set of clients which call on these services
Network allows clients to access servers

![[Screenshot 2025-05-07 at 6.22.08 PM.png]]

Pros:
- straightforward data distribution
- makes effective use of networked systems, may require cheaper hardware
- easy to add new servers or upgrade existing servers

Cons:
- no shared data model --> subsystems use different data organizations --> data interchange may be inefficient
- redundant management in each server
- no central register of names and services --> may be hard to find out what serves and services are available

#### Abstract Machine (layered) Model

used to model the interfacing of subsystems
organizes the system into a set of layers each of which provide a set of services
Constraint : layer uses only services from adjacent layer

Pros:
- each layer is about a problem, separation of concerns
- when a layer interface changes, only the adjacent layer is affected
Cons:
- artificial to structure systems this way

![[Screenshot 2025-05-07 at 6.30.43 PM.png|300]] ![[Screenshot 2025-05-07 at 6.31.00 PM.png|300]]

## Pipes and Filters

process data streams according to several steps
- must be possible to recombine steps
- non-adjacent steps don't share info
- user storing data after each step may result in errors or garbage

![[Screenshot 2025-06-16 at 2.17.30 PM.png|500]]

Broker : environment with distributed and possibly heterogeneous components
Components should be able to access others remotely and location independently
components can be changed at run-time
users should not see too many details
![[Screenshot 2025-06-16 at 2.21.53 PM.png|500]]

Example: skyscanner --> single entry point to get customer information, sends requests to several online airlines and sends back data

MVC Pattern: show and manage changes to data --> the rendering of the view is separated from the object, allowing for more representations
changes need to be propagated
- separation of responsabilities
- but more complexity --> less performance

## Microkernel

several APIs insist on a common core
HW and SW evolve continuously and independently --> platform should be portable and extendable
solution: isolate a stable core of services, extend core with plugins

![[Screenshot 2025-06-16 at 2.37.37 PM.png|500]]

2 different approaches in developing a complex application

- Monolith
	- one single DB
	- all services insisting on it
	- direct calls between services or data shared via DB
- Microservices
	- several microservices
	- each implements a service, with own DB
	- communication via REST APIs

microservice = one executable running in its process (real or virtual machine)
microservices communicatae via http calls
application made of many communicating microservices via orchestration and choreography

Monolith:
PRO
- simpler initial design and management
CON
- harder evolution
- function scale up more difficult

# Design

## High Level

Definition of classes:
- from glossary --> consider a class for each key entity in the glossary
- from context diagram

consider design patterns
## Low Level

for each attribute, define type and privacy
for each method define return type, number and type of parameters, privacy
define setters and getters
for each method, choose algorithms
for each relationship with other class, choose the implementation

decide persistency
- no
- yes --> serialization, to database
## Relationships
- 1-1*
![[Screenshot 2025-06-16 at 3.32.48 PM.png]]

- Many-Many
![[Screenshot 2025-06-16 at 3.33.16 PM.png]]

Design Patterns:
- description of communicating objects and classes that are customized to solve a general design problem in a particular context
- A design pattern names, abstract and identifies the key aspects of a common design structure that makes it useful for creating a reusable object-oriented design

### Creational Patterns:

#### Factory Method
delegates the creation of the objects to subclasses
encapsulate all problems of creating a sensor in a class method.
![[Screenshot 2025-06-16 at 3.38.28 PM.png]]
#### Abstract Factory
a family of related classes can have different implementation details
the client shouldn't know anything about which variant they are using/creating

![[Screenshot 2025-06-16 at 3.45.07 PM.png]]
#### Builder
![[Screenshot 2025-06-16 at 3.46.05 PM.png]]
#### Prototype
![[Screenshot 2025-06-16 at 3.46.26 PM.png]]
#### Singleton
class represents a concept  that requires a single instance
clients could use this class in an inappropriate way

![[Screenshot 2025-06-16 at 3.47.27 PM.png]]

### Structural Patterns

#### Adapter
Class provides required features, but its interface is not the one required
How is it possible to integrate the class without modifying it
	source code couldn't be available
	it's already used as it is somewhere else

inheritance plays a fundamental role
![[Screenshot 2025-06-16 at 3.50.31 PM.png|400]]
#### Bridge

Separate abstraction from implementation
![[Screenshot 2025-06-16 at 3.51.14 PM.png|500]]

#### Composite

need to represent part-whole hierarchies of objects
clients are complex
difference between composition objects and individual objects
![[Screenshot 2025-06-16 at 3.53.04 PM.png]]
don't know the amount of levels
recursive patterns inside the functions to take that into account
![[Screenshot 2025-06-16 at 3.52.32 PM.png]]
#### Decorator

add responsibilities to individual objects dynamically and transparently, without affecting other objects
avoid many subclasses for every combination of features
need flexibility to stack features dynamically at runtime
inheritance is too rigid, extending a class for every combination leads to class explosion
![[Screenshot 2025-06-16 at 3.55.26 PM.png]]
#### Facade
functionality is provided by a complex group of classes (interfaces, associations etc...)
how is it possible to use the classes without being exposed to the details
![[Screenshot 2025-06-16 at 3.58.37 PM.png]]
![[Screenshot 2025-06-16 at 3.58.58 PM.png]]
#### Flyweight

need to support a large number of fine-grained objects efficiently
creating thousands of similar objects consumes much memory
many objects share same data but you are duplicating it
goal: minimize memory usage by sharing as much data as possible
![[Screenshot 2025-06-16 at 4.00.19 PM.png]]

#### Proxy

need to control access to an object, add behavior before of after calls, or defer expensive object creation
Direct access to some objects can be inefficient, unsafe, or undesirable

![[Screenshot 2025-06-16 at 4.01.19 PM.png]]

# Verification

FR --> Traceability matrix, scenarios executed on architecture, inspection
NFR --> performance, scenarios enriched with time model (inspection)

![[Screenshot 2025-06-16 at 4.31.01 PM.png]]

if a line or column is completely clear, means something's missing

Scenarios: each scenario must be feasible. it's possible to define a sequence of calls to member functions of classes in the software design that matches the scenario
![[Screenshot 2025-06-16 at 4.32.54 PM.png]]

