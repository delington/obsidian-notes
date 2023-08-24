#java

#### Collections

Collections got a couple of new helper methods, to easily construct Lists, Sets and Maps.

```java
List<String> list = List.of("one", "two", "three");
Set<String> set = Set.of("one", "two", "three");
Map<String, String> map = Map.of("foo", "one", "bar", "two");
```

#### Streams

Streams got a couple of additions, in the form of takeWhile,dropWhile,iterate methods.

```java
Stream<String> stream = Stream.iterate("", s -> s + "s")
  .takeWhile(s -> s.length() < 10);
```

#### Optionals

Optionals got the sorely missed ifPresentOrElse method.

```java
user.ifPresentOrElse(this::displayAccount, this::displayLogin);
```

#### Interfaces

Interfaces got private methods:

```java
public interface MyInterface {

    private static void myPrivateMethod(){
        System.out.println("Yay, I am private!");
    }
}
```

#### Other Language Features

And a couple of other improvements, like an improved try-with-resources statement or diamond operator extensions.

#### JShell

Finally, Java got a shell where you can try out simple commands and get immediate results.

```
% jshell
|  Welcome to JShell -- Version 9
|  For an introduction type: /help intro

jshell> int x = 10
x ==> 10
```

#### HTTPClient

Java 9 brought the initial preview version of a new HttpClient. Up until then, Java’s built-in Http support was rather low-level, and you had to fall back on using third-party libraries like Apache HttpClient or OkHttp (which are great libraries, btw!).

With Java 9, Java got its own, modern client - although in preview mode, which means subject to change in later Java versions.

#### Project Jigsaw: Java Modules and Multi-Release Jar Files

Java 9 got the [Jigsaw Module System](https://www.oracle.com/corporate/features/understanding-java-9-modules.html), which somewhat resembles the good old [OSGI specification](https://en.wikipedia.org/wiki/OSGi). It is not in the scope of this guide to go into full detail on Jigsaw, but have a look at the previous links to learn more.

Multi-Release .jar files made it possible to have one .jar file which contains different classes for different JVM versions. So your program can behave differently/have different classes used when run on Java 8 vs. Java 10, for example.