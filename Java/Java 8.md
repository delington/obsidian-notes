#java

#### Language Features: Lambdas etc.

Before Java 8, whenever you wanted to instantiate, for example, a new Runnable, you had to write an anonymous inner class like so:

```java
 Runnable runnable = new Runnable(){
       @Override
       public void run(){
         System.out.println("Hello world !");
       }
     };
```

With lambdas, the same code looks like this:

```java
Runnable runnable = () -> System.out.println("Hello world two!");
```

You also got method references, repeating annotations, default methods for interfaces and a few other language features.

#### Collections & Streams

In Java 8 you also got functional-style operations for collections, also known as the Stream API. A quick example:

```java
List<String> list = Arrays.asList("franz", "ferdinand", "fiel", "vom", "pferd");
```

Now pre-Java 8 you basically had to write for-loops to do something with that list.

With the Streams API, you can do the following:

```java
list.stream()
    .filter(name -> name.startsWith("f"))
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
```

#### Optional

```java
Optional<String> empty = Optional.empty();
assertFalse(empty.isPresent());

Optional<String> opt = Optional.of("baeldung");
opt.ifPresent(name -> System.out.println(name.length()));
```

#### Interface - default methods

The method `newMethod()` in `MyInterface` is a default method, which means we need not to implement this method in the implementation class `Example`. This way we can add the default methods to existing interfaces without bothering about the classes that implements these interfaces.

```java
interface MyInterface{  
    /* This is a default method so we need not
     * to implement this method in the implementation 
     * classes  
     */
    default void newMethod(){  
        System.out.println("Newly added default method");  
    }  
    /* Already existing public and abstract method
     * We must need to implement this method in 
     * implementation classes.
     */
    void existingMethod(String str);  
}  
public class Example implements MyInterface{ 
	// implementing abstract method
    public void existingMethod(String str){           
        System.out.println("String is: " + str);  
    }  
    public static void main(String[] args) {  
    	Example obj = new Example();
    	
    	//calling the default method of interface
        obj.newMethod();     
        //calling the abstract method of interface
        obj.existingMethod("Java 8 is easy to learn"); 
  
    }  
}
```
