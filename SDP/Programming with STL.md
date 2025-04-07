instead of defining each operation as part of a container, the standard library defines a set of generic algorithms
- Generic = they operate on elements of different type
- Algorithms = they implement classical procedures, like sorting, searching etc...

4 headers: Algorithm, numeric, memory, cstdlib. Algorithm defines the most relevant parts to search, sort, create permutations or partitions, manipulating sets etc...

Essential to understand the structure of the algorithms rather than memorize the details
They perform an operation on a range of elements and a predicate
- range can be specified using pointers and any appropriate iterator type
- predicate is an expression that can be called and returns a value
	- value usually adopted to express a condition
	- default versions of the algorithm often use a standard predicate
	- extended version usually supplies a user predicate operator

(Search, Binary Search, Sort, Permutations, Set Algorithms in slides u4s4)

Predicates:
- Standard
- Implemented through an external function

```c++
bool isEven(int x) {
	return (x % 2) == 0;
}
...
auto it = std::find_if(numbers.begin(), numbers.end(), isEven);
```

In general a predicate can be any callable object, but it can be ambiguous to write, so we use Lambda expressions

### Lambda Expressions

concise way to define predicates inlline within the code
- conciseness
- readability
- flexibility

```c++
// standard practice
bool isEven(int x) {
	return (x % 2) == 0;
}

// predicate implemented as lambda expression
auto isEvenLambda = [] (int x) {return (x%2)==0; };
auto it = std::find_if(numbers.begin(), numbers.end(), isEvenLambda);

// lambda expression defined directly within the algorithm call
auto it = std::find_if(numbers.begin(), numbers.end(), 
	[](int x){return (x%2)==0;} 
);
```

Lambda expressions
```c++
[capture_list] (parameter_list) --> return_type {body}
```

callable unit of code, it can be thought of as an unnamed inline function
- Like any other function, a lambda has a parameter list, a return type and a function body
- Unlike any other function a lambda may be defined inside a function and internal function has a capture list

Capture list : local variables that can be used in the lambda function
![[Screenshot 2025-04-07 at 5.19.44 PM.png|500]]

Parameter list : comma-separated list of function parameters used in the body. Set of arguments used to initialize the lambda's parameters
Arguments and parameters types must match : a lambda may not have default arguments

Return Type : specifies the type of the object the function returns

if the body of a lambda includes:
- only a return statement, the type of the lambda expression is deduced by the return statement
- any statement other than a return, that lambda is supposed to return void
- in all other cases, we need to define a return type using a trailing return type

Body : includes the function body, the implementation

```c++
[](const string &a, const string &b) { return a.size() < b.size(); }

stable_sort (
	words.begin(), 
	words,end(), 
	[](const string &a, const string &b) { return a.size() < b.size(); } );

std::vector v = {3, 4, 1, 2}; 
std::sort(
	v.begin(), 
	v.end(), 
	[](unsigned lhs, unsigned rhs) {return lhs > rhs;}
);
// v is now {4, 3, 2, 1}
```

26/3 + 7/4