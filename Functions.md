- In C++, functions can be overloaded, which means that we can use the same name for several different functions.
- the return type may not be an array type or a function type. However, a function may return a pointer to an array or a function.
- Objects defined outside any function exist throughout the program’s execution. Such
objects are created when the program starts and are not destroyed until the program
ends.
- Local **statics** are not destroyed when a function ends; they are destroyed when the program terminates.
- parameter names are often omitted in a declaration. Although parameter names are not required, they can be used to help users of the function understand what the function does
- The header that declares a function should be included in the source file that
defines that function.s are also known as the function prototype.
- Despite appearances, its parameter list doesn’t
differ from the list in the first version of fcn.
```
void fcn(const int i) { /* fcn can read but not write to i */ }
void fcn(int i) { /* . . . */ } // error: redefines fcn(int)
```
- reference
```
const int &r2 = 42; // ok
int &r4 = 42; // error: can't initialize a plain reference from a literal
```
- swap two int pointers
```
void exchange(int *&a,int *&b){
	auto c=a;
	a = b;
	b = c;
}
int main()
{
	int a = 10, b = 5;
	int *a1 = &a, *b1 = &b;
	exchange(a1, b1);
	cout << *a1<<" "<<*b1 << endl;
	system("pause");
	return 0;
}
```
- the elements in an initializer_list are always const values
- Functions that return **void** are not required to contain a **return**. In a **void** function, an implicit **return** takes place after the function’s last statement.
- A function with a **void** return type may use the second form of the return statement only to return the result of calling another function that returns **void**.
- Calls to functions that return references are lvalues; other return types yield rvalues.
- Three ways to Declaring a Function That Returns a Pointer(reference) to an Array
```
//1. type alias
typedef int arrT[10]; // arrT is a synonym for the type array of ten ints
using arrtT = int[10]; // equivalent declaration of arrT; see § 2.5.1 (p. 68)
arrT* func(int i); // func returns a pointer to an array of five ints
//2. Trailing Return Type
auto func(int i) -> int(*)[10];
//3. Using decltype
int odd[] = {1,3,5,7,9};
int even[] = {0,2,4,6,8};
decltype(odd) *arrPtr(int i)
{
    return (i % 2) ? &odd : &even; // returns a pointer to the array
}
```
- top-level **const** has no effect on the objects that can be passed to the function. A parameter that has a top-level **const** is indistinguishable from one without a top-level **const**
- It is an error for two functions to differ only in terms of their return types
- A parameter that has a top-level **const** is
indistinguishable from one without a top-level **const**
- Function matching (also known as overload
resolution) is the process by which a particular function call is associated with a
specific function from a set of overloaded functions
- there are three possible outcomes:
  1. The compiler finds exactly one function that is a best match for the actual arguments and generates code to call that function.
  2. There is no function with parameters that match the arguments in the call, in which case the compiler issues an error message that there was no match.
  3. There is more than one function that matches and none of the matches is clearly best. This case is also an error; it is an ambiguous call.
- Overloading and Scope
```
string read();
void print(const string &);
void print(double); // overloads the print function
void fooBar(int ival)
{
    string s = read(); // error: read is a bool variable, not a function // bad practice: usually it's a bad idea to   declare functions at local scope
    void print(int); // new scope: hides previous instances of print
    print("Value: "); // error: print(const string &) is hidden
    print(ival); // ok: print(int) is visible
    print(3.14); // ok: calls print(int); print(double) is hidden
}
```
- In C++, name lookup happens before type checking
- Functions with default arguments can be called with or without that
argument.
- Default Arguments
```
typedef string::size_type sz; // typedef see § 2.5.1 (p. 67)
string screen(sz ht = 24, sz wid = 80, char backgrnd = ' ');
string window;
window = screen(); // equivalent to screen(24,80,' ')
window = screen(66);// equivalent to screen(66,80,' ')
window = screen(66, 256); // screen(66,256,' ')
window = screen(66, 256, '#'); // screen(66,256,'#')
```
- Part of the work of designing a function with default arguments is ordering the
parameters so that those least likely to use a default value appear first and those
most likely to use a default appear last.
- for a function has Default Argument Declarations , defaults can be specified only if all parameters to the right already have defaults.
- Default Argument Initializers
```
// the declarations of wd, def, and ht must appear outside a function
sz wd = 80;
char def = ' ';
sz ht();
string screen(sz = ht(), sz = wd, char = def);
string window = screen(); // calls screen(ht(), 80, ' ')
void f2()
{
	def = '*'; // changes the value of a default argument
	sz wd = 100; // hides the outer definition of wd but does not change the default
	window = screen(); // calls screen(ht(), 80, '*')
}
```
- inline functions
```
cout << shorterString(s1, s2) << endl;
equals to
cout << (s1.size() < s2.size() ? s1 : s2) <<endl;
// inline version: find the shorter of two strings
inline const string & shorterString(const string &s1, const string &s2)
{
return s1.size() <= s2.size() ? s1 : s2;
}
```
- function matching
```
void f();
void f(int);
void f(int, int);
void f(double, double = 3.14);
f(5.6); // calls void f(double, double)
```
- **candidate function**:Set of functions that are considered when resolving a function call. (all the functions with the name used in the call for which a declaration is in scope at the time of the call.)</br>
**viable function**:Subset of the candidate functions that could match a given call. It have the same number of parameters as arguments to the call, and each argument type can be converted to the corresponding parameter type.
- Conversions are ranked as follows:
 1. An exact match. An exact match happens when:
   - The argument and parameter types are identical.
   - The argument is converted from an array or function type to the corresponding pointer type.
   - A top-level const is added to or discarded from the argument.
  2. Match through a const conversion.
  3. Match through a promotion.
  4. Match through an arithmetic or pointer conversion.
  5. Match through a class-type conversion.
- conversion
  1. **Conversion to const**: We can convert a pointer to a nonconst type to a pointer to the corresponding const type, and similarly for references. That is, if T is a type, we can convert a pointer or a reference to T into a pointer or reference to const T, respectively
	```
	int i;
	const int &j = i; // convert a nonconst to a reference to const int
	const int *p = &i; // convert address of a nonconst to the address of a const
	int &r = j, *q = p; // error: conversion from const to nonconst not allowed
	```
  2. The integral **promotions** convert the small integral types to a larger integral type. The types bool, char, signed char, unsigned char, short, and unsigned short are promoted to int if all possible values of that type fit in an int.
  3. The **arithmetic conversions**, convert one arithmetic type to another. The rules define a hierarchy of type conversions in which operands to an operator are converted to the widest type.
  4. **Pointer Conversions**:</br>
	 1. In most expressions, when we use an array, the array is automatically converted to a pointer to the first element in that array</br>
         2. A constant integral value of 0 and the literal nullptr can be converted to any pointer type
	 3. a pointer to any nonconst type can be converted to void\*, and a pointer to any type can be converted to a const void\*
	 4. an additional pointer conversion that applies to types related by inheritance.
- Matches Requiring Promotion or Arithmetic Conversion
  1. it is important to remember that the small integral types always promote to int or to a larger integral type.
	```
	void ff(int);
  void ff(short);
  ff('a'); // char promotes to int; calls f(int)
	```
  2. All the arithmetic conversions are treated as equivalent to each other
 ```
 void manip(long);
 void manip(float);
 manip(3.14); // error: ambiguous call
 ```
- Using Function Pointers:
 we can use a pointer to a function to call the function to which the pointer points. We can do so directly—there is no need to dereference the pointer
- unlike what happens to parameters that have function type, the return type is not automatically converted to a pointer type.
```
using F = int(int*, int); // F is a function type, not a pointer
using PF = int(*)(int*, int); // PF is a pointer type
```
- The only tricky part in declaring getFcn is to remember that when we apply decltype to a function, it returns a function type, not a pointer to function type.
```
string::size_type sumLength(const string&, const string&);
string::size_type largerLength(const string&, const string&);
// depending on the value of its string parameter,
// getFcn returns a pointer to sumLength or to largerLength
decltype(sumLength) *getFcn(const string &);
```
