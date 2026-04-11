# 🔥 OOP Chapter 2 — Packages, Static Keyword, Singleton Class & Built-in Methods

> **Java OOP Series** | Chapter 2 of the Object-Oriented Programming Notes

---

## 📚 Table of Contents

- [Packages](#-1-packages)
- [Import Statement](#-2-import-statement)
- [Static Keyword](#-3-static-keyword)
- [Static Methods](#-4-static-methods)
- [Static vs Non-Static](#-5-static-vs-non-static)
- [Why `main()` is Static](#-6-why-main-is-static)
- [Static Blocks](#-7-static-blocks)
- [Inner Classes](#-8-inner-classes)
- [Singleton Class](#-9-singleton-class)
- [Internal Working & Best Practices](#-10-internal-working--best-practices)
- [Interview Traps Cheat Sheet](#-interview-traps-cheat-sheet)

---

## 📦 1. Packages

```java
package oops.staticExample;
```

### What is a Package?

A package is a **folder / namespace** used to organize classes logically.

### 💡 Real-life Analogy

Just like your system folders:
```
Documents/
Photos/
Videos/
```

Java organizes code the same way:
```
oops/
  staticExample/
  singleton/
  access/
```

> 💼 **Interview Insight:** Packages prevent **naming conflicts** and improve **code modularity**. Two classes can have the same name if they're in different packages.

---

## 📥 2. Import Statement

```java
import java.util.Scanner;
```

- Allows you to use classes from **another package**
- Without import, you'd need to write the full path every time: `java.util.Scanner sc = ...`

> ⚠️ **Common Trap:** Import does **NOT** copy or embed the class into your file — it only **grants access** to it.

---

## ⚡ 3. Static Keyword

> 🧠 **Core Concept:** `static` = belongs to the **class**, NOT to any individual object.

```java
static long population;
```

### What Happens in Memory?

```java
Human kunal = new Human(...);
Human rahul = new Human(...);
Human arpit = new Human(...);
```

```
Class Area (Method Area)
─────────────────────────────
population = 3       ← ONE shared copy

Heap
─────────────────────────────
[ kunal object ]
[ rahul object ]
[ arpit object ]
```

All three objects share the **exact same** `population` variable. No separate copies.

> ⚠️ **Interview Trap:**
> - ❌ *"Static variable is copied for every object"*
> - ✅ **Only one copy exists, shared across all objects**

---

## ⚙️ 4. Static Methods

```java
static void message() {
    System.out.println("Hello World");
}
```

### Key Rule

> A **static method** can only access **static data** directly.

### Why this fails inside a static method:

```java
static void message() {
    System.out.println(this.age); // ❌ Compile error
}
```

Because:
- `this` = reference to the **current object**
- Static methods have **no object** — so `this` doesn't exist here

> ⚠️ **Brutal Truth:** If this isn't clear, the OOP foundation isn't solid yet. Static = no object context.

---

## 🚨 5. Static vs Non-Static

### ❌ Static calling Non-Static — WRONG

```java
static void fun() {
    greeting(); // ❌ Error — greeting() is non-static
}
```

### ✅ Correct Way — Create an object first

```java
static void fun() {
    Main obj = new Main();
    obj.greeting(); // ✅ Works
}
```

### ✅ Non-Static calling Static — Always fine

```java
void greeting() {
    fun(); // ✅ Allowed — objects can access class-level things
}
```

### Summary Table

| Context | Can access static? | Can access non-static? |
|---------|:------------------:|:----------------------:|
| Static method | ✅ | ❌ (needs object) |
| Non-static method | ✅ | ✅ |

> 💡 **Rule:** Static context **cannot** directly access instance members. Instance context **can** access everything.

---

## 🔥 6. Why `main()` is Static

```java
public static void main(String[] args)
```

> The JVM needs to **call `main()`** before any object exists. Since static methods belong to the class (not an object), the JVM can call it directly using just the class name — no object required.

> ⚠️ **Interview Answer:** `main()` is static so the **JVM can invoke it directly via the class name** without creating an instance first.

---

## 🧱 7. Static Blocks

```java
static {
    System.out.println("I am in static block");
    b = a * 5;
}
```

### Behavior

- Runs **only once** — when the class is **first loaded** by the JVM
- Used to initialize static variables with complex logic

### Proof:

```java
StaticBlock obj1 = new StaticBlock();
StaticBlock obj2 = new StaticBlock();
// Static block output prints only ONCE, not twice
```

> ⚠️ **Interview Trap:**
> - ❌ *"Static block runs every time an object is created"*
> - ✅ **Runs only once per class load, regardless of how many objects are created**

---

## 🧩 8. Inner Classes

```java
class InnerClasses {

    static class Test {      // Static nested class
        String name;
    }

    class NonStaticTest {    // Non-static inner class
        String name;
    }
}
```

### Static Nested Class — No outer object needed

```java
InnerClasses.Test a = new InnerClasses.Test(); // ✅ Direct
```

### Non-Static Inner Class — Needs outer object first

```java
InnerClasses outer = new InnerClasses();
InnerClasses.NonStaticTest t = outer.new NonStaticTest(); // ✅
```

| Type | Needs Outer Object? |
|------|:-------------------:|
| Static nested class | ❌ |
| Non-static inner class | ✅ |

> 💡 A **static nested class** is independent of the outer class object. A **non-static inner class** holds an implicit reference to its outer class instance.

---

## 🧠 9. Singleton Class

> ✅ **Definition:** A class that allows **only ONE object** to ever exist.

### Your Implementation

```java
class Singleton {

    private static Singleton instance; // Step 2: hold the single instance

    private Singleton() {}             // Step 1: block direct instantiation

    public static Singleton getInstance() { // Step 3: controlled access
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### How It Works — Step by Step

**Step 1 — Private constructor blocks `new`:**
```java
private Singleton() {}

new Singleton(); // ❌ Not allowed from outside
```

**Step 2 — Static instance holds the one object:**
```java
private static Singleton instance;
```

**Step 3 — Controlled access via static method:**
```java
Singleton obj1 = Singleton.getInstance();
Singleton obj2 = Singleton.getInstance();
// obj1 == obj2 → ✅ Same object
```

### 💡 Real-life Use Cases

| Use Case | Why Singleton? |
|----------|----------------|
| Database connection | Only one connection pool needed |
| Logger | All logs go to one central logger |
| Configuration manager | One source of truth for app settings |

> ⚠️ **Interview Trap:** Why use Singleton?
> ✅ To **save memory** and **control access** — ensuring a shared resource is only instantiated once.

---

### 🚨 Hidden Problem — Not Thread-Safe!

The above implementation **breaks in multithreaded environments**:

```
Thread A checks: instance == null  → true
Thread B checks: instance == null  → true (before A finishes)
Both create new objects → ❌ Two instances exist!
```

**Fix — Thread-safe Singleton using `synchronized`:**

```java
public static synchronized Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

Or use the **double-checked locking** pattern for better performance:

```java
public static Singleton getInstance() {
    if (instance == null) {
        synchronized (Singleton.class) {
            if (instance == null) {
                instance = new Singleton();
            }
        }
    }
    return instance;
}
```

---

## 🔥 10. Internal Working & Best Practices

### How `static` variables update internally:

```java
Human.population += 1;
// Internally: population = population + 1
// This change is reflected across ALL objects instantly
```

### Accessing Static Variables — Best Practice

```java
Human.population  // ✅ Recommended — clear it's a class-level variable
kunal.population  // ⚠️ Works but BAD practice — misleads reader into thinking it's instance data
```

> ⚠️ **Interview Expectation:** Always access static members using the **class name**, not an object reference. It's a signal of good OOP understanding.

---

## 🚨 Interview Traps Cheat Sheet

```
❓ Where is a static variable stored?
   → Method Area (Class Area), NOT the Heap

❓ How many copies of a static variable exist?
   → ONE, shared across all objects

❓ Can a static method use 'this'?
   → NO — 'this' requires a current object; static methods have none

❓ Can static methods call non-static methods directly?
   → NO — must create an object first

❓ Why is main() static?
   → JVM calls it without creating an object first

❓ How many times does a static block run?
   → ONCE — when the class is first loaded by JVM

❓ What is a Singleton class?
   → A class that allows only ONE instance to exist

❓ Is the basic Singleton implementation thread-safe?
   → NO — use synchronized or double-checked locking

❓ Should you access static members via object reference?
   → NO — always use ClassName.member for clarity

❓ Does import copy a class into your file?
   → NO — it only grants access to it
```
