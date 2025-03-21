File = sequence of bytes written one after the other
Each byte = 8 bit with binary value 0 or 1
All files are binary
Normally Text files are ASCII or UNICODE (C sources, C++, Java, Perl etc..)
and binary files are executable, word, excel...

![[Screenshot 2025-03-21 at 3.23.39 PM.png|400]]

Files consisting of data encoded in ASCII (or UNICODE) --> sequence of binary values which encode ASCII Symbols

- Newline: go to the next line
	- UNIX/LINUX/MacOS : 1 character, LineFeed $10_{10}$ 
	- Windows: 2 characters, LineFeed $10_{10}$ + Carriage Return $13_{10}$ 

In C++ the headers files:
- **iostream** : provides the basic IO functionalities for standard streams 
- **sstream** : provides classes for string stream operations
- **fstream** : provides classes for file IO operations

When we perform IO an error can occur
```c++
// instead of:
cin >> s;

// do this:
if (cin >> s) { ... success ...}
```

### fstream library

Defines the types used to read and write named files
These types provide the same operations as those we have used on the objects **cin** and **cout**
- **ifstream** --> to read from a given file
- **ofstream** --> to write to given file
- **fstream** --> to read or write a given file

support languages that use wide characters with `wchart_t`

IO classes also define functions and flags to check correctness of stream:
![[Screenshot 2025-03-21 at 3.42.29 PM.png]]
![[Screenshot 2025-03-21 at 3.43.13 PM.png]]
Modes:
![[Screenshot 2025-03-21 at 3.45.39 PM.png]]


```c++
// Define an output stream but 
// it does not associate a file to it 
ofstream out; 

// Define out and open the file 
// The modes out and trunc are implicit 
ofstream out(″myfile″);

// Mode trunc is implicit 
ofstream out(″myfile″, ofstream::out); 
ofstream out(″myfile″, ofstream::out | ofstream::trunc);
ofstream out(″myfile″, ofstream::out | ofstream::app);

// Mode out is implicit; append new content to the file
ofstream out(″myfile″, ofstream::app);
```

Output Allignment:
- `std::setw(n)` sets the width of the next output field to n characters 
- `std::left` left-aligns the output within the field.
- `std::right` right-alignes the output within the field
- `std::internal` aligns the sign or base indicator to the left and the value to the right, padding the space in between

Open a file in reading mode:
```c++

#include <iostream>
#include <fstream>
using std::cout; 
using std::cerr; 
using std::endl; 
using std::string;
using std::ifstream;

int main() { 
	string filename("input.txt"); 
	int number;
	ifstream infile(filename);
	if (!infile.is_open()) { 
		cerr << “Error: " << filename << endl; return EXIT_FAILURE; 
	} ... 
	infile.close(); 
	return EXIT_SUCCESS;
}



for (auto p=argv_1; p!=argv+argc: ++p){
	ifstream input (*p);
	if (input) {
		... process file ...
	} else {
		cerr << "error opening file!" << endl;
	}
}


// read integer values
int number;
cout << unitbuf; // all writes are flushed immediatelly
while (infile >> number){
	cout << number << "; ";	
}
cout << endl;


// read single characters
while (infile.get(c)) {
	cout << c << "; ";
}
cout << endl;



// read line by line
string s; 
while (infile >> s) { 
	cout << s << "; "; 
} 
cout << endl;


// read entire file lines
string s; 
while (getline(infile,s)) { 
	cout << s << "; "; 
}
cout << endl;
```


Binary files: sequence of 0 and 1 not "byte-oriented"
Smallest unit that we can read/write is the bit
	not easy to manage single bites
	sequence of 8 bits doesn't necessarily correspond to printable characters

Why are they used?
	Compactness --> 100000 6 bytes, but in binary it's 4 bytes

fstream library allows manipulation of binary files
- to ensure file is treated in binary mode use **`ios::binary`** when opening it
- To store data in binary file, use function `write()`
- to read data from binary file use `read()`



Random Walk (lezione monday)

