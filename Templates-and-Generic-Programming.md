- Both object-oriented programming (OOP) and generic programming deal with types
that are not known at the time the program is written. The distinction between the
two is that OOP deals with types that are not known until run time, whereas in
generic programming the types become known during compilation.
- In a template definition, the template parameter list cannot be empty.
- Each type parameter of template must be preceded by the keyword **class** or **typename**.
- In addition to defining type parameters, we can define templates that take nontype parameters.
- Template arguments used for nontype template parameters must be constant expressions.
- The **inline** or **constexpr** specifier follows the template parameter list and precedes the return type.
- unlike nontemplate code, headers for templates typically include definitions as well as declarations.
- By default, a member of an instantiated class template is instantiated only if the member is used.
- There is one exception to the rule that we must supply template arguments when we use a class template type. Inside the scope of the class template itself, we may use the name of the template without arguments.
- Template Type Aliases
```
typedef Blob<string> StrBlob;
template<typename T> using twin = pair<T, T>;
template <typename T> using partNo = pair<T, unsigned>;
```
- static Members of Class Templates
```
template <typename T> class Foo {
public:
static std::size_t count() { return ctr; }
// other interface members
private:
static std::size_t ctr;
// other implementation members
};
```
All objects of type Foo<X> share the same ctr object and count function.
- declarations for all the templates needed by a given file usually should appear together at the beginning of a file before any code that uses those names.
- if we want to use a type member of a template type parameter, we must explicitly tell the compiler that the name is a type. We do so by using the keyword typename:
```
template <typename T>
typename T::value_type top(const T& c)
{
   if (!c.empty())
     return c.back();
   else
     return typename T::value_type();
}
```
By default, the language assumes that a name accessed through the scope operator is not a type.
- if a class template provides default arguments for all of its template parameters, and we want to use those defaults, we must put an empty bracket pair following the template’s name.
- What, if any, are the differences between a type parameter that is declared as a typename and one that is declared as a class? When must typename be used?  
There is no difference. typename and class are interchangeable in the declaration of a type template parameter. You do, however, have to use class (and not typename) when declaring a template template parameter. When we want to inform the compiler that a name represents a type, we must use the keyword typename, not class.
- Member templates may not be virtual.
- When we define a member template outside the body of a class template, we must provide the template parameter list for the class template and for the function template. The parameter list for the class template comes first, followed by the member’s own template parameter list:
```
template <typename T> // type parameter for the class
template <typename It> // type parameter for the constructor
Blob<T>::Blob(It b, It e):
data(std::make_shared<std::vector<T>>(b, e)) {
}
```
- When two or more separately compiled source files use the same template with the same template
arguments, there is an instantiation of that template in each of those files.
- There must be an explicit instantiation definition somewhere in the program for every instantiation declaration.
- Instantiation definitions instantiate all members
- Only a very limited number of conversions are automatically applied to such arguments. Rather
than converting the arguments, the compiler generates a new instantiation.  
As usual, top-level consts in either the parameter or the argument are ignored. The only other conversions performed in a call to a function template are
  - const conversions: A function parameter that is a reference (or pointer) to a const can be passed a reference (or pointer) to a nonconst object.
  - Array- or function-to-pointer conversions: If the function parameter is not a reference type, then the normal pointer conversion will be applied to arguments of array or function type. An array argument will be converted to a pointer to its first element. Similarly, a function argument will be converted to a pointer to the function’s type.
- **const** conversions and array or function to pointer are the only automatic conversions for arguments to parameters with template types.
- A template type parameter can be used as the type of more than one function parameter. Because there are limited conversions, the arguments to such parameters must have essentially the same type. If the deduced types do not match, then the call is an error.
```
long lng;
compare(lng, 1024); // error: cannot instantiate compare(long, int)
```
- Normal conversions are applied to arguments whose type is not a template parameter.
- Explicit template argument(s) are matched to corresponding template parameter(s) from left to right;An explicit template argument may be omitted only for the trailing (right-most) parameters.
```
// poor design: users must explicitly specify all three template parameters
template <typename T1, typename T2, typename T3>
T3 alternative_sum(T2, T1);
// error: can't infer initial template parameters
auto val3 = alternative_sum<long long>(i, lng);
// ok: all three parameters are explicitly specified
auto val2 = alternative_sum<long long, int, long>(i, lng);
```
- a trailing return
```
// a trailing return lets us declare the return type after the parameter list is seen
template <typename It>
auto fcn(It beg, It end) -> decltype(*beg)
{
  // process the range
  return *beg; // return a reference to an element from the range
}
```
- Standard Type Transformation Templates
![Standard Type Transformation Templates](https://github.com/zcenao21/Cpp/blob/master/photo/Standard%20Type%20Transformation%20Templates.PNG?raw=true)
- Using **remove_reference** and a trailing return with **decltype**, we can write our function to return a copy of an element’s value:
```
template <typename It> auto fcn2(It beg, It end) ->
typename remove_reference<decltype(*beg)>::type
{
  // process the range
  return *beg; // return a copy of an element from the range
}
```
- When we initialize or assign a function pointer from a function
template, the compiler uses the type of the pointer to deduce the template argument(s).
```
template <typename T> int compare(const T&, const T&);
// pf1 points to the instantiation int compare(const int&, const int&)
int (*pf1)(const int&, const int&) = compare;
```
- function’s parameter p is a reference to a template type parameter T, it is important to keep in mind two points:
  - Normal reference binding rules apply;
  - and consts are low level, not top level.
```
template <typename T> void f1(T&); // argument must be an lvalue
// calls to f1 use the referred-to type of the argument as the template parameter type
f1(i); // i is an int; template parameter T is int
f1(ci); // ci is a const int; template parameter T is const int
f1(5); // error: argument to a & parameter must be an lvalue
```
- the language defines two exceptions to normal binding rules
  1. When we pass an lvalue (e.g., i) to a function parameter that is an rvalue reference to a template type parameter (e.g, T&&), the compiler deduces the template type parameter as the argument’s lvalue reference type.
  2. If we indirectly create a reference to a reference, then those references “collapse”. That is, for a given type X:
    - **X& &**, **X& &&**, and **X&& &** all collapse to type **X&**
    - The type **X&& &&** collapses to **X&&**  
- In practice, rvalue reference parameters are used in one of two contexts:
  1. Either the template is forwarding its arguments,
  2. or the template is overloaded.
- A function parameter that is an rvalue reference to a template type
parameter (i.e., T&&) preserves the constness and lvalue/rvalue property of its corresponding argument.
- A function parameter, like any other variable, is an lvalue expression.
```
template <typename F, typename T1, typename T2>
void flip2(F f, T1 &&t1, T2 &&t2)
{
  f(t2, t1);
}
```
```
void g(int &&i, int& j)
{
  cout << i << " " << j << endl;
}
```
```
flip2(g, i, 42); // error: can't initialize int&& from an lvalue
```
- Like move, **forward**(std::forward) is defined in the utility header.
- As with **std::move**, it’s a good idea not to provide a using declaration for **std::forward**.
- Function matching is affected by the presence of function templates in the following ways:
  1. The candidate functions for a call include any function-template instantiation for which template argument deduction succeeds.
  2. The candidate function templates are always viable, because template argument deduction will have eliminated any templates that are not viable.
  3. As usual, the viable functions (template and nontemplate) are ranked by the conversions, if any, needed to make the call. Of course, the conversions used to call a function template are quite limited.
  4. Also as usual, if exactly one function provides a better match than any of the others, that function is selected. However, if there are several functions that provide an equally good match, then:
    - If there is only one nontemplate function in the set of equally good matches, the nontemplate function is called.
    - If there are no nontemplate functions in the set, but there are multiple function templates, and one of these templates is more specialized than any of the others, the more specialized function template is called.
    - Otherwise, the call is ambiguous.
- Ordinarily, if we use a function that we forgot to declare, our code won’t compile. Not so with functions that overload a template function.
- There are two kinds of parameter packs:
  1. A **template parameter pack** represents zero or more template parameters, and
  2. a **function parameter pack** represents zero or more function parameters.
- A declaration for the nonvariadic version must be in scope when the variadic version is defined. Otherwise, the variadic function will recurse indefinitely.
- Pack Expansions
```
// call debug_rep on each argument in the call to print
template <typename... Args>
ostream &errorMsg(ostream &os, const Args&... rest)
{
// print(os, debug_rep(a1), debug_rep(a2), ..., debug_rep(an)
return print(os, debug_rep(rest)...);
}
```
```
errorMsg(cerr, fcnName, code.num(), otherData, "other",
item);
```
```
print(cerr, debug_rep(fcnName), debug_rep(code.num()),
debug_rep(otherData), debug_rep("otherData"),
debug_rep(item));
```
```
// passes the pack to debug_rep; print(os, debug_rep(a1, a2, ..., an))
print(os, debug_rep(rest...)); // error: no matching function to call
```
```
print(cerr, debug_rep(fcnName, code.num(),
otherData, "otherData", item));
```
- Under the new standard, we can use variadic templates together with forward to write functions that pass their arguments unchanged to some other function.
- Specializations instantiate a template; they do not overload it. As a result, specializations do not affect function matching.
- We can partially specialize only a class template. We cannot partially specialize a function template.
