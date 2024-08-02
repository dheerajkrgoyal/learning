## Lambda Expression

The method by which we represent anonymous function in Java 8 is called lambda expression.

```java
() -> { System.out.println("Hello World); };
```

lambda expression can be only writeen for function interface. Since functional interface has only one unimplemented method, Java figures out that anonymous function we are trying to write using lambda expression is for that unimplemented function. If there are are more than one unimplemented function then Java wont be able to figure it out since we do noy provide function name in lambda expression.

### So What is Function Interface?

Functional interface is the interface which has one abstract method but it can have multiple static and default methods.
We usually annotate functional interface usning @FunctionalInterface , but it is not mandatory.

Example: Runannble, Callable, Comparator, Comparable.

```java
@FunctionalInterface
public Interface MyFunctionalInterface{
    void m1();

    default void m2(){
        System.out.println("Default method");
    }

    static void m3(){
        System.out.println("static method");
    }
}
```

Implement above interface using lambda expression

```java
MyFucntionalInterface myInterface = () -> {
    System.out.prinltn("My lambda expression");
}; 
myInterface.m1();

//braces are not mandatory if there is just a single statement.
MyFunctionalInterface myInterface2 = () -> System.out.println("My lambda expression 2");
myInterface2.m1();

```

If we have parameters in Functional Interface then

```java
public interface SumInterface{
    void sum(int a, int b);
}

SumInterface sumInterface = (a, b) -> retur a+b;   // we do not need return as well if there is single statement. (a, b) -> a +b;
System.out.println(sumInterface.sum(1, 2));   //will print 3
```

Let's see how we can use lambda expression using real-life example. Let's say we fetch List of Books from DB or API in our service layer and we want to return it by sorting the list using book name. We use Collections.sort() wehre we can pass comparator. Since we know Comparator is functional interface we can simply pass lambda expression in the Collections.sort() function.

```java
/**
 * public class Book {
 *  private int id;
 *  private String name;
 *  private int pages;
 * }
**/

List<Book> books = BookDAO.getBook();
Collection.sort(books, (a, b) -> a1.getName().compareTo(b.getName()));
return books;
```



