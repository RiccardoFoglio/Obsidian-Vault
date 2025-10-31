![[Screenshot 2025-04-30 at 11.49.04 AM.png|500]]

How much?
When will it be ready?
Who does what?

Activity = time passed by resource to performe defined, coherent task
Phase = set of activities

Milestone = key event or condition in the project with effects on subsequent activities

Deliverable = product in the process. Internal (for producer) or external (for customer)

## Measures

Process measures : time, effort, cost, productivity, earned value, fault, failure, change

Product measures : functionality (FP), size, price, modularity

![[Screenshot 2025-04-30 at 2.40.53 PM.png|400]]

absolute time: 1 calendar month absolute --> start august 1st but all in vacation until august 31, will end on September 30.

Effort : time take by a resource to complete a task. Depends on duration and number or resources available
$$duration \cdot resource$$
measured in person hours
- 1 person works 6h --> 6 ph
- 2 people work 3h --> 6 ph

typical conversions (depend on companies and nations): 
- 1 person day = 7 ph
- 1 person week = 35 ph
- 1 person month = 140 ph
- 1 person year = 1680 ph

Effort can be covered in cost, knowing cost per ph

Costs : personnel, staff, hardware, software

Total Cost of Ownership (TCO) = considers the complete time window involving the product: before acquisition, usage and dismissal
The longer timeframe, the less important the acquisition cost

Cost = cost for the vendor
Price = cost for the customer --> Price = Cost + Margin

Size of source code --> in LOC (lines of code)
Size of documents --> number of pages, words, characters...
Size of tests --> N test cases

Productivity : Output / Effort
in software: LOC / Effort or Functionality / Effort or Object Points / Effort

- LOC / Effort : lower level the language, the more productive the programmer, the more verbose the programmer, the higher productivity --> Problems

![[Screenshot 2025-04-30 at 4.01.41 PM.png|500]]

PM Activities
![[Screenshot 2025-04-30 at 4.18.19 PM.png]]

Inception = initial analysis of requirements, general architecture, estimate of duration and cost, commercial proposal

Vendor tends to overpromise to gain the contract --> counting on variations to be made later, counting on gains in maintenance or operation phase

## Estimation
![[Screenshot 2025-04-30 at 4.20.23 PM.png]]

Parkinson's Law : project costs whatever resources are available
- no overspend
- probability that customer gets system they want is small

By decomposition : easier to estimate smaller parts

WBS : Work Breakdown Structure --> hierarchical decomposition of activities in subactivities

PBS : Product Breakdown Structure --> hierarchical decomposition of product

Gantt chart
![[Screenshot 2025-04-30 at 4.32.08 PM.png]]
![[Screenshot 2025-04-30 at 4.32.23 PM.png]]

Estimation by size and productivity
1. estimate total size of project (LOC)
2. apply productivity rate P to obtain estimated effort
3. apply cost rate to obtain estimated cost

![[Screenshot 2025-04-30 at 4.42.41 PM.png|500]]

4. estimate calendar duration


Function Points : a metric for software project estimation. used to estimate time, cost and effort in software dev.
Independent of programming language
$$fp = A*EI + B*EO + C*EQ+D*EIF+E*ILF$$
- External Inputs (EIs) = processes that data comes into the system
- External Outputs (EOs) - Data outputs from the system.
- External Inquiries (EQs) - Interactive transactions made by users.
- External Interface Files (EIFs) - External systems data used by the application.
- Internal Logical Files (ILFs) - User identifiable groups of logically related data.

Procedure: 
1. identify and classify components as ILFs, EIFs, EIs etc...
2. assign each component a complexity weight (Low Average, High)
3. use weighting tables to allocate points based on complexity
4. sum all the points to get the total Function Points

Technical Complexity Factor (TCF) : adjusts the Function Points to reflect the technical complexity and the technological challenges encountered in the project
It accounts for computational performance, reusability and data processing needs

- Data Processing Complexity: Impacts of extensive data management and manipulation.
- Performance Requirements: The need for high-performance metrics under specific constraints.
- End-User Efficiency: Focus on user interface and experience optimizations.
- Complex Processing: The requirement for complex algorithms or computing processes.
- Reusability: Designing software components that can be reused in other applications.
- Installation Ease: Complexity involved in software deployment and installation.

Environmental Complexity Factor (ECF) : adjusts the Function Points to consider the project’s environmental variables like team experience, working conditions, and project continuity.
It reflects the non-technical influences that can affect the project timelines and efficiency

- Team Experience: Level of experience the development team has with similar projects.
- Application Experience: Familiarity with the technology and the application domain.
- Team Motivation: Impact of team morale and motivation on the project’s progress.
- Working Conditions: The work environment's adequacy for efficient project execution.
- Required Software Tools: Availability and adequacy of tools to support the project development.

# PM Approaches

- waterfall --> requirements give a project plan
- iterations --> dynamic requirements

![[Screenshot 2025-05-07 at 5.01.11 PM.png|600]]

