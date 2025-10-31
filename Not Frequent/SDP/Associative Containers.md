Associative containers support lookup and retrieval by a key

The 3 primary associative containers are:
- MAPS, whose elements are pairs key-value
- SETS, whose elements are just keys

Each associative container is either a map or a set, requires unique keys or allows multiple keys and stores elements in order or not

![[Screenshot 2025-04-06 at 6.42.15 PM.png|500]]

Main Operations
![[Screenshot 2025-04-06 at 6.42.47 PM.png|500]]

## Maps

Maps are associative containers consisting of pairs key-value
- in maps, keys are sorted
- in unordered maps, there is no order among keys

Complexity for random access, search, insertion and removal is
- $O(\log N)$ for maps --> internally are a tree
- $O(1)$ for unordered maps --> internally are hash-table using hash-function

Defined in the header **map** or **unordered_map**
Maps and unordered maps have a very similar user interface
Keys are required to be **unique**
To check if a key exists, we may use the function **count**

```c++

#include map 

map<string, int>mymap; // Empty map 

// Map with three elements 

map<string, string> authors = { 
								{″Joyce″,″James″}, 
								{″Austen″,″Jane″}, 
								{″Dickens″,″Charles″}
							};


// insertions
map<string, size_t> word_count;

// equivalent insertions
word_count.insert({″this″,1});
word_count.insert(make_pair{″this″,1});
word_count.insert(pair<string, size_t>(″this″,1)); 
word_count.insert(map<string, size_t>::value_type(″this″,1));

```

![[Screenshot 2025-04-06 at 7.04.39 PM.png|500]]

## Sets

A set is simply a collection of keys
They are useful when we want to know whether a value is present
- sets are usually implemented using trees and traversed using iterators
- unordered sets are often implemented using hash tables

```c++
#include <set> 
set<int> is = {0,1,2,3,4,5,6,7,8,9}; 
auto it1 = is.find(1);  // Returns an iterator to element with key=1 
auto it2 = is.find(11); // Returns an iterator to is.end()
auto n1 = is.count(1);  // Returns 1 
auto n2 = is.count(11); // Returns 0

set<int> myset; 
vector iv = {2,4,6,8,2,4,6,8,2,4,6,8};

myset.insert(iv.begin(),iv.end()); 
// myet includes 4 elements {2,4,6,8}  !! UNIQUE !!

myset.insert({1,3,5,7,1,3,5,7});   
// myet now includes 8 elements {1,2,3,4,5,6,7}

// iterators on sets
set<int> is = {0,1,2,3,4,5,6,7,8,9}; 
set<int>::iterator it = is.begin(); 
while (it |= is.end()) {
	cout << *it << endl; it++; 
}
```


