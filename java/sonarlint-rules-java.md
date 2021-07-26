#### S2204 ".equals()" should not be used to test the values of "Atomic" classes

".equals()"不应该用于检查"Atomic"类的值

**bug** **major**
缺陷 主要

`AtomicInteger`, and `AtomicLong` extend `Number`, but they’re distinct from `Integer` and `Long` and should be handled differently. `AtomicInteger` and `AtomicLong` are designed to support lock-free, thread-safe programming on single variables. As such, an `AtomicInteger` will only ever be "equal" to itself. Instead, you should `.get()` the value and make comparisons on it.

This applies to all the atomic, seeming-primitive wrapper classes: `AtomicInteger`, `AtomicLong`, and `AtomicBoolean`.

**Noncompliant Code Example**

```java
AtomicInteger aInt1 = new AtomicInteger(0);
AtomicInteger aInt2 = new AtomicInteger(0);

if (aInt1.equals(aInt2)) { ... }  // Noncompliant 
```

**Compliant Solution**

```java
AtomicInteger aInt1 = new AtomicInteger(0);
AtomicInteger aInt2 = new AtomicInteger(0);

if (aInt1.get() == aInt2.get()) { ... } 
```



------

