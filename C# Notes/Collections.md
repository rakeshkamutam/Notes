# C# Collections Notes

## Table of Contents
1. [Introduction to Collections](#introduction-to-collections)
2. [Non-Generic Collections](#non-generic-collections)
3. [Generic Collections](#generic-collections)
4. [Specialized Collections](#specialized-collections)
5. [Concurrent Collections](#concurrent-collections)
6. [Collection Interfaces](#collection-interfaces)
7. [Collection Performance Comparison](#collection-performance-comparison)
8. [Best Practices](#best-practices)

***

## Introduction to Collections

Collections are like **different types of containers** - each designed for specific storage needs. Think of arrays as fixed bookshelves, lists as expandable boxes, and dictionaries as phone books where you find values by keys. They provide dynamic sizing, rich functionality, and better organization than simple arrays.

```csharp
List names = new List { "Alice", "Bob", "Charlie" };
names.Add("David");
Console.WriteLine($"Count: {names.Count}"); // Output: Count: 4
```

[⬆ Back to Top](#table-of-contents)

***

## Non-Generic Collections

These are the "old school" collections where everything is stored as `Object` type. Think of them as a junk drawer - you can put anything in, but you have to cast (guess) what type it is when taking items out. They require boxing/unboxing which hurts performance and type safety.

### ArrayList
Like a magical expandable backpack that holds anything, but you need to cast when retrieving items.

```csharp
ArrayList arrayList = new ArrayList();
arrayList.Add("Hello");
arrayList.Add(42);
string text = (string)arrayList[0]; // Requires casting
```

### Hashtable
Old-school phone book - look up by key to get value, but requires casting for both keys and values.

```csharp
Hashtable hashtable = new Hashtable();
hashtable["name"] = "John";
string name = (string)hashtable["name"]; // Cast needed
```

### Stack
Last-In-First-Out container - like a stack of plates where you can only take from the top.

```csharp
Stack stack = new Stack();
stack.Push("First");
stack.Push("Second");
object top = stack.Pop(); // Returns "Second"
```

### Queue
First-In-First-Out container - like a line at a store where first person in line gets served first.

```csharp
Queue queue = new Queue();
queue.Enqueue("First");
object first = queue.Dequeue(); // Returns "First"
```

[⬆ Back to Top](#table-of-contents)

***

## Generic Collections

The modern, type-safe collections where you specify the exact type you're storing. No more casting nightmares! Think of them as labeled containers - if it says "Books" on the label, only books go in and only books come out.

### List
The workhorse collection - an expandable array that knows its type. Most commonly used collection for general purposes.

```csharp
List numbers = new List { 1, 2, 3 };
numbers.Add(4);           // No boxing
int first = numbers[0];   // No casting needed
```

### Dictionary
Type-safe phone book - fast lookups by key with guaranteed type safety for both keys and values.

```csharp
Dictionary ages = new Dictionary();
ages["Alice"] = 25;
int aliceAge = ages["Alice"]; // No casting!
```

### HashSet
Collection that automatically prevents duplicates - like a VIP list where each person can only appear once.

```csharp
HashSet uniqueNames = new HashSet();
uniqueNames.Add("Alice");
uniqueNames.Add("Alice"); // Won't add duplicate
```

### LinkedList
Chain of connected nodes - excellent for frequent insertions/deletions in the middle, like editing a document.

```csharp
LinkedList chain = new LinkedList();
chain.AddFirst("Middle");
chain.AddBefore(chain.First, "Start");
```

### Stack and Queue
Type-safe versions of their non-generic cousins - same behavior but with compile-time type checking.

```csharp
Stack stack = new Stack();
stack.Push(10);
int top = stack.Pop(); // Returns int, not object

Queue queue = new Queue();
queue.Enqueue("First");
string first = queue.Dequeue(); // Returns string
```

[⬆ Back to Top](#table-of-contents)

***

## Specialized Collections

Purpose-built collections for specific scenarios - like having specialized tools in your toolbox instead of trying to use a hammer for everything.

### NameValueCollection
String-based key-value pairs - perfect for configuration settings and web form data where everything is text.

```csharp
NameValueCollection config = new NameValueCollection();
config.Add("database", "localhost");
string db = config["database"];
```

### StringCollection
Collection specifically designed for strings - optimized for string operations and memory usage.

```csharp
StringCollection strings = new StringCollection();
strings.Add("Hello");
strings.Add("World");
```

### BitArray
Compact storage for boolean flags - like having a row of light switches where each position represents true/false.

```csharp
BitArray bits = new BitArray(8);
bits[0] = true;  // First bit is on
bits[2] = true;  // Third bit is on
```

[⬆ Back to Top](#table-of-contents)

***

## Concurrent Collections

Thread-safe collections for multi-threading scenarios - like having locks on your containers when multiple people need access simultaneously. Essential for applications where multiple threads modify collections.

### ConcurrentDictionary
Thread-safe dictionary that handles concurrent reads/writes safely - no more race conditions or corrupted data.

```csharp
ConcurrentDictionary concurrent = new ConcurrentDictionary();
concurrent.TryAdd("key1", 10);
concurrent.TryUpdate("key1", 20, 10); // Only updates if current value is 10
```

### ConcurrentQueue and ConcurrentStack
Thread-safe versions of Queue and Stack - multiple threads can safely add/remove items without conflicts.

```csharp
ConcurrentQueue queue = new ConcurrentQueue();
queue.Enqueue("Item1");
if (queue.TryDequeue(out string item)) { /* Use item */ }
```

### BlockingCollection
Smart collection that can block threads when empty or full - perfect for producer-consumer scenarios.

```csharp
BlockingCollection blocking = new BlockingCollection();
// Producer adds items, consumer waits for items to arrive
foreach (string item in blocking.GetConsumingEnumerable()) { /* Process */ }
```

[⬆ Back to Top](#table-of-contents)

***

## Collection Interfaces

Interfaces define contracts for collections - like having standard plugs that fit standard outlets. They enable writing flexible code that works with any collection implementing the interface.

### IEnumerable
The foundation interface that enables `foreach` loops - every collection implements this for iteration support.

```csharp
public void PrintItems(IEnumerable items)
{
    foreach (T item in items) Console.WriteLine(item);
}
```

### ICollection
Extends IEnumerable with basic operations - Count, Add, Remove, Clear. Think of it as the "basic container" contract.

```csharp
public void AddMultiple(ICollection collection, params T[] items)
{
    foreach (T item in items) collection.Add(item);
}
```

### IList
Adds indexed access and insertion capabilities - for collections where position matters and direct access is needed.

```csharp
public void InsertAt(IList list, int index, T item)
{
    list.Insert(index, item);
}
```

[⬆ Back to Top](#table-of-contents)

***

## Collection Performance Comparison

| Collection | Best For | Access Time | Memory Usage |
|------------|----------|-------------|--------------|
| Array | Fixed-size, fast access | O(1) | Lowest |
| List | General purpose, dynamic | O(1) | Low |
| Dictionary | Fast key lookups | O(1) | Medium |
| HashSet | Unique items, membership testing | O(1) | Medium |
| LinkedList | Frequent middle insertions | O(n) access | High |
| Queue/Stack | FIFO/LIFO operations | O(1) | Low |

**Rule of thumb:** Use List for most scenarios, Dictionary for lookups, HashSet for uniqueness.

```csharp
// Choose based on primary usage:
List names = new List();              // General storage
Dictionary employees = new Dictionary(); // Fast lookup
HashSet uniqueEmails = new HashSet(); // No duplicates
```

[⬆ Back to Top](#table-of-contents)

***

## Best Practices

**Golden Rules:** Always use generic collections, choose the right collection for your access pattern, and use interfaces for method parameters to keep code flexible.

```csharp
// ✅ Good: Use generic collections and interfaces
public void ProcessItems(IEnumerable items) { }

// ❌ Bad: Non-generic collections and concrete types
public void ProcessItems(ArrayList items) { }

// ✅ Best: Initialize with known capacity and use collection initializers
var fruits = new List(100) { "Apple", "Banana" };
```

**Memory Tip:** List for general use, Dictionary for lookups, HashSet for uniqueness, Queue/Stack for ordered processing.

[⬆ Back to Top](#table-of-contents)

***

### Summary

Collections are your data organization toolkit. **Generic collections are always preferred** for type safety and performance. **Choose based on access patterns** - List for general use, Dictionary for fast lookups, HashSet for uniqueness. **Use interfaces** in method signatures for flexibility, and always consider thread safety in multi-threaded applications.
