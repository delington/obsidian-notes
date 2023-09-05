#java 

Java 17 is the new long-term support (LTS) release of Java, after Java 11.

#### Pattern Matching for switch (Preview)

Already available in many other languages:

```java
public String test(Object obj) {

    return switch(obj) {

    case Integer i -> "An integer";

    case String s -> "A string";

    case Cat c -> "A Cat";

    default -> "I don't know what it is";

    };

}
```

Now you can pass `_Objects_` to switch functions and check for a particular type.

#### Sealed Classes (Finalized)

A feature that was delivered in Java 15 as a preview is now finalized.

Recap: If you ever wanted to have an even closer grip on who is allowed to subclass your classes, there’s now the `_sealed_` feature.

```java
public abstract sealed class Shape
    permits Circle, Rectangle, Square {...}
```

This means that while the class is `_public_`, the only classes allowed to subclass `_Shape_` are `_Circle_`, `_Rectangle_` and `_Square_`.

#### Foreign Function & Memory API (Incubator)

A replacement for the Java Native Interface (JNI). Allows you to call native functions and access memory `_outside_` the JVM. Think C calls for now, but with plans for supporting additional languages (like C++, Fortran) over time.

#### Deprecating the Security Manager

Since Java 1.0, there had been a Security Manager. It’s now deprecated and will be removed in a future version.