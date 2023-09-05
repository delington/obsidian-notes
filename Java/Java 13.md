#java 

You can find a complete feature list [here](https://www.oracle.com/technetwork/java/13-relnote-issues-5460548.html), but essentially you are getting Unicode 12.1 support, as well as two new or improved preview features (subject to change in the future):

#### Switch Expression (Preview)

Switch expressions can now return a value. And you can use a lambda-style syntax for your expressions, without the fall-through/break issues:

Old switch statements looked like this:

```java
switch(status) {
  case SUBSCRIBER:
    // code block
    break;
  case FREE_TRIAL:
    // code block
    break;
  default:
    // code block
}
```

Whereas with Java 13, switch statements can look like this:

```java
boolean result = switch (status) {
    case SUBSCRIBER -> true;
    case FREE_TRIAL -> false;
    default -> throw new IllegalArgumentException("something is murky!");
};
```

#### Multiline Strings (Preview)

You can _finally_ do this in Java:

```java
String htmlBeforeJava13 = "<html>\n" +
              "    <body>\n" +
              "        <p>Hello, world</p>\n" +
              "    </body>\n" +
              "</html>\n";

String htmlWithJava13 = """
              <html>
                  <body>
                      <p>Hello, world</p>
                  </body>
              </html>
              """;
```