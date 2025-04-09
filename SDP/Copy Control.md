When a C++ class is defined, we implicitly or explicitly specify what happens when the class is copied, moved, assigned, and destroyed

A class controls these operations with 5 special class member functions, called copy control functions

If we don't write them, the compiler creates them automatically, but sometimes they are a mess

Copy control is performed by:
- **Primary constructor** : initializes the object when it's created
- **Copy and Move constructors** : define the behavior when an object is initialized from another object
- **Copy and Move Assignment Operators** : define the behavior when we assign an object to another
- **Destructor** : defines the behavior when an object ceases to exist

![[Screenshot 2025-04-09 at 4.58.59 PM.png]]

# Copying
## Copy Constructor 

special constructor to create a new object as a copy of an existing object of the same class

given a class C, a copy constructor has
- same name of the class
- argument of type C& or const C& (preferred)
- possible additional parameters with default values

```c++
class Foo { 
	public: 
		Foo ();             // Primary constructor
		Foo (const Foo&);   // Copy constructor 
}
```

It's called in the following situations:
- creating a new object using existing object
```c++
MyClass obj2 = obj1;
```
- object passed by value to a function
```c++
void foo(MyClass obj);
...
foo(obj1);
```
- object returned from a function by value
```c++
MyClass foo(){
	MyClass temp; return temp;
}
...
MyClass my = foo();
```
## Copy Assignment Operator

special member function used to assigned the contents of one object of the same class

given a class C, a copy assignment operator has
- the name operator=
- an argument of type C& or const C& (preferred)
- a return type (usually a C&)

```c++
class Foo {
	public:
		Foo& operator= (const Foo&);
}
```

Called in the following situations:
- when the assignment operator (=) to assign one object to another (both already exist)
```c++
obj2 = obj1;
```
- returning from a function by value and then assigning
```c++
Myclass foo1;
foo2 = foo1;
```


Rule of Three is a guideline:
- if you define any of the following three special member functions in a class, you should define all three
	- Destructor  `˜C()`
	- Copy constructor `C(Const C&...)`
	- Copy Assignment Operator `operator= (const C&...)`

Defining these three functions ensures your class can correctly manage resources when objects are copied, assigned or destroyed
# Moving

Moving Semantic: moving instead of copying may enhance performance
## Move Constructor

A move constructor is a special member function that transfers ownership of resources from a temporary object to a new object, avoiding unnecessary data copying

Given a class C, the move constructor has
- the name C
- an argument of type C&&
- the noexcept keyword added to indicate that the constructor never throws an exception

```c++
class Foo { 
	public: 
		Foo (Foo&&) noexcept { ... };
}
```

Called in the following situations:
- temporary object initialization
```c++
MyClass obj2 = MyClass(obj1);
```
- container operations with temporaries
```c++
std::vector vec; 
vec.push_back(MyClass());
```
- Container reallocations
```c++
std::vector vec(10); 
vec.resize(100);
```
- Explicit std::move usage
```c++
MyClass obj1; 
MyClass obj2 = std::move(obj1);
```
- return value
```c++
MyClass createObj() { 
	MyClass tmp; 
	return tmp; 
}
```

The automatic generation occurs only if there are no user-declared copy operations, destructor or move operations.
It optimizes resource management by enabling efficient transfer of object
	particularly when working with resource-heavy objects and standard library containers
## Move Assignment

transfers resources from a temporary object to an existing object, optimizing performance by avoiding unnecessary copies

Given a class C, the destructor has:
- the name operator= of type C&
- an argument of type C&&
- the noexcept keyword added to indicate that the constructor never throws an exception

```c++
class Foo {
	public:
		Foo& operator= (Foo&& in) noexcept {...}
}
```


Called in the following situations:
- Explicit std::move usage
```c++
MyClass obj1, obj2; 
obj2 = std::move(obj1);
```
- Assigning temporaries
```c++
MyClass obj; 
obj = MyClass();
```
- Container reallocations
```c++
std::vector vec(5); 
vec[0] = MyClass();
```
- Returning local objects
```c++
MyClass createObj() { 
	MyClass tmp; 
	return tmp;
}
```


Rule of Five extends the Rule of Three, adding the Move Constructor and Move Assignment Operator



