## Lambda Expression

The method by which we represent anonymous function in Java 8 is called lambda expression.

```java
() -> { System.out.println("Hello World"); };
```

lambda expression can be only writeen for function interface. Since functional interface has only one unimplemented method, Java figures out that anonymous function we are trying to write using lambda expression is for that unimplemented function. If there are are more than one unimplemented function then Java wont be able to figure it out since we do noy provide function name in lambda expression.

### So What is Function Interface?

Functional interface is the interface which has one abstract method but it can have multiple static and default methods.

Before Java 8, interfaces were very strict. We were only allowed to add abstract methods in it. If someone add a new method, it violates the contract for all its implementations and everyone will have to overrride the new method. But Java8 wanted to introduce stream functionality in collection interface with backward compatibility. That is why they changed the way how we write interface. This is called interface evolution.
So we can write a default method or static method in interfaces, still it wont violate any implementation contract.

By introducing static methods in interface, we do not need to create sepearte utility class like Collections.
We can have utility functions inside interface only now.

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

## Consumer, Predicate, Supplier and Function Functional Interface

### Consumer
Consumer is a functional interface introduced in Java 8 where we want to accept some value and perform some operation on it without returning anything.

It has `void accept(T t)` function which we implement, using lamba expression.

```java
Consumer consumer = (t) -> System.out.println(t)
consumer.accept(10); //will print 10
```

Example: forEach function uses consumer interface in java streams.

### Predicate
Predicate is a functional interface where want to accept some value and return true/false.

It has `boolean test(T t)` function  which we implement using lambda expresison.

```java
Predicate predicate = (t) -> t%2 == 0
predicate.test(4) //will return true
predicate.test(3) //will return false 
```

Example: filter uses predicate interface in java stream.

### Supplier
Supplier is a functional interface which does not accept anything but return something.

It has `T get()` function which we implement using lambda expression.

```java 
Supplier supplier = () -> return 1;
suuplier.get() //will return 1
```

Example: orElseGet function uses supplier interface in java streams.

### Function
Function is a functional interface which accepts value in parameter, may be apply some transformation and return the value.

It has `R apply(T t)` function which we implement using lambda expression.

```java
Function function = (a) -> i*2;
function.apply(2);  //will return 4
```

Example: map function in stream uses Function Interface.

## Java Streams

Stream API in java 8 is a way to process the collection of object from collections, array or I/O channel.
It doesn't change the original collection but perform immutable operations and return entirely new collection. In stream there are various methods which can be pipelined together to perform operation in sequential manner. Most of these functions accept functional interface discussed above which means we can use lambda expression to define our custom operation. We just have to convert our collection or array to stream then we can apply sequential pipeline of operation on stream and return new collection.

```java

List<String> list = new ArrayList<>(Arrays.asList("apple", "ball", "cat", "dog"));
list.stream().forEach(t -> System.out.prinltn(t)); //will print apple, ball, cat and dog

Map<Integer, String> map = new HAshMap<>();
map.put(1, "a");
map.put(2, "b");
map.put(3, "c");

map.stream().forEach((key, value) -> System.out.println("key: " + key + " value: " + value)); //will print key, value

list.stream().filter(t -> t.startsWith("a")).forEach(t-> System.out.println(t)); //will only print apple

map.stream().filter(e -> e.getKey() % 2 == 0).forEach(t -> System.out.println(t)); //will print entry with even key

list.stream().sorted().forEach(t -> System.out.println(t)); //will sort and display the element in ascending order

list.stream().sorted(Comparator.reverseOrder()).forEach(t -> System.out.println(t)); //will sort and display the element in descending order

map.entrySet().stream().sorted(Map.Entry.comparingByKey()).forEach(t -> System.out.println(t)) // will sort the map based on key

map.entrySet().stream().sorted(Map.Entry.comparingByValue()).forEach(t -> System.out.println(t)) // will sort the map based on value

```

Let's seee with the real-life example:

```java
/**
 * public class Employee{
 *  private int id;
 *  private String name;
 *  private String department;
 *  private long salary;
 * }
 * */

List<Employee> listOfEmployee = EmployeeDAO.getEmployees();
Map<Employee, Integer> mapOfEmployee = new HAshMap<>(); //Let's assume we have data in map

//employees whose salary is more than 5 lakh
lisOfEmployee.stream().filter(emp -> emp.ghetSalary() > 500000).collect(Collectors.toList()); //collect is a terminal function which will accumulate the result of peipeline in the list

//employees sort based on salary
listOfEmployee.stream().sorted(Comparator.comparing(emp -> emp.getSalary())).collect(Collectors.toList());

//employees sort based on name
listOfEmployee.stream().sorted(Comparator.comparing(Employee::getName)).collect(Collectors.toList()); //Just different way to use function name (method reference) in comparing method

//Employee map sort by salary ascending
mapOfEmployee.entrySet().stream().sorted(Map.Entry.comparingByKey(Comparator.comparing(Employee::getSalary))).forEach(System.out::println); // will print map sorted by key which is employee's salary

//Employee map sort by salary descending
mapOfEmployee.entrySet().stream().sorted(Map.Entry.comparingByKey(Comparator.comparing(Employee::getSalary).reversed())).forEach(System.out::println);
```

### Map and FlatMap

Both `map()` and `flatMap()` returns another stream after applying transformation.
`map()` is used for transformation. It produces single value for each input value. That is why it is called one-to-one mapping.
`flatMap()` is used for transformation + flattening. It takes stream of stream as input and produces stream of object. It produces multiple value for each input hence it is called one-to-many mapping.

```java
/**
 * public class Customer{
 *  private int id;
 *  private String name;
 *  private String email;
 *  private List<String> phoneNumbers;
 * }
 * */

List<Customer> listOfCustomer = CustomerDAO.getCustomers();

//Get list of emails from list of customers
listOfCustomer.stream().map(cust -> cust.getEmail()).collect(Collectors.toList());

 //Get list of phone numbers
List<List<String>> phoneNumbers = listOfCustomer.stream().map(cust -> cust.getPhoneNumbers()).collect(Collectors.toList());  //we will get list of list here

//Get list of phone numbers by flattening it
List<String> phoneNumber = listOfCustomer.stream().flatMap(cust -> cust.getPhoneNumbers().stream()).collect(Collectors.toList());
```

## Optional

Optional class was introduced in Java 8 to avoid null pointer exception as much as possible. It might happen that we are fetching some object from somewhere and that object is null. Before checking if that object is null we try to do some operation on that object and then we encounter null pointer exception during runtime.
We might forget to check the null condition in the object may be because we dont expect it to be null. In such cases when object might be null Java tells that instead of returning the object, we should wrap the object with Optional and then send it. Then it becomes kind-of mandatory for us to check if the object is present or not.

There are various ways to created Optionapl object.
1. `Optional.empty()`: It will return empty Optional object.
2. `Optional.of()`: We can pass our object inside `of(object)` if we are sure object is never null.
3. `Optional.ofNullable()`: Similar to above except we can pass null object as well here. Optional class automatically checks if the passed object is null or not. If it is null it makes the Optional object empty.

So when we recieve any Optional object we kind-of remember to check if it is empty or not using `Optional.isPresent()`. If is it not empty then we fetch actual object using `Optional.get()`

Also there are may utility functions inside Optional class to handle situation when actual object is null or Optional object is empty.

```java
Optional.get().orElse("Default Value");

Optional.get().orElseGet(() -> {
    //any custom logic in-case actual object is null
    return something;
})

Optional.get().orElseThrow(() -> new CustomException());

/**
 * public class Customer{
 *  private int id;
 *  private String name;
 *  private String email;
 *  private List<String> phoneNumbers;
 * }
 **/

List<Customer> listOfCustomer = CustomerDAO.getCustomers();

//Get Customer which matches passed email, if it not present get empty Customer
listOfCustomer.stream().filter(cust -> customer.getEmail().equals(email))
    .findAny()
    .get()
    .orElse(new Customer());
```

## Map and Reduce

`map()`: is used to tranform the data as discussed above
`reduce()`: is used to aggreagate the data i.e. produce a single value from the stream of object

Exmaple: if we have a stream of integer [2, 3, 4, 5, 6] and we want to find the sum then reduce method is useful.

Reduce Signature:

```java
T reduce(T identity, BinaryOperator<T> accumulator);

Integer sum = Stream.of(2, 4, 6, 9, 1, 3, 7)
    .reduce(0, (a, b) -> a + b);

//identity: initial value
//accumulator: (a,b) -> a+b function

List<Integer> numbers = Arrays.asList(3, 5, 22, 1, 6, 7);

//If we have list of numbers we use map to convert Integer object to primitive and then use sum reduce in-built function
numbers.stream().mapToInt(i -> i).sum();
//or
numbers.stream().reduce(0, (a, b) -> a + b);

//Multiply the integers
numbers.stream().reduce(1, (a,b) -> a * b);

//Max number
numbers.stream().reduce(Integer.MIN_VALUE, (a, b) -> a>b ? a : b);

List<String> strings = Arrays.asList("apple", "ball", "cat", "dog");

//Find the longest String
strings.stream().reduce((a, b) -> a.length() > b.length() ? a : b); //initial value is not mandatory
```

Real-life Example:

```java
/**
 * public class Employee{
 *  private int id;
 *  private String name;
 *  private String grade;
 *  private double salary;
 * }
 * */

List<Employee> employees = EmployeeDAO.getEmployees();

//Find the average salary of employees whose grade is A
employees.stream()
    .filter(emp -> emp.getGrade().equals("A"))
    .map(emp -> emp.getSalary())
    .mapToDouble(i -> i)
    .average()
    .getAsDouble();
```

## Parallel Stream

It utilizes multiple core of the processor.
By default, streams are sequential and are run in single core. But we can divide the stream into chunks and process them into multiple cores and accumulate result automatically using parallel stream.

However, the order of execution is not under our control.

```java
List<Integer> list = Arrays.asList(2, 3, 6, 4, 1, 8);

list.parallelStream().mapToInt(i -> i).average();
```

## Exercises

```java
/**
 * class Book {
 *  private String title;
 *  private List<Author> authors;
 *  private int pages;
 *  private Subject subject;
 *  private int year;
 *  private String isbn;
 * }
**/

List<Book> list = BookDAO.getBooks();

//print all books
list.stream().forEach(System.out::println);

//find books with more than 400 pages
list.stream().filter(b -> b.getPages() > 400).toList();

//find all the java books more than 400 pages
list.stream.filter(b -> b.getSubject() == Subject.JAVA).filter(b -> b.getPages() > 400).toList();

//Get top 3 longest books
list.stream().sorted(Compartor.comparing(Book::getPages).reversed()).limit(3).toList();

//Get all the books sorted by pages in descending order from 4th book onward
list.stream().sorted(Comparator.comparing(Book::getPages).reversed()).skip(3).toList();

//Get all the publishing years
list.stream().map(Book::getYear).toList();

//Print all the authors
list.stream().flatMap(b -> b.getAuthors().stream()).forEach(System.out::println);

//Print the unique countries of all authors
list.stream().flatMap(b -> b.getAuthors().stream()).map(Author::getCountry).distinct().forEach(System.out::println);

//Find the most recent book published
list.stream().sorted(Comparator.comparing(Book::getYear).reversed()).limit(1).findFirst().get();
//or
list.stream().max(Comparator.comparing(Book::getYear)).get();

//Total no. of pages published for all books
list.stream().maptoInt(Book::getPages).sum();

//We want to know how many pages does longest book has
list.stream().map(Book::getPages()).reduce(Integer::max);

//We want to know average no. of pages of the books
list.stream().collect(Collectors.averagingDouble(Book::getPages));

//We want map of book per year
list.stream().collect(Collectors.groupingBy(Book::getYear));

//We want to count how many books are published per year
list.stream().collect(Collectors.groupingBy(Book::getYear, Collectors.counting()));
```