#java 

#### Switch Expression (Standard)

The switch expressions that were _preview_ in versions 12 and 13, are now standardized.

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    default      -> {
      String s = day.toString();
      int result = s.length();
      yield result;
    }
};
```

#### Records (Preview)

There are now record classes, which help alleviate the pain of writing a lot of boilerplate with Java.

Have a look at this pre Java 14 class, which only contains data, (potentially) getters/setters, equals/hashcode, toString.

```java
final class Point {
    public final int x;
    public final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
    // state-based implementations of equals, hashCode, toString
    // nothing else
```

With records, it can now be written like this:

```java
record Point(int x, int y) { }
```

Again, this is a preview feature and subject to change in future releases.

#### Helpful NullPointerExceptions

Finally NullPointerExceptions describe _exactly_ which variable was null.

```java
author.age = 35;
---
Exception in thread "main" java.lang.NullPointerException:
     Cannot assign field "age" because "author" is null
```

#### Pattern Matching For InstanceOf (Preview)

Whereas previously you had to (cast) your objects inside an instanceof like this:

```java
if (obj instanceof String) {
    String s = (String) obj;
    // use s
}
```

You can now do this, effectively dropping the cast.

```java
if (obj instanceof String s) {
    System.out.println(s.contains("hello"));
}
```

#### Packaging Tool (Incubator)

There’s an incubating _jpackage_ tool, which allows to package your Java application into platform-specific packages, including all necessary dependencies.

- Linux: deb and rpm
    
- macOS: pkg and dmg
    
- Windows: msi and exe
    

#### Garbage Collectors

The Concurrent Mark Sweep (CMS) Garbage Collector has been removed, and the experimental Z Garbage Collector has been added.