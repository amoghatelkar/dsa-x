# 🧠 DS-X

> A modern, TypeScript-based **Data Structures Library** for JavaScript developers.  
> Build, learn, and use fundamental data structures with clean, production-ready code.

---

## 🚀 Overview

**DS-X** provides a robust, modular, and type-safe implementation of essential data structures —  
from arrays and linked lists to trees, graphs, and tries — written in **TypeScript**.

The goal of DS-X is to make data structures **easy to use, test, and understand**,  
both for learners and professionals looking to integrate them into real-world applications.

---

## 📦 Installation

You can install DS-X using your favorite package manager:

```bash
# npm
npm install ds-x

# yarn
yarn add ds-x

# pnpm
pnpm add ds-x

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
- [CircularQueue](#circularqueue)

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

