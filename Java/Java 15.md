#java 

#### Text-Blocks / Multiline Strings

Introduced as an experimental feature in Java 13 (see above), multiline strings are now production-ready.

```java
String text = """
                Lorem ipsum dolor sit amet, consectetur adipiscing \
                elit, sed do eiusmod tempor incididunt ut labore \
                et dolore magna aliqua.\
                """;
```

#### Sealed Classes - Preview

If you ever wanted to have an even closer grip on who is allowed to subclass your classes, there’s now the `_sealed_` feature.

```java
public abstract sealed class Shape
    permits Circle, Rectangle, Square {...}
```

This means that while the class is `_public_`, the only classes allowed to subclass `_Shape_` are `_Circle_`, `_Rectangle_` and `_Square_`.

#### Records & Pattern Matching

The `_Records_` and `_Pattern Matching_` features from Java 14 (see above), are still in preview and not yet finalized.

#### Nashorn JavaScript Engine

After having been deprecated in Java 11, the Nashorn Javascript Engine was now finally removed in JDK 15.

#### ZGC: Production Ready

The [Z Garbage Collector](https://wiki.openjdk.java.net/display/zgc/Main) is not marked experimental anymore. It’s now production-ready.