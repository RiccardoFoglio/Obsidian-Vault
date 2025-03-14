
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

