- An overloaded operator function has the same number of parameters as the
operator has operands.
- When an overloaded operator is a member function, this is bound to the left-hand operand. Member operator functions have one less (explicit) parameter than the number of operands.
- An operator function must either be a member of a class or have at least one
parameter of class type:
```
// error: cannot redefine the built-in operator for ints
int operator+(int, int);
```
This restriction means that we cannot change the meaning of an operator when
applied to operands of built-in type.
- ![Operators](https://github.com/zcenao21/Cpp/blob/master/photo/Operators.JPG?raw=true)
- An overloaded operator has the same precedence and associativity as the corresponding built-in operator.
- Ordinarily, the **comma**, **address-of**, logical **AND**, and logical **OR** operators should not be overloaded.
- Those operations with a logical mapping to an operator are good candidates
for defining as overloaded operators:
  - If the class does IO, define the shift operators to be consistent with how IO is done for the built-in types.
  - If the class has an operation to test for equality, define operator==. If the class has operator==, it should usually have operator!= as well.
  - If the class has a single, natural ordering operation, define operator<. If the class has operator<, it should probably have all of the relational operators.
  - The return type of an overloaded operator usually should be compatible with the return from the built-in version of the operator: The logical and relational operators should return bool, the arithmetic operators should return a value of the class type, and assignment and compound assignment should return a reference to the left-hand operand.
- If a class has an arithmetic or bitwise operator, then it is usually a good idea to provide the corresponding compound-assignment operator as well.
- The following guidelines can be of help in deciding whether to make an operator a
member or an ordinary nonmember function:
  - The assignment (=), subscript ([]), call (()), and member access arrow (->) operators must be defined as members.
  - The compound-assignment operators ordinarily ought to be members. However, unlike assignment, they are not required to be members.
  - Operators that change the state of their object or that are closely tied to their given type—such as increment, decrement, and dereference—usually should be members.
  - Symmetric operators—those that might convert either operand, such as the arithmetic, equality, relational, and bitwise operators—usually should be defined as ordinary nonmember functions.
- Generally, output operators should print the contents of the object, with minimal formatting. They should not print a newline.
- Input operators must deal with the possibility that the input might fail; output operators generally don’t bother.
- Classes that define both an arithmetic operator and the related compound assignment ordinarily ought to implement the arithmetic operator by using
the compound assignment.
- If a single logical definition for **<** exists, classes usually should define the **<** operator. However, if the class also has **==**, define **<** only if the definitions of **<** and **==** yield consistent results.
- If a class has a subscript operator, it usually should define two versions: one
that returns a plain reference and the other that is a const member and returns a reference to const.
- Classes that define increment or decrement operators should define both the
prefix and postfix versions. These operators usually should be defined as
members.
- To be consistent with the built-in operators, the postfix operators should
return the old (unincremented or undecremented) value. That value is
returned as a value, not a reference.
- The overloaded arrow operator must return either a pointer to a class type or
an object of a class type that defines its own operator arrow.
- The function-call operator must be a member function. A class may define multiple versions of the call operator, each of which must differ as to the
number or types of their parameters.
- Function objects are most often used as arguments to the generic algorithms.
```
for_each(vs.begin(), vs.end(), PrintString(cerr, '\n'));
```
- Classes generated from a lambda expression have a deleted default constructor, deleted assignment operators, and a default destructor. Whether the class has a defaulted or deleted copy/move constructor depends in the usual ways on the types of the captured data members.
- ![Library Function Objects](https://github.com/zcenao21/Cpp/blob/master/photo/Library%20Function%20Objects.JPG?raw=true)
- It is also worth noting that the associative containers use less<key_type> to order their elements. As a result, we can define a set of pointers or use a pointer as the key in a map without specifying less directly.
- C++ has several kinds of callable objects: functions and pointers to functions, lambdas , objects created by bind, and classes that overload the function-call operator.
- ![Operations on function](https://github.com/zcenao21/Cpp/blob/master/photo/Operations%20on%20function.JPG?raw=true)
- We cannot (directly) store the name of an overloaded function in an object of type
function.
- A conversion function must be a member function, may not specify a return type, and must have an empty parameter list. The function usually should be const.
- It is not uncommon for classes to define conversions to bool.
- Conversion to bool is usually intended for use in conditions. As a result,
operator bool ordinarily should be defined as explicit.
- Ordinarily, it is a bad idea to define classes with mutual conversions or to define conversions to or from two arithmetic types.
- if a class defines both conversion operators and overloaded operators. A few rules of thumb can be helpful:
    1. Don’t define mutually converting classes—if class Foo has a constructor that takes an object of class Bar, do not give Bar a conversion operator to type Foo.
    2. Avoid conversions to the built-in arithmetic types. In particular, if you do define a conversion to an arithmetic type, then
      - Do not define overloaded versions of the operators that take arithmetic types. If users need to use these operators, the conversion operation will convert objects of your type, and then the built-in operators can be used.
      - Do not define a conversion to more than one arithmetic type. Let the standard conversions provide conversions to the other arithmetic types.
- The easiest rule of all: With the exception of an explicit conversion to
bool, avoid defining conversion functions and limit nonexplicit constructors to those that are “obviously right.”
- In a call to an overloaded function, the rank of an additional standard conversion (if any) matters only if the viable functions require the same userdefined conversion. If different user-defined conversions are needed, then the call is ambiguous.
- the order of conversions:
  1. exact match
  2. const conversion
  3. promotion
  4. arithmetic or pointer conversion
  5. class-type conversion
- The set of candidate functions for an operator used in an expression can contain both nonmember and member functions.
- Providing both conversion functions to an arithmetic type and overloaded operators for the same class type may lead to ambiguities between the overloaded operators and the built-in operators.
