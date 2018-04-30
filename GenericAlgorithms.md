- Algorithms may change the values of the elements stored in the container, and they may move elements around within the container. They do not, however, ever add or remove elements directly.
- **equal** makes one critically important assumption: It assumes that the second sequence is at least as big as the first.
```
// roster2 should have at least as many elements as roster1
equal(roster1.cbegin(), roster1.cend(), roster2.cbegin());
```
- Algorithms that take a single iterator denoting a second sequence assume that the second sequence is at least as large at the first.
- Assuming v is a vector<double>, what, if anything, is wrong with calling ```accumulate(v.cbegin(), v.cend(), 0)```?  
*the result will be integer value.*
- Algorithms that write to a destination iterator assume the destination is large
enough to hold the number of elements being written.
- **back_inserter** takes a reference to a container and returns an insert iterator
bound to that container. When we assign through that iterator, the assignment calls
**push_back** to add an element with the given value to the container:
```
vector<int> vec; // empty vector
// ok: back_inserter creates an insert iterator that adds elements to vec
fill_n(back_inserter(vec), 10, 0); // appends ten elements to vec
```
- how to get size of array
```
int a1[] = {0,1,2,3,4,5,6,7,8,9};
sizeof(a1)/sizeof(*a1);
```
- We said that algorithms do not change the size of the containers over which they operate. Why doesn’t the use of back_inserter
invalidate this claim?   
Inserters like **back_inserter** is part of <**iterator**> rather than <**algorithm**>.
- callable object: functions and function pointers, classes that overload the function-call operator,lambda expressions
- lamba expression
```
[capture list] (parameter list) -> return type { function body }
```
- Lambdas with function bodies that contain anything other than a single return statement that do not specify a return type return void.
- Lambda Capture List
![lambdacapturelist](https://github.com/zcenao21/Cpp/blob/master/photo/LambdaCapturelist.JPG?raw=true)
- When we capture a variable by reference, we must ensure that the variable exists at the time that the lambda executes.
- The **&** tells the compiler to capture by reference, and the **=** says the values are captured by value.
- By default, a lambda may not change the value of a variable that it copies by value. If we want to be able to change the value of a captured variable, we must follow the parameter list with the keyword **mutable**. Whether a variable captured by reference can be changed (as usual) depends only on whether that reference refers to a const or nonconst type.  
```
void fcn3()
{
  size_t v1 = 42; // local variable
  // f can change the value of the variables it captures
  auto f = [v1] () mutable { return ++v1; };
  v1 = 0;
  auto j = f(); // j is 43
}
```
- By default, if a lambda body contains any statements other than a **return**, that lambda is assumed to return **void**.
- Lambda expressions are most useful for simple operations that we do not need to use in more than one or two places.
- Insert Iterator Operations
![InsertIteratorOperations](https://github.com/zcenao21/Cpp/blob/master/photo/InsertIteratorOperations.JPG?raw=true)
- istream_iterator Operations and ostream_iterator Operations
![IOStreamIterator](https://github.com/zcenao21/Cpp/blob/master/photo/IOStreamIterator.JPG?raw=true)
- useful way to initialize container
```
istream_iterator<int> in_iter(cin), eof; // read ints from cin
vector<int> vec(in_iter, eof); // construct vec from an iterator range
```
- Operations on ostream_iterators
```
ostream_iterator<int> out_iter(cout, " ");
for (auto e : vec)
    *out_iter++ = e; // or: out_iter = e; // the assignment writes this element to cout
cout << endl;
```
```
copy(vec.begin(), vec.end(), out_iter);
cout << endl;
```
- The fact that reverse iterators are intended to represent ranges and that
these ranges are asymmetric has an important consequence: When we initialize or assign a reverse iterator from a plain iterator, the resulting iterator does not refer to the same element as the original.
- ![IteratorCatigories](https://github.com/zcenao21/Cpp/blob/master/photo/IteratorCatigories.JPG?raw=true)
- List the five iterator categories and the operations that each supports.
Input iterators : ==, !=, ++, \*, ->  
Output iterators : ++, *  
Forward iterators : ==, !=, ++, \*, ->  
Bidirectional iterators : ==, !=, ++, --, \*, ->  
Random-access iterators : ==, !=, <, <=, >, >=, ++, --, +, +=, -, -=, -(two iterators), \*, ->, iter[n] == * (iter + n)
- ![AlgorithmsThatAreMembersofListandForwrdList](https://github.com/zcenao21/Cpp/blob/master/photo/AlgorithmsThatAreMembersofListandForwrdList.JPG?raw=true)
- ![ListAndForwardListSpliceMember](https://github.com/zcenao21/Cpp/blob/master/photo/ListAndForwardListSpliceMember.JPG?raw=true)
