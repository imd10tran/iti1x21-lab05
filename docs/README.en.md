
# ITI1121 - Lab 05 - Interface, Abstract Class and Inheritance

## Submission

Please read the [junit instructions](JUNIT.en.md) for help
running the tests for this lab.

Please read the [Submission Guidelines](SUBMISSION.en.md) carefully.
Errors in submitting will affect your grades.

Submit your answers to

* Book.java
* Library.java
* BookComparator.java
* Series.java
* AbstractSeries.java
* Arithmetic.java
* Geometric.java


# First Part

## Learning Objectives

* **Create** your own method **equals(Object o)**
* **Implement** a parametrized interface
* **Use** the interface **java.util.Comparator\<T>**

Section 1: the method **equals**
--------------------------------

As you know, the class **Object** defines a method **[equals](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#equals-java.lang.Object-)**.

```java
public class Object {
  // ...
  public boolean equals(Object obj) {
    return this == other;
  }
}
```


Since in Java, every class inherits from the class **Object**, every class inherits the method **boolean equals(Object obj)**. This is true for the predefined classes, such as **String** and **Integer**, but also for any class that you will define.

The method **equals** is used to compare **references** variables. Consider two references, **a** and **b**, and the designated objects.

* If you need to compare the **identity** of two objects (do **a** and **b** designate the same object?), use "**\==**" (i.e. **a == b** );
* If you need to compare the **content** of two objects (is the **content** of the two objects the same?), use the method "**equals**" (i.e. **a.equals(b)**).

In this first part, we are going to create a simple class **Book**. This class is quite minimal. It contains the following:

* An instance variable **author** of type **String**;
* An instance variable **title** of type **String**;
* An instance variable **year** of type **int** (the publication year of that book);
* A contructor **Book(String author, String title, int year)**, which receives that new instance's author, title and publication year as paramters;
* A method **toString()**, which returns a **String** representation of that instance, using the format "_author: title (year)_";
* A method **equals(Object o)**.

We want our implementation to be as robust as possible. In particular, the method **equals** should handle every particular case you can think about. Make sure your code passes all the provided JUnit tests.

### Question 1.1:

Provide an implementation of the class **Book**. Use the provided template as a starting point.

* Book.java

Section 2: the interface **java.util.Comparator\<T>**
----------------------------------------------------

In the previous laboratory, we have used the interface [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html) to create objects of type Post. A comparable object (implementing the **Comparable** interface) overrides the method **compareTo** and is capable of comparing itself with another object.

Unlike Comparable, a class implementing the interface **Comparator** is external to the element type we are comparing. Such a class must implement the methods **compare** and **equals**, to define and impose the sort order.

Sort methods such as **[Collections.sort](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-java.util.Comparator-)** or **[Arrays.sort](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-java.util.Comparator-)** take a instance of **Comparator** as a parameter.

For this exercise, we will implement an application for managing a library. The Library class will have a list of Book instances. We will be using the resizable-array implementation offered by the class [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), which automatically increases the capacity as new elements are added to the list.

The idea is that the method **sort** of the class **ArrayList** will use the method **compare** of the comparator when sorting the elements of the list. Therefore, the method **compare** needs to handle elements that are of a type which is compatible with the type of the elements in the ArrayList.

Since **Comparator** is an interface, it needs a concrete implementation. Depending on the criterion for ordering, we can have multiple implementations through different classes and obtain different results when sorting.

Note that the class ArrayList is parameterized. In our case, we use **ArrayList\<Book>**. Our ArrayList instance will hold instances of the class **Book**. Therefore, its method **sort** needs to compare instances of the Book class, and requires an instance of **Comparator\<Book>** for this.

Thus, the class **Library** will have a list of Book instances stored in an instance of **ArrayList**. Using a **Comparator**, we want to be able to sort our library by authors first, then by title, then by year of publication (of course, in a real case we would want to have several Comparators and sort our library in different orders).

The class **Library** has the following methods:

* **void addBook (Book b)**: adds a Book instance to the library;
* **void sort()**: sorts the books using a Comparator;
* **void printLibrary()**: prints all the book in their current order of the list.

### Question 1.2:

Provide an implementation of the class **Library** as well as of **BookComparator**. Use the provided template as a starting point for the class Library and the provided template for BookComparator

* Library.java
* BookComparator.java

# Second Part

## Learning Objectives

* **Create** and **implement** an interface
* **Create** an abstract class
* **Further understanding** of inheritance

Abstract classes and interfaces
-------------------------------

In mathematics, a series is an infinite sequence of terms added together. The **partial sum of the series**, <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n">, is the sum of the first <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20n"> terms.

<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20a_i">
</div>

Here, you must create a class hierarchy, as illustrated by the UML diagram below, such that all the series have a method **next**, which returns the next <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n">. The first call to the method **next** returns <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_1">, which is <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a_1">. The next call to the method *next* returns <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_2">, which is <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a_1&plus;a_2">. The next call to the method **next** returns <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_3">, which is <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a_1&plus;a_2&plus;a_3"> and so on. The implementation of the method **next** is specific to the type of series, here **Arithmetic** and **Geometric**. Specifically, this hierarchy consists of the interface **Series**, an abstract class called **AbstractSeries**, as well as two concrete implementations, called **Arithmetic** and **Geometric**.

![UML display](uml_series-720.png)

Here is a test program that illustrates the intended use of the classes:

* for the partial sum of an Arithmetic Series with 1 as the first term and 1 as common difference;
* for the partial sum of a Geometric Series with 1 as the first term and 0.5 as common ration.

```java
public class TestSeries {

  public static void main(String[] args) {

    AbstractSeries sn;
    double[] tuple;

    sn = new Arithmetic();

    System.out.println("The first five terms of the arithmetic series are:");

    for (int n=0; n<5; n++) {
      System.out.println(sn.next());
    }

    sn = new Geometric();

    System.out.println();
    System.out.println("The first five terms of the geometric series are:");

    tuple = sn.take(5);

    for (int n=0; n<5; n++) {
      System.out.println(tuple[n]);
    }

  }
}
```


Here is the expected output:

```java
The first five terms of the arithmetic series are:
1.0
3.0
6.0
10.0
15.0
The first five terms of the geometric series are:
1.0
1.5
1.75
1.875
1.9375
```

The first loop displays the values 1.0, 3.0, 6.0, 10.0, 15.0, whilst the second one displays, 1.0, 1.5, 1.75, 1.875, 1.9375.

### Question 2.1: Series

Create an interface called **Series**. It declares a method called **next** that has a return-type **double**.

Use the provided template as a starting point.

* Series.java

### Question 2.2: AbstractSeries

Write the implementation of the abstract class **AbstractSeries**. It implements the interface **Series**. The class implements a method called **take** that returns an array containing the next **k** partial sums of this series, where **k** is the formal parameter of the method **take**.

Use the provided template as a starting point.

* AbstractSeries.java

### Question 2.3: Arithmetic

Implement the class **Arithmetic**, which is a subclass of **AbstractSeries**. In this class, the first call to the method **next** returns the value 1.0, the second call returns the value 3.0, the third call returns 6.0, the fourth call returns 10.0, etc. The general formula is as follows, the <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20i">th call to the method **next** returns <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_%7Bi-1%7D&plus;i">, where <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20i%5Cin1%2C2%2C3..."> and <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_%7Bi-1%7D"> is the value that was returned by the previous call to the method **next**.

<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20i">
</div>

The first 5 partial sums of the arithmetic series are:
<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_1%20%3D%201"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_2%20%3D%201&plus;2%3D%203"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_3%20%3D%201&plus;2&plus;3%20%3D%206"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_4%20%3D%201&plus;2&plus;3&plus;4%20%3D%2010"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_5%20%3D%201&plus;2&plus;3&plus;4&plus;5%20%3D%2015"><br/>
</div>

Use the provided template as a starting point.

* Arithmetic.java

### 2.3 Geometric

Implement the class **Geometric**, which is a subclass of **AbstractSeries**. Each call to the method **next** produces the next partial sum of the series according to the formula below. The first call returns 1.0, the second call returns 1.5, the third call returns 1.75, etc. You can use **Math.pow(a, b)**, which returns <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a%5Eb">, for your implementation.

<div align="center">
<img src = "https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n%20%3D%20%5Csum_%7Bi%3D0%7D%5E%7Bn%7D%5Cfrac%7B1%7D%7B2%5Ei%7D">
</div>

The first 5 partial sums of the geometric series are:

<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_1%20%3D%201"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_2%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D%3D%201.5"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_3%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D&plus;%5Cfrac%7B1%7D%7B4%7D%3D%201.75"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_4%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D&plus;%5Cfrac%7B1%7D%7B4%7D&plus;%5Cfrac%7B1%7D%7B8%7D%3D%201.875"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_5%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D&plus;%5Cfrac%7B1%7D%7B4%7D&plus;%5Cfrac%7B1%7D%7B8%7D&plus;%5Cfrac%7B1%7D%7B16%7D%20%3D%201.9375"><br/>
</div>

A call to the method next produces the next partial sum, i.e. the next value of <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n">.

Use the provided template as a starting point.

* Geometric.java

## Resources

* [https://docs.oracle.com/javase/tutorial/getStarted/application/index.html](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)
