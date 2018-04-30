## Exception Handling
- Throwing a pointer requires that the object to which the pointer points exist wherever the corresponding handler resides.
- Ordinarily, a catch that takes an exception of a type related by inheritance ought to define its parameter as a reference.
- the types of the exception and the catch declaration must match exactly with only a few possible differences:
  1. Conversions from non**const** to **const** are allowed. That is, a **throw** of a non**const** object can match a **catch** specified to take a reference to **const**.
  2. Conversions from derived type to base type are allowed.
  3. An array is converted to a pointer to the type of the array; a function is converted to the appropriate pointer to function type.
- A rethrow is a **throw** that is not followed by an expression: **throw**; An empty **throw** can appear only in a **catch** or in a function called (directly or indirectly) from a **catch**.
- In general, a **catch** might change the contents of its parameter. If, after changing its parameter, the **catch** rethrows the exception, then those changes will be propagated only if the **catch**’s exception declaration is a **reference**.
- **catch-all** handlers, have the form **catch(...)**. A **catch-all** clause matches any type of exception.
- If a **catch(...)** is used in combination with other **catch** clauses, it must be last. Any **catch** that follows a catch-all can never be matched.
- Standard exception Class Hierarchy
![Standard exception Class Hierarchy](https://github.com/zcenao21/Cpp/blob/master/photo/Standard-exception-Class-Hierarchy.PNG?raw=true)
- A function try block lets us associate a group of catch clauses with the initialization phase of a constructor
```
template <typename T>
Blob<T>::Blob(std::initializer_list<T> il) try:
data(std::make_shared<std::vector<T>>(il)) {
/* empty body */
} catch(const std::bad_alloc &e) { handle_out_of_memory(e); }
```
- Knowing that a function will not throw simplifies the task of writing code that calls that function. Moreover, if the compiler knows that no exceptions will be thrown, it can (sometimes) perform optimizations that must be suppressed if code might throw.
- The **noexcept** specifier must appear on all of the declarations and the corresponding definition of a function or on none of them. The specifier precedes a trailing return. We may also specify **noexcept** on the declaration and definition of a function pointer. It may not appear in a **typedef** or type alias. In a member function the **noexcept** specifier follows any **const** or reference qualifiers, and it precedes **final**, **override**, or **= 0** on a virtual function.
- **noexcept** should be used in two cases: if we are confident that the function won’t throw, and/or if we don’t know what we’d do to handle the error anyway.
- These declarations of recoup are equivalent. Both say that recoup won’t throw.
```
void recoup(int) noexcept; // recoup doesn't throw
void recoup(int) throw(); // equivalent declaration
```
- The **noexcept** specifier takes an optional argument that must be convertible to **bool**: If the argument is **true**, then the function won’t **throw**; if the argument is **false**, then the function might **throw**:
```
void recoup(int) noexcept(true); // recoup won't throw
void alloc(int) noexcept(false); // alloc can throw
```
- **noexcept** does not evaluate its operand, **noexcept(e)** is **true** if all the functions called by e have nonthrowing specifications and **e** itself does not contain a throw. Otherwise, **noexcept(e)** returns **false**.
- **noexcept** has two meanings: It is an exception specifier when it follows a function’s parameter list, and it is an operator that is often used as the **bool** argument to a **noexcept** exception specifier.
- A pointer to function and the function to which that pointer points must have compatible specifications. That is, if we declare a pointer that has a nonthrowing exception specification, we can use that pointer only to point to similarly qualified
functions. A pointer that specifies (explicitly or implicitly) that it might throw can point to any function, even if that function includes a promise not to throw.
- If a virtual function includes a promise not to throw, the inherited virtuals must also promise not to throw. On the other hand, if the base allows exceptions, it is okay for the derived functions to be more restrictive and promise not to throw.
- The only operations that the exception types define are the copy constructor, copy-assignment operator, a virtual destructor, and a virtual member named what.
The what function returns a const char* that points to a null-terminated character array, and is guaranteed not to throw any exceptions.

## namespace
- A namespace scope does not end with a semicolon.
- Namespaces that define multiple, unrelated types should use separate files to represent each type (or each collection of related types) that the namespace
defines.  
- Template specializations must be defined in the same namespace that contains the original template
- the notation **::member_name** refers to a member of the global namespace.
- Unlike ordinary nested namespaces, names in an inline namespace can be used as if they were direct members of the enclosing namespace.
- Unlike other namespaces, an unnamed namespace is local to a particular file and never spans multiple files.
- The use of file static declarations is deprecated by the C++ standard. File statics should be avoided and unnamed namespaces used instead.
- namespace alias
```
namespace primer = cplusplus_primer;
```
- A namespace can have many synonyms, or aliases. All the aliases and the original namespace name can be used interchangeably.
- A using declaration can appear in global, local, namespace, or class scope. In class scope, such declarations may only refer to a base class member
- A using directive may appear in global, local, or namespace scope. It may not appear in a class scope.(using directive: Unlike a using declaration, we retain no control over which names are made visible—they all are.)
- using Directives Example
```
namespace blip {
int i = 16, j = 15, k = 23;
// other declarations
}
int j = 0; // ok: j inside blip is hidden inside a namespace
void manip()
{
  // using directive; the names in blip are ''added'' to the global scope
  using namespace blip; // clash between ::j and blip::j
  // detected only if j is used
  ++i; // sets blip::i to 17
  ++j; // error ambiguous: global j or blip::j?
  ++::j; // ok: sets global j to 1
  ++blip::j; // ok: sets blip::j to 16
  int k = 97; // local k hides blip::k
  ++k; // sets local k to 98
}
```
- One place where using directives are useful is in the implementation files of the namespace itself.
- With the exception of member function definitions that appear inside the class body, scopes are always searched upward; names must be declared before they can be used.Had that name been defined in **A** before the definition of **C1**, the use of h would be legal. Similarly, the use of **h** inside **f3** is okay, because f3 is defined after **A::h**.
```
namespace A {
int i;
int k;
class C1 {
    public:
    C1(): i(0), j(0) { } // ok: initializes C1::i and C1::j
    int f1() { return k; } // returns A::k
    int f2() { return h; } // error: h is not defined
    int f3();
    private:
    int i; // hides A::i within C1
    int j;
    };
int h = i; // initialized from A::i
}
// member f3 is defined outside class C1 and outside namespace A
int A::C1::f3() { return h; } // ok: returns A::h
```
- When we pass an object of a class type to a function, the compiler searches the namespace in which the argument’s class is defined in addition to the normal scope lookup.
```
std::string s;
std::cin >> s;
```
- Argument-Dependent Lookup and Overloading
```
namespace NS {
class Quote { /* ... */ };
void display(const Quote&) { /* ... */ }
}
// Bulk_item's base class is declared in namespace NS
class Bulk_item : public NS::Quote { /* ... */ };
int main() {
Bulk_item book1;
display(book1);
return 0;
}
```
The function display(const Quote&) declared in namespace NS is added to the set of candidate functions.
- Overloading and using Declarations
```
using NS::print(int); // error: cannot specify a parameter list
using NS::print; // ok: using declarations specify names only
```
When we write a using declaration for a function, all the versions of that function are brought into the current scope.
- The compiler makes no attempt to distinguish between base classes in terms of a
derived-class conversion.
```
void print(const Bear&);
void print(const Endangered&);
Panda ying_yang("ying_yang");
print(ying_yang); // error: ambiguous
```
- Under multiple inheritance,a lookup happens simultaneously among all the
direct base classes. If a name is found through more than one base class, then use of
that name is ambiguous.
- It is perfectly legal for a class to inherit multiple members with the same
name. However, if we want to use that name, we must specify which version we want
to use.
- Virtual inheritance lets a class specify that it is willing to share its base class. The shared base-class subobject is called a virtual base class.
- Virtual derivation affects the classes that subsequently derive from a class
with a virtual base; it doesn’t affect the derived class itself.
- In a virtual derivation, the virtual base is initialized by the most derived constructor.
- The construction order for an object with a virtual base is slightly modified from the normal order: The virtual base subparts of the object are initialized first, using
initializers provided in the constructor for the most derived class. Once the virtual base subparts of the object are constructed, the direct base subparts are constructed in the order in which they appear in the derivation list.
- Virtual base classes are always constructed prior to nonvirtual base classes
regardless of where they appear in the inheritance hierarchy.
- A class can have more than one virtual base class. In that case, the virtual subobjects are constructed in left-to-right order as they appear in the derivation list.
