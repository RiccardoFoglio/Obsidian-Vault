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