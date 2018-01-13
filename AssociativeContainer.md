- Associative Container Types
![Associative Container Types](https://github.com/zcenao21/Photo/blob/master/AssociativeContainerTypes.JPG?raw=true)
- The associative container iterators are bidirectional
- When we initialize a map, we have to supply both the key and the value. We wrap
each key–value pair inside curly braces:
```
{key, value}
```
- For the ordered containers—map, multimap, set, and multiset—the key type must
define a way to compare the elements. By default, the library uses the < operator for
the key type to compare the keys.
- Operations on Pairs ![OperationsOnPairs](https://github.com/zcenao21/Photo/blob/master/OperationsOnPairs.JPG?raw=true)
- ways to make Pairs
```
 1. {string,int}
 2. make_pair(string,int)
 3. pair<string,int>(str,i)
```
- ![AssociativeContianerAddtionalTypeAliases](https://github.com/zcenao21/Photo/blob/master/AssociativeContianerAddtionalTypeAliases.JPG?raw=true)
```
set<string>::value_type v1; // v1 is a string
set<string>::key_type v2; // v2 is a string
map<string, int>::value_type v3; // v3 is a pair<const string, int>
map<string, int>::key_type v4; // v4 is a string
map<string, int>::mapped_type v5; // v5 is an int
```
- ![AssociativeContainerInsertOperations](https://github.com/zcenao21/Photo/blob/master/AssociativeContainerInsertOperations.JPG?raw=true)
- It is essential to remember that the value_type of a map is a pair and that we can change the value but not the key member of that pair.
- the keys in a set are also const. We
can use a set iterator to read, but not write.
- In general, we do not use the generic algorithms with the associative
containers.   
  1. keys are const
  2. Associative containers can be used with the algorithms that read elements. However, many of these algorithms search the sequence. Because elements in an associative container can be found (quickly) by their key, it is almost always a bad idea to use a generic search algorithm.
- Removing Elements from an Associative Container
![Removin ElementsfromanAssociativeContainer](https://github.com/zcenao21/Photo/blob/master/Removin%20ElementsfromanAssociativeContainer.JPG?raw=true)
- Subscript Operation for map and unordered_map
![Subscrip Operationformapandunorderedmap](https://github.com/zcenao21/Photo/blob/master/Subscrip%20Operationformapandunorderedmap.JPG?raw=true)
- unlike other
subscript operators, if the key is not already present, a new element is created and inserted into the map for that key.
- Ordinarily, the type returned by dereferencing an iterator and
the type returned by the subscript operator are the same. Not so for maps: when we subscript a map, we get a mapped_type object; when we dereference a map iterator,
we get a value_type object.
- Operations to Find Elements in an Associative Container
![OperationstoFindElementsinanAssociativeContainer](https://github.com/zcenao21/Photo/blob/master/OperationstoFindElementsinanAssociativeContainer.JPG?raw=true)
- Three ways to find elements in multiset or multimap
   1. **find** and **count**
   ```
   string search_item("Alain de Botton"); // author we'll look for
   auto entries = authors.count(search_item); // number of elements
   auto iter = authors.find(search_item); // first entry for this author
   // loop through the number of entries there are for this author
   while(entries) {
   cout << iter->second << endl; // print each title
   ++iter; // advance to the next title
   --entries; // keep track of how many we've printed
   }
   ```
   2. **lower_bound** and **upper_bound**
   ```
   // beg and end denote the range of elements for this author
   for (auto beg = authors.lower_bound(search_item),
   end = authors.upper_bound(search_item);
   beg != end; ++beg)
   cout << beg->second << endl; // print each title
   ```
   3. use **equal_range**
   ```
   // pos holds iterators that denote the range of elements for this key
   for (auto pos = authors.equal_range(search_item);
   pos.first != pos.second; ++pos.first)
   cout << pos.first->second << endl; // print each title
   ```
- An unordered container is most useful when we have a key type for which there is no obvious ordering relationship among the
elements.
- Unordered Container Management Operations
![UnorderedContainerManagementOperations](https://github.com/zcenao21/Photo/blob/master/UnorderedContainerManagementOperations.JPG?raw=true)
