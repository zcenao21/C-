- Sequential Container Types
![a](https://github.com/zcenao21/Photo/blob/master/SequentialContainer.PNG?raw=true)
- The library containers almost
certainly perform as well as (and usually better than) even the most carefully
crafted alternatives. Modern C++ programs should use the library containers
rather than more primitive structures like arrays.
- Ordinarily, it is best to use vector unless there is a good reason to prefer
another container.
- a few rules of thumb that apply to selecting which container to use:
Unless you have a reason to use another container, use a vector.
 - If your program has lots of small elements and space overhead matters, don’t use list or forward_list.
 - If the program requires random access to elements, use a vector or a deque.
 - If the program needs to insert or delete elements in the middle of the container, use a list or forward_list.
 - If the program needs to insert or delete elements at the front and the back, but not in the middle, use a deque.
 - If the program needs to insert elements in the middle of the container only while reading input, and subsequently needs random access to the elements:
    1. First, decide whether you actually need to add elements in the middle of a container. It is often easier to append to a vector and then call the library sort function to reorder the container when you’re done with input.
    2. If you must insert into the middle, consider using a list for the input phase. Once the input is complete, copy the list into a vector.
- When write access is not needed, use **cbegin** and **cend**.
- Container Operations ![a](https://github.com/zcenao21/Photo/blob/master/ContainerOperations.PNG?raw=true)
- Defining and Initializing Containers
![b](https://github.com/zcenao21/Photo/blob/master/DefiningandIntializingContainer.PNG?raw=true)
- There are two ways to create a new container as a copy of another one: We can directly copy the container, or (excepting array) we can copy a range of elements denoted by a pair of iterators.
- To create a container as a copy of another container, the container and element types must match. When we pass iterators, there is no requirement that the container types be identical.
```
list<string> authors = {"Milton", "Shakespeare", "Austen"};
vector<const char*> articles = {"a", "an", "the"};
list<string> list2(authors); // ok: types match
deque<string> authList(authors); // error: container types don't match
vector<string> words(articles); // error: element types must match
// ok: converts const char* elements to string
forward_list<string> words(articles.begin(), articles.end());
```
- The constructors that take a size are valid only for sequential containers; they are not supported for the associative containers.
- Just as the size of a built-in array is part of its type, the size of a library array is part
of its type.
```
array<int, 42> // type is: array that holds 42 ints
array<string, 10> // type is: array that holds 10 strings
```
- If we list initialize the array, the number of the initializers must be equal to or less than the
size of the **array**. If there are fewer initializers than the size of the **array**, the initializers are used for the first *elements* and any remaining elements are value initialized.
- It is worth noting that although we cannot copy or assign objects of **built-in array** types, there is no such restriction on **array**.
- As with any container, the initializer must have the same type as the container we are creating. For arrays, the element type and the size must be the same, because the size of an array is part of its type.
- six ways to create and initialize a vector.
```
vector<int> a;
vector<int> b(10);
vector<int> c = {1,2,3};
vector<int> d=c;
vector<int> e(c.begin(), c.end());
vector<int> f(10, 4);
```
- Unlike built-in arrays, the library array type does allow assignment. The left-and right-hand operands must have the same type:
```
array<int, 10> a1 = {0,1,2,3,4,5,6,7,8,9};
array<int, 10> a2 = {0}; // elements all have value 0
a1 = a2; // replaces elements in a1
a2 = {0}; // error: cannot assign to an array from a braced list
```
- The sequential containers (except array) also define a member named **assign** that lets us **assign** from a different but compatible type, or assign from a subsequence of a container.
```
list<string> names;
vector<const char*> oldstyle;
names = oldstyle; // error: container types don't match
// ok: can convert from const char*to string
names.assign(oldstyle.cbegin(), oldstyle.cend());
```
A second version of **assign**
```
// equivalent to slist1.clear();
// followed by slist1.insert(slist1.begin(), 10, "Hiya!");
list<string> slist1(1); // one element, which is the empty string
slist1.assign(10, "Hiya!"); // ten elements; each one is Hiya !
```
- The **swap** operation exchanges the contents of two containers of the same type.
- Excepting **array**, **swap** does not copy, delete, or insert any elements and is guaranteed to run in constant time.
- Unlike how **swap** behaves for the other containers, swapping two arrays does exchange the elements. As a result, swapping two **arrays** requires time proportional to the number of elements in the **array**.
- Container Assignment Operations
![d](https://github.com/zcenao21/Photo/blob/master/ContainerAssignment.PNG?raw=true)
- Every container type supports the equality operators (== and !=); all the containers except the unordered associative containers also support the relational operators (>,>=, <, <=). The right- and left-hand operands must be the same kind of container and must hold elements of the same type.
- Operations That Add Elements to a Sequential Container
![e](https://github.com/zcenao21/Photo/blob/master/AddElementToSequentialContainer.PNG?raw=true)
- A deque
guarantees constant-time insert and delete of elements at the beginning and end of the container. As with vector, inserting elements other than at the front or back of a deque is a potentially expensive operation.
- inserting anywhere but at the end of a vector might be slow.
- It is legal to insert anywhere in a **vector**, **deque**, or **string**. However, doing so can be an expensive operation.
- When we call a push or insert member, we pass objects of the element type and those objects are copied into the container. When we call an emplace member, we pass arguments to a constructor for the element type.
```
// construct a Sales_data object at the end of c
// uses the three-argument Sales_data constructor
c.emplace_back("978-0590353403", 25, 15.99);
// error: there is no version of push_back that takes three arguments
c.push_back("978-0590353403", 25, 15.99);
// ok: we create a temporary Sales_data object to pass to push_back
c.push_back(Sales_data("978-0590353403", 25, 15.99));
```
- Operations to Access Elements in a Sequential Container
![c](https://github.com/zcenao21/Photo/blob/master/AccessElement.PNG?raw=true)
- The members that access elements in a container (i.e., **front**, **back**, **subscript**, and **at**) return references.
- Using an out-of-range value for an index is a serious programming error, but one that
the compiler will not detect. If we want to ensure that our index is valid, we can use the at member instead. The at member acts like the subscript operator, but if the index is invalid, at throws an out_of_range exception.
- **erase** Operations on Sequential Containers
![a](https://github.com/zcenao21/Photo/blob/master/EraseOperations.PNG?raw=true)
- Operations to Insert or Remove Elements in a forward_list
![a](https://github.com/zcenao21/Photo/blob/master/OperationsForwardList.PNG?raw=true)
- Sequential Container Size Operations
![a](https://github.com/zcenao21/Photo/blob/master/SequentialContainerResize.JPG?raw=true)
- After an operation that adds elements to a container
  - Iterators, pointers, and references to a vector or string are invalid if the container was reallocated. If no reallocation happens, indirect references to elements before the insertion remain valid; those to elements after the insertion are invalid.
  - Iterators, pointers, and references to a deque are invalid if we add elements anywhere but at the front or back. If we add at the front or back, iterators are invalidated, but references and pointers to existing elements are not.
  - Iterators, pointers, and references (including the off-the-end and the beforethe-beginning iterators) to a list or forward_list remain valid
- After we remove an element,
  - All other iterators, references, or pointers (including the off-the-end and the before-the-beginning iterators) to a list or forward_list remain valid.
  - All other iterators, references, or pointers to a deque are invalidated if the removed elements are anywhere but the front or back. If we remove elements at the back of the deque, the off-the-end iterator is invalidated but other iterators, references, and pointers are unaffected; they are also unaffected if we remove from the front.
  - All other iterators, references, or pointers to a vector or string remain valid for elements before the removal point. Note: The off-the-end iterator is always invalidated when we remove elements.
- Don’t cache the iterator returned from **end()** in loops that insert or delete elements in a **deque**, **string**, or **vector**.
- ![a](https://github.com/zcenao21/Photo/blob/master/ContainerSizeManagement.JPG?raw=true)
- Additional Ways to Construct **string** s
![b](https://github.com/zcenao21/Photo/blob/master/AdditionalConstructString.JPG?raw=true)
- ![a](https://github.com/zcenao21/Photo/blob/master/SubString.JPG?raw=true)
- Operations to Modify **string** s
![a](https://github.com/zcenao21/Photo/blob/master/ModifyStrings.JPG?raw=true)
- ![a](https://github.com/zcenao21/Photo/blob/master/stringSearchOperations.JPG?raw=true)
- The string search functions return string::size_type, which is an unsigned type. As a result, it is a bad idea to use an int, or other signed type, to hold the return from these functions
- ![a](https://github.com/zcenao21/Photo/blob/master/CompareString.JPG?raw=true)
- ![a](https://github.com/zcenao21/Photo/blob/master/ConvertStringToNumber.JPG?raw=true)
- ![a](https://github.com/zcenao21/Photo/blob/master/OperationsandTypesCommonTtotheContainerAdaptors.JPG?raw=true)
- By default both stack and queue are implemented in terms of deque, and a
priority_queue is implemented on a vector. We can override the default
container type by naming a sequential container as a second type argument when we create the adaptor:
```
// empty stack implemented on top of vector
stack<string, vector<string>> str_stk;
// str_stk2 is implemented on top of vector and initially holds a copy of svec
stack<string, vector<string>> str_stk2(svec);
```
- All adaptors:cannot bebuilt on an **array**,**forward_list**.  
**queue**:cannot bebuilt on a **vector**  
**priority_queue**:cannot bebuilt on a **list**
- ![a](https://github.com/zcenao21/Photo/blob/master/StackOperationsAddition.JPG?raw=true )
- **queue**, **priority_queue** Operations in Addition to Table 9.17 ![a](https://github.com/zcenao21/Photo/blob/master/QueuePriorityQueueAddition.JPG?raw=true)
