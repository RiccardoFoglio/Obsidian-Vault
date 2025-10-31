Multi person construction of multi version software
- issues in communication
- issues in coordination
- issues in maintenance over many years

Software = collection of computer programs, procedures, rules, associated documentation, data
Software is not only code

Software Types:
- Stand Alone
- Embedded in business process (enterprise)
- Embedded in production process (enterprise, factory)

Software Criticality:
- Safety critical: harms people or environment (self driving cars, embedded software)
- Mission critical: harms business (banking, finance, retail)
- Other

Embedded software can have a direct effect on real world, and be safety critical
Stand alone software doesn't have (usually) a direct effect on real world and it's not safety critical

Beware:
- Software is not free
- Software changes
- Software is not perfect
- Software is complex

## Process and Product

Process = activities, people, tools
Products = documents, data, code
quality of the product depends on the quality of the process (garbage in, garbage out)

- Design product --> Software product
- Design factory --> Software process
- Manufacture product --> Deployment and Delivery
- Maintain product --> Evolution and Maintenance

Software product: functional and non functional properties
- Functional : do something
- Non Functional : 
	- **correctness** -->  Capability of the product to provide the intended functionality in all cases
	- **reliability/availability** --> defects visible by end user per time period / probability of defect over a time period. Percentage of time the product is / isn't available to end user
	- **Security** --> protection from malicious access / access only to authorized users
	- **Safety** --> absence of harm to people, and of hazardous situation 
	- **Dependability** = safety + security + reliability 
	- **Usability** --> effort needed to learn using the product, satisfaction expressed by user, existence of functions needed by user
	- **Efficiency** --> response time, memory, CPU, Bandwidth, Energy used...
	- **Maintainability** --> effort needed to add/modify/cancel software function, effort to fix defect, effort to deploy on a different platform (DB, OS...)

![[Screenshot 2025-02-25 at 11.43.00 AM.png]]

Non functional properties are difficult to engineer and verify, are often forgotten, make the difference between competing products

Functional and non functional properties should be clearly defined before starting the development, engineered in the product and verified during and after the development, before deployment

## Software Processes

Requirements --> Design --> Coding --> Testing

- Cost: money
- Effort: person hours
- Punctuality: delivery dates
- Conformance (to standards, norms etc...)

Classical Workbench: repository + version and configuration management tools
- Git
- Subversion
- Polarion
- Clearcase
- Team Foundation Server

Design:
- Component diagram
- Package diagram
- Class diagram
- Interaction diagram

Test: 
- Unit test, white box
- Unit test, black box

### Software Engineering Laws
- Requirements deficiencies are the prime source of project failures
- Requirements and design cause the majority of defects
- Defects from requirements and design are more expensive to fix
- Modularity, hierarchical structures allow to manage complexity
- Reuse guarantees higher quality and lower cost
- Good designs require deep application domain knowledge
- Testing can show the presence of defects, not their absence
- A developer is unsuited to test his/her code
- A system that is used will be changed
- An evolving system will increase its complexity, unless work is done to reduce it (architecture erosion, requirements creep, refactoring)
- Developer productivity varies considerably
- Development effort is a (more than linear) function of size
- Adding resources to a late project makes it later
- The process should be adapted to the project

Information System Laws:
- Conway's law : Structure of a system produced by an organization mirrors the communication structure of the organization:
	- Applied to organizational structure: the structure should mirror how members work together
	- Applied to information systems: IS parts will reflect the organizational structure
	- Applied to software projects: module interfaces will reflect the team's structure

### Principles

KISS : Keep It Simple, Stupid
Never add accidental complexity to essential complexity

Given a large difficult problem, try to split it in many (independent) parts (concerns) and consider a part at a time --> divide and conquer approach

Separation of concerns: divide a complex system in modules, with high cohesion and low coupling
![[Screenshot 2025-02-25 at 3.02.22 PM.png|500]]

in Software Engineering (SE):
- Software process: concentrate on what the system should do, then on how, then do it
- Design: split code in (independent) components
- Programming languages: separate error handling and error generation, separate data and function definition

Abstraction
- given a difficult problem or system, extract a simpler view of it, avoiding unneeded details, then reason on the abstract view (model)
![[Screenshot 2025-02-25 at 3.04.28 PM.png|500]]

