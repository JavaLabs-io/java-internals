# 02 — Garbage Collection

> Java manages memory automatically. Garbage Collection is the mechanism behind it.

## What is Garbage Collection?

Garbage Collection (GC) is the process of reclaiming memory occupied by objects that are no longer reachable by the application.

In languages with manual memory management, memory must be released explicitly. Java delegates that responsibility to the JVM.

Objects are created by the application. Memory reclamation is handled by the JVM.

---

## What does "reachable" mean?

An object is reachable if an active reference still points to it.

```java
String name = new String("Java");
```

The object is reachable because the variable `name` holds a reference to it.

```java
name = null;
```

The reference is removed.

If no other references exist, the object becomes eligible for Garbage Collection.

Eligible does not mean immediate collection. The JVM decides when collection occurs.

---

## How does the JVM identify garbage?

The JVM starts from a set of GC Roots, such as:

* Local variables
* Static fields
* Active threads

Objects that can be reached from these roots are considered alive.

```text
GC Root
   |
   v
Object A ---> Object B ---> Object C

Object D
```

Objects A, B, and C are reachable.

Object D is unreachable and can be reclaimed.

---

## Where does Garbage Collection happen?

GC primarily operates on the Heap.

### Young Generation

New objects are created here.

Most objects have short lifetimes and are collected in this region.

### Old Generation

Objects that survive multiple collections are promoted here.

Collections occur less frequently but usually take longer.

### Metaspace

Class metadata is stored here.

Metaspace is separate from the Heap and replaced PermGen in Java 8.

---

## How does Garbage Collection work?

At a high level:

1. Reachable objects are identified
2. Reachable objects are marked as alive
3. Unreachable objects are reclaimed
4. Memory may be compacted to reduce fragmentation

Different garbage collectors implement these steps differently, but the goal remains the same: preserve reachable objects and reclaim unreachable ones.

---

## Common Issues

| Error                                          | Meaning                                               |
| ---------------------------------------------- | ----------------------------------------------------- |
| `OutOfMemoryError: Java heap space`            | Heap memory is exhausted                              |
| `OutOfMemoryError: GC overhead limit exceeded` | GC is running frequently but recovering little memory |
| Long GC pauses                                 | Collection is affecting application responsiveness    |

---

## The One Thing Worth Remembering

Garbage Collection removes the burden of manual memory management, but memory usage still matters.

Objects that remain referenced cannot be collected. Excessive object creation and poor memory practices can still lead to performance problems.

Understanding Garbage Collection makes memory-related issues easier to diagnose and fix.
