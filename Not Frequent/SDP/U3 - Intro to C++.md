
In 2022 C++ was 3rd most used programming language in the world.
In 2024 C++ less used, overcome by java and javascript

C++ Strength: performance and low level control, adaptable (Linux, Unix, Windows, MacOS...)

C++ has expanded over time and it now includes:
- Procedural --> program composed by procedures
- Imperative --> sequence of commands for the computer to execute
- Generic Programming --> algorithms are written in terms of programs to-be specified later
- Functional --> programs are constructed by composing functions
- Object-oriented --> programs are organized around data (or objects)
- Low-level Construct --> Programs allow low-level manipulation

Portability is paramount in the world of software dev.
- the ability to write code once and have it run across diverse OS is a significant advantage

C++ strength shines:
- Often associated with performance and low-level control
- adaptability across Linux, Unix, Windows, MacOS and more

C++ inherits most of C's syntax. almost always implemented as a compiled language.
C++ Project must be organized as:
- set of header files including the declarations --> \<name>.h 
- set of C++ files including the header files and implementation --> \<name>.cpp
- if a strict separation is maintained, it's possible to create a library

# Cpp Basics

I/O Streams: l/O Library defines 4 objects
- `cin` --> an istream to read from the standard input, normally the keyboard. Usually buffered (when you type input it's usually stored in a buffer until you press Enter)
- `cout` --> an ostream for standard output, normally the screen. Usually buffered
- `cerr` --> an ostream for standard error, like errors and warnings. Unbuffered, critical errors are written immediately
- `clog` --> an ostream for standard logs, general info about execution

```cpp
std::cout << expression;
```

sends expression to standar output (console). 
- We don't need the type of the objects, done automatically by compiler
Multiple << can be chained together
- `std::endl` --> predefined end of line (\n)
- `std::flush` --> flushes the buffer
- `std::ends` --> writes a null and then flushes the buffer

```cpp
std::cout << “We have " << classNum << " classes!";
std::cout << "I am " << age << " years old!" << std::endl; //new line
std::cout << “hi!” << std::endl;
std::cout << “hi!” << std::flush; std::cout << “hi!” << std::ends;
```

Console Input:
```cpp
std::cin >> expression;
```
Reads input from console & stores in variable, no need to type the object

```cpp
int age, weight; 

std::cout << “Enter age and weight: "; 
std::cin >> age >> weight; 

std::cout << “Enter age and weight: "; 
cin >> age; 
cin >> weight;

string s; 
cin >> s;

```
## Namespace
region that provides a scope to the identifiers:
- namespaces are used to organize code into logical groups
- they prevent name collisions when the code includes multiple libraries

`std::` indicates the names are defined in the namespace `std` (standard)

`using std::cin;` says that we want to use the name cin from the namespace std

Each using declaration introduces a single namespace member at the time, DONT use **using** inside header files

```cpp
using namespace std:
```
says that we want to used the namespace std, it takes all the names from the specific namespace visible without quantification

```cpp
std::cout << ... << std::endl; 
std::cin >> … 
std::cerr <<

// becomes

using namespace std; 
// must be written in all cpp files even for multi-file projects

cout << ... << endl;
cin >> ... 
cerr << ...
```

![[Screenshot 2025-03-13 at 4.37.15 PM.png]]

Variables: the extern is used as in C
![[Screenshot 2025-03-13 at 5.09.01 PM.png]]

Pointers are used as in C

## Qualifiers
- const --> used when we want to define an object we know cannot be changed, must be initialized
- volatile --> used for objects that can be changed in ways outside the control of the program (variable changed by system clock)

Auto --> compiler figures out the type of an object, must be initialized

## Range for statement

```cpp
for (declaration: expression)
	statement

vector v = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

for (auto r : v) 
	cout << r;
	// r becomes a copy of v[i]
```
# Functions

Many similarities with C, but some exceptions:
- Argument passing
- varying parameters
- overloading
- default arguments
- pointers to functions

### Argument Passing

- **By value** --> provide a value to the function parameter, local parameter is equivalent to a local variable, it holds a COPY of the parameter. Changes to the local parameter will not affect the original variable
- **By address** (pointer) {in C this was by reference} --> to pass a pointer "by value" we copy the pointer, after they are distinct, however the pointer may give indirect access to the object to which it points. By de-referencing the address, the function may access and modify the original data. Unfortunately an address can be **nullptr** thus a variable by reference can be **invalid**
- **By reference** --> pass a pointer to a verified variable. The parameter is accessed directly without de-referencing the pointer. NEVER have **nullptr** pointers
### Varying parameters

It's possible to write functions with a variable number of parameters
in C++ 2 strategies:
- if all arguments have same type, it's possible to use the library **initializer_list**
- otherwise it's possible to use a special parameter type called ellipsis

### Overloading

it's possible to have multiple definitions for the same function in the same scope.
Overloaded functions have:
- same name
- different parameter lists
- appear in the same scope region

Overloading eliminates the necessity to remind different names and is implemented by the compiler

The definitions of the functions must differ from each other for:
- type of arguments
- number of arguments
- cannot overload function declarations that differ only by the return type

### Default arguments

Functions may have parameters that have a particular value in most, but not all, calls
In those cases, we can declare that value as a default argument
	each parameter can have a single default value in a given scope
	if a parameter has a default argument, ALL the parameters that follow MUST also have a default value

### Pointers to functions

A function pointer is just like any other pointer but denotes a function
The name of a function is automatically converted into the function pointer
function pointers can be
 - passed to a function as a parameter
 - returned by a function

# Classes

in C++ we define our own data structures using classes

a class defines a type including:
- set of objects
- collection of operations related to that object

core ideas:
- data abstraction : Separate interface and implementation
	- interface specifies the operations the user can execute
	- implementation includes the data members and defines the body of functions
- encapsulation : bundling of data and methods that operate on that data into a single unit

`struct` --> heterogeneous data linked together by logical constraints. No automatic data hiding
```c++
struct product {
	int weight;
	float price;
}
```
![[Screenshot 2025-03-21 at 12.10.18 PM.png|300]]

`union` --> single memory location to access the same "bit-level" config based on different types. Overall size = largest type in a union
```c++
union mytypes_t {
	char c;
	int i;
	float f;
}
```
![[Screenshot 2025-03-21 at 12.12.06 PM.png|300]]

Version 1: struct

```c++
struct my_class { 
	int code; 
	
	int get_code() {      // inline method
		return (code); 
	} 
	
	void print_code();    // external method
}; 

void my_class::print_code () { cout << code << endl; } // method implementation
```

we can use access specifiers to enforce encapsulation
Members defined after a
- **public** specifiers are accessible to all parts of the program
- **private** specifiers are accessible to the member functions of the class but not to the ones that use the class

Version 2: explicit access specifiers

```c++
struct my_class { 
	private: 
		int code; 
	public: 
		int get_code() { 
		return (code); 
	} 
	void print_code(); 
}; 

void my_class::print_code () { cout << code << endl; }
```


Encapsulation: in C++ we often use the keyword class rather than struct, the difference is the default access level.
- Structs: objects are public by default
- Class: objects are private by default

in OOP define objects private as long as possible, methods as private and as protected when they need to be inherited by sub-classes

```c++
class my_class { 
	private: 
		int code; 
	public: 
		int get_code() { 
			return (code); 
		} 
		void print_code(); 
}; 

void my_class::print_code () { cout << code << endl; }
```

keyword "THIS" acts as a pointer to the current instance of the class, prevents naming conflicts 

```c++
class MyClass { 
	private: 
		int value; 
	public: 
		MyClass(int value) { 
			this->value = value; // allow distinguishing the parameter value                                         from the member value
		} 
		MyClass& increment() { 
			this->value++; 
			return *this;   // return *this for method chaining
		}
	...
}

int main() { MyClass obj(10); 
	// Method chaining
	obj.increment().increment();
	... 
	return 0;
```

**inheritance**: allows the generation of new classes based on existing classes. Promotes code reusability. Forms the basis for polymorphism (object of different classes can be treated as objects of a common base class)
A derived class is a class that inherits the members of a base class. It can add its own members and override the member of the base class

```C++
// Base class 
class Vehicle { 
	public: 
		string brand = "Ford"; 
		void honk() {
			 cout << "Tuut, tuut! \n" ; 
		 } 
 }; 
// Derived class 
class Car: public Vehicle {
	 public: 
		 string model = "Mustang"; 
 }
```

Class Initialization: classes are instantiated into objects. It's the only time when all data associated to the class is ever allocated in memory
Data is not shared among objects, 2 instances of the same class have different **ptr** pointers
Code is shared among objects

Classes control what happens when we operate on objects:
- **Constructed** : when they are created
- **Copied** : when we initialize a variable we pass a value by variable, we return an object by value
- **Assigned** : when we use the assignment operator
- **Destroyed** : when they cease to exist

If we don't define these operations, the compiler defines them for us
- in some cases the default doesn't behave correctly, for example when the classes allocate resources that reside outside the class object

**Primary Constructor** = special function that initializes the object when it's created.
Constructor has same names as the class, and no return
Thanks to overloading it's possible to have different primary constructors for the same object
and different primary constructors must have a different set of parameters in number and/or type
If the programmer doesn't define a primary constructor, the compiler implicitly defines a default one.
The default constructor initializes each data member as follows:
- if there is an in-class initializer it runs the initializer
- otherwise it initializes it with a default value
Primary constructor automatically invoked when an object is created

**Destructor** = unique function that is deputed to clear any internal resources handled by the object before destruction. There is only one destructor for each class
- it has the exact name of the class with a ~ before
- it has no parameters (impossible overloading)
- it is not called directly by the user (only compiler schedules its calls, there's an automatic call, one for each abandoned object)
There is no need for a destructor if the class handles only static resources, but it's needed to free dynamic memory, object descriptors etc...

**Friend Classes** : a class can allow another class to access its non-public members by making that class a friend
- this mechanism bypasses the normal access restrictions
- friendship is non-transitive and cannot be inherited
- must be used judiciously to allow certain interactions that would otherwise be impossible
A class makes a class (function) a friend by including a declaration for that class (function) preceded by the keyword friend
	Friend declarations may appear only inside a class definition
	They may appear anywhere in the class
```c++
class A { 
	... 
	friend ; 
	friend ; 
	friend ;
}
```



