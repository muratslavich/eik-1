# HashMap



**table** - array of Entry\[], the storage for references chain**loadFactor** - 0.75 by default**treshhold** - count of elements, when table size is doubled (capacity \* loadFactor)**capacity** - by default - 16\
new HashMap(capacity)new HashMap(capacity, loadFactor)\
In constructor table is initializedtable = new Entry\[capacity];\
\
Putting valuehashmap.put("0", "zero");

![](https://habrastorage.org/storage1/f5998744/554eeee7/3d597647/2f404c04.png)

1. The key is being checked for null.
2.
   1. true -> **putForNullKey(value)**
   2. all elements from table\[0] chain is being looked for null key element
   3. if element with null key is found, it's rewrited
   4. if don't, **addEntry(0, null, value, 0)** will be called.
3. Hash code is generated for key
4. **indexFor(hash, tableLength)** determine array position in table

hash & (length - 1)

1. Get list from those index. Hash and key of new element is comparing with hash and key of list element. If hash and key match value is rewritten.
2.
   1. If matches aren't founded, called **addEntry(hash, key, value, index)**
   2. If match is found (collision) -> new element is added to the top of the chain. Ref next is set to old element

\
\
Conclusion

![](https://hsto.org/storage1/9cd24c82/9dd96244/dc1dd8aa/72923c80.png)

* adding for O(1) time, because all new elements is added to the top of chain
* get/delete - O(1 + a), a - loadFactor (the worst - O(n))
* not synchronized
* null key able
