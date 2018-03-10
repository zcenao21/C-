- The key ideas in object-oriented programming are data abstraction, inheritance, and dynamic binding.
- In C++, dynamic binding happens when a virtual function is called through a reference (or a pointer) to a base class.
- Base classes ordinarily should define a virtual destructor. Virtual destructors
are needed even if they do no work.
- Member functions that are not declared as virtual are resolved at compile time,
not run time.
- Each class controls how its members are initialized.
- The base class is initialized first, and then the members of the derived class are initialized in the order in which they are declared in the class.
- The declaration contains the class name but does not include its derivation list:
```
class Bulk_quote : public Quote; // error: derivation list can't appear here
class Bulk_quote; // ok: right way to declare a derived class
```
- we can prevent a class from being used as a base by following the class name with **final**.
- we must define every virtual function, regardless of whether it is used, because the compiler has no way to determine whether a virtual function is used.
- A function that is virtual in a base class is implicitly virtual in its derived classes. When a derived class overrides a virtual, the parameters in the base and derived classes must match exactly.
- Virtual functions that have default arguments should use the same argument values in the base and derived classes.
- Ordinarily, only code inside member functions (or friends) should need to use
the scope operator to circumvent the virtual mechanism.
- we cannot provide a function body inside the class for a function that is = 0(pure virtual).
- We may not create objects of a type that is an abstract base class.
- A derived class member or friend may access the protected members of the
base class only through a derived object. The derived class has no special access to the protected members of base-class objects.
- The purpose of the derivation access specifier is to control the access that users of
the derived class—including other classes derived from the derived class—have to the members inherited from Base.
- The derivation access specifier used by a derived class also controls access from classes that inherit from that derived class.
- Assuming D inherits from B:
  1. User code may use the derived-to-base conversion only if D inherits publicly from B. User code may not use the conversion if D inherits from B using either protected or private.
  2. Member functions and friends of D can use the conversion to B regardless of how D inherits from B. The derived-to-base conversion to a direct base class is always accessible to members and friends of a derived class.
  3. Member functions and friends of classes derived from D may use the derived-tobase conversion if D inherits from B using either public or protected. Such code may not use the conversion if D inherits privately from B.
- Friendship is not inherited; each class controls access to its members.
- A derived class may provide a using declaration only for names it is permitted to access.
- A derived-class member with the same name as a member of the base class
hides direct use of the base-class member.
- Aside from overriding inherited virtual functions, a derived class usually should not reuse names defined in its base class.
- The base member is hidden even if the functions have different parameter lists:
```
struct Base {
    int memfcn();
};
struct Derived : Base {
    int memfcn(int); // hides memfcn in the base
};
Derived d; Base b;
b.memfcn(); // calls Base::memfcn
d.memfcn(10); // calls Derived::memfcn
d.memfcn(); // error: memfcn with no arguments is hidden
d.Base::memfcn(); // ok: calls Base::memfcn
```
- a using declaration for a base-class member function adds all the overloaded instances of that function to the scope of the derived class.
- Executing delete on a pointer to base that points to a derived object has undefined behavior if the base’s destructor is not virtual.
- Destructors for base classes are an important exception to the rule of thumb that if a class needs a destructor, it also needs copy and assignment.
- If a class defines a destructor—even if it uses **= default** to use the synthesized version—the compiler will not synthesize a move operation for that class
- In addition to the same reasons as in any other class, the way in which a
base class is defined can cause a derived-class member to be defined as deleted:
  1. If the default constructor, copy constructor, copy-assignment operator, or destructor in the base class is deleted or inaccessible, then the corresponding member in the derived class is defined as deleted, because the compiler can’t use the base-class member to construct, assign, or destroy the base-class part of the object.
  2. If the base class has an inaccessible or deleted destructor, then the synthesized default and copy constructors in the derived classes are defined as deleted, because there is no way to destroy the base part of the derived object.
  3. As usual, the compiler will not synthesize a deleted move operation. If we use = default to request a move operation, it will be a deleted function in the derived if the corresponding operation in the base is deleted or inaccessible, because the base class part cannot be moved. The move constructor will also be deleted if the base class destructor is deleted or inaccessible.
- When a derived class defines a copy or move operation, that operation is
responsible for copying or moving the entire object, including base-class
members.
- unlike the constructors and assignment operators, a derived destructor is responsible only for destroying the resources allocated by the derived class.
- Under the new standard, a derived class can reuse the constructors defined by its direct base class. A class cannot inherit the default, copy, and move constructors.
```
class Bulk_quote : public Disc_quote {
  public:
  using Disc_quote::Disc_quote; // inherit Disc_quote's constructors
  double net_price(std::size_t) const;
};
```
- When we define a class as publicly inherited from another, the derived
class should reflect “Is A” relationship and “Has A” relationship to the base class.
