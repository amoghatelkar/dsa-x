# 🧠 DSA-X

> A modern, TypeScript-based **Data Structures Library** for JavaScript developers.  
> Build, learn, and use fundamental data structures with clean, production-ready code.

---

## 🚀 Overview

**DSA-X** provides a robust, modular, and type-safe implementation of essential data structures —  
from arrays and linked lists to trees, graphs, and tries — written in **TypeScript**.

The goal of DSA-X is to make data structures **easy to use, test, and understand**,  
both for learners and professionals looking to integrate them into real-world applications.

---

## 📦 Installation

You can install DSA-X using your favorite package manager:

```bash
# npm
npm install dsa-x

# yarn
yarn add dsa-x

# pnpm
pnpm add dsa-x

```       

---

## 📘 Table of Contents

### 🔹 Arrays
- [DynamicArray](#dynamicarray)
- [SparseArray](#sparsearray)

### 🔗 Linked Lists
- [SinglyLinkedList](#singlylinkedlist)
- [DoublyLinkedList](#doublylinkedlist)

### 🧱 Stacks
- [Stack](#stack)

### 🧱 Queues
- [Queue](#queue)
- [PriorityQueue](#priorityqueue)
- [CircularQueue](#circularqueue)

### Sets
- [HashSet](#hashset)

### 🌐 Graphs
- [Graph](#graph)
- [WeightedGraph](#weightedgraph)

### 🌲 Trees
- [BinarySearchTree](#binarysearchtree)
- [AVLTree](#avltree)
- [Trie](#trie)
- [MinHeap](#minheap)
- [MaxHeap](#maxheap)

---

## DynamicArray

A **resizable array implementation** similar to Java's `ArrayList`.  
It automatically **grows** and **shrinks** its internal capacity as elements are added or removed.  
Perfect for cases where the number of elements can change dynamically.

---

### Overview

The `DynamicArray<T>` class provides an efficient, type-safe array structure that supports:

- Constant-time access `O(1)`
- Amortized constant-time insertion `O(1)`
- Automatic resizing (doubling/halving)
- TypeScript generics for strong typing

---

### Constructor

```ts
const arr = new DynamicArray<number>(initialCapacity?: number);
```

**Parameters:**

- `initialCapacity` *(optional, default = 4)* — Initial internal storage size.

---

### Methods

#### `push(value: T): void`

Adds a new element at the end of the array.  
Automatically doubles capacity if the array is full.

```ts
arr.push(10);
arr.push(20);
```

---

#### `pop(): T | null`

Removes and returns the last element.  
Shrinks capacity if size becomes one-fourth of the total capacity.

```ts
const last = arr.pop(); // returns last element or null if empty
```

---

#### `get(index: number): T | null`

Returns the element at the specified index, or `null` if out of bounds.

```ts
console.log(arr.get(1)); // → 20
```

---

#### `set(index: number, value: T): void`

Updates the element at a given index.  
Throws `RangeError` if index is invalid.

```ts
arr.set(0, 99);
```

---

#### `size(): number`

Returns the current number of elements.

```ts
console.log(arr.size()); // → 2
```

---

#### `getCapacity(): number`

Returns the current allocated capacity.

```ts
console.log(arr.getCapacity()); // → 8 (if resized)
```

---

#### `toArray(): (T | null)[]`

Returns a trimmed copy of the array (without null padding).

```ts
console.log(arr.toArray()); // → [99, 20]
```

---

### Example Usage

```ts
import DynamicArray from "./DynamicArray";

const arr = new DynamicArray<number>(2);

arr.push(5);
arr.push(10);
arr.push(15); // triggers resize

console.log(arr.toArray()); // [5, 10, 15]
console.log("Size:", arr.size()); // 3
console.log("Capacity:", arr.getCapacity()); // 4

arr.pop();
console.log(arr.toArray()); // [5, 10]
```

---

### Complexity Analysis

| Operation | Time Complexity | Space Complexity |
|------------|----------------|------------------|
| `push()`   | O(1) amortized | O(n) |
| `pop()`    | O(1) | O(n) |
| `get()`    | O(1) | O(1) |
| `set()`    | O(1) | O(1) |
| `resize()` | O(n) | O(n) |

---

### Internal Design

- Uses a private array `data: (T | null)[]` for storage.
- Resizing strategy:
  - Double capacity when full.
  - Halve capacity when utilization drops below 25%.
- Maintains two properties:
  - `length`: current number of elements.
  - `capacity`: total allocated space.

---

[⬆️ Back to Top](#dynamicarray)

## SparseArray

A **memory-efficient array implementation** optimized for large arrays with many empty slots.  
Internally, it uses a **Map** to store only the indices that contain values — saving space when most entries are empty.

---

### Overview

The `SparseArray<T>` class is ideal for datasets where most indices are unoccupied.  
It provides fast lookups and flexible storage without allocating memory for unused elements.

**Key Features:**
- Stores only non-empty indices.
- Efficient `O(1)` average-time access and updates.
- Memory-efficient for sparse data.
- TypeScript generics ensure strong typing.

---

### Constructor

```ts
const arr = new SparseArray<number>(length?: number);
```

**Parameters:**

- `length` *(optional, default = 0)* — The logical length of the sparse array.

---

### Methods

#### `set(index: number, value: T): void`

Sets a value at a specific index.  
Automatically expands the logical length if necessary.  
Throws a `RangeError` if the index is negative.

```ts
arr.set(5, 42);
arr.set(100, 99);
```

---

#### `get(index: number): T | null`

Retrieves a value at the given index.  
Returns `null` if the index has no stored value.

```ts
console.log(arr.get(5)); // → 42
console.log(arr.get(10)); // → null
```

---

#### `delete(index: number): void`

Removes the value at the specified index (if any).

```ts
arr.delete(5);
```

---

#### `size(): number`

Returns the logical length of the array (highest index + 1).

```ts
console.log(arr.size()); // → 101
```

---

#### `keys(): number[]`

Returns an array of all active (non-empty) indices.

```ts
console.log(arr.keys()); // → [100]
```

---

#### `values(): T[]`

Returns all stored values without their indices.

```ts
console.log(arr.values()); // → [99]
```

---

#### `toObject(): Record<number, T>`

Converts the sparse array into a plain JavaScript object  
with numeric keys and stored values.

```ts
console.log(arr.toObject()); // → {100: 99}
```

---

### Example Usage

```ts
import SparseArray from "./SparseArray";

const arr = new SparseArray<number>(10);

arr.set(2, 50);
arr.set(5, 100);
arr.set(100, 999);

console.log(arr.get(5)); // 100
console.log(arr.get(20)); // null

arr.delete(2);

console.log(arr.keys()); // [5, 100]
console.log(arr.toObject()); // {5: 100, 100: 999}
```

---

### Complexity Analysis

| Operation     | Time Complexity | Space Complexity |
|----------------|----------------|------------------|
| `set()`        | O(1) average   | O(n)             |
| `get()`        | O(1) average   | O(1)             |
| `delete()`     | O(1) average   | O(1)             |
| `keys()`       | O(n)           | O(n)             |
| `values()`     | O(n)           | O(n)             |
| `toObject()`   | O(n)           | O(n)             |

---

### Internal Design

- Uses a private `Map<number, T>` called `data` for storage.
- Maintains a `length` representing the logical array size.
- No preallocation — only indices that have values are stored.
- Efficient for large arrays with many empty elements.

---

[⬆️ Back to Top](#sparsearray)

## LinkedList

A **generic singly linked list implementation** in TypeScript.  
It supports common operations such as **append**, **prepend**, **delete**, **find**, and **traversal**.

---

### Overview

The `LinkedList<T>` class provides an ordered, dynamic data structure where each node points to the next.  
This makes insertion and deletion efficient (O(1) at head or tail).

- Sequential structure with nodes connected by pointers.
- Ideal for use cases where array resizing or index shifting is expensive.
- Type-safe using TypeScript generics.

---

### Constructor

```ts
const list = new LinkedList<number>();
```

Creates an empty linked list of generic type `T`.

---

### Methods

#### `append(value: T): void`
Adds a new element to the **end** of the list.

```ts
list.append(10);
list.append(20);
```

---

#### `prepend(value: T): void`
Adds a new element to the **beginning** of the list.

```ts
list.prepend(5);
```

---

#### `delete(value: T): boolean`
Removes the **first occurrence** of the specified value.

```ts
list.delete(10); // returns true if deleted, false otherwise
```

---

#### `find(value: T): Node<T> | null`
Searches for the first node with the given value.

```ts
const node = list.find(20);
if (node) console.log(node.value);
```

---

#### `toArray(): T[]`
Converts the linked list to a regular JavaScript array.

```ts
console.log(list.toArray()); // [5, 20]
```

---

#### `clear(): void`
Removes all nodes from the linked list.

```ts
list.clear();
```

---

#### `length: number`
Returns the total number of nodes in the list.

```ts
console.log(list.length); // → 0 (after clearing)
```

---

### Example Usage

```ts
import { LinkedList } from "./LinkedList";

const list = new LinkedList<number>();

list.append(10);
list.append(20);
list.prepend(5);

console.log(list.toArray()); // [5, 10, 20]
console.log("Length:", list.length); // 3

list.delete(10);
console.log(list.toArray()); // [5, 20]

const found = list.find(20);
console.log(found?.value); // 20
```

---

### Complexity Analysis

| Operation     | Time Complexity | Space Complexity |
|----------------|----------------|------------------|
| `append()`     | O(1) | O(1) |
| `prepend()`    | O(1) | O(1) |
| `delete()`     | O(n) | O(1) |
| `find()`       | O(n) | O(1) |
| `toArray()`    | O(n) | O(n) |
| `clear()`      | O(1) | O(1) |

---

### Internal Design

- Each node is an instance of the `Node<T>` class with two properties:
  - `value`: the stored data
  - `next`: pointer to the next node or `null`
- The linked list maintains:
  - `head`: reference to the first node
  - `tail`: reference to the last node
  - `size`: current length of the list

---

[⬆️ Back to Top](#linkedlist)

## DoubleLinkedList

A **generic doubly linked list implementation** supporting **bidirectional traversal**, **efficient insertions**, and **deletions**.  
This data structure is ideal when you need quick access to both ends and frequent insertions/removals.

---

### Overview

The `DoubleLinkedList<T>` class provides:

- Efficient `O(1)` insertions and deletions at both ends
- Traversal in both **forward** and **reverse** directions
- TypeScript generics for strong typing
- Utility methods for conversion, searching, and clearing

---

### Constructor

```ts
const list = new DoubleLinkedList<number>();
```

Creates an empty doubly linked list.

---

### Methods

#### `append(value: T): void`

Adds an element at the **end** of the list.

```ts
list.append(10);
list.append(20);
```

---

#### `prepend(value: T): void`

Adds an element at the **beginning** of the list.

```ts
list.prepend(5);
```

---

#### `delete(value: T): boolean`

Removes the **first node** that matches the given value.  
Returns `true` if the element was deleted, else `false`.

```ts
list.delete(10);
```

---

#### `find(value: T): DoubleNode<T> | null`

Finds and returns the first node with the specified value.

```ts
const node = list.find(20);
if (node) console.log(node.value); // 20
```

---

#### `toArrayForward(): T[]`

Returns all elements in **forward order**.

```ts
console.log(list.toArrayForward()); // [5, 20]
```

---

#### `toArrayBackward(): T[]`

Returns all elements in **reverse order**.

```ts
console.log(list.toArrayBackward()); // [20, 5]
```

---

#### `clear(): void`

Clears the entire list, resetting it to an empty state.

```ts
list.clear();
```

---

### Example Usage

```ts
import { DoubleLinkedList } from "./DoubleLinkedList";

const list = new DoubleLinkedList<number>();

list.append(10);
list.append(20);
list.prepend(5);

console.log("Forward:", list.toArrayForward());  // [5, 10, 20]
console.log("Backward:", list.toArrayBackward()); // [20, 10, 5]

list.delete(10);
console.log("After deletion:", list.toArrayForward()); // [5, 20]
```

---

### Complexity Analysis

| Operation             | Time Complexity | Space Complexity |
|------------------------|----------------|------------------|
| `append()` / `prepend()` | O(1) | O(1) |
| `delete()`             | O(n) | O(1) |
| `find()`               | O(n) | O(1) |
| `toArrayForward()`     | O(n) | O(n) |
| `toArrayBackward()`    | O(n) | O(n) |

---

### Internal Design

Each node (`DoubleNode<T>`) stores:

```ts
{
  value: T;
  next: DoubleNode<T> | null;
  prev: DoubleNode<T> | null;
}
```

The list maintains two references:

- `head` — first node
- `tail` — last node

This allows traversal and updates from both ends efficiently.

---

[⬆️ Back to Top](#doublelinkedlist)

## Stack (Array Implementation)

**File:** `Stack.ts`  
**Description:** A stack is a Last-In-First-Out (LIFO) data structure that supports push, pop, peek, and size operations.

---

### Code

```typescript
/**
 * Stack Data Structure (Array Implementation)
 * 
 * A stack is a Last-In-First-Out (LIFO) data structure.
 * Supports push, pop, peek, and size operations.
 */

export default class Stack<T> {
  private items: T[];

  constructor() {
    this.items = [];
  }

  /** Adds an element to the top of the stack */
  push(element: T): void {
    this.items.push(element);
  }

  /** Removes and returns the top element of the stack */
  pop(): T | undefined {
    return this.items.pop();
  }

  /** Returns the top element without removing it */
  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  /** Returns true if the stack is empty */
  isEmpty(): boolean {
    return this.items.length === 0;
  }

  /** Returns the number of elements in the stack */
  size(): number {
    return this.items.length;
  }

  /** Clears all elements from the stack */
  clear(): void {
    this.items = [];
  }
}
```

---

### Operations

| Method | Description | Time Complexity |
|---------|--------------|----------------|
| `push()` | Adds an element to the top | O(1) |
| `pop()` | Removes the top element | O(1) |
| `peek()` | Returns the top element without removing it | O(1) |
| `isEmpty()` | Checks if stack is empty | O(1) |
| `size()` | Returns number of elements | O(1) |
| `clear()` | Removes all elements | O(1) |

---

### Example Usage

```typescript
const stack = new Stack<number>();
stack.push(10);
stack.push(20);
console.log(stack.peek()); // 20
console.log(stack.pop());  // 20
console.log(stack.size()); // 1
```

[⬆️ Back to Top](#linkedlist)

## Queue

A **First-In-First-Out (FIFO)** data structure where the **first element added** is the **first one to be removed**.  
It is ideal for use cases like **task scheduling**, **request handling**, and **data buffering**.

---

### Overview

The `Queue<T>` class provides an efficient and simple interface for managing elements in FIFO order.

- Constant-time enqueue and dequeue operations `O(1)`  
- Supports front element access using `peek()`  
- Type-safe using TypeScript generics  
- Simple internal array-based implementation  

---

### Constructor

```ts
const queue = new Queue<number>();
```

**Parameters:**  
None — creates an empty queue.

---

### Methods

#### `enqueue(value: T): void`

Adds a new element at the **end** of the queue.

```ts
queue.enqueue(10);
queue.enqueue(20);
```

---

#### `dequeue(): T | undefined`

Removes and returns the **front element** of the queue.  
Returns `undefined` if the queue is empty.

```ts
const first = queue.dequeue(); // → 10
```

---

#### `peek(): T | undefined`

Returns the **front element** without removing it.

```ts
console.log(queue.peek()); // → 20
```

---

#### `isEmpty(): boolean`

Checks whether the queue is empty.

```ts
console.log(queue.isEmpty()); // → false
```

---

#### `size(): number`

Returns the number of elements currently in the queue.

```ts
console.log(queue.size()); // → 1
```

---

#### `clear(): void`

Removes all elements from the queue.

```ts
queue.clear();
console.log(queue.isEmpty()); // → true
```

---

### Example Usage

```ts
import Queue from "./Queue";

const queue = new Queue<number>();

queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);

console.log(queue.peek()); // 10
console.log(queue.dequeue()); // 10
console.log(queue.size()); // 2
console.log("Empty:", queue.isEmpty()); // false

queue.clear();
console.log(queue.isEmpty()); // true
```

---

### Complexity Analysis

| Operation   | Time Complexity | Space Complexity |
|--------------|----------------|------------------|
| `enqueue()`  | O(1)           | O(n) |
| `dequeue()`  | O(1)           | O(n) |
| `peek()`     | O(1)           | O(1) |
| `isEmpty()`  | O(1)           | O(1) |
| `size()`     | O(1)           | O(1) |
| `clear()`    | O(1)           | O(1) |

---

### Internal Design

- Uses a private internal array:  
  ```ts
  private items: T[];
  ```
- `enqueue()` pushes to the end using `Array.push()`.  
- `dequeue()` removes from the front using `Array.shift()`.  
- Maintains FIFO order naturally via array indexing.  
- Simple yet effective for small to medium datasets.

---

[⬆️ Back to Top](#queue)

## PriorityQueue

A **binary heap–based priority queue** implementation.  
Each element is stored with an associated **priority**, and the element with the **highest** or **lowest** priority is dequeued first.  
Supports both **min-heap** and **max-heap** configurations for flexible usage.

---

### Overview

The `PriorityQueue<T>` class provides an efficient data structure for managing elements by priority, supporting:

- Logarithmic-time insertion and removal `O(log n)`  
- Configurable **min-heap** (default) or **max-heap** behavior  
- Peek access to the highest/lowest priority element  
- Automatic reordering using a **binary heap**

---

### Constructor

```ts
const pq = new PriorityQueue<T>(isMinHeap?: boolean);
```

**Parameters:**

- `isMinHeap` *(optional, default = true)* —  
  Determines whether the queue behaves as a **min-heap** (smallest priority dequeued first) or **max-heap** (largest priority dequeued first).

---

### Methods

#### `enqueue(value: T, priority: number): void`

Adds an element with the given priority.  
Automatically positions it according to the heap order.

```ts
pq.enqueue("Task A", 5);
pq.enqueue("Task B", 2);
```

---

#### `dequeue(): T | undefined`

Removes and returns the element with the **highest** (max-heap) or **lowest** (min-heap) priority.  
Returns `undefined` if the queue is empty.

```ts
const next = pq.dequeue(); // returns element with top priority
```

---

#### `peek(): T | undefined`

Returns the element with the top priority **without removing it**.

```ts
console.log(pq.peek()); // "Task B" (for min-heap)
```

---

#### `isEmpty(): boolean`

Checks whether the priority queue is empty.

```ts
console.log(pq.isEmpty()); // → false
```

---

#### `size(): number`

Returns the current number of elements in the queue.

```ts
console.log(pq.size()); // → 2
```

---

### Example Usage

```ts
import PriorityQueue from "./PriorityQueue";

// Min-Heap (default)
const minPQ = new PriorityQueue<string>(true);
minPQ.enqueue("Low Priority", 5);
minPQ.enqueue("High Priority", 1);

console.log(minPQ.peek());    // "High Priority"
console.log(minPQ.dequeue()); // "High Priority"

// Max-Heap
const maxPQ = new PriorityQueue<number>(false);
maxPQ.enqueue(10, 10);
maxPQ.enqueue(5, 5);

console.log(maxPQ.peek());    // 10
console.log(maxPQ.dequeue()); // 10
```

---

### Complexity Analysis

| Operation     | Time Complexity | Space Complexity |
|----------------|----------------|------------------|
| `enqueue()`    | O(log n)       | O(n) |
| `dequeue()`    | O(log n)       | O(n) |
| `peek()`       | O(1)           | O(1) |
| `isEmpty()`    | O(1)           | O(1) |
| `size()`       | O(1)           | O(1) |

---

### Internal Design

- Stores elements in a private array `heap: { value: T; priority: number }[]`.
- Maintains **heap order** using two key methods:
  - `bubbleUp()` – moves new elements up until the heap property is restored.
  - `bubbleDown()` – moves root elements down after removal.
- Uses `compare(a, b)` to determine heap type:
  - Min-heap → `a < b`
  - Max-heap → `a > b`

---

[⬆️ Back to Top](#priorityqueue)

## CircularQueue

A **fixed-size circular queue implementation** that efficiently reuses memory by wrapping around when the end of the array is reached.  
Perfect for **bounded buffering**, **task scheduling**, or **streaming** scenarios where memory usage must remain constant.

---

### Overview

The `CircularQueue<T>` class provides a memory-efficient, fixed-size queue implementation with constant-time operations:

- Constant-time enqueue and dequeue `O(1)`
- No dynamic resizing (fixed capacity)
- Automatically wraps around the array
- Prevents overflow/underflow using modular arithmetic

---

### Constructor

```ts
const queue = new CircularQueue<number>(capacity: number);
```

**Parameters:**

- `capacity` — Total number of elements the queue can hold (must be > 0).

---

### Methods

#### `enqueue(element: T): void`

Adds an element to the rear of the queue.  
Throws an error if the queue is full.

```ts
queue.enqueue(10);
queue.enqueue(20);
```

---

#### `dequeue(): T | null`

Removes and returns the front element.  
Returns `null` if the queue is empty.

```ts
console.log(queue.dequeue()); // → 10
```

---

#### `peek(): T | null`

Returns the front element without removing it.  
Returns `null` if the queue is empty.

```ts
console.log(queue.peek()); // → 20
```

---

#### `isEmpty(): boolean`

Checks if the queue has no elements.

```ts
console.log(queue.isEmpty()); // → false
```

---

#### `isFull(): boolean`

Checks if the queue is full.

```ts
console.log(queue.isFull()); // → true
```

---

#### `size(): number`

Returns the current number of elements in the queue.

```ts
console.log(queue.size()); // → 3
```

---

#### `clear(): void`

Removes all elements from the queue and resets pointers.

```ts
queue.clear();
```

---

### Example Usage

```ts
import CircularQueue from "./CircularQueue";

const queue = new CircularQueue<number>(3);

queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);

console.log(queue.isFull()); // true
console.log(queue.dequeue()); // 1

queue.enqueue(4); // wraps around
console.log(queue.peek()); // 2
console.log(queue.size()); // 3
```

---

### Complexity Analysis

| Operation     | Time Complexity | Space Complexity |
|----------------|----------------|------------------|
| `enqueue()`    | O(1) | O(n) |
| `dequeue()`    | O(1) | O(n) |
| `peek()`       | O(1) | O(1) |
| `isEmpty()`    | O(1) | O(1) |
| `isFull()`     | O(1) | O(1) |
| `size()`       | O(1) | O(1) |
| `clear()`      | O(n) | O(1) |

---

### Internal Design

- Uses a fixed-size array `items: (T | null)[]` for storage.
- Maintains **three key pointers**:
  - `front`: index of the next element to dequeue.
  - `rear`: index of the last enqueued element.
  - `count`: number of current elements.
- Uses **modular arithmetic** `(index % capacity)` for circular wraparound.
- Throws overflow/underflow errors when attempting invalid operations.

---

[⬆️ Back to Top](#circularqueue)


## HashTable

A **hash table implementation** using **separate chaining** to handle collisions.  
Provides efficient **O(1)** average-time complexity for insert, lookup, and delete operations.  
Ideal for **fast key-value lookups**, **caching**, or **indexing** use cases.

---

### Overview

The `HashTable<K, V>` class provides a dynamic, resizable hash map structure with predictable performance:

- Constant-time average lookup, insert, and delete `O(1)`
- Automatically resizes when load factor exceeds `0.75`
- Uses **separate chaining** (linked bucket arrays) to handle collisions
- Supports both `string` and `number` keys

---

### Constructor

```ts
const table = new HashTable<string, number>(capacity?: number);
```

**Parameters:**

- `capacity` — Initial number of buckets in the hash table (default: `16`).

---

### Methods

#### `set(key: K, value: V): void`

Inserts or updates a key-value pair in the hash table.  
If the key already exists, its value is **updated**.  
If the load factor exceeds `0.75`, the table **resizes** automatically.

```ts
table.set("username", 101);
table.set("age", 25);
```

---

#### `get(key: K): V | undefined`

Retrieves the value associated with the given key.  
Returns `undefined` if the key does not exist.

```ts
console.log(table.get("age")); // → 25
```

---

#### `has(key: K): boolean`

Checks if the given key exists in the table.

```ts
console.log(table.has("username")); // → true
```

---

#### `delete(key: K): boolean`

Removes the key-value pair associated with the given key.  
Returns `true` if the key was found and deleted, `false` otherwise.

```ts
table.delete("age");
```

---

#### `size(): number`

Returns the total number of key-value pairs currently stored in the hash table.

```ts
console.log(table.size()); // → 1
```

---

#### `clear(): void`

Removes all key-value pairs and resets the table to its initial state.

```ts
table.clear();
```

---

### Example Usage

```ts
import HashTable from "./HashTable";

const table = new HashTable<string, number>();

table.set("a", 10);
table.set("b", 20);
table.set("c", 30);

console.log(table.get("b"));   // → 20
console.log(table.has("a"));   // → true

table.delete("a");
console.log(table.size());     // → 2
```

---

### Complexity Analysis

| Operation  | Average Case | Worst Case | Space Complexity |
|-------------|--------------|-------------|------------------|
| `set()`     | O(1) | O(n) | O(n) |
| `get()`     | O(1) | O(n) | O(1) |
| `has()`     | O(1) | O(n) | O(1) |
| `delete()`  | O(1) | O(n) | O(1) |
| `clear()`   | O(n) | O(n) | O(1) |
| `resize()`  | O(n) | O(n) | O(n) |

---

### Internal Design

- Uses an array of **buckets** → each bucket stores an array of `[key, value]` pairs.
- Hash function converts each key to an integer index within `[0, capacity - 1]`.
- Handles **collisions** using **separate chaining** — multiple pairs stored in one bucket.
- Automatically **resizes (doubles capacity)** when load factor > 0.75 to maintain efficiency.
- Keys must be **strings** or **numbers**.

---

[⬆️ Back to Top](#hashtable)

## Graph

A **Graph data structure implementation** using an **adjacency list** representation.  
It supports **directed** and **undirected** graphs with efficient methods for managing vertices, edges, and performing graph traversals such as **BFS** and **DFS**.

---

### Overview

The `Graph<T>` class provides a versatile and type-safe graph representation that supports:

- Both **directed** and **undirected** edges  
- Efficient vertex and edge manipulation  
- **Breadth-First Search (BFS)** and **Depth-First Search (DFS)** traversals  
- Constant-time neighbor lookups using `Map` and `Set`  

---

### Constructor

```ts
const graph = new Graph<number>(directed?: boolean);
```

**Parameters:**

- `directed` *(optional, default = false)* —  
  If `true`, creates a directed graph; otherwise, an undirected graph.

---

### Methods

#### `addVertex(vertex: T): void`

Adds a new vertex to the graph if it doesn’t already exist.

```ts
graph.addVertex("A");
```

---

#### `addEdge(vertex1: T, vertex2: T): void`

Adds an edge between two vertices.  
Automatically creates vertices if they don’t exist.  
For undirected graphs, adds the edge in both directions.

```ts
graph.addEdge("A", "B");
```

---

#### `removeEdge(vertex1: T, vertex2: T): void`

Removes the edge between two vertices.  
For undirected graphs, removes the edge in both directions.

```ts
graph.removeEdge("A", "B");
```

---

#### `removeVertex(vertex: T): void`

Removes a vertex and all edges connected to it.

```ts
graph.removeVertex("A");
```

---

#### `getNeighbors(vertex: T): Set<T> | undefined`

Returns a set of all neighbors of a given vertex.  
Returns `undefined` if the vertex doesn’t exist.

```ts
console.log(graph.getNeighbors("B")); // → Set { "C", "D" }
```

---

#### `bfs(start: T): T[]`

Performs a **Breadth-First Search** traversal starting from the given vertex.  
Returns the order of visited vertices.

```ts
console.log(graph.bfs("A")); // → ["A", "B", "C", "D"]
```

---

#### `dfs(start: T): T[]`

Performs a **Depth-First Search** traversal starting from the given vertex.  
Returns the order of visited vertices.

```ts
console.log(graph.dfs("A")); // → ["A", "B", "C", "D"]
```

---

### Example Usage

```ts
import Graph from "./Graph";

const graph = new Graph<string>(false); // undirected graph

graph.addEdge("A", "B");
graph.addEdge("A", "C");
graph.addEdge("B", "D");

console.log("BFS:", graph.bfs("A")); // BFS: [ 'A', 'B', 'C', 'D' ]
console.log("DFS:", graph.dfs("A")); // DFS: [ 'A', 'B', 'D', 'C' ]
console.log("Neighbors of B:", graph.getNeighbors("B")); // Set { 'A', 'D' }
```

---

### Complexity Analysis

| Operation        | Time Complexity | Space Complexity |
|------------------|----------------|------------------|
| `addVertex()`    | O(1) | O(1) |
| `addEdge()`      | O(1) | O(1) |
| `removeEdge()`   | O(1) | O(1) |
| `removeVertex()` | O(V + E) | O(V + E) |
| `getNeighbors()` | O(1) | O(1) |
| `bfs()`          | O(V + E) | O(V) |
| `dfs()`          | O(V + E) | O(V) |

---

### Internal Design

- Uses a **Map** `adjacencyList: Map<T, Set<T>>` to store vertices and their neighbors.
- Each vertex maps to a **Set** of adjacent vertices for fast lookup and deletion.
- Supports **directed** or **undirected** configuration at construction.
- Traversals (`bfs` and `dfs`) use:
  - `Set` for tracking visited nodes.
  - `Array` as a queue (BFS) or recursive stack (DFS).

---

[⬆️ Back to Top](#graph)

## WeightedGraph (Adjacency List)

A **weighted graph** implementation using an **adjacency list**.  
Supports **directed** and **undirected** graphs and includes two shortest-path algorithms: **Dijkstra** (non-negative weights) and **Bellman‑Ford** (supports negative weights, detects negative cycles).

---

### Overview

The `WeightedGraph<T>` class represents a graph where edges have numeric weights.  
It is implemented with a `Map<T, Map<T, number>>` (map from vertex → map of neighbor → weight), which is memory-efficient for sparse graphs.

- **Use adjacency list** when the graph is sparse (E ≪ V²).  
- Supports both **directed** and **undirected** graphs via the constructor flag.

---

### ASCII Adjacency List Diagram

```
Graph:
A → B (5) , C (2)
B → D (10)
C → B (-2), D (3)

Adjacency List (Map) representation:
{
  "A": { "B": 5,  "C": 2  },
  "B": { "D": 10 },
  "C": { "B": -2, "D": 3  },
  "D": {}
}
```

---

### Implementation (TypeScript)

```ts
/**
 * Weighted Graph (Adjacency List)
 *
 * Supports directed or undirected graphs with edge weights.
 * Includes Dijkstra’s and Bellman-Ford shortest path algorithms.
 */

export default class WeightedGraph<T> {
    private adjacencyList: Map<T, Map<T, number>>;
    private directed: boolean;

    constructor(directed: boolean = false) {
        this.adjacencyList = new Map();
        this.directed = directed;
    }

    /** Add a vertex if it doesn't exist */
    addVertex(vertex: T): void {
        if (!this.adjacencyList.has(vertex)) {
            this.adjacencyList.set(vertex, new Map());
        }
    }

    /** Add a weighted edge */
    addEdge(vertex1: T, vertex2: T, weight: number): void {
        this.addVertex(vertex1);
        this.addVertex(vertex2);
        this.adjacencyList.get(vertex1)?.set(vertex2, weight);
        if (!this.directed) {
            this.adjacencyList.get(vertex2)?.set(vertex1, weight);
        }
    }

    /** Remove an edge */
    removeEdge(vertex1: T, vertex2: T): void {
        this.adjacencyList.get(vertex1)?.delete(vertex2);
        if (!this.directed) {
            this.adjacencyList.get(vertex2)?.delete(vertex1);
        }
    }

    /** Remove a vertex and its edges */
    removeVertex(vertex: T): void {
        this.adjacencyList.delete(vertex);
        for (const [v, edges] of this.adjacencyList.entries()) {
            edges.delete(vertex);
        }
    }

    /** Get all edges from a vertex */
    getEdges(vertex: T): Map<T, number> | undefined {
        return this.adjacencyList.get(vertex);
    }

    /** Dijkstra’s shortest path algorithm */
    dijkstra(start: T): Map<T, number> {
        const distances = new Map<T, number>();
        const visited = new Set<T>();
        const pq: [T, number][] = [];

        for (const vertex of this.adjacencyList.keys()) {
            distances.set(vertex, vertex === start ? 0 : Infinity);
        }

        pq.push([start, 0]);

        while (pq.length > 0) {
            pq.sort((a, b) => a[1] - b[1]);
            const [current, dist] = pq.shift()!;
            if (visited.has(current)) continue;
            visited.add(current);

            const neighbors = this.adjacencyList.get(current);
            if (neighbors) {
                for (const [neighbor, weight] of neighbors.entries()) {
                    const newDist = dist + weight;
                    if (newDist < (distances.get(neighbor) ?? Infinity)) {
                        distances.set(neighbor, newDist);
                        pq.push([neighbor, newDist]);
                    }
                }
            }
        }

        return distances;
    }

    /**
     * Bellman-Ford Algorithm
     * Works even with negative edge weights.
     * Detects negative weight cycles.
     */
    bellmanFord(start: T): Map<T, number> {
        const distances = new Map<T, number>();
        const vertices = Array.from(this.adjacencyList.keys());

        // Step 1: Initialize distances
        for (const v of vertices) {
            distances.set(v, Infinity);
        }
        distances.set(start, 0);

        // Step 2: Relax all edges |V| - 1 times
        for (let i = 0; i < vertices.length - 1; i++) {
            for (const [u, edges] of this.adjacencyList.entries()) {
                const distU = distances.get(u)!;
                if (distU === Infinity) continue;

                for (const [v, weight] of edges.entries()) {
                    const newDist = distU + weight;
                    if (newDist < distances.get(v)!) {
                        distances.set(v, newDist);
                    }
                }
            }
        }

        // Step 3: Detect negative weight cycle
        for (const [u, edges] of this.adjacencyList.entries()) {
            const distU = distances.get(u)!;
            if (distU === Infinity) continue;

            for (const [v, weight] of edges.entries()) {
                if (distU + weight < distances.get(v)!) {
                    throw new Error("Graph contains a negative weight cycle");
                }
            }
        }

        return distances;
    }
}
```

---

### Dijkstra’s Algorithm (Notes)

- **Use when** edge weights are non-negative.
- **Idea:** Greedy selection of the unvisited vertex with the smallest tentative distance, relax outgoing edges.
- **Implementation detail:** The example above uses a simple array-based priority queue (sorted array) for clarity. For better performance use a binary heap (min-heap) or Fibonacci heap.
- **Complexity with binary heap:** `O((V + E) log V)`. With the simple sorted array shown: `O(V^2 + E log V)` in practice.

---

### Bellman‑Ford Algorithm (Notes)

- **Use when** negative edge weights exist (but no negative cycles).
- **Idea:** Relax all edges repeatedly (|V| - 1) times; then check for negative cycles.
- **Complexity:** `O(V * E)`.
- Detects negative weight cycles and throws an error if found.

---

### Complexity Analysis

| Operation         | Time Complexity | Space Complexity |
|-------------------|-----------------|------------------|
| addVertex()       | O(1)            | O(V)             |
| addEdge()         | O(1)            | O(E)             |
| getEdges()        | O(1)            | O(1)             |
| dijkstra()        | O((V + E) log V) with heap | O(V + E) |
| bellmanFord()     | O(V * E)        | O(V + E)         |

---

### Example Usage

```ts
import WeightedGraph from "./WeightedGraph";

const g = new WeightedGraph<string>(true); // directed
g.addEdge("A", "B", 4);
g.addEdge("A", "C", 2);
g.addEdge("C", "B", -2);
g.addEdge("B", "D", 2);
g.addEdge("C", "D", 3);

console.log("Dijkstra from A:", g.dijkstra("A"));
console.log("Bellman-Ford from A:", g.bellmanFord("A"));
```

---

[⬆️ Back to Top](#weightedgraph)

## AVLTree

A **self-balancing binary search tree (BST)** where the height difference between left and right subtrees (the *balance factor*) is never more than 1.  
It ensures **O(log n)** performance for insertions, deletions, and lookups.

---

### Overview

The `AVLTree<T>` class provides a robust, type-safe implementation of an AVL tree that automatically balances itself after every insertion and deletion.  
It supports:

- Fast ordered data storage
- Self-balancing rotations (LL, LR, RR, RL)
- Inorder traversal for sorted output
- Custom comparison logic via comparator function

---

### Constructor

```ts
const tree = new AVLTree<number>((a, b) => a - b);
```

**Parameters:**

- `compareFn` *(optional)* — Custom comparison function for ordering. Defaults to numeric/string comparison.

---

### Methods

#### `insert(value: T): void`

Inserts a new value into the tree and rebalances it automatically.

```ts
tree.insert(10);
tree.insert(5);
tree.insert(15);
```

---

#### `delete(value: T): void`

Removes a value from the tree, rebalancing it if needed.

```ts
tree.delete(10);
```

---

#### `contains(value: T): boolean`

Checks whether a given value exists in the tree.

```ts
console.log(tree.contains(5)); // → true
console.log(tree.contains(99)); // → false
```

---

#### `inorder(): T[]`

Performs an inorder traversal, returning sorted values.

```ts
console.log(tree.inorder()); // → [5, 10, 15]
```

---

### Internal Rotations

AVL trees use four types of rotations to maintain balance:

| Rotation Type | Trigger Condition | Action |
|----------------|------------------|--------|
| **LL (Right Rotation)** | Left-heavy after left insert | Rotate right |
| **RR (Left Rotation)** | Right-heavy after right insert | Rotate left |
| **LR (Left-Right)** | Left-heavy after right insert | Left rotate child, then right rotate parent |
| **RL (Right-Left)** | Right-heavy after left insert | Right rotate child, then left rotate parent |

---

### Example Usage

```ts
import AVLTree from "./AVLTree";

const tree = new AVLTree<number>();

tree.insert(30);
tree.insert(20);
tree.insert(40);
tree.insert(10); // triggers rotation

console.log(tree.inorder()); // [10, 20, 30, 40]

tree.delete(20);
console.log(tree.inorder()); // [10, 30, 40]
```

---

### Complexity Analysis

| Operation | Time Complexity | Space Complexity |
|------------|----------------|------------------|
| `insert()` | O(log n) | O(n) |
| `delete()` | O(log n) | O(n) |
| `contains()` | O(log n) | O(1) |
| `inorder()` | O(n) | O(n) |

---

### Internal Design

- Each node stores:
  - `value`: The element
  - `height`: Node height for balancing
  - `left` and `right`: Child pointers
- Balancing uses height difference between left and right subtrees.
- Rotations are used to maintain balance after every modification.

---

[⬆️ Back to Top](#avltree)


## BinarySearchTree

A **classic Binary Search Tree (BST)** implementation supporting insertion, deletion, search, and traversal operations.  
Provides an ordered structure for efficient lookups and in-order traversals.

---

### Overview

The `BinarySearchTree<T>` class offers:

- Efficient **insert**, **search**, and **delete** operations
- **In-order traversal** for sorted data output
- Average-case logarithmic performance `O(log n)` for key operations

---

### Constructor

```ts
const bst = new BinarySearchTree<number>();
```

Creates an empty binary search tree instance.

---

### Methods

#### `insert(value: T): void`

Inserts a new value into the BST, maintaining order.

```ts
bst.insert(10);
bst.insert(5);
bst.insert(15);
```

---

#### `search(value: T): boolean`

Checks whether a value exists in the tree.

```ts
console.log(bst.search(10)); // → true
console.log(bst.search(7));  // → false
```

---

#### `delete(value: T): void`

Removes a value from the tree if present.  
Handles all three deletion cases (leaf, one child, two children).

```ts
bst.delete(5);
```

---

#### `inorder(): T[]`

Performs an **in-order traversal** and returns elements in sorted order.

```ts
console.log(bst.inorder()); // → [10, 15, 20]
```

---

### Example Usage

```ts
import { BinarySearchTree } from "./BinarySearchTree";

const bst = new BinarySearchTree<number>();

bst.insert(50);
bst.insert(30);
bst.insert(70);
bst.insert(20);
bst.insert(40);
bst.insert(60);
bst.insert(80);

console.log("Inorder:", bst.inorder()); // [20, 30, 40, 50, 60, 70, 80]

bst.delete(20);
bst.delete(30);
bst.delete(50);

console.log("After Deletions:", bst.inorder()); // [40, 60, 70, 80]

console.log("Search 60:", bst.search(60)); // true
console.log("Search 100:", bst.search(100)); // false
```

---

### Complexity Analysis

| Operation | Average Time | Worst Time | Space Complexity |
|------------|--------------|-------------|------------------|
| `insert()` | O(log n) | O(n) | O(n) |
| `search()` | O(log n) | O(n) | O(1) |
| `delete()` | O(log n) | O(n) | O(n) |
| `inorder()` | O(n) | O(n) | O(n) |

---

### Internal Design

- Each node is represented by the `BSTNode<T>` class containing:
  - `value`: Node data
  - `left`: Left child reference
  - `right`: Right child reference
- Maintains the BST property:
  - Left subtree < Node < Right subtree
- Recursive algorithms handle insertion, deletion, and traversal.

---

[⬆️ Back to Top](#binarysearchtree)


# Trie (Prefix Tree)

A **Trie** is a tree-based data structure used for efficient retrieval of strings. It is particularly useful for tasks involving prefix matching, autocomplete systems, and dictionary word lookups.

---

## 📘 Overview

The Trie (also known as a **Prefix Tree**) stores strings character by character, where each node represents a single character of the word. Words that share a common prefix share the same path in the tree up to that prefix.

---

## 🧩 Structure

### **TrieNode**
Each node contains:
- `children`: A map of characters to child nodes.
- `isEndOfWord`: A boolean flag to indicate whether the current node marks the end of a valid word.

### **Trie**
The main Trie class provides methods for insertion, searching, prefix checking, and deletion.

---

## ⚙️ Methods

### **insert(word: string): void**
Inserts a word into the Trie, creating nodes for characters as needed.

#### **Steps**
1. Start from the root node.
2. For each character:
   - If the character doesn’t exist as a child, create a new TrieNode.
   - Move to the child node.
3. Mark the last node’s `isEndOfWord` as `true`.

#### **Time Complexity**
- **O(n)**, where *n* is the length of the word.

---

### **search(word: string): boolean**
Checks whether a word exists in the Trie.

#### **Steps**
1. Traverse character by character from the root.
2. If a character is missing, return `false`.
3. After processing all characters, return `true` only if `isEndOfWord` is `true`.

#### **Time Complexity**
- **O(n)**

---

### **startsWith(prefix: string): boolean**
Checks whether there exists any word in the Trie that starts with the given prefix.

#### **Steps**
1. Traverse through the Trie for each prefix character.
2. If traversal is possible for all prefix characters, return `true`.

#### **Time Complexity**
- **O(n)**

---

### **delete(word: string): void**
Deletes a word from the Trie, removing unnecessary nodes after deletion.

#### **Steps**
1. Traverse recursively using `deleteHelper`.
2. When the end of the word is reached, unset `isEndOfWord`.
3. Backtrack and remove nodes that are no longer needed (no children and not an endpoint).

#### **Time Complexity**
- **O(n)**

---

## 🧠 Example Usage

```ts
const trie = new Trie();
trie.insert("apple");
trie.insert("app");

console.log(trie.search("apple"));   // true
console.log(trie.search("app"));     // true
console.log(trie.startsWith("ap"));  // true

trie.delete("app");
console.log(trie.search("app"));     // false
```

---

## 📈 Summary Table

| Operation | Description | Time Complexity |
|------------|--------------|-----------------|
| `insert` | Add a word to the trie | O(n) |
| `search` | Check if word exists | O(n) |
| `startsWith` | Check prefix existence | O(n) |
| `delete` | Remove word from trie | O(n) |

---

## 🏗️ Use Cases

- Autocomplete systems
- Spell checkers
- IP routing (Longest Prefix Match)
- Dictionary word search
- Search engines

---

## 🧾 Code Implementation (TypeScript)

```ts
class TrieNode {
  children: Map<string, TrieNode>;
  isEndOfWord: boolean;

  constructor() {
    this.children = new Map();
    this.isEndOfWord = false;
  }
}

export default class Trie {
  private root: TrieNode;

  constructor() {
    this.root = new TrieNode();
  }

  insert(word: string): void {
    let node = this.root;
    for (const char of word) {
      if (!node.children.has(char)) {
        node.children.set(char, new TrieNode());
      }
      node = node.children.get(char)!;
    }
    node.isEndOfWord = true;
  }

  search(word: string): boolean {
    let node = this.root;
    for (const char of word) {
      if (!node.children.has(char)) return false;
      node = node.children.get(char)!;
    }
    return node.isEndOfWord;
  }

  startsWith(prefix: string): boolean {
    let node = this.root;
    for (const char of prefix) {
      if (!node.children.has(char)) return false;
      node = node.children.get(char)!;
    }
    return true;
  }

  delete(word: string): void {
    this.deleteHelper(this.root, word, 0);
  }

  private deleteHelper(node: TrieNode, word: string, depth: number): boolean {
    if (depth === word.length) {
      if (!node.isEndOfWord) return false;
      node.isEndOfWord = false;
      return node.children.size === 0;
    }

    const char = word[depth];
    const child = node.children.get(char);
    if (!child) return false;

    const shouldDelete = this.deleteHelper(child, word, depth + 1);

    if (shouldDelete) {
      node.children.delete(char);
      return node.children.size === 0 && !node.isEndOfWord;
    }

    return false;
  }
}
```

---

## 🧭 Conclusion

The **Trie** is a powerful data structure for handling string-based operations efficiently, especially where prefix searches or word retrieval are common. Its node-based structure enables shared memory usage across common prefixes, optimizing space and time for large datasets.

[⬆️ Back to Top](#trie)

## MinHeap

A **binary heap data structure** that always maintains the **smallest element at the top**.  
Efficiently supports **insertion**, **extraction of the minimum**, and **peek** operations.  
Ideal for **priority queues**, **scheduling**, and **graph algorithms** like Dijkstra’s shortest path.

---

### Overview

The `MinHeap<T>` class is a generic, type-safe heap implementation that maintains the **heap invariant** —  
each parent node is always **less than or equal to** its child nodes.

- Insertions and deletions in **O(log n)** time.  
- Access to the minimum element in **O(1)** time.  
- Can be customized with a custom comparison function.

---

### Constructor

```ts
const heap = new MinHeap<number>();
```

or with a custom comparator:

```ts
const heap = new MinHeap<number>((a, b) => a - b);
```

**Parameters:**

- `compareFn` *(optional)* — A custom comparison function.  
  Should return:
  - `-1` if `a < b`
  - `0` if `a === b`
  - `1` if `a > b`

---

### Methods

#### `size(): number`

Returns the number of elements currently in the heap.

```ts
console.log(heap.size()); // → 5
```

---

#### `peek(): T | null`

Returns the smallest element **without removing** it from the heap.

```ts
console.log(heap.peek()); // → 2
```

---

#### `insert(value: T): void`

Inserts a new element into the heap and restores the heap property using **heapify-up**.

```ts
heap.insert(10);
heap.insert(3);
heap.insert(5);
```

---

#### `extractMin(): T | null`

Removes and returns the smallest element (root) from the heap.  
Rebalances the heap using **heapify-down**.

```ts
console.log(heap.extractMin()); // → 3
```

---

### Example Usage

```ts
import MinHeap from "./MinHeap";

const heap = new MinHeap<number>();

heap.insert(5);
heap.insert(2);
heap.insert(8);
heap.insert(1);

console.log("Min:", heap.peek()); // → 1
console.log("Extracted:", heap.extractMin()); // → 1
console.log("Next Min:", heap.peek()); // → 2
console.log("Size:", heap.size()); // → 3
```

---

### Complexity Analysis

| Operation       | Time Complexity | Space Complexity |
|-----------------|----------------|------------------|
| `insert()`      | O(log n) | O(n) |
| `extractMin()`  | O(log n) | O(n) |
| `peek()`        | O(1) | O(1) |
| `size()`        | O(1) | O(1) |

---

### Internal Design

- Uses an internal **array representation** for the binary heap.
- Maintains the **heap invariant** via:
  - `_heapifyUp()` — Adjusts new elements upwards after insertion.
  - `_heapifyDown()` — Adjusts root downwards after extraction.
- Supports **custom comparators** for flexible ordering logic.

---

### Applications

- **Priority Queues** (task scheduling, resource allocation)
- **Graph Algorithms** (Dijkstra’s, Prim’s)
- **Event Simulation Systems**
- **Median or Top-K computations**

---

[⬆️ Back to Top](#minheap)


## MaxHeap

A **Max Heap implementation** that extends the functionality of a `MinHeap` by inverting the comparison logic.  
This ensures that the **maximum element is always at the root**.  
Useful for **priority scheduling**, **heap sort**, and **max-priority queue** implementations.

---

### Overview

The `MaxHeap<T>` class inherits from the `MinHeap<T>` and overrides its comparison function to maintain a **descending order** heap property.

- The largest element is always at the top.
- Insertion and extraction operations take `O(log n)` time.
- Supports custom comparison functions for complex data types.

---

### Constructor

```ts
const maxHeap = new MaxHeap<number>();
```

**Parameters:**

- `compareFn?`: *(optional)* — A custom comparator function `(a, b) => number`.  
  If not provided, the heap uses the default numeric comparison.

**Default Comparison Logic:**

```ts
(a, b) => (a > b ? -1 : a < b ? 1 : 0)
```

This reverses the normal MinHeap order, ensuring higher values have higher priority.

---

### Example Usage

```ts
import MaxHeap from "./MaxHeap";

const heap = new MaxHeap<number>();

heap.insert(10);
heap.insert(5);
heap.insert(30);
heap.insert(20);

console.log(heap.extract()); // 30
console.log(heap.peek());    // 20
```

With a **custom comparator**:

```ts
const heap = new MaxHeap<{ key: number }>((a, b) => a.key - b.key);

heap.insert({ key: 10 });
heap.insert({ key: 50 });
heap.insert({ key: 30 });

console.log(heap.extract()); // { key: 50 }
```

---

### Complexity Analysis

| Operation       | Time Complexity | Space Complexity |
|-----------------|----------------|------------------|
| `insert()`      | O(log n) | O(n) |
| `extract()`     | O(log n) | O(n) |
| `peek()`        | O(1) | O(1) |
| `size()`        | O(1) | O(1) |
| `clear()`       | O(1) | O(1) |

---

### Internal Design

- Inherits from the `MinHeap<T>` class.
- Overrides the comparator logic to invert priority.
- Uses an internal array to maintain heap structure.

**Parent–Child Relationships:**
- `parent(i) = Math.floor((i - 1) / 2)`
- `left(i) = 2 * i + 1`
- `right(i) = 2 * i + 2`

This design allows efficient element insertion and removal while maintaining heap balance.

---

[⬆️ Back to Top](#maxheap)


