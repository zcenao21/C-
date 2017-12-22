- ![a](https://github.com/zcenao21/Photo/blob/master/IOlibrary.PNG?raw=true)
- we can use an object of an inherited class as if it were an object of the same type as the class from which it inherits.
- Reaching end-of-file sets both eofbit and failbit.
- Flushing the Output Buffer
```
cout << "hi!" << endl; // writes hi and a newline, then flushes the buffer
cout << "hi!" << flush; // writes hi, then flushes the buffer; adds no data
cout << "hi!" << ends; // writes hi and a null, then flushes the buffer
```
- The unitbuf Manipulator
```
cout << unitbuf; // all writes will be flushed immediately
// any output is flushed immediately, no buffering
cout << nounitbuf; // returns to normal buffering
```
- When an fstream object is destroyed, close is called
-  ![a](https://github.com/zcenao21/Photo/blob/master/filemode.PNG?raw=true)
- The only way to preserve the existing data in a file opened by an ofstream is to specify app or in mode explicitly.
- Any time open is called, the file mode is set, either explicitly or implicitly. Whenever a mode is not specified, the default value is used.
- stringstream-Specific Operations
![a](https://github.com/zcenao21/Photo/blob/master/stringstream.PNG?raw=true)
