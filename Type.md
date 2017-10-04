### Literal
- When we assign one of the nonbool arithmetic types to a bool object, the
result is false if the value is 0 and true otherwise.
- When we assign a bool to one of the other arithmeti-c types, the resulting
value is 1 if the bool is true and 0 if the bool is false.
- If we assign an out-of-range value to an object of unsigned type, the result is
the remainder of the value modulo the number of values the target type can
hold.
- If we assign an out-of-range value to an object of signed type, the result is
undefined.
- Advice: Avoid Undefined and Implementation-Defined Behavior
- compiler appends a null character (\0) to every string literal.
- escape sequence to represent such characters. An escape sequence begins with a backslash.
---
### variable
- std::string book("0-201-78345-X"); //a way to initialize string object
- object initialize: When a
definition defines two or more variables, the name of each object becomes visible
immediately.
- Initialization is not assignment. Initialization happens when a variable is given
a value when it is created. Assignment obliterates an object’s current value
and replaces that value with a new one.
- ![photo](http://ouewqfsxw.bkt.clouddn.com/Capture.PNG)
- The
identifiers we define in our own programs may not contain two consecutive
underscores; nor can an identifier begin with an underscore followed immediately by
an uppercase letter; identifiers defined outside a function may not begin
with an underscore.
- conventions can improve the readability of a program
  1. An identifier should give some indication of its meaning.
  2. Variable names normally are lowercase—index, not Index or INDEX.
  3. Like Sales_item, classes we define usually begin with an uppercase letter.
  4. Identifiers with multiple words should visually distinguish each word, for
example, student_loan or studentLoan, not studentloan.
- int \_; (correct expression)
---
### compound types
#### reference
- initializer must be an object.
- initializer must be the same type.(const type is allowed to have different type)
- it should be initialized when it is defined.

#### pointer
- types must match when assigning address.
- In declarations, & and \* are used to form compound types. In expressions,
these same symbols are used to denote an operator(& means to get the address,* means to get dereference value).
- the value stored in a pointer can be in one of four states:
  1. it can point to an object.
  2. it can point to the location just immediately past the end of an object.
  3. it can be a null pointer, indicating that it is not bound to any object.
  4. it can be invalid; values other than the preceding three are invalid.
- several ways to obtain a null pointer
  1. ```int *p1=nullptr;```
  2. ```int *p2=0;```
  3. ```int *p3=NULL;```
- ModernC++ programs generally should avoid using NULL and use nullptr instead.
- It is illegal to assign an int variable to a pointer, even if the variable’s value
happens to be 0.
- reference & pointer: the most important is that a reference is not an object. Once we have defined a reference, there is no way to make that reference
refer to a different object; there is no such identity between a pointer and the address that it holds.
- we can use a pointer in a condition, if the pointer is 0,
then the condition is false, any nonzero pointer evaluates as true.
- **void*** pointer is a special pointer type that can hold the address of any object, there are only a limited number of things we can do with a it:
  1. compare it to another pointer.
  2. pass it to or return it from a function.
  3. assign it to another void* pointer.
---
### const Qualifier
- we can make a variable unchangeable by defining the variable's type as const, it must be initialized when it is created.
- To share a const object among multiple files, you must define the variable
as extern.
- ```const int &const r2; %illegal, reference cannot be a const```
- const pointer
```
const double pi = 3.14159;
const double *const pip = &pi;
int *const cp; %illegal, cp must be initialized
const int *p; %legal, p is a pionter to const int
```
- We use the term top-level const to indicate that the
pointer itself is a const. When a pointer can point to a const object, we refer to
that const as a low-level const.
```
const int val = 12;
const int *va = &val; %low-level
const int *const va = &val; %top-level
```
- conclusion: low-level const only exist in reference or pointer type, and reference cannot be a top-level const, that means reference cannot be a const.
- In general, we can convert a
nonconst to const but not the other way round.
```
const int v2=0;int v1=v2;
int *p1=&v1;
const int *p2=&v2;
p1=p2; %illegal
p2=p1; %legal
```
- **constant expression**: is an expression whose value cannot change and that can be
evaluated at compile time.
```
const int max_files = 20; // max_files is a constant expression
const int limit = max_files + 1; // limit is a constant expression
int staff_size = 27; // staff_size is not a constant expression
const int sz = get_size(); // sz is not a constant expression
```
- **constexpr Variables**: variables
declared as constexpr are implicitly const and must be initialized by constant
expressions.
- Although we can define both pointers and reference as constexprs, the objects we
use to initialize them are strictly limited. We can initialize a constexpr pointer from
the nullptr literal or the literal (i.e., constant expression) 0. We can also point to
(or bind to) an object that remains at a fixed address.
---
#### Dealing with Types
- there are two methods to define type aliases:
```
typedef double wages;
using SI=Sales_item; // SI is a synonym for Sales_item
```
- Pointers, const, and Type Aliases
```
typedef char *pstring;
const pstring cstr = 0; // cstr is a constant pointer to char
```
the following interpretation is incorrect
```
const char *cstr = 0; // wrong interpretation of const pstring cstr
```
- we can let
figure out the type for us by using the **auto** type specifier to specify the type of an expression
- when we define multiple variables using **auto**, the declaration must have types that are consistent with each other
```
auto i = 0, *p = &i; // ok: i is int and p is a pointer to int
auto sz = 0, pi = 3.14; // error: inconsistent types for sz and pi
```
- points type: **auto** ordinarily ignores top-level consts, low-level consts are kept:
```
int i = 0;
const int ci = i, &cr = ci;
auto b = ci; // b is an int (top-level const in ci is dropped)
auto c = cr; // c is an int (cr is an alias for ci whose const is top-level)
auto d = &i; // d is an int*(& of an int object is int*)
auto e = &ci; // e is const int*(& of a const object is low-level const)
const auto f = ci; // deduced type of ci is int; f has type const int
```
- reference type：when we ask for a reference to an auto-deduced type, top-level consts in the
initializer are not ignored.
```
auto &g = ci; // g is a const int& that is bound to ci
auto &h = 42; // error: we can't bind a plain reference to a literal
const auto &j = 42; // ok: we can bind a const reference to a literal
```
- When we define several variables in the same statement, it is important to
remember that a reference or pointer is part of a particular declarator and not part of
the base type for the declaration. As usual, the initializers must provide consistent
auto-deduced types:
```
auto k = ci, &l = i; // k is int; l is int&
auto &m = ci, *p = &ci; // m is a const int&;p is a pointer to const int
// error: type deduced from i is int; type deduced from &ci is const int
auto &n = i, *p2 = &ci;
```
- The way decltype handles top-level const and references differs subtly from the way auto does. **decltype** returns the type of that variable, including top-level const and references:
```
decltype(f()) sum = x; // sum has whatever type f returns
```
It is worth noting that decltype is the only context in which a variable defined as
a reference is not treated as a synonym for the object to which it refers.
- the dereference operator is an example of an expression for
which decltype returns a reference.
```
int i = 42, *p = &i;
decltype(*p) c; // error: c is int& and must be initialized
```
- If we wrap the variable’s name in one or more sets of parentheses, **decltype** on such an expression yields a reference
---
### Struct
- The names defined inside the class must be unique;The close curly that ends the class body must be followed by a semicolon.
- Members without an initializer are default initialized.

### heead file
- difference between "" and <>: if the head file is included in the files that has been compiled by C++, we use <>; if the file is defined by ourselves, <> should be replace by "", or we may obtain some errors.

----
## summary
- Each type defines the storage requirements and the operations that may be
performed on objects of that type. The language provides a set of fundamental builtin
types such as int and char, which are closely tied to their representation on the
machine’s hardware. Types can be nonconst or const; a const object must be
initialized and, once initialized, its value may not be changed. In addition, we can
define compound types, such as pointers or references. A compound type is one that
is defined in terms of another type.
The language lets us define our own types by defining classes. The library uses the
class facility to provide a set of higher-level abstractions such as the IO and string
types.
