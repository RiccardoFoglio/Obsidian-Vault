Data structures often include different types of data
in C++ we can write code whose functionalities are independent on the specific type

The problem is how to do a similar thing for our functions, classes etc...

T is an independent type

We can define function templates : formula from which we can generate type-specific versions of that function

```c++
template <typename T>
int compare (const T &v1, const T &v2) {
	if (v1 < v2)
		return -1;
	if (v2 < v1)
		return 1;
	return 0;
}
```

When you call a template function the compiler deduces what type to use instead of the template parameters and **instantiates** (generates) a function with the correct types from the templates

Templates can be used also with personal types: templates usually put some requirements on the argument types

The compiler must always be able to understand to which type T is referring to, possible to set the type during the call

```c++
cout << compare<int>(1, 0) << endl; 

string s1 = "hello"; 
string s2 = "world"; 
cout << compare<string>(s1, s2) << endl;
```

A function template can receive more than one type: differentiation must make sense and operation must be feasible

Template function can also have a template type, to return a variable type

```c++
template <typename T> 
T compare(const T &v1, const T &v2) { 
	if (v1 < v2) 
		return -1; 
	if (v2 < v1) 
		return 1; 
	return 0; 
}

template <typename T1, typename T2> 
T3 compare(const T1 &v1, const T2 &v2) { 
	if (v1 < v2) { return -1; } 
	if (v2 < v1) { return 1; } 
	return 0; 
}
```

Parameters can be passed by value, pointer or reference

Limitations:
- make sure the type we pass is compatible with the different types we expect

Classes can also be programmed to deal with generic data types
- definition of a class template is similar to the definition of a function template
- the main difference is that the compiler cannot deduce the type of T

Templates don't have a proper implementation, compiler will take care of it, no source code to compile
Thus, given a template, we must understand where to insert its declaration and implementation

Approach 1
![[Screenshot 2025-04-09 at 4.51.31 PM.png|500]]

Approach 2
![[Screenshot 2025-04-09 at 4.51.56 PM.png|500]]

Approach 3
![[Screenshot 2025-04-09 at 4.52.23 PM.png|500]]

Approach 4
![[Screenshot 2025-04-09 at 4.52.39 PM.png|500]]