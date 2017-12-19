- Defining a Member Function outside the Class
```
double Sales_data::avg_price() const {
if (units_sold)
    return revenue/units_sold;
else
    return 0;
}
```
- Unlike other member functions, constructors may not be declared as const. Thus, constructors can write to const objects during their construction.
- The compiler-generated constructor is known as the synthesized default constructor。
- If we define any constructors, the class will not have a default constructor unless we define that constructor ourselves.
- Classes that have members of built-in or compound type usually should rely on the synthesized default constructor only if all such members have in-class
initializers.
- Constructors should not override in-class initializers except to use a different initial value. If you can’t use in-class initializers, each constructor should explicitly initialize every member of built-in type.
- Each access specifier specifies the access level of the succeeding members. The specified access level remains in effect until the
next access specifier or the end of the class body.
- The only difference between **struct** and **class** is the default access level. If we use the **struct** keyword, the members defined before the first access specifier are public; if we use **class**, then the members are private.
- A class can allow another class or function to access its **nonpublic** members by making that class or function a friend. Ordinarily it is a good idea to group friend declarations together at the beginning or end of the class definition.
- To make a friend visible to users of the class, we usually declare each friend (outside the class) in the same header as the class itself.
- It sometimes (but not very often) happens that a class has a data member that we
want to be able to modify, even inside a const member function. We indicate such members by including the mutable keyword in their declaration.
- When we provide an in-class initializer, we must do so following an **=** sign or inside braces.
- Because a class is not defined until its class body is complete, a class cannot
have data members of its own type. However, a class is considered declared (but not yet defined) as soon as its class name has been seen. Therefore, a class can have data members that are pointers or references to its own type:
```
class Link_screen {
    Screen window;
    Link_screen *next;
    Link_screen *prev;
};
```
- Although overloaded functions share a common name, they are still different
functions. Therefore, a class must declare as a friend each function in a set of overloaded functions that it wishes to make a friend.
- When a member function is defined outside the class body, any
name used in the return type is outside the class scope. As a result, the return type must specify the class of which it is a member.
```
public:
// add a Screen to the window and returns its index
ScreenIndex addScreen(const Screen&);
// other members as before
};
// return type is seen before we're in the scope of Window_mgr
Window_mgr::ScreenIndex
Window_mgr::addScreen(const Screen &s)
{
    screens.push_back(s);
    return screens.size() - 1;
}
```
- Class definitions are processed in two phases:
  1. First, the member declarations are compiled.
  2. Function bodies are compiled only after the entire class has been seen.
- The compiler considers only declarations
inside Account that appear before the use of Money. Because no matching member
is found, the compiler then looks for a declaration in the enclosing scope(s). In this example, the compiler will find the typedef of Money.
```
typedef double Money;
string bal;
class Account {
public:
    Money balance() { return bal; }
private:
    Money bal;
    // ...
};
```
- in a class, if a member uses a
name from an outer scope and that name is a type, then the class may not
subsequently redefine that name:
```
class Account {
public:
Money balance() { return bal; } // uses Money from the outer
scope
private:
typedef double Money; // error: cannot redefine Money
Money bal;
// ...
};
```
- Members are initialized in the order in which they appear in the class definition.
```
//error example
struct X {
    X (int i, int j): base(i), rem(base % j) { }
    int rem, base;
};
```
- A constructor that can be called with a single argument defines an implicit
conversion from the constructor’s parameter type to the class type.
- We can prevent the use of a constructor in a context that requires an implicit
conversion by declaring the constructor as explicit.
- The explicit keyword is used only on the constructor declaration inside
the class.
```
// error: explicit allowed only on a constructor declaration in a class header
explicit Sales_data::Sales_data(istream& is)
{
    read(is, *this);
}
```
- explicit Constructors Can Be Used Only for Direct Initialization
```
Sales_data item1 (null_book); // ok: direct initialization
// error: cannot use the copy form of initialization with an explicit constructor
Sales_data item2 = null_book;
```
- Assuming the Sales_data constructors are not explicit,
what operations happen during the following definitions
```
string null_isbn("9-999-99999-9");
Sales_data item1(null_isbn);
Sales_data item2("9-999-99999-9");
```
nothing happens, both, distinguish it from Implicit Conversions
- aggregate class
  1. All of its data members are public
  2. It does not define any constructors
  3. It has no in-class initializers
  4. It has no base classes or virtual functions
- In addition to the arithmetic types, references, and pointers, certain classes are also literal types.
- we can define a static member function inside
or outside of the class body. When we define a static member outside the class, we
do not repeat the static keyword. The keyword appears only with the declaration
inside the class body:
```
void Account::rate(double newRate)
{
   interestRate = newRate;
}
```
- Even if a const static data member is initialized in the class body, that
member ordinarily should be defined outside the class definition.
- static Members Can Be Used in Ways Ordinary Members Can’t。
  1. a static data member can have incomplete type
  ```
  class Bar {
  public:
  // ...
  private:
    static Bar mem1; // ok: static member can have incomplete type
    Bar *mem2; // ok: pointer member can have incomplete type
    Bar mem3; // error: data members must have complete type
  ```
  2. we can use a static member as a default argument
  ```
  class Screen {
  public:
      // bkground refers to the static member
      // declared later in the class definition
      Screen& clear(char = bkground);
      private:
      static const char bkground;
  };
  ```
