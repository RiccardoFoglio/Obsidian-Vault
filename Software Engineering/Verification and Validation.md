![[Screenshot 2025-06-17 at 2.19.03 PM.png]]

Validation
- is it the right software system?
- effectiveness
- external
- reliability

Verification
- is the software system right?
- efficiency
- internal
- correctness

![[Screenshot 2025-06-17 at 2.20.19 PM.png]]

cost of fixing the defect increases the further along in the production process you are

Failure = execution event where the software behaves in an unexpected way
Fault = feature of software that cases a failure. may be due to an error in code or incomplete/incorrect requirements
Defect = Failure or Fault
![[Screenshot 2025-06-17 at 2.22.25 PM.png|400]]

Goal of VV: 
- minimize number of defects inserted
- maximize number of defects discovered and removed
- minimize time span between insertion and discover and removal

![[Screenshot 2025-06-17 at 2.44.04 PM.png]]
![[Screenshot 2025-06-17 at 2.44.26 PM.png]]

Cost of quality: all V&V activities (development) --> Developer pays

Cost of non quality: defect fixing (operation and maintenance) --> developer pays warranty period, after the user pays

Technical Debt: define the trade off quality vs non quality
effort not spent today, will be spent later + interest

V&V Techniques: Static vs Dynamic

# Testing

process under specified conditions observing if the actual and expected result are the same

testing is a dynamic technique, requires execution of executable system or executable unit (system test and unit test)

purpose of testing process = find defects in the software products

test successful if it reveals a defect

Defect testing and debugging are different:
- Testing tries to find failures
- Debugging searches for and removes the corresponding faults

Debugging:
![[Screenshot 2025-06-17 at 3.01.56 PM.png|500]]
Testing:
![[Screenshot 2025-06-17 at 3.02.07 PM.png|500]]
Test case: certain stimulus applied to executable (system or unit) composed of name, input and expected output
with defined constraints/context (version and type of OS, DBMS, GUI...)

Test Suite = set of (related) test cases
Test Case Log = test case reference + time and date of application, actual output, result (pass/fail)
![[Screenshot 2025-06-17 at 3.02.50 PM.png]]

## Theory and Constraints

 Correctness: correct output for all possible inputs, requires exhaustive testing

Digital vs Analog
Analog: continuity hypothesis --> if bridge holds 1 ton and 2 tons, it will hold also values in between
Digital: no continuity hypothesis --> add(x,y) works for 1,1 and 1,10 doesn't guarantee it will work for values in between

Exhaustive tests: not possible, "good enough" level of confidence

Dijkstra thesis: testing can only reveal the presence of errors, never their absence

test cases: criterion of selection is evaluated by: reliability and validity

80% of the defects come from 20% of the modules, better to concentrate on the faulty modules

Weinberg's law: developer is unsuitable to test their own code.
- not emotionally willing to find defects
- can't find errors if misunderstands the problem
Testing should be carried out by a separate QA Team or Peers
### Regression Testing
![[Screenshot 2025-06-17 at 3.30.36 PM.png|400]]
Run same test again, because code changes. fixed a defect, but maybe you added new problems.

Oracle in Testing: entity that gives the expected output, must be compared to the software and see if they match
![[Screenshot 2025-06-17 at 3.32.26 PM.png]]
Oracle is based on the program specifications, in most cases it's automatic

Test Classification
- per item under test 
	- unit --> individual modules
	- integration --> some modules working together 
	- system --> all modules together (complete system)
- per approach
	- black box (functional)
	- white box (structural)
	- reliability assessment
	- risk based
- per formality --> exploratory/informal, formal

informal testing: not formalized, write an input to test if it works but then the result is not stored, it's lost
Formal testing: tests are formalized, written down and techniques are used

Coverage:
Entities considered by at least one test case / total entities
Entity depends on type of test, can be Test cases, Requirement, Function, Structural element, Statement, decision, module
 
![[Screenshot 2025-06-18 at 11.17.28 AM.png]]


# Unit Testing

Test of one independent unit: function, class and methods

- Black Box (functional): random, equivalence classes partitioning, boundary conditions
- White Box (structural): coverage of structural elements

## Black Box
### Random
extract randomly the input value for the function
![[Screenshot 2025-06-17 at 6.37.56 PM.png]]

PROs: independent of requirements
CONs: requires many test cases

### Equivalence classes Partitioning
divide input space in partitions that have similar behavior from POV of unit. Take one/two test cases per partition
Boundary conditions: boundary between partitions, take test cases on the boundary
![[Screenshot 2025-06-17 at 6.40.28 PM.png|400]]

A class corresponds to set of valid or invalid inputs for a predicate on the input variables
- if a test in a class has not success the other tests may have the same behavior
- valid: acceptable
- invalid: not acceptable

- Criterion: attribute
- Predicate: condition on attribute, defines a partition (== eq class)

![[Screenshot 2025-06-17 at 6.42.10 PM.png|500]]

Common predicates:
- single value
- interval
- boolean
- discrete set

![[Screenshot 2025-06-17 at 6.43.59 PM.png]]

Every equivalence class must be covered by one test case at least

![[Screenshot 2025-06-17 at 6.44.42 PM.png]]

## White Box

Coverage of structural elements in the code: statements, decisions, conditions, paths, loops
### Statement Coverage
Test both cases of the condition:
![[Screenshot 2025-06-18 at 11.15.21 AM.png|500]]

Statement coverage = no. statements covered / no. statements

- at least 1 test case per functional requirement
- at least 1 test case per scenario
- at least 1 test case per NFR

Transform program in control flow:
- Node = atomic instruction, decision
- Edge = transfer of control 
### <font color="#00b050">Node Coverage</font>

![[Screenshot 2025-06-18 at 11.25.16 AM.png|500]]

Node coverage = number of nodes executed / total number of notes
for each test
cumulative: for a test suite

### Decision Coverage

try to cover all decisions in the program with true and false
boolean expression
decision coverage = no. decisions covered / no. decisions

### <font color="#00b050">Edge Coverage</font>

![[Screenshot 2025-06-18 at 12.05.51 PM.png|500]]

Edge coverage implies Node coverage, but not vice versa

### Condition Coverage

a decision can be made of a combination of terms (== conditions)

![[Screenshot 2025-06-18 at 12.12.21 PM.png|500]]

- Simple condition coverage: each condition set at least once to T and F
- Multiple condition coverage: all combinations T/F are tried

![[Screenshot 2025-06-18 at 1.00.15 PM.png]]
![[Screenshot 2025-06-18 at 1.01.26 PM.png]]

### Path Coverage

Path = sequence of nodes in a graph
select test cases such that every path in the graph is visited
![[Screenshot 2025-06-18 at 1.31.43 PM.png|400]]
cycles can be an issue:
![[Screenshot 2025-06-18 at 1.31.58 PM.png|400]]

in most cases impossible --> approximations
- Path-n : path-4 --> loop 0 to 4 times in each loop
- Loop coverage : in each loop cycle 0, 1, >1 times

Loop coverage: select tests such that every loop boundary and interior is tested:
- boundary: 0 iterations
- interior: 1 iteration and >1 iterations
Coverage formula: x/3

consider each loop separately and write 3 test cases: no enter, cycle once, cycle more than once

# Integration Testing

Test of some dependent units
Unit = function, class and its methods
Some functions depend on others --> dependency graph
![[Screenshot 2025-06-18 at 2.12.19 PM.png|400]]

How to test the dependency?
- stubs
- try to eliminate the dependency using substitutes

![[Screenshot 2025-06-18 at 2.14.02 PM.png|500]]

Driver = Unit developed to pilot another unit
Stub = unit developed to substitute another unit (fake unit)
also called mock ups, mocks

Stub = substitute of module, whose result is known
substitute sensors / actuators with pure software units

Test dependencies between functions:
- two units work perfectly when isolated, defects happens when connected

Technique:
- test units in isolation first
- integrate them, test again (focus on dependency)
- in case of more units, integrate incrementally
- in any case, avoid BIG BANG integration

### Incremental Integration
Goal : add one unit a ta time, test the partial aggregate
Pro : defects found, most likely, come by last unit/interaction added
Con : more tests to write, stubs/drivers to write

Top Down
Bottom up : defined relatively to the dependency graph

![[Screenshot 2025-06-18 at 2.22.52 PM.png]]

Bottom Up
- Step 1 : f4 and f3 have no dependencies, test in isolation (unit test)
- Step 2 : test f2+f4
- Step 3 : test all

PROs : testing can start early in the development process, lower levels are directly observable
CONs : lower levels are directly observable

Top Down
- Step 1 : start on f1 (f2, f3 stubs)
- Step 2 : then f2 and f3 (stub f4)
- Step 3 : test f4

PROs : allows early detection of architectural flaws, a limited working system is available early
CONs : requires the definition of stubs for all lower-level units, sustainable only for top-down development, lower levels not directly observable

Incremental integration is a mix of top down and bottom up, trying to minimize stubs creation, compromise with availability of units

Big Bang integration: all functions developed, testing applied directly to the aggregate as if it were a single unit
	hard to locate defects

# System Testing

Test all units composing an applications
Focus on functional properties and non functional properties
Considers different platforms (development, production)
consider different players (devs / testers, end users)

Environment on which the app runs is defined by:
- OS
- database
- network
- memory
- CPU 
- libraries
- other apps installed
- other users
- ...

An element can be tested on 
- target / operation / production platform
- development platform
	- where element is developed, cannot be equal to target platform

what can change in system testing:
- who performs it --> end user, developer
- on what platform --> development, target

System test performed by end users is also called **acceptance testing** or **beta testing**

Test of functional properties: starting point is requirements document
consider usage profile:
- some functions are used a lot more than others --> test them more
