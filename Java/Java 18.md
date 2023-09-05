#java 

#### UTF-8 by Default

If you tried, e.g. reading in files without specifying an explicit character ending, the operating system encoding was used in previous Java versions (e.g. UTF-8 on Linux and macOS, and Windows-1252 on Windows). With Java 18 this changed to UTF-8 by default.

#### Simple Web Server

Java 18 now comes with a rudimentary HTTP server, that you can start with:

```java
jwebserver
```

Learn more about its features [here](https://openjdk.org/jeps/408).

#### Other Not-So-Exciting-Stuff / Incubating Features

For a full list and overview, check out [this article](https://www.happycoders.eu/java/java-18-features/).