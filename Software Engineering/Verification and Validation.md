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
	- black box (functional) --> 
	- white box (structural) --> 
	- reliability assessment --> 
	- risk based --> 
- per formality --> exploratory/informal, formal

informal testing: not formalized, write an input to test if it works but then the result is not stored, it's lost
Formal testing: tests are formalized, written down and techniques are used

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

