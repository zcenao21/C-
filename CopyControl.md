- . A class controls when objects of that class type are copied, moved, assigned, and destroyed operations by defining five special member functions: **copy constructor**, **copy-assignment operator**, **move constructor**, **move-assignment operator**, and **destructor**.The copy and move constructors define what happens when an object is initialized from another object of the same type. The copy- and move-assignment operators define what happens when we assign an object of a class type to another object of that same class type. The destructor defines what happens when an object of the type ceases to exist. Collectively, we’ll refer to these operations as **copy control**.
- A constructor is the copy constructor if its first parameter is a reference to the class type and any additional parameters have default values:
```
class Foo {
public:
    Foo(); // default constructor
    Foo(const Foo&); // copy constructor
// ...
};
```
- Copy initialization happens not only when we define variables using an =, but also
when we
  1. Pass an object as an argument to a parameter of nonreference type
  2. Return an object from a function that has a nonreference return type
  3. Brace initialize the elements in an array or the members of an aggregate class
- Assignment operators ordinarily should return a reference to their left-hand operand.
- In a destructor, the function body is executed first and then the members are destroyed. Members are destroyed in reverse order from the order in which they were initialized.
- Members of class type are
destroyed by running the member’s own destructor. The built-in types do not have destructors, so nothing is done to destroy members of built-in type.
- The implicit destruction of a member of built-in pointer type does not delete the object to which that pointer points.
- The destructor is used automatically whenever an object of its type is destroyed:
  - Variables are destroyed when they go out of scope.
  - Members of an object are destroyed when the object of which they are a part is destroyed.
  - Elements in a container—whether a library container or an array—are destroyed when the container is destroyed.
  - Dynamically allocated objects are destroyed when the delete operator is applied to a pointer to the object.
  - Temporary objects are destroyed at the end of the full expression in which the temporary was created.
  - The destructor is not run when a reference or a pointer to an object goes out of scope.
- rule of thumb to use copy **control**
  - If a class needs a destructor, it almost surely also needs the copy-assignment operator and a copy constructor.
  - Classes That Need Copy Need Assignment, and Vice Versa
- we can prevent copies by defining the copy constructor and copy-assignment operator as deleted functions.
```
struct NoCopy {
  NoCopy() = default; // use the synthesized default constructor
  NoCopy(const NoCopy&) = delete; // no copy
  NoCopy &operator=(const NoCopy&) = delete; // no assignment
  ~NoCopy() = default; // use the synthesized destructor
// other members
};
```
- It is worth noting that we did not delete the destructor. If the destructor is deleted, then there is no way to destroy objects of that type.
- It is not possible to define an object or delete a pointer to a dynamically allocated object of a type with a deleted destructor.
- For some classes, the compiler
defines these synthesized members as deleted functions:
  - The synthesized destructor is defined as deleted if the class has a member whose own destructor is deleted or is inaccessible (e.g., private).
  - The synthesized copy constructor is defined as deleted if the class has a member whose own copy constructor is deleted or inaccessible. It is also deleted if the class has a member with a deleted or inaccessible destructor.
  - The synthesized copy-assignment operator is defined as deleted if a member has a deleted or inaccessible copy-assignment operator, or if the class has a const or reference member.
  - The synthesized default constructor is defined as deleted if the class has a member with a deleted or inaccessible destructor; or has a reference member that does not have an in-class initializer; or has a const member whose type does not explicitly define a default constructor and that member does not have an in-class initializer.  
**In essence, these rules mean that if a class has a data member that cannot be default constructed, copied, assigned, or destroyed, then the corresponding member will be a
deleted function**.
- There are two points to keep in mind when you write an assignment
operator:
  - Assignment operators must work correctly if an object is assigned to itself.
  - Most assignment operators share work with the destructor and copy constructor.  
- A good pattern to use when you write an assignment operator is to first copy the right-hand operand into a local temporary. After the copy is done, it is safe to destroy the existing members of the left-hand operand. Once the lefthand operand is destroyed, copy the data from the temporary into the members of the left-hand operand.
- Assignment operators that use copy and swap are automatically exception
safe and correctly handle self-assignment.
- about **pair**
```
#include <utility>
using std::pair;
```
- The library containers, string, and shared_ptr classes support move as
well as copy. The IO and unique_ptr classes can be moved but not copied.
- An rvalue reference is a reference that must be bound to an rvalue. An rvalue reference is obtained by using **&&** rather than **&**. rvalue
references have the important property that they may be bound only to an object that is about to be destroyed.
- Generally speaking, an lvalue expression refers to an object’s identity whereas an rvalue expression refers to an object’s value.
- Rvalue references have the opposite binding properties: We can bind an rvalue reference to these kinds of expressions, but we cannot directly bind an rvalue reference to an lvalue:
```
int i = 42;
int &r = i; // ok: r refers to i
int &&rr = i; // error: cannot bind an rvalue reference to an lvalue
int &r2 = i * 42; // error: i * 42 is an rvalue
const int &r3 = i * 42; // ok: we can bind a reference to const to an rvalue
int &&rr2 = i * 42; // ok: bind rr2 to the result of the multiplication
```
- lvalue&rvalue
  1. Functions that return lvalue references, along with the assignment, subscript, dereference, and prefix increment/decrement operators, are all examples of expressions that return lvalues. We can bind an lvalue reference to the result of any of these expressions.  
  2. Functions that return a nonreference type, along with the arithmetic, relational, bitwise, and postfix increment/decrement operators, all yield rvalues. We cannot bind an lvalue reference to these expressions, but we can bind either an lvalue reference to const or an rvalue reference to such expressions.
- A variable is an lvalue; we cannot directly bind an rvalue reference to a variable even if that variable was defined as an rvalue reference type.
- Although we cannot directly bind an rvalue reference to an lvalue, we can explicitly
cast an lvalue to its corresponding rvalue reference type. We can also obtain an rvalue
reference bound to an lvalue by calling a new library function named **move**, which is
defined in the utility header.
```
int &&rr2 = rr1; // error: the expression rr1 is an lvalue!
int &&rr3 = std::move(rr1); // ok
```
After a call to move, we cannot make any assumptions about the value of the moved-from object.
- We can destroy a moved-from object and can assign a new value to it, but we cannot use the value of a moved-from object.
- Move constructors and move assignment operators that cannot throw exceptions should be marked as noexcept.
- a move operation doesn’t throw because of two interrelated facts:
  - First, although move operations usually don’t throw exceptions, they are permitted to do so.
  - Second, the library containers provide guarantees as to what they do if an exception happens.
- After a move operation, the “moved-from” object must remain a valid, destructible object but users may make no assumptions about its value.
- The compiler will synthesize a move constructor or a move-assignment operator
only if the class doesn’t define any of its own copy-control members and if every nonstatic data member of the class can be moved. The compiler can move members of built-in type. It can also move members of a class type if the member’s
class has the corresponding move operation.
- Unlike the copy operations, a move operation is never implicitly defined as a **deleted**
function. However, if we explicitly ask the compiler to generate a move operation by
using **= default** , and the compiler is unable to move all the members, then the move operation will be defined as **deleted**.
  - Unlike the copy constructor, the move constructor is defined as deleted if the class has a member that defines its own copy constructor but does not also define a move constructor, or if the class has a member that doesn’t define its own copy operations and for which the compiler is unable to synthesize a move constructor. Similarly for move-assignment.
  - The move constructor or move-assignment operator is defined as deleted if the class has a member whose own move constructor or move-assignment operator is deleted or inaccessible.
  - Like the copy constructor, the move constructor is defined as deleted if the destructor is deleted or inaccessible.
  - Like the copy-assignment operator, the move-assignment operator is defined as deleted if the class has a const or reference member
- There is one final interaction between move operations and the synthesized copycontrol
members: Whether a class defines its own move operations has an impact on how the copy operations are synthesized. If the class defines either a move constructor and/or a move-assignment operator, then the synthesized copy constructor and copy-assignment operator for that class will be defined as deleted.
- Classes that define a move constructor or move-assignment operator must also define their own copy operations. Otherwise, those members are deleted by default.
- When a class has both a move constructor and a copy constructor, the compiler uses ordinary function matching to determine which constructor to use.
- What if a class has a copy constructor but does not define a move constructor? In this
case, the compiler will not synthesize the move constructor, which means the class has a copy constructor but no move constructor. If a class has no move constructor, function matching ensures that objects of that type are copied, even if we attempt to move them by calling **move**.
- Overloaded functions that distinguish between moving and copying a parameter typically have one version that takes a **const T&** and one that takes a **T&&**.
- A function can be both const and reference qualified. In such cases, the reference qualifier must follow the const qualifier.
- There is no similar default for reference qualified functions. When we define two or more members that have the same name and the same parameter list, we must provide a reference qualifier on all or none of those functions:
