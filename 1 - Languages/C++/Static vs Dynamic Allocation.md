# Summary

|**Type**|**Example**|**Lifetime**|**Memory Location**|**Cleanup**|**Shared Across Instances?**|
|---|---|---|---|---|---|
|**Statically Allocated Variable**|`int x = 10;`|Scope-limited|Stack|Automatic (when scope ends)|No|
|**Statically Allocated Object**|`MyClass obj;`|Scope-limited|Stack|Automatic (when scope ends)|No|
|**Dynamically Allocated Variable**|`int* x = new int(10);`|Manual (until `delete`)|Heap|Manual (`delete` required)|No|
|**Dynamically Allocated Object**|`MyClass* obj = new MyClass();`|Manual (until `delete`)|Heap|Manual (`delete` required)|No|
|**Statically Allocated Static Variable**|`static int x;`|Entire program lifetime|Data Segment|Automatic (program ends)|N/A|
|**Statically Allocated Static Object**|`static MyClass obj;`|Entire program lifetime|Data Segment|Automatic (program ends)|N/A|
|**Dynamically Allocated Static Variable**|`static int* x = new int(10);`|Entire program lifetime|Heap|Manual (`delete` required)|N/A|
|**Dynamically Allocated Static Object**|`static MyClass* obj = new MyClass();`|Entire program lifetime|Heap|Manual (`delete` required)|N/A|

### **Explanation of Table Columns**

- **Lifetime**: How long the variable/object exists in memory (e.g., limited to scope or entire program).
- **Memory Location**: Whether the variable resides in the stack, heap, or data segment of RAM.
- **Cleanup**: How memory is managed (automatic or requires manual cleanup using `delete`).
- **Shared Across Instances**: Whether the variable is shared among multiple instances of a class (applies to `static` members).
