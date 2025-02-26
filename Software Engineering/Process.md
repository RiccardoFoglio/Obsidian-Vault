Goal: produce software with defined predictable process properties and product properties

We want the executable code, but we start from bottom: source code.
Source code too large --> several physical units (files and directories) and several logical units (functions classes packages...) --> need design first
to design the software you need to know what it should do --> requirements

- Requirements
- Architecture and Design
- Implementation
- Integrate Units

Logically each activity depends on the previous ones.
First approach is to do these activities in sequence. In practice feedbacks and recycles must be provided.
Requirements and Design are written down in documents

![[Screenshot 2025-02-26 at 3.38.37 PM.png]]

After, we do Validation and Verification activities (V&V)
- control requirements correctness
- control design correctness
- control code correctness

![[Screenshot 2025-02-26 at 3.39.56 PM.png]]

Project Management: assign work and monitor progress, estimate and control budget
Configuration Management: identify, store documents and units, keep track of relationships and history
Quality assurance: define quality goals, define how work will be done, control results

![[Screenshot 2025-02-26 at 3.44.02 PM.png]]
![[Screenshot 2025-02-26 at 3.50.59 PM.png]]

## Phases

- Development is only the first part of the game
	- operate the software (deployment)
	- modify the software (maintenance)
	- end up (retirement)
![[Screenshot 2025-02-26 at 3.52.13 PM.png]]

Maintenance: can be seen as a consequence of developments. First development usually longer, next constrained by previous ones and related choices.
Development and maintenance do the same activities (requirement, design, etc...)

Scenarios in dev / maintenance / operation
1. IT to support businesses
	- Dev : several months
	- Operation: Years
	- Maintenance: years, up to 60% of overall costs
2. Consumer software (games)
	- Dev: months
	- Op: months (weeks)
	- Virtually no maintenance
3. Operating System
	- Dev: years
	- Op: years
	- Maintenance: years, up to 60% of overall costs 

