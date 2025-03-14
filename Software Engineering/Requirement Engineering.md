it's about defining the product properties before starting the development.
Without this step, product properties are unclear and testing is not feasible.

Properties:
- Functional : what the program should do
- Non-Functional : modalities applied to functions offered
	- Reliability
	- Usability
	- Performance
	- Maintainability
	- Security

Outline: concepts and definitions, techniques for formalization elicitation and V&V
![[Screenshot 2025-02-26 at 4.40.48 PM.png]]

The final product may, or may not, have properties that match the requirements
Requirements are requests, that may or may not become properties of the product

Requirements should be both complete and consistent:
- Complete --> should include descriptions of all features required
- Consistent --> there should be no conflicts or contradictions in the descriptions of the system features

Defects:
- omissions / incompleteness
- incorrect fact
- inconsistency / contradiction
- ambiguity
- extraneous information
- redundancy

Techniques:
- **Stakeholders** : ask informal description of product. List relevant stakeholders is essential to consider relevant POVs for an application 
- **Business model** : involves some set requirements on payments for example
- **Personas**, **stories** : technique to informally define what the application should do
	- **Profiling** : characterize people under a number of dimensions to better define and sell targeted products / services:
		- demographic profile
		- psychographic profile
		- geografic profile
		E-commerce : Website behavior, number of sessions, number of pages visited etc...
		Profiling used for: first time visitors, returning visitors, returning customers (for discounts)
- **Context diagram and interfaces** : define what is inside the app to be developed, what is outside. Define interfaces between inside and outside
	- **Interfaces** :
		- cashier / admin --> screen+keyboard (physical) GUI (logical)
		- product --> laser beam (physical) bar code (logical)
		- credit card system --> internet connection (physical) web services (logical)
		Three types of interface may have to be defined:
		- user interfaces, GUIs
		- Procedural interfaces
		- Data exchanged
		These are essential to agree on what is the scope of the system
		The scope changes radically cost and time to develop
- **Requirements, F, NF** : 
	- Functional : description of services / behaviors provided by system
	  ![[Screenshot 2025-03-14 at 12.11.41 PM.png|300]]
	useful for management and testing purposes too
	- Non Functional : constrains on the services
		- **Usability** --> effort needed to learn how to use the product
		- **Efficiency** --> response time or memory/cpu/bandwidth/energy used
		- **Correctness** --> capability to provide intended functionality in all cases
		- **Reliability** --> defects visibly by end user per time period, percentage of time the product is not available to end user
		- **Maintainability** --> Effort needed to add / modify / cancel a software function, fix defects, deploy on different platforms...
		- **Portability** --> effort to redeploy application on another platform
		- **Security** --> protection from malicious access, authorized users only
		- **Safety** --> absence of harm to persons or hazardous situations for persons
		- **Dependability** --> **Safety** + **Security** + **Reliability**
		We can have
		- Product Requirements
		- Organisational Requirements
		- External Requirements
		less than 0.1s is not percepted as waiting time, more than 1s is annoying
		Non-functional requirements may be more critical than functional requirements. If these are not met, the system is useless.
		NF must be measurable : speed size ease of use reliability robustness portability have measures
		NF like : ease of use, maintainable, portable are NOT TESTABLE = USELESS
		Key stakeholders must decide the ranking of NF requirements: more important security or efficiency?
		
- **Table of rights** : who can do what, some have access to FR or not
- **Scenarios**, **sequence diagrams**
- **Use cases**
- **Glossary** --> give definitions to terms, so everyone is on the same page. Class Diagram can help (ULM)
	- physical objects
	- legal / organizational entities
	- descriptors
	- time evens and time intervals
	- commercial transactions
- **System design**


## Scenarios and Use Cases
Scenario = sequence of steps that describe a typical interaction with the system
Sequence = time is a key part of this model

Sale1
![[Screenshot 2025-03-14 at 1.17.29 PM.png|500]]
Precondition: Cashier is authenticated
Postcondition: Sale completed with payment

Login
![[Screenshot 2025-03-14 at 1.24.21 PM.png|500]]
Precondition: cashier is not authorized
Postcondition: cashier is authorized and authenticated

**Precondition** = condition to be satisfied before starting the scenario
**Postcondition** = condition satisfied at the end of the scenario

### Use Case
set of scenarios with common user goal
![[Screenshot 2025-03-14 at 1.28.29 PM.png|500]]
Captures a contract between the actors of a system about its behavoir
Describes the system's behavior under various conditions as it responds to a request
The primary actor initiates an interaction with the system to accomplish some goal
The system responds, protecting the interests of all the stakeholders

Scenario = sequence of steps describing an interaction between a user and a system
Use Case = set of scenarios tied together by a common user goal
- Use case similar to Class (Model)
- Scenario similar to instance of class

Key Elements:
- Actor involved
- System being used
- Functional Goal that the actor achieves using the system

Goals = must be a value to the primary actor
	As a actor type i want to do something so that some value is created

Use case Diagram and Class Diagram must be consistent

## System Design

subsystems that compose the system
![[Screenshot 2025-03-14 at 2.09.56 PM.png|400]]

Class that represents a computing component --> node
Class that represents a software component --> artifact

## Requirement doc structure
1. Overall description
2. Stakeholders
3. Context diagram and interfaces
4. Requirements
	- functional
	- non functional
	- domain
5. use case diagram
6. scenarios
7. glossary
8. system design



