in C++ we can use C-like arrays
- integer arrays
- character arrays
- strings = special type of character arrays (with \0 ending)
- pointers used to manage all sorts of C-like arrays
- multidimentional arrays can be defined

c-like strings are not a type, but a convention. they are arrays of characters so must use strcpy, strcmp etc... many manual things

C++ Containers: object that 
- stores other objects
- manages the storage space
- provides member functions for accessing elements
- guarantees the complexity of all operations

![[Screenshot 2025-04-04 at 6.55.13 PM.png|500]]

Sequential containers provide fast sequential access to their element
They offer different performance to
- add or delete an element
- perform non-sequential access
- with the exception of arrays, they provide efficient and flexible memory management

Standard sequential containers available in C++
![[Screenshot 2025-04-04 at 6.57.34 PM.png|600]]

Vector and Strings : 
- hold their elements in contiguous memory cells
- fast access given an index
- expensive to add or remove elements in the middle
- adding new element may require reallocation to a new storage area

Lists and forward_lists:
- efficient to add or remove an element anywhere
- don't support random access
- memory overhead is substantial when compared to other containers

Deques:
- more complicated
- like strings and vectors
- fast for adding or removing elements at either end of the queue

Arrays
- similar to vectors
- fixed size
- added with C++11 for efficiency reasons


Use containers whenever possible. When in doubt use a vector
- small elements and memory matter = don't use list or forward list
- random access = use vector or deque
- insert or delete elements in middle = use list or forward list
- insert or delete elements at the front and back = deque


```C++

#include <vector>
#include <list>
... 

using std::vector; 
using std::list 

... 

vector<int> v1(10);           // 10 values equal to 0 
vector<int> v2{10};           // One element with value 10
vector<int> v3(10, 1);        // 10 values equal to 1 
vector<float> v4{10.5, 1.6};  // Two elements: 10.5 and 1.6

vector<string> vs1{‟a″, ‟b″, ‟d″}; 
vector<string> vs2{‟a″, ‟b″, ‟d″};        // List initialization
vector<string> vs3(‟a″, ‟b″, ‟d″);        // Error


list<float> myconst = {3.14159, 2.71828, 9.80655}; 
list<float> authors = {″Leopardi″, ″Manzoni″, ″Verga″};

deque<string> sd(10); // 10 empty strings 
array<int, 10> ia; // 10 integer (default initializer)

array<int, 3> a1{ {1, 2, 3} }; // Double-braces required before C++11 
array<int, 10> a2 = {1,2,3}; // double braces not required after C++11 
                    // 3 integers list intializer with he values 1, 2, 3

array<string, 2> a3 = { string("a"), "b" }; 

vector<vector<int>> vvi = {{1,2,3},{4,5,6}}; 
vector<vector<string>> vvs = {{″one″,″two″} {″three″,″four″}};
```

Assignments:

```c++
vector vs1(10); // vector with 10 elements
vector vs2(20); // vector with 20 elements

swap(sv1,sv2);  // Now sv1 containes 20 elements and sv2 10
```

```c++
vector v1 = {1, 3, 5}; 
vector v2 = {1, 3, 5, 7}; 
vector v3 = {1, 3, 5, 9}; 
vector v4 = {1, 3, 5}; 

if (v1 == v4) ... // True 
if (v1 < v2)  ... // True 
if (v2 < v3)  ... // True
```

Adding Elements:
```c++
struct student_t { 
	int rn; 
	string last_name, first_name; 
	int mark; 
} myt; 
vector<student_t> sv;

sv.push_back(student_t(123456, ″Potter″, ″Harry″, 28)); // insert the type
sv.emplace_back(123456, ″Wisley″, ″Ronald″, 26);        // done automatically

sv.push_back(123457, ″Granger″, ″Hermione″, 30);


list<string> sl;
string word;

while (cin>>word)
	sl.push_back(word);

while (cin>>word)
	sl.push_front(word)

sl.insert(sl.begin(), "Start");
```

Accessing Elements:

```c++

deque id;
... 

if (!id.empty()) { 
	id.front() = 10; // assign 10 to the first element
	auto &v1 = id.back(); // v1 is a reference 
	v1 = 100; // change element value 
	
	auto v2 = id.back(); // v2 s a copy 
	v2 = 1000; // does not change the value of the element in c
 }
```

Erasing Elements:

```c++
list lst = {0,1,2,3,4,5,6,7,8,9}; 
while (!lst.empty()) { 
	... Manipulate lst.front() ... 
	lst.pop_front(); 
}

list lst = {0,1,2,3,4,5,6,7,8,9}; 
auto it = lst.begin(); 
while (it != lst.end()) { 
	if (*it % 2 == 0) // If the element is odd 
		it = lst.erase(it); // erate it 
	else 
		it++; // othewise move to the next element
}

lst.clear (); // Erase all elements
lst.erase(lst.begin(), lst.end()); // Equivalent instruction
```







24-25-26/3 videos