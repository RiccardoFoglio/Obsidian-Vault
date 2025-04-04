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


47:26 17/3
slide 20/66 u4e2
