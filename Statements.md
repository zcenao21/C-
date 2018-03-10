- switch</br>
case labels must be integral constant expressions
```
int ival = 42;
switch(ch) {
case 3.14: // error: noninteger as case label
case ival: // error: nonconstant as case label
```
- It is an error for any two case labels to have the same value.
- Although it is not necessary to include a **break** after the last label of a **switch**, the safest course is to provide one. That way, if an additional **case** is added later, the **break** is already in place.
- Variables defined in a **while** condition or **while** body are created and destroyed on each iteration.
- It is worth remembering that the visibility of any object defined within the **for** header is limited to the body of the for loop.
- **for** header:Omitting condition is equivalent to writing **true** as the condition
- A **do while** ends with a semicolon after the parenthesized condition.
- Variables used in condition must be defined outside the body of the **do while** statement.
- C++ offers four jumps: **break**,
**continue**, and **goto**, **return**.
- 	The **goto** and the labeled statement to which it transfers control must be in the same function.
- As with a **switch** statement, a **goto** cannot transfer control from a point where an
initialized variable is out of scope to a point where that variable is in scope:
  ```
  goto end;
  int ix = 10; // error: goto bypasses an initialized variable definition end:
  // error: code here could use ix but the goto bypassed its declaration
  ix = 42;
  ```
