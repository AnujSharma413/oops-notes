# 🔥 OOP Chapter 1 — Introduction, Classes, Objects, Constructors & Keywords

> **Java OOP Series** | Chapter 1 of the Object-Oriented Programming Notes

---

## 📚 Table of Contents

- [Why OOP Exists](#-1-why-oop-exists)
- [Class vs Object](#-2-class-vs-object)
- [Object Properties](#-3-object-properties)
- [Memory Concept](#-4-memory-concept)
- [Dynamic Memory Allocation](#-5-dynamic-memory-allocation)
- [Accessing Instance Variables](#-6-accessing-instance-variables)
- [Methods & `this` Keyword](#-7-methods--this-keyword)
- [Constructors](#-8-constructors)
- [Constructor Chaining](#-9-constructor-chaining)
- [Reference Behavior](#-10-reference-behavior)
- [Primitive vs Object Memory](#-11-primitive-vs-object-memory)
- [Why `new` is Not Used for Primitives](#-12-why-new-is-not-used-for-primitives)
- [Wrapper Classes](#-13-wrapper-classes)
- [Swap Failure](#-14-swap-failure)
- [The `final` Keyword](#-15-final-keyword)
- [Garbage Collection](#-16-garbage-collection)
- [Interview Traps Cheat Sheet](#-interview-traps-cheat-sheet)

---

## 🔥 1. Why OOP Exists

### ❌ The Problem — Bad Design (Before OOP)

```java
int[]    rollNum = new int[5];
String[] name    = new String[5];
float[]  marks   = new float[5];
```

This approach is broken because:
- Data is **scattered** across unrelated arrays
- No logical relation between `rollNum[i]`, `name[i]`, `marks[i]`
- Hard to **manage**, **scale**, and **debug**

> 💼 **Interview Insight:** This anti-pattern is called **Primitive Obsession** / poor data modeling.

---

### ✅ The Solution — OOP

> Group related **data + behavior** into one unit → **Class**

---

## 🧠 2. Class vs Object

### Class — The Blueprint

```java
class Student {
    int rno;
    String name;
    float marks;
}
```

- A **blueprint / template**
- Logical structure only — **no memory allocated yet**

---

### Object — The Real Entity

```java
Student student1 = new Student();
```

- A **real, tangible entity**
- **Occupies memory** (on the Heap)

---

### 💡 Real-life Analogy

| Concept | Real-life |
|---------|-----------|
| Class | Car **Design / Blueprint** |
| Object | Actual car on the road *(DL-01-1234)* |

---

> ⚠️ **Interview Trap:**
> - ❌ *"Class stores data"*
> - ✅ **Class defines structure; object stores actual data in memory**

---

## ⚡ 3. Object Properties

Every object has exactly **three properties**:

| Property | Meaning |
|----------|---------|
| **State** | Data stored (`rno`, `name`, `marks`) |
| **Behavior** | Functions/methods (`greeting()`) |
| **Identity** | Unique memory reference in the heap |

---

## 🧩 4. Memory Concept

```java
Student student1;           // Step 1
student1 = new Student();   // Step 2
```

| Step | What Happens | Memory |
|------|--------------|--------|
| `Student student1` | Reference variable created | **Stack** → points to `null` |
| `new Student()` | Actual object created | **Heap** |
| Assignment `=` | Stack reference now points to Heap object | Stack → Heap |

---

> ⚠️ **Classic Interview Questions:**
> - *Where is the object stored?* → **Heap**
> - *Where is the reference stored?* → **Stack**

---

## 🚨 5. Dynamic Memory Allocation

```java
new Student();
```

- Memory is allocated **at runtime**, not compile time
- Size doesn't need to be known beforehand

> 💡 **Real-life:** Like booking seats only when people arrive — not pre-booking all seats upfront.

---

## 🧠 6. Accessing Instance Variables

```java
student1.name
student1.rno
```

> **Rule:** Always use the **object reference** to access instance variables.

---

## ⚙️ 7. Methods & `this` Keyword

```java
void greeting() {
    System.out.println("Hello! My name is " + this.name);
}
```

### What is `this`?

> `this` = **reference to the current object**

Mentally replace it as:
```
this.name  →  student1.name   (when called on student1)
this.name  →  student2.name   (when called on student2)
```

> ⚠️ **Interview Trap:** Removing `this` still compiles sometimes — but `this` is **required** when local variable names clash with instance variable names, and it improves clarity.

---

## 🏗️ 8. Constructors

Automatically called when an object is created. Three types:

---

### 1️⃣ Default Constructor

```java
Student() {
    this(13, "default person", 98); // calls parameterized constructor
}
```

- Called automatically when no arguments are passed
- Can delegate to another constructor using `this(...)`

---

### 2️⃣ Parameterized Constructor

```java
Student(int rno, String name, float marks) {
    this.rno   = rno;
    this.name  = name;
    this.marks = marks;
}
```

- Initializes object with **specific values**

---

### 3️⃣ Copy Constructor

```java
Student(Student other) {
    this.rno   = other.rno;
    this.name  = other.name;
    this.marks = other.marks;
}
```

- Creates a **new independent copy** of another object

---

### 💡 Real-life Analogy

| Constructor | Analogy |
|-------------|---------|
| Default | New student registered with placeholder values |
| Parameterized | Student fills out the admission form with real data |
| Copy | Duplicate an existing student's record |

---

## 🔁 9. Constructor Chaining

```java
Student() {
    this(13, "default person", 98); // calls another constructor
}
```

- `this(...)` calls **another constructor** within the same class
- **Must be the first line** inside the constructor — no exceptions

> ⚠️ **Rule:** `this(...)` call must always be on **line 1** of the constructor body.

---

## ⚡ 10. Reference Behavior

```java
Student one = new Student();
Student two = one;           // two points to SAME object

one.name = "Something";

System.out.println(two.name); // Output: Something
```

### Why?

Both `one` and `two` point to the **exact same object** in the heap:

```
one ──┐
      ├──▶  [ Student object in Heap ]
two ──┘
```

Changing `one.name` also changes `two.name` — they're the same thing.

> ⚠️ **Interview Trap:** Java is **pass-by-value** — but for objects, the **reference (address) is passed by value**, not a copy of the object.

---

## 🔥 11. Primitive vs Object Memory

```java
int     a   = 10;   // primitive
Integer num = 45;   // object (wrapper)
```

| Property | Primitive | Object |
|----------|-----------|--------|
| Stored in | **Stack** | **Heap** |
| Stores | Actual value | Reference to value |
| Speed | Faster | Slower |

---

## 🧠 12. Why `new` is Not Used for Primitives?

```java
int a = 10; // No new keyword
```

- Size is **known at compile time**
- No dynamic allocation needed
- Lives directly on the **Stack** → faster

---

## 📦 13. Wrapper Classes

```java
Integer num = 45; // int → Integer (autoboxing)
```

Converts a **primitive → Object**

### Why are they needed?

- Java **Collections** don't work with primitives → `ArrayList<Integer>` not `ArrayList<int>`
- Provides utility methods: `compareTo()`, `parseInt()`, `valueOf()`, etc.

---

## ⚠️ 14. Swap Failure

```java
swap(a, b);     // primitives — originals NOT swapped
swapInt(n1, n2); // Integer objects — still NOT swapped
```

### Why?

> Java is strictly **pass-by-value**. The method gets a copy of the value/reference, not the original variable.

### Hidden Truth about `Integer`:

> `Integer` is **immutable** (internally `final`) — its value can't be changed once created. A new object is always created instead.

---

## 🔒 15. `final` Keyword

### For primitives:

```java
final int bonus = 3;
bonus = 5; // ❌ Compile error — cannot reassign
```

### For objects:

```java
final Student student3 = new Student();
student3.name = "Changed"; // ✅ Allowed — modifying object data
student3 = new Student();  // ❌ Not allowed — changing the reference
```

| Action | Allowed? |
|--------|----------|
| Change the **reference** | ❌ |
| Change the **object's data** | ✅ |

> `final` locks the **reference**, not the object itself.

---

## ♻️ 16. Garbage Collection

Java automatically **cleans up unused objects** from the heap.

### When does an object become eligible for GC?

```java
Student obj = new Student();
obj = null; // No reference → eligible for garbage collection
```

- When **no reference points** to an object, the GC can reclaim that memory
- You don't call GC manually — the JVM handles it
- `System.gc()` is a *suggestion*, not a guarantee

---

## 🚨 Interview Traps Cheat Sheet

```
❓ Where is the object stored?         → Heap
❓ Where is the reference stored?      → Stack
❓ Does Java pass objects by reference? → NO. Pass-by-value (reference value is copied)
❓ Can final object's fields change?   → YES. Only the reference is final, not the data.
❓ Why does swap() fail in Java?        → Pass-by-value. Only copy is swapped.
❓ Why Integer swap also fails?         → Integer is immutable (internally final)
❓ What is 'this'?                      → Reference to the current object
❓ Must this() be first in constructor? → YES, always line 1.
❓ What is Primitive Obsession?         → Using scattered primitives instead of classes
```
