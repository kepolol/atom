#HSLIDE
# Java
lecture 3
## Generics & Collections


#HSLIDE
## Отметьтесь на портале
https://atom.mail.ru/


#HSLIDE
### get ready
```bash
> git fetch upstream
> git checkout -b lecture03 upstream/lecture03
```

#HSLIDE
### Agenda
1. External libraries usage
1. Exceptions
1. Generics
1. Collections
1. Homework 2


#HSLIDE
### External libraries usage
1. **[External libraries usage]**
1. Exceptions
1. Generics
1. Collections
1. Homework 2


#HSLIDE
### External libraries usage
1. External libraries usage
1. **[Exceptions]**
1. Generics
1. Collections
1. Homework 2


#HSLIDE
### Anything that can go wrong will go wrong

- lost connection
- inconsistent object state
- out of memory
- file not found


#HSLIDE
### Some problems could be fixed in runtime
Recovery:
- reconnect
- fix/rebuild object
- free some memory
- create new file

or
- cancel operations


#HSLIDE
### Exceptions hierarchy
<img src="lecture03/presentation/assets/img/exception.png" alt="exception" style="width: 750px;"/>


#HSLIDE
##Checked exceptions must be handled


#HSLIDE
### Throwable
Superclass of all errors and exceptions

```java
Throwable()
Throwable(String message)
Throwable(Throwable cause)
Throwable(String message, Throwable cause)

StackTraceElement getStackTrace()
Message getMessage()
Throwable getCause()
```

#HSLIDE
### Stacktrace
```java
Exception in thread "main" java.lang.NullPointerException: Ой всё
        at ru.atom.makejar.HelloWorld.getHelloWorld(HelloWorld.java:12)
        at ru.atom.makejar.HelloWorld.main(HelloWorld.java:8)

```

Build and run jar with
```java
package ru.atom.makejar;

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println(getHelloWorld());
    }

    public static String getHelloWorld() {
        throw new NullPointerException("Ой всё");
    }
}
```


#HSLIDE
### Exception handling 
### try-catch-finally
```java
try {
    statements
} catch (ExceptionType1 e) {
    statements
// more catches    
} catch (ExceptionType2 e) {
    statements
} finally {
    statments
}
```


#HSLIDE
### Exception handling example
```java
try {
    String possibleInt = "itsNotANumber";
    return Integer.parseInt(possibleInt);
} catch (ExceptionType e) {
    log.error("Exception in possibleInt parsing. {} is not an integer", possibleInt);
    return DEFAULT_INT_VALUE; 
} finally {
    if (dbConnection != null 
        && dbConnection.isOpened()) {
        
        dbConnection.close();
    }
}
```


#HSLIDE
### Exceptions
@See ru.atom.exception

1. try-catch-finally
1. catch or throws
1. multiple catches
1. try with resources


#HSLIDE
### Generics
1. External libraries usage
1. Exceptions
1. **[Generics]**
1. Collections
1. Homework 2

#HSLIDE
### non-generic

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

#HSLIDE
### Problem
In our code
```java
Box box = new Box();
String content = "Awesome content";
box.set(content);
```

In a galaxy far far away
```java
Object content = box.getContent();
// Nooooooooooooooo
// cast is not type safe
```

      
#HSLIDE
### Generic Solution
Definition
```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

Usage
```java
Box<String> box = new Box<>();
String content = "Awesome content";
box.set(content);

String outgoing = box.getContent();

assertThat(outgoint, is(equalTo(content))); // <-- OK
```
      
      
#HSLIDE
### Generics in Java 

1. Way to implement general purpose algorithms
1. Way to avoid type casting
1. Provides type casting under the hood
1. Presents and processed in **Compile-time**. Absents in runtime*       
      

#HSLIDE
### Generics Definition
Generic class
```java
class Clazz<T1, T2, T3> {
}

Clazz<Boolean, Integer, String> instance = new Clazz<>();
```

Generic method
```java
class Clazz {
    public <T> void foo(T t) {
        log.info("Generic method call with param {}", t);
    }
}

Clazz instance = new Clazz();
instance.foo("string value");
```
[Read more in official documentation](https://docs.oracle.com/javase/tutorial/java/generics/methods.html)


#HSLIDE
### Generics buzzwords
- Wildcards
- Type bounds
- Type erasure
- Bridge method
- Raw-type
      
#HSLIDE
### What about inheritance?
```java
class Clazz<T> { }
```
 
```
Clazz<Object> is not a superclass for Clazz<Integer>
```

#HSLIDE
### Bounds and restrictions
```java  
<T extends A & B & C>
<? extends A> - wildcard with restriction
<? super A> - wildcard with restriction
<?> - wildcard without restriction  
```
[Read more in official documentation](https://docs.oracle.com/javase/tutorial/java/generics/bounded.html)

#HSLIDE
### Type erasure
 
@TODO 


#HSLIDE
### Collections
1. External libraries usage
1. Exceptions
1. Generics
1. **[Collections]**
1. Homework 2


#HSLIDE
### Quiz  
```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.addAll(Arrays.asList(2, 3, 4, 5, 6));
list.remove(new Integer(2));

assertThat(list.size(), is(equalTo( ??? )));
assertThat(list.get(2), is(equalTo( ??? )));
assertThat(list.contains(42), is( ??? ));
```

#HSLIDE
### ArrayList. Summary
- Auto resizable-array implementation of the `List` interface. e.g. dynamic array
- Place in hierarchy:
```java
    java.lang.Object
        java.util.AbstractCollection<E>
            java.util.AbstractList<E>
                java.util.ArrayList<E>
```
- Providing interfaces
    - List 
    - RandomAccess
    - Serializable 
    - Cloneable
    - Collection
    - Iterable

[Read more (RU)](https://habrahabr.ru/post/128269/)


#HSLIDE
### ArrayList is a List
List is a Ordered Collection or sequence
```java
interface List<E> extends Collection<E> {
    E get(int index);
    E set(int index, E element);
    void add(int index, E element);
    E remove(int index);
    List<E> subList(int fromIndex, int toIndex);
    // ...
}
```


#HSLIDE
### ArrayList. Internals #1

```java
List<String> list = new ArrayList<>();
```
<img src="lecture03/presentation/assets/img/newarray.png" alt="exception" style="width: 600px;"/>

```java
list.add("0");
list.add("1");
```

<img src="lecture03/presentation/assets/img/array1.png" alt="exception" style="width: 600px;"/>


#HSLIDE
### ArrayList. Internals #2
```java
list.addAll(Arrays.asList("2","3", "4", "5", "6", "7", "8"));
list.add("9");
```

<img src="lecture03/presentation/assets/img/array9.png" alt="exception" style="width: 600px;"/>


#HSLIDE
### ArrayList. Internals #3
```java
list.add("10");
```
Not enough capacity. Need (auto)resize.

<img src="lecture03/presentation/assets/img/arrayresized.png" alt="exception" style="width: 750px;"/>

<img src="lecture03/presentation/assets/img/array10.png" alt="exception" style="width: 750px;"/>


#HSLIDE
### Summary

#HSLIDE
**Оставьте обратную связь**
(вам на почту придет анкета)  

**Это важно!**
