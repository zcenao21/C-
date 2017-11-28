- unary operators, binary operators, ternary operator
- **lvalues** could stand on the left-hand side of an assignment
whereas **rvalues** could not
- when we use an object as an rvalue,
we use the object’s value (its contents). When we use an object as an lvalue, we use the object’s identity (its location in memory).
- **lvalues** & **rvalues**
  1. Assignment requires a (nonconst) lvalue as its left-hand operand and yields its left-hand operand as an lvalue.
  2. The address-of operator requires an lvalue operand and returns a pointer to its operand as an rvalue.
  3. The built-in dereference and subscript operators and the iterator dereference and string and vector subscript operators all yield lvalues.
  4. The built-in and iterator increment and decrement operators require lvalue operands and the prefix versions also yield lvalues.
- **Lvalues** and **rvalues** also differ when used with decltype. assume p is an int\*. Because dereference yields an lvalue, decltype(\*p) is int&. On the other hand, because the address-of operator yields an rvalue, decltype(&p) is int\*\*, that is, a pointer to a pointer to type int.
- The arithmetic operators are left associative, which means operators
at the same precdence group left to right
- it is an error for an expression to
refer to and change the same object.
```
int i = 0;
cout << i << " " << ++i << endl; // undefined
```
- There are four operators that do guarantee the order in which operands are evaluated.
```
AND (&&)
OR(||)
conditional (? :)
comma (,)
```
- Advice: Managing Compound Expressions
When you write compound expressions, two rules of thumb can be helpful:
 1. When in doubt, parenthesize expressions to force the grouping that the
logic of your program requires.
 2. If you change the value of an operand, don’t use that operand elsewhere
in the same expresion.
- Arithmetic Operators
![a](http://ouewqfsxw.bkt.clouddn.com/arithmetic.PNG)
- For most operators, operands of type bool are promoted to int. In this case, the
value of b is true, which promotes to the int value 1 . That(promoted) value is negated, yielding -1. The value -1 is converted back to bool
and used to initialize b2. This initializer is a nonzero value, which when converted to
bool is true. Thus, the value of b2 is true!
```
bool b = true;
bool b2 = -b; // b2 is true!
```
- The operands to % must have integral type:
```
int ival = 42;
ival % dval; // error: floating-point operand
```
- Logical and Relational Operators
![a](http://ouewqfsxw.bkt.clouddn.com/logical.PNG)
- It is usually a bad idea to use the boolean literals **true** and **false** as operands in a comparison. These literals should be used only to compare to an object of type **bool**.
- If the left-hand operand is of a built-in type, the initializer list may contain at most
one value, and that value must not require a narrowing conversion
```
k = {3.14}; // error: narrowing conversion
```
- Unlike the other binary operators, assignment is right associative
```
int ival, jval;
ival = jval = 0; // ok: each assigned 0
```
- There are two forms of Increment and Decrement Operators: prefix and postfix
- Advice: Use Postfix Operators only When Necessary
- The conditional operator has fairly low precedence
```
cout << ((grade < 60) ? "fail" : "pass"); // prints pass or fail
cout << (grade < 60) ? "fail" : "pass"; // prints 1 or 0!
cout << grade < 60 ? "fail" : "pass"; // error: compares cout to 60
```
- Because there are no guarantees for how the sign bit is handled, we strongly
recommend using unsigned types with the bitwise operators.
- It is a common error to confuse the bitwise and logical operators， for example, the bitwise **~** and the logical **!**.
- **sizeof** does not convert the array to a pointer.
```
// sizeof(ia)/sizeof(*ia) returns the number of elements in ia
constexpr size_t sz = sizeof(ia)/sizeof(*ia);
int arr2[sz]; // ok sizeof returns a constant expression
```
- comma
```
someValue ? ++x, ++y : --x, --y
equals to
(someValue ? ++x, ++y : --x), --y
```
- type conversion: The types bool, char, signed char, unsigned char, short, and unsigned short are promoted to int if all possible values of that type fit in an int.
Otherwise, the value is promoted to unsigned int..</br>
The larger char types (wchar_t, char16_t, and char32_t) are promoted to the smallest type of int, unsigned int, long,  unsigned long, long long, or unsigned long long in which all possible values of that character type fit.
- When the signedness differs and the type of the unsigned operand is the same as or
larger than that of the signed operand, the signed operand is converted to unsigned.
- The remaining case is when the signed operand has a larger type than the unsigned
operand. In this case, the result is machine dependent. If all values in the unsigned
type fit in the larger type, then the unsigned operand is converted to the signed type.
If the values don’t fit, then the signed operand is converted to the unsigned type.
- This conversion is not performed when an array is used with decltype or as the
operand of the address-of (&), sizeof, or typeid operators. The conversion is also omitted when we initialize a reference to an array
```
int ia[10]; // array of ten ints
int* ip = ia; // convert ia to a pointer to the first element
```
- Pointer Conversions: There are several other pointer conversions: A constant integral value of 0 and the literal **nullptr** can be converted to any pointer type; a pointer to any nonconst type can be converted to **void\***, and a pointer to any type can be converted to a **const void\***.
- if **T** is a type, we
can convert a pointer or a reference to **T** into a pointer or reference to **const T**, respectively.
```
int i;
const int &j = i; // convert a nonconst to a reference to const int
const int *p = &i; // convert address of a nonconst to the address of a const
```
- Named Casts:
A named cast has the following form:
```
cast-name<type>(expression);
```
The *cast-name* may be one of **static_cast**, **dynamic_cast**, **const_cast**, and **reinterpret_cast**.
```
int i; double d; const string *ps; char *pc; void
*pv;
pv = (void*)ps;
equals to:
pv = const_cast<string*>(ps);
equals to:
pv = static_cast<void*>(const_cast<string*>(ps));
```
