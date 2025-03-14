Unified Modeling Language
Standardized by OMG
Several diagrams:
- Class diagrams
- Activity diagrams
- Use Case diagrams

## Class Diagrams

- class
- instance/object
- attribute
- operation
- association

Object = model of item characterized by identity, attributes, operations and messages

Class = describe a set of objects: common properties, autonomous existence, facts things people etc...
An instance of a class is an object of the type that the class represents

Attribute = elementary property of classes (name, type)
An attribute associates to each object a value of the corresponding type
	Name : string
	ID : Numeric
	Salary : Currency

Usage of class diagrams:
- Model of concepts (glossary)
- Model of system (hw + sw) == system design
- Model of software classes (software design)

Before doing a class diagram: DECIDE WHAT U WANT TO MODEL

Where to look for:
- Physical entities : Person, Car
- Roles : Employee, Director, Doctor, Student
- Social / Legal / Organizational entities : University, Company
- Events: Sale, Order, Request, Claim, Call, Payment
- Time Intervals: Car Rental, Booking, Course, Meeting
- Geographical Entities: City, Road, Nation
- Reports, Summaries : Weather Report, Bank Account Statement

Link = model of property between objects

Association = represent set of links between objects of different classes or pairs of objects (one per class)
![[Screenshot 2025-03-14 at 2.41.57 PM.png|400]]

![[Screenshot 2025-03-14 at 2.42.27 PM.png|400]]

Style suggestions:
- Class names : singular noun
- Association name: verb
- Attributes : type of attribute not needed in conceptual model

Multiplicity: describe the maximum and minimum number of links in which an object of a class can participate
should be specified for each class participating in an association

![[Screenshot 2025-03-14 at 2.45.43 PM.png|500]]

Typically only used: 0, 1 or * (many)
- minimum: 0 or 1
- maximum: 1 or *

Associations:
- Aggregation : B is part of A, or A has B
- Composition : aggregation where the link is more strict: lifecycle of both classes is the same (if person disappears, so the corresponding objects Leg Hand Head)
- Association : allows to attach attributes to the association, link between two objects includes 2 linked objects and attributes of the link 
- Specialization / Generalization : B Specializes A means that objects described by B have the same properties of objects described by A and objects described by B may have additional properties
![[Screenshot 2025-03-14 at 3.06.02 PM.png|400]]

Class one above = parent class
Class one below = child class
class one or more above = Superclass, Ancestor class, Base class
Class one or more below = Subclass, Descendent class, Derived class

subclass inherits all attributes and all associations of all ancestors

DOs in Class Diagram
- Decide the goal of the model

DONT
- use plurals for classes
- forget multiplicities
- forget roles / association classes (when needed)

![[Screenshot 2025-03-14 at 3.11.01 PM.png|500]]