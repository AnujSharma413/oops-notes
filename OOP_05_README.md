# 🔥 OOP Chapter 5 — Abstract Classes, Interfaces, Annotations & Composition

> **Java OOP Series** | Chapter 5 of the Object-Oriented Programming Notes

---

## 📚 Table of Contents

- [Multiple Inheritance Problem](#-part-1-multiple-inheritance-problem)
- [Abstract Classes](#-part-2-abstract-class)
  - [Abstract Methods](#-abstract-methods)
  - [Constructors in Abstract Class](#-constructors-in-abstract-class)
  - [Static Methods](#-static-methods-in-abstract-class)
  - [Illegal Combinations](#-illegal-combinations)
- [Interfaces](#-part-3-interfaces)
  - [Multiple Inheritance via Interfaces](#-multiple-inheritance-using-interfaces)
  - [Interface Variables](#-interface-variables)
  - [Extending Interfaces](#-extending-interfaces)
  - [Default Methods](#-default-methods-in-interfaces)
  - [Static Methods](#-static-methods-in-interfaces)
  - [Nested Interfaces](#-nested-interfaces)
- [Composition](#-part-4-composition-advanced-oop)
- [Annotations](#-annotations)
- [Abstract Class vs Interface](#-abstract-class-vs-interface)
- [When to Use What?](#-when-to-use-what)
- [Interview Traps Cheat Sheet](#-interview-traps-cheat-sheet)

---

## 🧠 Part 1: Multiple Inheritance Problem

```java
class A {
    void greet() { ... }
}

class B {
    void greet() { ... }
}

class C extends A, B // ❌ Not allowed in Java
```

If this were allowed:

```java
C obj = new C();
obj.greet(); // ❓ Which greet() runs — A's or B's?
```

This is the **Diamond Problem** — the compiler can't resolve the ambiguity.

> ✅ **Java's Solution:** No multiple inheritance with classes. Use **Interfaces** instead.

---

## 🏗️ Part 2: Abstract Class

> **Definition:** A class that **cannot be instantiated directly** and may contain abstract methods (methods with no body).

```java
abstract class Parent {
    abstract void career();          // no body — child MUST implement

    void normalMethod() {            // concrete method — optional to override
        System.out.println("Normal method");
    }
}

class Son extends Parent {
    @Override
    void career() {
        System.out.println("Engineer");
    }
}
```

### Rules at a Glance

```java
Parent p = new Parent(); // ❌ Cannot instantiate
Parent p = new Son();    // ✅ Reference of parent, object of child (upcasting)
```

### 💡 Real-life Analogy

```
abstract class Vehicle
    ├── Car   → starts with key
    ├── Bike  → starts with kick
    └── Truck → starts with ignition switch
```

Every vehicle starts — but **how** it starts is defined by each child.

---

### 🔥 Abstract Methods

```java
abstract void start(); // no body
```

- The child class **must implement** all abstract methods
- Unless the child is also declared `abstract` — then it can skip

---

### 🔥 Normal Methods Also Allowed

```java
void stop() {
    System.out.println("Vehicle stopped");
}
```

Abstract classes can mix **abstract** (no body) and **concrete** (with body) methods — unlike interfaces (pre-Java 8).

---

### 🔥 Constructors in Abstract Class

```java
abstract class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}
```

```java
new Son(); // → "Parent constructor" prints first
```

> ⚠️ **Why does an abstract class need a constructor if you can't instantiate it?**
> Because when a child object is created, the **parent constructor always runs first** to initialize shared/common data.

---

### 🔥 Static Methods in Abstract Class

```java
abstract class A {
    static void greet() {
        System.out.println("Hello");
    }
}

A.greet(); // ✅ Call directly via class name
```

---

### 🚨 Illegal Combinations

```java
abstract static void test(); // ❌ Not allowed
```

Why?
- `abstract` means: *"must be overridden by child"*
- `static` means: *"belongs to class, cannot be overridden"*
- Direct contradiction → **compile error**

```java
final abstract class A // ❌ Not allowed
```

Why?
- `abstract` means: *"must be inherited"*
- `final` means: *"cannot be inherited"*
- Direct contradiction → **compile error**

---

## 🔌 Part 3: Interfaces

> **Definition:** A blueprint of behavior. Defines **what** a class must do, not **how**.

```java
public interface Engine {
    void start();
    void stop();
    void acc();
}

class Car implements Engine {
    public void start() { System.out.println("Engine starts"); }
    public void stop()  { System.out.println("Engine stops");  }
    public void acc()   { System.out.println("Accelerating");  }
}
```

> 💡 **Key Insight:** Interface focuses on **capabilities**, not identity.
> Examples: `Flyable`, `Drivable`, `Payable`, `Runnable`, `Comparable`

---

### 🔥 Multiple Inheritance Using Interfaces

```java
class Car implements Engine, Brake, MediaPlayer // ✅ Allowed
```

Why is this safe (but class multiple inheritance isn't)?

> Interfaces only define the **contract** (method signatures), not the implementation. There's no ambiguity about which method body to use — the class provides the one body itself.

---

### ⚠️ Design Problem — Same Method Names

```java
MediaPlayer carMedia = new Car();
carMedia.start(); // → starts the ENGINE, not the music!
```

When `Engine` and `MediaPlayer` both have `start()`, using the same class to implement both causes **confusion and bad design**.

> **Solution:** Use **Composition** (see Part 4 below).

---

### 🔥 Interface Variables

```java
interface Engine {
    static final int PRICE = 78000;
}
```

All variables in an interface are **automatically** `public static final`:
- `public` — accessible everywhere
- `static` — belongs to the interface, not any object
- `final` — **cannot be modified**

```java
Engine.PRICE = 90000; // ❌ Cannot assign to final variable
```

---

### 🔥 Interface as Reference Type

```java
Engine car = new Car();
```

| Part | Meaning |
|------|---------|
| `Engine` | Reference type — limits what you can access |
| `Car` | Object type — decides what actually runs |

Same **runtime polymorphism** principle from Chapter 3.

---

### 🔥 Extending Interfaces

```java
interface A {
    void greet();
}

interface B extends A {
    void fun();
}

class Main implements B {
    // Must implement BOTH greet() and fun()
    public void greet() { ... }
    public void fun()   { ... }
}
```

> When a class implements `B`, it inherits the contract of `A` too — must implement all methods from both.

---

### 🔥 Default Methods in Interfaces

Introduced in **Java 8** to add methods to interfaces without breaking existing implementations.

```java
interface A {
    default void fun() {
        System.out.println("I am in A");
    }
}
```

Now old classes that implement `A` don't need to be updated — they get `fun()` for free.

### ⚠️ Default Method Conflict

```java
interface A { default void fun() {} }
interface B { default void fun() {} }

class C implements A, B // ❌ Compile error — ambiguous
```

**Fix:** Override manually in `C`:

```java
class C implements A, B {
    @Override
    public void fun() {
        A.super.fun(); // or B.super.fun() — you choose
    }
}
```

---

### 🔥 Static Methods in Interfaces

```java
interface A {
    static void greetings() {
        System.out.println("Hey");
    }
}

A.greetings();  // ✅ Call via interface name
obj.greetings(); // ❌ NOT accessible via object reference
```

> Static methods in interfaces are **not inherited** by implementing classes or sub-interfaces.

---

### 🔥 Nested Interfaces

```java
class A {
    interface NestedInterface {
        boolean isOdd(int num);
    }
}

class B implements A.NestedInterface {
    public boolean isOdd(int num) {
        return num % 2 != 0;
    }
}

B obj = new B();
obj.isOdd(5); // true
```

> 💡 **Why useful?** Logical grouping — the interface is scoped inside the class it belongs to, keeping code organized.

---

## 🧱 Part 4: Composition (Advanced OOP)

### The Problem with Car Implementing Everything

```java
class Car implements Engine, Brake, MediaPlayer
// start() conflict between Engine and MediaPlayer
// Car class becomes bloated and confusing
```

### ✅ The Solution — Composition

```java
class NiceCar {
    private Engine engine;          // Car HAS-A engine
    private MediaPlayer mediaPlayer; // Car HAS-A media player

    NiceCar() {
        engine      = new PowerEngine();
        mediaPlayer = new CDPlayer();
    }

    void start() {
        engine.start();       // delegate to engine
    }

    void startMusic() {
        mediaPlayer.start();  // delegate to media player
    }

    void upgradeEngine() {
        engine = new ElectricEngine(); // swap engine without touching Car logic
    }
}
```

### Why Composition is Powerful

| Scenario | Inheritance | Composition |
|----------|-------------|-------------|
| Want to upgrade engine | Rewrite Car | `engine = new ElectricEngine()` ✅ |
| Want to swap music player | Rewrite Car | `mediaPlayer = new BluetoothPlayer()` ✅ |
| Flexibility | Low | High |
| Coupling | Tight | Loose |

> 🚨 **Interview Gold Line:**
> **"Favor composition over inheritance."**
> Composition gives you flexibility to swap behaviors at runtime without changing the class structure.

---

## 🏷️ Annotations

> **Definition:** Annotations are **metadata** that provide instructions to the **compiler**, **JVM**, or **tools** — they don't change logic but guide behavior.

### Common Annotations

| Annotation | Purpose |
|------------|---------|
| `@Override` | Verifies the method actually overrides a parent method |
| `@Deprecated` | Marks method as outdated — warns users to avoid it |
| `@SuppressWarnings` | Tells compiler to ignore specific warnings |

### Why `@Override` Matters

```java
// Without @Override:
void Start() { ... } // typo — creates a NEW method silently, no warning!

// With @Override:
@Override
void Start() { ... } // ❌ Compile error — "does not override any method"
```

> `@Override` is a **safety net** — it catches typos and signature mismatches at compile time before they become runtime bugs.

---

## 🚨 Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Constructor | ✅ Yes | ❌ No |
| Instance variables | ✅ Yes | ❌ No (only `public static final`) |
| Multiple inheritance | ❌ No | ✅ Yes |
| Concrete methods | ✅ Yes | ✅ `default` / `static` only (Java 8+) |
| Abstract methods | ✅ Yes | ✅ Yes (implicitly) |
| Best for | Common base class with shared state | Defining capabilities / contracts |

---

## 🎯 When to Use What?

### Use Abstract Class When:
- Classes are **closely related** (share identity)
- You need **shared state** (instance variables)
- You want to provide **common base code**

```
Examples: Vehicle, Animal, Employee
```

### Use Interface When:
- **Unrelated** classes need to share a capability
- You need **multiple inheritance**
- You're defining a **contract / behavior**

```
Examples: Runnable, Comparable, Serializable, Engine, Brake
```

> 💡 **Rule of thumb:**
> - *"IS-A"* relationship with shared data → **Abstract Class**
> - *"CAN-DO"* / capability → **Interface**

---

## 🚨 Interview Traps Cheat Sheet

```
❓ Why can't Java have multiple inheritance with classes?
   → Diamond problem — ambiguity about which method body to use

❓ Can you instantiate an abstract class?
   → NO — but you can use it as a reference type

❓ Why does an abstract class have a constructor?
   → To initialize common/shared data when a child object is created

❓ Can abstract class have static methods?
   → YES — but NOT abstract static methods (contradiction)

❓ Can you have final abstract class?
   → NO — final prevents inheritance, abstract requires it (contradiction)

❓ Are interface variables mutable?
   → NO — all interface variables are public static final (constants)

❓ Are static interface methods inherited by implementing class?
   → NO — call only via interface name (InterfaceName.method())

❓ What happens when two interfaces have same default method?
   → Compile error — implementing class must override manually

❓ Favor composition or inheritance?
   → Composition — more flexible, loosely coupled, swappable

❓ What is @Override actually for?
   → Compile-time safety — ensures you're overriding, not accidentally creating new method

❓ Interface = WHAT to do, class = HOW to do?
   → YES — that's the core contract idea
```