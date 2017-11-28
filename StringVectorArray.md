- arrays are less convenient to use than the library string and vector types
- Each using declaration introduces a single namespace member.
---
### string
- ways to initialize strings:
 1. string s1;
 2. string s1(s2);
 3. string s2=s1;
 4. string s3("value");
 5. string s3="value";
 6. string s4(n,'c');

- getline: reads the given stream up to and including the first newline and stores what it read—not including the newline—in its
string argument. If the first character in the input is a newline,
then the resulting string is the empty string.
- **empty** function: It returns a bool indicating whether the string is empty
```
while (getline(cin, line))
    if (!line.empty())
      cout << line << endl;
```
- **size** function: size member returns the length of a string
```
while (getline(cin, line))
    if (line.size() > 20)
      cout << line << endl;
```
size returns a string::size_type value. Under the new standard, we can ask the compiler to provide the appropriate type by using auto or decltype
```
auto len = line.size(); // len has type string::size_type
```
**tip**: because size returns an unsigned type, not using ints in expressions that use size().

- Comparing strings
  1. If two strings have different lengths and if every character in the shorter string is equal to the corresponding character of the longer string, then the shorter string is less than the longer one.
  2. If any characters at corresponding positions in the two strings differ, then the result of the string comparison is the result of comparing the first character at which the strings differ.
- Dealing with the Characters in a string
![tu](http://ouewqfsxw.bkt.clouddn.com/stringcharacter.PNG)

- the names defined in the cname headers are defined inside the std namespace, whereas those defined in the .h versions are not.

- question: is the string introduce twice
```
#include <string>
using std::string;
```
- Pointers and Multidimensional Arrays
```
int ia[3][4]; // array of size 3; each element is an array of ints of size 4
int (*p)[4] = ia; // p points to an array of four ints
p = &ia[2]; // p now points to the last element in ia
```
```
int *ip[4]; // array of pointers to int
int (*ip)[4]; // pointer to an array of four ints
```
- With the advent of the new standard, we can often avoid having to write the type
of a pointer into an array by using auto or decltype
```
// print the value of each element in ia, with each inner array on its own line
// p points to an array of four ints
for (auto p = ia; p != ia + 3; ++p) {
// q points to the first element of an array of four ints; that is, q points to an int
for (auto q = *p; q != *p + 4; ++q)
cout << *q << ' ';
cout << endl;
}
```
```
// p points to the first array in ia
for (auto p = begin(ia); p != end(ia); ++p) {
// q points to the first element in an inner array
    for (auto q = begin(*p); q != end(*p); ++q)
      cout << *q << ' '; // prints the int value to which q points
      cout << endl;
    }
```
- the library classes should be used in preference to lowlevel
array and pointer alternatives built into the language
- **->** operator Arrow operator. Combines the operations of dereference and dot
operators: a->b is a synonym for (*a).b.

