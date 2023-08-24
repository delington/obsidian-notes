#java 

There have been a few changes to Java 10, like Garbage Collection etc. But the only real change you as a developer will likely see is the introduction of the "var"-keyword, also called local-variable type inference.

#### Local-Variable Type Inference: var-keyword

```java
// Pre-Java 10

String myName = "Marco";

// With Java 10

var myName = "Marco"
```

Feels Javascript-y, doesn’t it? It is still strongly typed, though, and only applies to variables _inside methods_ (thanks, [dpash](https://www.reddit.com/user/dpash), for pointing that out again).