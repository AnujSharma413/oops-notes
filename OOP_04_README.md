# 🔥 OOP Chapter 4 — Access Control, Packages & Object Class

> **Java OOP Series** | Chapter 4 of the Object-Oriented Programming Notes

---

## 📚 Table of Contents

- [Access Modifiers](#-part-1-access-modifiers)
- [Getters & Setters](#-part-2-getters--setters)
- [Packages](#-part-3-packages)
- [Object Class](#-part-4-object-class)
- [Interview Traps](#-interview-traps)

---

## 🧠 Part 1: Access Modifiers

Java has **4 access levels**:

| Modifier | Same Class | Same Package | Subclass (Same Pkg) | Subclass (Diff Pkg) | Other World |
|----------|:----------:|:------------:|:-------------------:|:-------------------:|:-----------:|
| `public` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ✅ *(via inheritance)* | ❌ |
| `default` *(no modifier)* | ✅ | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ | ❌ |

---

### 🔒 1. `private`

```java
class A {
    private int num;
}
```

- Only accessible **inside the same class**
- Direct access via `obj.num` → ❌ Not allowed
- Must use `obj.getNum()` → ✅

**Use when:** Sensitive data, internal fields, encapsulation  
**Real-life analogy:** ATM PIN — hidden from everyone outside

---

### 🌍 2. `public`

```java
public String name;
```

- Accessible **everywhere**
- `obj.name` → ✅ from anywhere

**Use when:** APIs, constants, methods meant for everyone

---

### 📦 3. `default` (Package-private)

```java
String name; // no keyword
```

- Accessible only inside the **same package**

```java
package oops.access;
// Classes inside this package → ✅
// Outside this package → ❌
```

**Use when:** Package-internal helper classes

---

### 🛡️ 4. `protected`

> ⚠️ **Most misunderstood modifier!**

```java
class Animal {
    protected void sound() {}
}

class Dog extends Animal {
    void test() {
        sound(); // ✅ allowed through inheritance
    }
}
```

**Accessible:**
- ✅ Same package
- ✅ Subclass even in a **different** package

> **Brutal Truth:** Outside a package, access is only valid through **inheritance context**, not direct object reference.

**Use when:** Child classes need access to parent members

---

### 🧠 When to Use Which?

| Modifier | When to Use |
|----------|-------------|
| `private` | **Default choice** for all variables |
| `public` | For methods users/clients should call |
| `protected` | When child classes need direct access |
| `default` | Package-internal helper logic |

> 🚨 **Industry Rule:** Most fields should be `private`. If everything is `public` → bad design.

---

## 🔥 Part 2: Getters & Setters

Instead of exposing fields directly:

```java
obj.num = -100; // ❌ No validation possible
```

Use getters and setters:

```java
public int getNum() {
    return num;
}

public void setNum(int num) {
    this.num = num;
}
```

### ✅ Better Version — With Validation

```java
public void setNum(int num) {
    if (num > 0) {
        this.num = num;
    }
}
```

**Why?** You control how data is read and written — the foundation of **encapsulation**.

---

## 🔥 Part 3: Packages

### What is a Package?

A **folder + namespace** for organizing classes.

```java
package oops.access;
```

### Why Use Packages?

- ✅ Organize code into logical groups
- ✅ Avoid class name conflicts
- ✅ Fine-grained access control

---

### 📦 Built-in Java Packages

| Package | Contains |
|---------|----------|
| `java.lang` | `String`, `Math`, `Object`, `Integer`, `System` *(auto-imported)* |
| `java.util` | `ArrayList`, `HashMap`, `Scanner`, `Collections` |
| `java.io` | `File`, `BufferedReader`, `PrintWriter` |
| `java.net` | `URL`, `Socket` |
| `java.awt` | GUI tools *(older Java UI)* |
| `java.applet` | Browser applets *(obsolete)* |

> 💡 `java.lang` is **automatically imported** — you never need to import `String` or `System` manually.

---

## 🔥 Part 4: Object Class

### Every Class Extends `Object`

```java
class A { }

// Internally treated as:
class A extends Object { }
```

Every class in Java **implicitly** inherits from `Object`. This means every class gets these methods for free:

| Method | Purpose |
|--------|---------|
| `toString()` | String representation of the object |
| `equals()` | Checks equality |
| `hashCode()` | Returns integer hash for collections |
| `getClass()` | Returns runtime class info |
| `clone()` | Creates a copy of the object |
| `finalize()` | *(Deprecated — do not use)* |

---

### 🖨️ `toString()`

**Default output:**
```
ObjectClass@1a2b3c  // Not human-readable
```

**Override it:**
```java
@Override
public String toString() {
    return "num = " + num;
}
```

Now `System.out.println(obj)` prints something readable.

---

### #️⃣ `hashCode()`

- Returns an **integer** used internally by hashing collections
- Used in `HashMap`, `HashSet`, etc.

> ⚠️ Two **equal** objects should always have the **same** `hashCode`.

---

### ⚖️ `equals()`

**Default behavior:** Checks memory reference (same as `==`)

```java
// String overrides equals() → compares content
new String("abc").equals(new String("abc")); // ✅ true

// Custom class — must override equals() yourself
class Student {
    int id;
    // Without override, compares references, not id
}
```

---

### 🔍 `instanceof`

```java
if (obj instanceof ObjectClass) {
    System.out.println(true);
}
```

Use to **safely check object type** at runtime before casting.

---

### 🏷️ `getClass()`

```java
obj.getClass();
// Output: class oops.access.ObjectClass
```

Returns **runtime type information** about the object.

---

### 📋 `clone()`

Creates a shallow copy of the object. Rarely used directly in modern Java — prefer copy constructors or serialization.

---

### 🗑️ `finalize()`

Old cleanup method called by the garbage collector.  
**Do not rely on it** — GC behavior is unpredictable. Use `try-with-resources` instead.

---

## 🚨 Interview Traps

### Q1: Difference between `==` and `equals()`

| `==` | `equals()` |
|------|------------|
| Compares **references** (memory address) | Compares **content** (if overridden) |

---

### Q2: Why override `hashCode()` when you override `equals()`?

For **correct behavior** in `HashMap` and `HashSet`. If two objects are `equal()`, they must return the same `hashCode()` — otherwise collections break.

---

### Q3: Is `Object` the parent of all classes?

✅ **Yes** — every class in Java implicitly extends `Object`.

---

### Q4: Is `String` in `java.lang`?

✅ **Yes** — that's why you never need to import it.

---

### Q5: Can `private` members be inherited?

✅ **Yes** — they are inherited internally by the subclass  
❌ **But** — they are not directly accessible in the child class (must use `public`/`protected` getters)

---

## 🗂️ Quick Revision Cheat Sheet

```
ACCESS MODIFIERS (most restrictive → least):
private → default → protected → public

OBJECT CLASS METHODS:
toString()  → readable output
equals()    → content comparison (override!)
hashCode()  → for HashMap/HashSet (override with equals!)
getClass()  → runtime type info
instanceof  → type check before casting

PACKAGE RULE:
java.lang   → auto-imported (String, Math, Object)
everything else → must be imported manually
```