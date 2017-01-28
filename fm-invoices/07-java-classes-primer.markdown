---
layout: page
comments: false
title: A primer on Java classes
permalink: fm-invoices-java-classes-primer.html
---


#### What is a class?

As FileMaker developers, we don't have to deal with classes. We think about entities and their attributes, and we model them using tables and fields. So before we start creating our first class, let's go over what Java are classes and how they work.

The simplest definition of a class is:

>A class is the blueprint from which individual objects are created.
>[Source](https://docs.oracle.com/javase/tutorial/java/concepts/class.html)

#### What is an object?

Now that we know that a class is a blueprint for creating objects, what exactly is an object? Here's a good explanation:

>An object stores its state in fields (variables in some programming languages) and exposes its behavior through methods (functions in some programming languages).
>[Source](https://docs.oracle.com/javase/tutorial/java/concepts/object.html)

#### Using a FileMaker table to understand classes

Let's look at our `sample_data` file in FileMaker. We have a table for customers. That table is like a blueprint to create customer records. To write a Java class to manage our customers, we start writing it like this:

```java
public class Customers {
    // The customer fields
    int id;
    String firstName;
    String lastName;
    String company;
    // TODO: Add all other fields here
}
```
We declared our `public` so classes outside our package can use it. Notice the comment in the first line inside the curly braces. It's the same format we use in FileMaker calculations. Below the comment, we declare the class fields. Notice that before each field, we declare their type. Just like we define the type of our FileMaker fields, for example as Text or Number, we define our class fields as Java types like String or int.

#### So how do I create customer objects?

To create Customer objects your class needs a constructor.

#### What is a constructor?

>A class contains constructors that are invoked to create objects from the class blueprint.
>[Source](https://docs.oracle.com/javase/tutorial/java/javaOO/constructors.html)
>

It's very easy to create a Constructor. They look similar to FileMaker calculation functions, or custom functions. But since we don't have a Calculation dialog box, we need to declare any functionality inside the curly braces.

```java
    public Customers() {
    }
```

This is a non-argument constructor that creates an empty customer object. The access modifier `public` allows any class in the app to call it. Let's write at a constructor that does a bit more than just creating an empty object.

```java
    public Customers(int id, String firstName, String lastName, String company) {
        this.id = id;
        this.firstName = firstName;
        this.lastName = lastName;
        this.company = company;
    }
```
This constructor accepts four arguments. The customer id, first name, last name, and company. In the body, we are setting the object fields with the arguments. This means that we are both creating an object and setting the fields with data, which would be equivalent to writing a script in FileMaker that creates a new record and then uses Set Field[] to set some data in the fields.

#### Class methods

Now that we can create customer objects, whether they are blank or already populated with some date, how do we get and modify the data of an existing customer object? We can do so through some basic methods commonly known as getters and setters.

```java
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getCompany() {
        return company;
    }

    public void setCompany(String company) {
        this.company = company;
    }
```

These methods are straightforward. Some of them set data, and others get data. The setter methods accept an argument for the data that you want to set. And the getter methods don't have an argument because they just return the data from a particular field. Since the setter methods don't return anything, the return type is `void`. And since the getter methods return data, we specify the data type we're returning. For example, `private int getId()` returns an int. 

#### Private fields

Now that we have methods to access and modify the customer data, we don't need to provide direct access to our fields. All the interaction with the fields happens through a getter or setter method. For this reason, we can make all our fields private. This is referred to as encapsulation.

```java
public class Customers {
    // The customer fields
    private int id;
    private String firstName;
    private String lastName;
    private String company;
    // TODO: Add all other fields here
}
```

#### Congratulations!

Now that you understand what a Java class is, let's now write a class to connect our app with the FileMaker server.

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-the-api-class.html">Create the API class</a>