#java 
#### Strings & Files

Strings and Files got a couple new methods (not all listed here):

```java
"Marco".isBlank();
"Mar\nco".lines();
"Marco  ".strip();

Path path = Files.writeString(Files.createTempFile("helloworld", ".txt"), "Hi, my name is!");
String s = Files.readString(path);
```

#### Run Source Files

Starting with Java 10, you can run Java source files _without_ having to compile them first. A step towards scripting.

```
ubuntu@DESKTOP-168M0IF:~$ java MyScript.java
```

#### Local-Variable Type Inference (var) for lambda parameters

The header says it all:

```java
(var firstName, var lastName) -> firstName + lastName
```

#### HttpClient

The HttpClient from Java 9 in its final, non-preview version.

#### Other stuff

Flight Recorder, No-Op Garbage Collector, Nashorn-Javascript-Engine deprecated etc