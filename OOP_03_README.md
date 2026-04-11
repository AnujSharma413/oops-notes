# 🔥 OOP Chapter 3 — The 4 Pillars: Inheritance, Polymorphism, Abstraction & Encapsulation

> **Java OOP Series** | Chapter 3 of the Object-Oriented Programming Notes

---

## 📚 Table of Contents

- [Principle 1: Inheritance](#-principle-1-inheritance)
  - [What is Inheritance?](#-1-what-is-inheritance)
  - [Why Inheritance?](#-2-why-inheritance)
  - [Constructor Chaining](#-4-constructor-chaining)
  - [The `super` Keyword](#-5-super-keyword)
  - [Reference vs Object](#-8-reference-vs-object-most-important)
  - [Types of Inheritance](#-11-types-of-inheritance)
  - [Memory Model](#-12-memory-model)
- [Principle 2: Polymorphism](#-principle-2-polymorphism)
  - [Method Overloading](#-part-1-method-overloading-static-polymorphism)
  - [Method Overriding](#-part-2-method-overriding-runtime-polymorphism)
  - [Dynamic Method Dispatch](#-3-dynamic-method-dispatch)
  - [Overloading vs Overriding](#-9-overloading-vs-overriding-comparison)
- [Principle 3: Abstraction](#-principle-3-abstraction)
  - [Abstract Class](#-3-abstract-class)
  - [Interface](#-5-interface-pure-abstraction)
  - [Abstract Class vs Interface](#-6-abstract-class-vs-interface)
- [Principle 4: Encapsulation](#-principle-4-encapsulation)
  - [Data Hiding](#-3-data-hiding)
  - [Abstraction vs Encapsulation](#-6-abstraction-vs-encapsulation-final-difference)
- [Master Interview Cheat Sheet](#-master-interview-cheat-sheet)

---

# 🧬 Principle 1: Inheritance

## 🧠 1. What is Inheritance?

> **Inheritance** = a child class acquires the **properties and behavior** of a parent class.

```java
class Child extends Parent
```

### Your Example:

```java
class BoxWeight extends Box
```

| Term | Class |
|------|-------|
| Parent (Superclass) | `Box` |
| Child (Subclass) | `BoxWeight` |

### 💡 Real-life Analogy

- **Parent:** `Vehicle` → has `speed`, `engine`
- **Child:** `Car`, `Bike` → automatically inherit `speed` and `engine`

---

## ⚡ 2. Why Inheritance?

| Without Inheritance | With Inheritance |
|---------------------|-----------------|
| ❌ Duplicate code everywhere | ✅ Reusability |
| ❌ Hard to maintain | ✅ Cleaner structure |
| ❌ Poor scalability | ✅ Logical hierarchy |

---

## 🧩 3. Multilevel Inheritance — Code Structure

```
Box  →  BoxWeight  →  BoxPrice
```

| Class | Adds |
|-------|------|
| `Box` | `l`, `w`, `h` |
| `BoxWeight` | `weight` |
| `BoxPrice` | `cost` |

> This is **Multilevel Inheritance** — each child extends the previous child.

---

## 🏗️ 4. Constructor Chaining

```java
BoxWeight(double l, double h, double w, double weight) {
    super(l, h, w);       // calls Box constructor first
    this.weight = weight; // then sets own field
}
```

### What happens internally:

```
new BoxWeight(...)
      │
      ▼
  Box constructor runs first   ← parent always first
      │
      ▼
  BoxWeight constructor runs
```

> 🔥 **Rule:** The **parent constructor is ALWAYS called first**, before the child constructor body executes.

> ⚠️ If you don't explicitly write `super(...)`, Java **automatically inserts** `super()` (no-arg) as the first line. If the parent has no no-arg constructor, it's a **compile error**.

---

## 🚨 5. `super` Keyword

### Use 1 — Call parent constructor:
```java
super(l, h, w);
```

### Use 2 — Access parent variable/method:
```java
super.weight
super.displayInfo()
```

> ⚠️ **Interview Trap:**
> - ❌ *"`super` refers to the parent class object"*
> - ✅ **`super` refers to the parent part of the CURRENT object** — there is only one object in memory

---

## ⚡ 6. Copy Constructor Flow

```java
BoxWeight(BoxWeight other) {
    super(other);          // Step 1: copies Box part (l, w, h)
    weight = other.weight; // Step 2: copies BoxWeight part
}
```

Full object copy happens **layer by layer**, from parent down to child.

---

## 🧠 7. Method Behavior in Inheritance

```java
public void displayInfo() {
    System.out.println("Running the box");
}
```

A child class can:
- ✅ **Use** the parent's method as-is
- ✅ **Override** it to provide its own version (covered in Polymorphism)

---

## 🚨 8. Reference vs Object (Most Important)

```java
Box box5 = new BoxWeight(2, 3, 4, 5);
```

| Part | Meaning |
|------|---------|
| `Box` | **Reference type** — what the compiler sees |
| `BoxWeight` | **Object type** — what actually exists in memory |

### What this means in practice:

```java
box5.weight; // ❌ Compile error!
```

The **compiler only knows** `Box` → it sees `l`, `w`, `h`. It has no idea `weight` exists.

```
Compiler sees:   Box reference → knows l, w, h only
Runtime sees:    BoxWeight object → full data exists
```

> ⚠️ **Brutal Truth:** If this concept isn't clear, **polymorphism will not make sense**.

---

## 🚨 9. Invalid Case

```java
BoxWeight box6 = new Box(2, 3, 4); // ❌ INVALID
```

Why? A **parent object doesn't contain child properties**. The child reference would expect `weight` to exist, but the `Box` object has none.

```
Parent reference → Child object  ✅  (upcasting — safe)
Child reference  → Parent object ❌  (downcasting without cast — unsafe)
```

---

## ⚡ 10. Static Methods & Inheritance

```java
Box b2 = new BoxWeight();
b2.greetings(); // → calls Box.greetings()
```

Static methods are **NOT overridden** — they are **hidden**.

> ⚠️ **Interview Trap:**
> - ❌ *"Static methods support overriding"*
> - ✅ **Static methods use method hiding — the reference type decides which version runs, not the object type**

---

## 🧠 11. Types of Inheritance

### 1. Single
```
Box  →  BoxWeight
```

### 2. Multilevel
```
Box  →  BoxWeight  →  BoxPrice
```

### 3. Hierarchical
```
         Box
        /   \
 BoxWeight  BoxColor
```

### 4. Multiple — ❌ NOT in Java
```
  A     B
   \   /
     C     ← Ambiguity: which A/B method does C inherit?
```

> **Why not allowed?** The **Diamond Problem** — if both A and B have the same method, Java can't decide which one C inherits.
> **Solution:** Use **Interfaces** (Java supports multiple interface implementation).

### 5. Hybrid — ❌ Not in Java
Combination of types — blocked because it can include multiple inheritance.

---

## 🔥 12. Memory Model

```java
BoxWeight obj = new BoxWeight(2, 3, 4, 5);
```

```
Heap Memory
─────────────────────────
│  Box part             │
│    l = 2              │
│    w = 3              │
│    h = 4              │
│─────────────────────  │
│  BoxWeight part       │
│    weight = 5         │
─────────────────────────
      ▲
      │
   Stack: obj (reference)
```

> One object in memory — but it contains **all layers** (parent + child data together).

---

---

# 🎭 Principle 2: Polymorphism

## 🧠 1. What is Polymorphism?

> **Polymorphism** = "many forms" → same method name, different behavior depending on context.

### 💡 Real-life Analogy
The same "send" button:
- In Camera → takes a photo
- In WhatsApp → sends a message
- In Gmail → sends an email

---

## ⚡ 2. Two Types

| Type | Also Called | When Resolved |
|------|-------------|---------------|
| Method Overloading | Static / Compile-time Polymorphism | At **compile time** |
| Method Overriding | Dynamic / Runtime Polymorphism | At **runtime** |

---

## 🔥 Part 1: Method Overloading (Static Polymorphism)

```java
int sum(int a, int b)
int sum(int a, int b, int c)
double sum(double a, double b)
```

> ✅ **Definition:** Same method name, **different parameter list**

### What can differ:
- ✅ Number of arguments
- ✅ Type of arguments
- ✅ Order of arguments

### What is NOT enough:
```java
int   sum(int a, int b)
float sum(int a, int b) // ❌ Only return type differs — compile error
```

> ⚠️ **Interview Trap:** You **cannot** overload by return type alone — the compiler gets confused when the return value isn't used.

> 💡 **Why "compile-time"?** The compiler can figure out which version to call just by looking at the argument types — no need to wait for runtime.

---

## 🔥 Part 2: Method Overriding (Runtime Polymorphism)

```java
class Shapes {
    void area() {
        System.out.println("I am in shapes");
    }
}

class Circle extends Shapes {
    @Override
    void area() {
        System.out.println("Area = π * r * r");
    }
}

class Square extends Shapes {
    @Override
    void area() {
        System.out.println("Area = side * side");
    }
}
```

> ✅ **Definition:** Child class provides its **own implementation** of a parent class method.

---

## ⚡ 3. Dynamic Method Dispatch

```java
Shapes square = new Square();
square.area(); // Output: Area = side * side
```

| Part | Value |
|------|-------|
| Reference type | `Shapes` |
| Object type | `Square` |
| Method called | `Square`'s `area()` |

### How it works internally:

```
Compile time:
  Compiler sees Shapes reference
  Checks: does area() exist in Shapes? → YES → OK ✅

Runtime:
  JVM sees actual object is Square
  Calls Square's area() → dynamic dispatch ✅
```

> 🔥 **Golden Rule:**
> - **Reference type** → limits what you can *access*
> - **Object type** → decides what actually *executes*

---

## 🧩 4. Upcasting

```java
Shapes s = new Circle(); // Upcasting — child object stored in parent reference
```

This enables writing **flexible, scalable code**:

```java
Shapes s1 = new Circle();
Shapes s2 = new Square();
Shapes s3 = new Triangle();

s1.area(); // Circle's area
s2.area(); // Square's area
s3.area(); // Triangle's area
```

One interface → three behaviors. This is what companies want: **flexible + scalable code**.

---

## 🧩 5. Overriding Rules

| Rule | Detail |
|------|--------|
| Same method name | ✅ Required |
| Same parameters | ✅ Required |
| Same or covariant return type | ✅ Required |
| Access modifier | ✅ Cannot be *more restrictive* than parent |

```java
// Parent:
public void area()

// Child:
protected void area() // ❌ Less accessible — not allowed
public void area()    // ✅ Same or broader — allowed
```

---

## ⚠️ 6. Static Methods & Polymorphism

> Static methods are **NOT overridden** — they are **hidden**.

```java
Shapes s = new Circle();
s.staticMethod(); // → calls Shapes.staticMethod(), NOT Circle's
```

Why? Static belongs to the **class**, not the object. Runtime polymorphism requires an object context — static has none.

---

## 🔥 7. `@Override` Annotation

```java
@Override
void area() { ... }
```

- Tells the compiler: *"I intend to override a parent method"*
- If you make a typo (`Area()` instead of `area()`), the **compiler catches it immediately**
- Without `@Override`, the typo silently creates a new method instead — a hard-to-find bug

---

## 🔥 8. `toString()` Override (Object Class)

```java
System.out.println(obj);
// Default: ObjectPrint@1a2b3c  ← not useful
```

Override it:

```java
@Override
public String toString() {
    return "ObjectPrint{ num=" + num + " }";
}

System.out.println(obj);
// Output: ObjectPrint{ num=54 }
```

> 💡 Every class extends `Object` — so every class can (and should) override `toString()`.

---

## 🧠 9. Overloading vs Overriding Comparison

| Feature | Overloading | Overriding |
|---------|-------------|------------|
| When resolved | **Compile time** | **Runtime** |
| Class | Same class | Parent-child classes |
| Method name | Same | Same |
| Parameters | **Different** | **Same** |
| Return type | Can differ | Same (or covariant) |
| `@Override` | ❌ Not used | ✅ Recommended |
| Static methods | ✅ Can be overloaded | ❌ Cannot be overridden |

---

---

# 🎭 Principle 3: Abstraction

## 🧠 1. What is Abstraction?

> **Abstraction** = show only **essential details**, hide the internal implementation.

### 💡 Real-life Analogy — Driving a Car
- You use: steering ✅, brake ✅, accelerator ✅
- You DON'T see: engine combustion ❌, piston movement ❌, fuel injection ❌

> ⚠️ **Interview Warning:**
> - ❌ Weak answer: *"Abstraction = hiding details"* (too generic)
> - ✅ Strong answer: *"Abstraction exposes only relevant functionalities and hides implementation using abstract classes or interfaces."*

---

## ⚡ 2. How Abstraction is Achieved in Java

1. **Abstract Class** — partial abstraction
2. **Interface** — pure / full abstraction

---

## 🏗️ 3. Abstract Class

```java
abstract class Shape {
    abstract void area();     // no body — child MUST implement

    void display() {
        System.out.println("I am a shape"); // concrete method — optional
    }
}

class Circle extends Shape {
    @Override
    void area() {
        System.out.println("Area = π * r²");
    }
}
```

### Rules:

```java
Shape s = new Shape(); // ❌ Cannot instantiate an abstract class
```

- ❌ You **cannot create objects** of an abstract class
- ✅ Child class **must override** all abstract methods (or be abstract itself)
- ✅ Can have both **abstract** and **concrete** methods
- ✅ Can have **instance variables** and **constructors**

---

## ⚡ 4. Why Abstraction?

| Without Abstraction | With Abstraction |
|---------------------|-----------------|
| ❌ Everything exposed | ✅ Clean, minimal API |
| ❌ Tight coupling | ✅ Loose coupling |
| ❌ Hard to change internals | ✅ Change implementation freely |

---

## 🔥 5. Interface (Pure Abstraction)

```java
interface Engine {
    void start(); // implicitly public abstract
    void stop();
}

class Car implements Engine {
    public void start() {
        System.out.println("Car starts");
    }

    public void stop() {
        System.out.println("Car stops");
    }
}
```

> 💡 **Interface = Contract**
> The interface says **WHAT** must be done. The class says **HOW** to do it.

---

## 🚨 6. Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Methods | Abstract + concrete | Abstract (+ default/static in Java 8+) |
| Variables | Can have state (instance vars) | Only `public static final` constants |
| Inheritance | Single (`extends`) | Multiple (`implements`) |
| Constructor | ✅ Can have | ❌ Cannot have |
| Use case | Shared base behavior | Define a capability / contract |

---

## 🧠 7. Stack Example — Abstraction in Action

```java
Stack s = new Stack();
s.push(10);
s.pop();
s.peek();
```

You use the stack without knowing:
- Is it backed by an array or a linked list? ❌ (don't know, don't care)
- How memory is managed? ❌
- How it resizes? ❌

**That's abstraction** — the interface is clean, the complexity is hidden.

---

---

# 🔒 Principle 4: Encapsulation

## 🧠 1. What is Encapsulation?

> **Encapsulation** = wrapping **data + methods** into a single unit AND **restricting direct access** to the data.

### 💡 Real-life Analogy — A Capsule 💊
- **Outer shell** = protection (access modifiers)
- **Inside** = actual data (private fields)

---

## ⚡ 2. How it's Done

```java
class Student {
    private int marks; // data hidden

    public int getMarks() {     // controlled read
        return marks;
    }

    public void setMarks(int marks) { // controlled write
        this.marks = marks;
    }
}
```

---

## 🔒 3. Data Hiding

```java
private double l; // data hiding
```

- Data is **not directly accessible** from outside
- All access is **controlled through methods** (getters/setters)

> ⚠️ **Interview Trap — Very Important:**
> - ❌ *"Data hiding = encapsulation"*
> - ✅ **Data hiding is a SUBSET of encapsulation**
>   - **Encapsulation** = wrapping + access control (the whole concept)
>   - **Data hiding** = making fields private (one technique within encapsulation)

---

## 🧠 4. Why Encapsulation?

| Without Encapsulation | With Encapsulation |
|----------------------|-------------------|
| ❌ Anyone can modify data | ✅ Controlled access |
| ❌ No validation possible | ✅ Validate before setting |
| ❌ Bug-prone | ✅ Predictable, safe |

---

## ⚡ 5. Validation Example

```java
class BankAccount {
    private double balance;

    public void setBalance(double balance) {
        if (balance >= 0) {          // ← validation
            this.balance = balance;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

Without encapsulation: `account.balance = -5000` → nothing stops it.
With encapsulation: the setter blocks invalid data entirely.

---

## 🔥 6. Abstraction vs Encapsulation (Final Difference)

| Feature | Abstraction | Encapsulation |
|---------|-------------|---------------|
| Focus | **What** to show | **How** to protect |
| Goal | Hide implementation complexity | Hide data |
| Level | Design / architecture level | Code level |
| Tool | `abstract` class, `interface` | Access modifiers (`private`, getters/setters) |

---

## 💣 7. Real-life Combined Example — ATM Machine

```
ATM Machine
────────────────────────────────
Abstraction (what you see):
  → Withdraw button
  → Deposit button
  → Balance check

Encapsulation (what's protected):
  → balance is private
  → updated only via internal methods
  → no direct access from outside
```

> **Abstraction** answers: *What can the user do?*
> **Encapsulation** answers: *How is the data kept safe?*

---

---

## 🚨 Master Interview Cheat Sheet

```
INHERITANCE
───────────────────────────────────────────────
❓ Why no multiple inheritance in Java?
   → Diamond problem (ambiguity)

❓ What does super() do?
   → Calls the parent constructor (must be line 1)

❓ Is super a reference to parent object?
   → NO — it's the parent part of the CURRENT object

❓ Can child access parent's private fields?
   → NO — use getters/protected

❓ Parent ref = new Child() — what can you access?
   → Only what's declared in Parent (reference limits access)

❓ Can static methods be overridden?
   → NO — only hidden (method hiding)

POLYMORPHISM
───────────────────────────────────────────────
❓ Difference: overloading vs overriding?
   → Overloading: same class, different params, compile-time
   → Overriding: parent-child, same params, runtime

❓ What is dynamic method dispatch?
   → JVM decides which overridden method to call at runtime based on object type

❓ Can you overload by return type only?
   → NO — compile error

❓ Can you override static methods?
   → NO — method hiding only

❓ Can main() be overloaded?
   → YES

ABSTRACTION
───────────────────────────────────────────────
❓ Difference: abstract class vs interface?
   → Abstract: partial abstraction, single inheritance, can have state
   → Interface: full abstraction, multiple inheritance, only constants

❓ Can you instantiate an abstract class?
   → NO

❓ Interface defines WHAT or HOW?
   → WHAT — the implementing class defines HOW

ENCAPSULATION
───────────────────────────────────────────────
❓ Is data hiding the same as encapsulation?
   → NO — data hiding is a subset of encapsulation

❓ Why use private + getters/setters instead of public fields?
   → Validation, control, security

❓ Difference: abstraction vs encapsulation?
   → Abstraction hides complexity (design level)
   → Encapsulation hides data (code level)
```