#### **S2204 ".equals()"不应该用于检查"Atomic"类的值**

**主要** **缺陷** 

`AtomicInteger`和 `AtomicLong`继承`Number`，但它们不同于`Integer`和`Long`，应该以不同的方式处理。`AtomicInteger`和`AtomicLong`设计用于支持单变量上的无锁、线程安全编程。因此，一个`AtomicInteger`对象只会与对象自身“相等”。相反，您应该`.get（）`值并对其进行比较。

这适用于所有原子的、类似原子的包装类：`AtomicInteger`、`AtomicLong`和`AtomicBoolean`。

**不合规代码示例**

```java
AtomicInteger aInt1 = new AtomicInteger(0);
AtomicInteger aInt2 = new AtomicInteger(0);

if (aInt1.equals(aInt2)) { ... }  // Noncompliant 
```

**合规代码示例**

```java
AtomicInteger aInt1 = new AtomicInteger(0);
AtomicInteger aInt2 = new AtomicInteger(0);

if (aInt1.get() == aInt2.get()) { ... } 
```



> #### S2204 ".equals()" should not be used to test the values of "Atomic" classes
>
> **major** **bug** 
>
> `AtomicInteger`, and `AtomicLong` extend `Number`, but they’re distinct from `Integer` and `Long` and should be handled differently. `AtomicInteger` and `AtomicLong` are designed to support lock-free, thread-safe programming on single variables. As such, an `AtomicInteger` will only ever be "equal" to itself. Instead, you should `.get()` the value and make comparisons on it.
>
> This applies to all the atomic, seeming-primitive wrapper classes: `AtomicInteger`, `AtomicLong`, and `AtomicBoolean`.
>
> **Noncompliant Code Example**
>
> ```java
> AtomicInteger aInt1 = new AtomicInteger(0);
> AtomicInteger aInt2 = new AtomicInteger(0);
> 
> if (aInt1.equals(aInt2)) { ... }  // Noncompliant 
> ```
>
> **Compliant Solution**
>
> ```java
> AtomicInteger aInt1 = new AtomicInteger(0);
> AtomicInteger aInt2 = new AtomicInteger(0);
> 
> if (aInt1.get() == aInt2.get()) { ... } 
> ```
>



------




#### S2757 不能用“=+”代替“+=”

**主要** **缺陷**



使用相反的运算符对（`=+`，`=-`或`=!`），意味着运算符按照（`+=`，`-=`或`!=`）编译并运行， 但不会产生预期的结果。

此规则会在 =+、=- 或 =！时引起问题，在两个运算符之间没有任何空格并且后面至少有一个空格字符时使用。

**不合规代码示例**

```java
int target = -5;
int num = 3;

target =- num;  // Noncompliant; target = -3. Is that really what's meant?
target =+ num; // Noncompliant; target = 3
```

**合规代码示例**

```java
int target = -5;
int num = 3;

target = -num;  // Compliant; intent to assign inverse value of num is clear
target += num; 
```



> #### S2757 "=+" should not be used instead of "+="
>
> **major** **bug**
>
> The use of operators pairs ( =+, =- or =! ) where the reversed, single operator was meant (+=, -= or !=) will compile and run, but not produce the expected results.
>
> This rule raises an issue when =+, =-, or =! is used without any spacing between the two operators and when there is at least one whitespace character after.
>
> **Noncompliant Code Example**
>
> ```java
> int target = -5;
> int num = 3;
> 
> target =- num;  // Noncompliant; target = -3. Is that really what's meant?
> target =+ num; // Noncompliant; target = 3
> ```
>
> **Compliant Solution**
>
> ```java
> int target = -5;
> int num = 3;
> 
> target = -num;  // Compliant; intent to assign inverse value of num is clear
> target += num; 
> ```





------

