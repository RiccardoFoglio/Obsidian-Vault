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

75/212 Design