## tuple
- Operations on tuples
![Operations on tuples](https://github.com/zcenao21/Cpp/blob/master/photo/Operations%20on%20tuples.PNG?raw=true)
- A tuple is most useful when we want to combine some data into a single object but do not want to bother to define a data structure to represent those data.
- **tuple** constructor is explicit, so we must use the direct initialization syntax.
- We can compare two tuples only
if they have the same number of members. Moreover, to use the equality or inequality operators, it must be legal to compare each pair of members using the **==** operator;to use the relational operators, it must be legal to use **<**.

## bitset
- Ways to Initialize a bitset
![Ways to Initialize a bitset](https://github.com/zcenao21/Cpp/blob/master/photo/WaystoInitialize%20abitset.PNG?raw=true)
- bitset Operations
![bitset Operations](https://github.com/zcenao21/Cpp/blob/master/photo/bitsetOperations.PNG?raw=true)

## Regular Expression
- Regular Expression Library Components
![Regular Expression Library Components](https://github.com/zcenao21/Cpp/blob/master/photo/Regular%20Expression%20Library%20Components.PNG?raw=true)
- Arguments to regex_search and regex_match
![Arguments to regex_search and regex_match](https://github.com/zcenao21/Cpp/blob/master/photo/Arguments%20to%20regex_search%20and%20regex_match.PNG?raw=true)
- regex (and wregex) Operations
![regex (and wregex) Operations](https://github.com/zcenao21/Cpp/blob/master/photo/regex%20Operations.PNG?raw=true)
- It is important to realize that the syntactic correctness of a regular expression
is evaluated at run time.
- Regular Expression Error Conditions
![Regular Expression Error Conditions](https://github.com/zcenao21/Cpp/blob/master/photo/Regular%20Expression%20Error%20Conditions.PNG?raw=true)
- Regular Expression Library Classes
![Regular Expression Library Classes](https://github.com/zcenao21/Cpp/blob/master/photo/Regular%20Expression%20Library%20Classes.PNG?raw=true)
- smatch Operations
![smatch Operations](https://github.com/zcenao21/Cpp/blob/master/photo/smatch%20Operations.PNG?raw=true)
- Submatch Operations
![Submatch Operations](https://github.com/zcenao21/Cpp/blob/master/photo/Submatch%20Operations.PNG?raw=true)
- ECMAScript regular-expression language:
  - \\{d} represents a single digit and \\{d}{n} represents a sequence of n digits. (E.g., \{d}{3} matches a sequence of three digits.)
  - A collection of characters inside square brackets allows a match to any of those characters. (E.g., [-. ] matches a dash, a dot, or a space. Note that a dot has no special meaning inside brackets.)
  - A component followed by ’?’ is optional. (E.g., \\{d}{3}[-. ]?\\{d}{4} matches three digits followed by an optional dash, period, or space, followed by four more digits. This pattern would match 555-0132 or 555.0132 or 555 0132 or 5550132.)
  - Like C++, ECMAScript uses a backslash to indicate that a character should represent itself, rather than its special meaning. Because our pattern includes parentheses, which are special characters in ECMAScript, we must represent the parentheses that are part of our pattern as \\(or \\).
- Regular Expression Replace Operations
![Regular Expression Replace Operations](https://github.com/zcenao21/Cpp/blob/master/photo/Regular%20Expression%20Replace%20Operations.PNG?raw=true)
- Match Flags
![Match Flags](https://github.com/zcenao21/Cpp/blob/master/photo/Match%20Flags.PNG?raw=true)
- The match and format flags have type match_flag_type. These values are defined in a namespace named regex_constants.
```
using std::regex_constants::format_no_copy;
```

## Random Numbers
- Prior to the new standard, both C
and C++ relied on a simple C library function named **rand**. That function produces pseudorandom integers that are uniformly distributed in the range from 0 to a systemdependent maximum value that is at least 32767.
- An engine
generates a sequence of unsigned random numbers. A distribution uses an engine to
generate random numbers of a specified type, in a given range, distributed according to a particular probability distribution.
- Random Number Library Components
![Random Number Library Components](https://github.com/zcenao21/Cpp/blob/master/photo/Random%20Number%20Library%20Components.PNG?raw=true)
- Random Number Engine Operations
![Random Number Engine Operations](https://github.com/zcenao21/Cpp/blob/master/photo/Random%20Number%20Engine%20Operations.PNG?raw=true)
- When we refer to a random-number generator, we mean the combination of a distribution object with an engine.
- it is worth noting that the output of calling a default_random_engine object is similar to the output of rand.
- Even though the numbers that are generated appear to be random, a given generator
returns the same sequence of numbers each time it is run.
- The right way to write our function is to make the engine and associated  distribution objects **static**.
```
// returns a vector of 100 uniformly distributed random numbers
vector<unsigned> good_randVec()
{
  // because engines and distributions retain state, they usually should be
  // defined as static so that new numbers are generated on each call
  static default_random_engine e;
  static uniform_int_distribution<unsigned> u(0,9);
  vector<unsigned> ret;
  for (size_t i = 0; i < 100; ++i)
  ret.push_back(u(e));
  return ret;
}
```
- A seed is a value that an engine can use to cause it to start generating numbers at a new point in its sequence.
- Picking a good seed, the most common approach is to call the system time function. This function, defined in the **ctime** header,
```
default_random_engine e1(time(0)); // a somewhat random seed
```
Because **time** returns time as the number of seconds, this seed is useful only for applications that generate the seed at second-level, or longer, intervals.
- Distribution Operations
![Distribution Operations](https://github.com/zcenao21/Cpp/blob/master/photo/Distribution%20Operations.PNG?raw=true)
- Because the distribution types have only one template parameter, when we want to use the default we must remember to follow the template’s name with empty angle brackets to signify that we want the default.
```
// empty <> signify we want to use the default result type
uniform_real_distribution<> u(0,1); // generates double by default
```

## The IO Library Revisited
- Manipulators Defined in iostream
![Manipulators Defined in iostream](https://github.com/zcenao21/Cpp/blob/master/photo/Manipulators%20Defined%20in%20iostream.PNG?raw=true)
- Manipulators are used for two broad categories of output control: controlling the presentation of numeric values and controlling the amount and placement of padding.
- Manipulators that change the format state of the stream usually leave the format state changed for all subsequent IO.
- The **hex**, **oct**, and **dec** manipulators affect only integral operands; the representation of floating-point values is unaffected.
- By default, precision controls the total number of digits that are printed. When printed, floating-point values are rounded, not truncated, to the current precision.
- The setprecision manipulators and other manipulators that take arguments are defined in the **iomanip** header.
- Manipulators Defined in iomanip
![Manipulators Defined in iomanip](https://github.com/zcenao21/Cpp/blob/master/photo/Manipulators%20Defined%20in%20iomanip.PNG?raw=true)
- Single-Byte Low-Level IO Operations
![Single-Byte Low-Level IO Operations](https://github.com/zcenao21/Cpp/blob/master/photo/Single-Byte%20Low-Level-IO-Operations.PNG?raw=true)
- Multi-Byte Low-Level IO Operations
![Multi-Byte Low-Level IO Operations](https://github.com/zcenao21/Cpp/blob/master/photo/Multi-Byte%20Low-Level-IO-Operations.PNG?raw=true)
- Random IO is an inherently system-dependent. To understand how to use these features, you must consult your system’s documentation.
- On most systems, the streams bound to **cin**, **cout**, **cerr**, and **clog** do not support random access.
- Seek and Tell Functions
![Seek and Tell Functions](https://github.com/zcenao21/Cpp/blob/master/photo/Seek-and-Tell-Functions.PNG?raw=true)
- Because there is only a single marker, we must do a seek to reposition the marker whenever we switch between reading and writing.
