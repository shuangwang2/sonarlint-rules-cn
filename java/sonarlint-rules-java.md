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



#### S1698 当"equals"被重写时不应使用"=="和"!="

> #### S1698 "==" and "!=" should not be used when "equals" is overriden
>
> **minor** **code smell**
>
> It is equivalent to use the equality == operator and the `equals` method to compare two objects if the `equals` method inherited from `Object` has not been overridden. In this case both checks compare the object references.
>
> But as soon as `equals` is overridden, two objects not having the same reference but having the same value can be equal. This rule spots suspicious uses of == and != operators on objects whose `equals` methods are overridden.
>
> **Noncompliant Code Example**
>
> ```java
> String firstName = getFirstName(); // String overrides equals
> String lastName = getLastName();
> 
> if (firstName == lastName) { ... }; // Non-compliant; false even if the strings have the same value
> ```
>
> **Compliant Solution**
>
> ```java
> String firstName = getFirstName();
> String lastName = getLastName();
> 
> if (firstName != null && firstName.equals(lastName)) { ... };
> ```
>
> **Exceptions**
>
> Comparing two instances of the Class object will not raise an issue:
>
> ```java
> Class c;
> if(c == Integer.class) { // No issue raised
> }
> ```
>
> Comparing `Enum` will not raise an issue:
>
> ```java
> public enum Fruit {
>     APPLE, BANANA, GRAPE
> }
> public boolean isFruitGrape(Fruit candidateFruit) {
>     return candidateFruit == Fruit.GRAPE; // it's recommended to activate S4551 to enforce comparison of Enums using ==
> } 
> ```
>
> Comparing with `final` reference will not raise an issue:
>
> ```java
> private static final Type DEFAULT = new Type(); 
> 
> void foo(Type other) {
>     if (other == DEFAULT) { // Compliant
>     //...
>     }
> }
> ```
>
> Comparing with this will not raise an issue:
>
> ```java
> public boolean equals(Object other) {
>     if (this == other) {  // Compliant
>         return false;
>     }
> } 
> ```
>
> Comparing with `java.lang.String` and boxed types `java.lang.Integer`, … will not raise an issue.
>
> **See**
>
> {rule:java:S4973} - Strings and Boxed types should be compared using "equals()"
>
> [MITRE, CWE-595](http://cwe.mitre.org/data/definitions/595.html) - Comparison of Object References Instead of Object Contents
>
> [MITRE, CWE-597](http://cwe.mitre.org/data/definitions/597.html) - Use of Wrong Operator in String Comparison
>
> [CERT, EXP03-J.](https://wiki.sei.cmu.edu/confluence/display/java/EXP03-J.+Do+not+use+the+equality+operators+when+comparing+values+of+boxed+primitives) - Do not use the equality operators when comparing values of boxed primitives
>
> [CERT, EXP50-J.](https://wiki.sei.cmu.edu/confluence/display/java/EXP50-J.+Do+not+confuse+abstract+object+equality+with+reference+equality) - Do not confuse abstract object equality with reference equality 

