- Dynamically allocated objects have a lifetime that is independent
of where they are created; they exist until they are explicitly freed.
- Static memory is used for local
static objects, for class static data members, and for variables defined outside any function.
- Operations Common to shared_ptr and unique_ptr
![OperationsCommontoshared_ptrandunique_ptr](https://github.com/zcenao21/Photo/blob/master/OperationsCommontoshared_ptrandunique_ptr.JPG?raw=true)
- Operations Specific to shared_ptr  
![OperationsSpecifictoshared_ptr](https://github.com/zcenao21/Photo/blob/master/OperationsSpecifictoshared_ptr.JPG?raw=true)
- If you put shared_ptrs in a container, and you subsequently need to use some, but not all, of the elements, remember to erase the elements you no longer need.
- Programs tend to use dynamic memory for one of three purposes:
  1. They don’t know how many objects they’ll need
  2. They don’t know the precise type of the objects they need
  3. They want to share data between several objects
- we can use **auto** only with a single initializer inside parentheses.
- It is legal to use new to allocate **const** objects.
- By default, if new is unable to allocate the requested storage, it throws an exception of type bad_alloc
```
// if allocation fails, new returns a null pointer
int *p1 = new int; // if allocation fails, new throws std::bad_alloc
int *p2 = new (nothrow) int; // if allocation fails, new returns a null pointer
```
- The pointer we pass to delete must either point to dynamically allocated memory or be a null pointer
- Dynamic memory managed through built-in pointers (rather than smart
pointers) exists until it is explicitly freed.
- A **dangling pointer** is one that refers to memory that once held an
object but no longer does so.
- Other Ways to Define and Change shared_ptrs
![Other Ways to Define and Change shared_ptrs](https://github.com/zcenao21/Photo/blob/master/Other%20Ways%20to%20Define%20and%20Change%20shared_ptrs.JPG?raw=true)
- The smart pointer constructors that take pointers are **explicit**;we must use the direct form of initialization to initialize a smart pointer:
```
shared_ptr<int> p1 = new int(1024); // error: must use direct initialization
shared_ptr<int> p2(new int(1024)); // ok: uses direct initialization
```
For the same reason, a function that returns a **shared_ptr** cannot implicitly convert a plain pointer in its **return** statement:
```
shared_ptr<int> clone(int p) {
return new int(p); // error: implicit conversion to shared_ptr<int>
}
```
- It is dangerous to use a built-in pointer to access an object owned by a
smart pointer, because we may not know when that object is destroyed.
- Use get only to pass access to the pointer to code that you know will not
delete the pointer. In particular, never use get to initialize or assign to
another smart pointer.
- To use smart pointers
correctly, we must adhere to a set of conventions:  
  1. Don’t use the same built-in pointer value to initialize (or reset) more than one smart pointer.
  2. Don’t delete the pointer returned from get().
  Don’t use get() to initialize or reset another smart pointer.
  3. If you use a pointer returned by get(), remember that the pointer will become invalid when the last corresponding smart pointer goes away.
  4. If you use a smart pointer to manage a resource other than memory allocated by new, remember to pass a deleter
- Because a unique_ptr owns the object to which it points, unique_ptr does not
support ordinary copy or assignment.
- Although we can’t copy or assign a unique_ptr, we can transfer ownership from one (nonconst) unique_ptr to another by calling release or reset:
```
// transfers ownership from p1 (which points to the string Stegosaurus) to p2
unique_ptr<string> p2(p1.release()); // release makes p1 null
unique_ptr<string> p3(new string("Trex"));
// transfers ownership from p3 to p2
p2.reset(p3.release()); // reset deletes the memory to which p2 had
pointed
```
- Calling release breaks the connection between a unique_ptr and the object it
had been managing. Often the pointer returned by release is used to initialize or assign another smart pointer. However, if we do not use
another smart pointer to hold the pointer returned from release, our program takes over responsibility for freeing that resource:
```
p2.release(); // WRONG: p2 won't free the memory and we've lost the pointer
auto p = p2.release(); // ok, but we must remember to delete(p)
```
- unique_ptr Operations
![unique_ptr Operations](https://github.com/zcenao21/Photo/blob/master/unique_ptr%20Operations.JPG?raw=true)
- weak_ptrs
![weak_ptrs](https://github.com/zcenao21/Photo/blob/master/weak_ptrs.JPG?raw=true)
- Binding a weak_ptr to a shared_ptr does not change the reference count of that shared_ptr.
- Because the object might no longer exist, we cannot use a weak_ptr to access its object directly. To access that object, we must call lock.
```
if (shared_ptr<int> np = wp.lock()) { // true if np is not null
// inside the if, np shares its object with p
}
```
- It is important to remember that what we call a dynamic array does not have
an array type.
- We ask new to allocate an array of objects by specifying the number of objects to
allocate in a pair of square brackets after a type name.
```
int *pia = new int[get_size()]; // pia points to the first of these ints
```
```
typedef int arrT[42]; // arrT names the type array of 42 ints
int *p = new arrT; // allocates an array of 42 ints; p points to the first one
```
```
int *pia2 = new int[10]();
```
```
int *pia3 = new int[10]{0,1,2,3,4,5,6,7,8,9};
```
- To free a dynamic array, we use a special form of delete that includes an empty pair
of square brackets:
```
delete p; // p must point to a dynamically allocated object or be null
delete [] pa; // pa must point to a dynamically allocated array or be null
```
- To use a unique_ptr to manage a dynamic array, we must include a pair of
empty brackets after the object type:
```
// up points to an array of ten uninitialized ints
unique_ptr<int[]> up(new int[10]);
up.release(); // automatically uses delete[] to destroy its pointer
```
- unique_ptrs to Arrays
![unique_ptrs to Arrays](https://github.com/zcenao21/Photo/blob/master/unique_ptrs%20to%20Arrays.JPG?raw=true)
- If we want to use a shared_ptr to manage a dynamic array, we
must provide our own deleter:
```
// to use a shared_ptr we must supply a deleter
shared_ptr<int> sp(new int[10], [](int *p) { delete[] p; });
sp.reset(); // uses the lambda we supplied that uses delete[] to free the array
```
-  shared_ptrs don't have subscript operator and don't support pointer arithmetic.
```
for (size_t i = 0; i != 10; ++i)
*(sp.get() + i) = i; // use get to get a built-in pointer
```
- Standard allocator Class and Customized Algorithms
![Standard allocator Class and Customized Algorithms](https://github.com/zcenao21/Photo/blob/master/Standard%20allocator%20Class%20and%20Customized%20Algorithms.JPG?raw=true)
- how to use allocator
```
allocator<string> alloc; // object that can allocate strings
auto const p = alloc.allocate(n); // allocate n unconstructed strings
```
```
auto q = p; // q will point to one past the last constructed element
alloc.construct(q++); // *q is the empty string
alloc.construct(q++, 10, 'c'); // *q is cccccccccc
alloc.construct(q++, "hi"); // *q is hi!
```
```
cout << *p << endl; // ok: uses the string output operator
cout << *q << endl; // disaster: q points to unconstructed memory!
```
```
while (q != p)
alloc.destroy(--q); // free the strings we actually allocated
```
```
alloc.deallocate(p, n);
```
- allocator Algorithms
![allocator Algorithms](https://github.com/zcenao21/Photo/blob/master/allocator%20Algorithms.JPG?raw=true)
