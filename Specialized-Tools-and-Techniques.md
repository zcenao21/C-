### Controlling Memory Allocation
- The library defines eight overloaded versions of operator new and delete functions. The first four support the versions of new that can throw a bad_alloc
exception. The next four support nonthrowing versions of new:
```
// these versions might throw an exception
void *operator new(size_t); // allocate an object
void *operator new[](size_t); // allocate an array
void *operator delete(void*) noexcept; // free an object
void *operator delete[](void*) noexcept; // free an array
// versions that promise not to throw; see § 12.1.2 (p. 460)
void *operator new(size_t, nothrow_t&) noexcept;
void *operator new[](size_t, nothrow_t&) noexcept;
void *operator delete(void*, nothrow_t&) noexcept;
void *operator delete[](void*, nothrow_t&) noexcept;
```
- When defined as members of a class, these operator functions are implicitly static. There is no need to declare them static explicitly.
- An **operator new** or **operator new[]** function must have a return type of **void*** and its first parameter must have type **size_t**.
- Although
generally we may define our version of operator new to have whatever parameters are needed, we may not define a function with the following form:
```
void *operator new(size_t, void*); // this version may not be redefined
```
- An **operator delete** or **operator delete[]** function must have a **void** return type and a first parameter of type **void***.
- When passed a single argument that is a pointer, a placement **new** expression constructs an object but does not allocate memory.
- Calling a destructor destroys an object but does not free the memory.

### Run-Time Type Identification(RTTI)
- two operators:
  1. The typeid operator, which returns the type of a given expression
  2. The dynamic_cast operator, which safely converts a pointer or reference to a base type into a pointer or reference to a derived type
- RTTI should be used with caution. When possible, it is better to define a
virtual function rather than to take over managing the types directly.
- A **dynamic_cast** has the following form:
```
dynamic_cast<type*>(e)
dynamic_cast<type&>(e)
dynamic_cast<type&&>(e)
```
- We can do a **dynamic_cast** on a null pointer; the result is a null pointer of the requested type.
- Performing a dynamic_cast in a condition ensures that the cast and test of its result are done in a single expression.
- A **typeid** expression has the form **typeid(e)** where **e** is any expression or a type name. The result of a **typeid** operation is a reference to a **const** object of a library type named **type_info**, or a type publicly derived from **type_info**.
- Operations on type_info
![Operations on type_info](https://github.com/zcenao21/Cpp/blob/master/photo/Operations-on-type_info.PNG?raw=true)
- The only way to create
a type_info object is through the typeid operator.

### Enumerations
- Enumerations let us group together sets of integral constants. Enumerations are literal types.
- C++ has two kinds of enumerations: **scoped** and **unscoped**.
- scoped enumeration
```
enum class open_modes {input, output, append};
```
- unscoped enumeration
```
enum color {red, yellow, green}; // unscoped enumeration
```
- By default, enumerator values start at 0 and each enumerator has a value 1 greater than the preceding one. However, we can also supply initializers for one or more enumerators:
```
enum class intTypes {
    charTyp = 8, shortTyp = 16, intTyp = 16,
    longTyp = 32, long_longTyp = 64
};
```
- each enumerator is itself a constant expression.
- Under the new standard, we may specify that type by following the enum name with a colon and the name of the type we want to use:
```
enum intValues : unsigned long long {
charTyp = 255, shortTyp = 65535, intTyp = 65535,
longTyp = 4294967295UL,
long_longTyp = 18446744073709551615ULL
};
```
If we do not specify the underlying type, then by default scoped enums have int as the underlying type. There is no default for unscoped enums; all we know is that the underlying type is large enough to hold the enumerator values.
- An enum forward
declaration must specify (implicitly or explicitly) the underlying size of the enum:
```
// forward declaration of unscoped enum named intValues
enum intValues : unsigned long long; // unscoped, must specify a type
enum class open_modes; // scoped enums can use int by default
```

### Pointer to Class Member
- A pointer to member
```
// pdata can point to a string member of a const (or non const) Screen object
const string Screen::*pdata;
pdata = &Screen::contents;
auto pdata = &Screen::contents;
```
- Analogous to the member access operators, **.** and **->**, there are two pointer-tomember
access operators, **.*** and **->***
- Because of the relative precedence of the call operator, declarations of pointers to member functions and calls through such pointers must use parentheses: (C::*p)(parms) and (obj.*p)(args).
- One common use for function pointers and for pointers to member functions is to store them in a function table.
- to make a call through a pointer to member function, we must use the .* or ->* operators to bind the pointer to a specific object. As a result, unlike ordinary function pointers, a pointer to member is not a callable object
```
auto fp = &string::empty; // fp points to the string empty function
// error: must use .* or ->* to call a pointer to member
find_if(svec.begin(), svec.end(), fp);
```
- One way to obtain a callable from a pointer to member function is by using the library function template
```
function<bool (const string&)> fcn = &string::empty;
find_if(svec.begin(), svec.end(), fcn);
```
- Like function, mem_fn generates a callable object from a pointer to member. Unlike
function, mem_fn will deduce the type of the callable from the type of the pointer to member.
- we can also use bind to generate a callable from a member function.

### Nested Classes
- Until the actual definition of a nested class that is defined outside the class
body is seen, that class is an incomplete type.

### A Space-Saving Class
- A union is a special kind of class. A union may have multiple data members, but at any point in time, only one of the members may have a value.
- By default, like structs, members of a union are public.
- a union may not inherit from another class, nor may a union be used as
a base class. As a result, a union may not have virtual functions.
- An anonymous union cannot have private or protected members, nor can an anonymous union define member functions.

### Local Classes
- A class can be defined inside a function body. Such a class is called a local class.The members of a local class are severely restricted.
- All members, including functions, of a local class must be completely defined inside the class body. As a result, local classes are much less useful than nested classes.
- A local class is not permitted to declare static data members.
- A local class can access only type names, static variables, and enumerators defined within the enclosing local scopes.

### Inherently Nonportable Features
- A nonportable feature is one that is machine specific.
- The address-of operator (&) cannot be applied to a bit-field, so there can be no pointers referring to class bit-fields.
- Ordinarily it is best to make a bit-field an unsigned type. The behavior of bit-fields stored in a signed type is implementation defined.
- The precise meaning of volatile is inherently machine dependent and can
be understood only by reading the compiler documentation. Programs that use volatile usually must be changed when they are moved to new machines or compilers.
- An object should be declared volatile when its value might be changed in ways outside the control or detection of the program.
- Only volatile member functions may be called on volatile objects.
- C++ uses linkage directives to indicate the language used for any non-C++ function.
- every declaration of a function defined with a linkage directive must use the same linkage directive. Moreover, pointers to functions written in other languages must be declared with the same linkage directive as the function itself.
- Because a linkage directive applies to all the functions in a declaration, we must use a type alias if we wish to pass a pointer to a C function to a C++ function.
- To allow the same source file to be compiled under either C or C++, the
preprocessor defines _ _cplusplus (two underscores) when we compile
C++. Using this variable, we can conditionally include code when we are
compiling C++:
```
#ifdef __cplusplus
// ok: we're compiling C++
extern "C"
#endif
int strcmp(const char*, const char*);
```
