#java 

#### Pattern Matching for instanceof

Instead of:

```java
if (obj instanceof String) {
    String s = (String) obj;
    // e.g. s.substring(1)
}
```

You can now do this:

```java
if (obj instanceof String s) {
    // Let pattern matching do the work!
    // ... s.substring(1)
}
```

#### Unix-Domain Socket Channels

You can now connect to Unix domain sockets (also supported by macOS and Windows (10+).

```java
 socket.connect(UnixDomainSocketAddress.of(
        "/var/run/postgresql/.s.PGSQL.5432"));
```

#### Foreign Linker API - Preview

A planned replacement for JNI (Java Native Interface), allowing you to bind to native libraries (think C).

#### Records & Pattern Matching

Both features are now production-ready, and not marked `_in preview_` anymore.

#### Sealed Classes

Sealed Classes (from Java 15, see above) are still in preview.

