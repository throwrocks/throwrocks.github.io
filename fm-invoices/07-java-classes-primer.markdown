---
layout: page
comments: false
title: A primer on Java classes
permalink: fm-invoices-java-classes-primer.html
---


#### What is a class?

As FileMaker developers we don't have to deal with classes. We think in terms of entities and their attributes, and we model them using tables and fields. So before we start creating our first class, let's go over what are Java classes and how they work.

The simplest definition of a class is:

>A class is the blueprint from which individual objects are created.
>[Source](https://docs.oracle.com/javase/tutorial/java/concepts/class.html)

#### What is an object?

Now that we know that a class is a blueprint for creating objects, what exactly is an object? Here's a good explanation:

>An object stores its state in fields (variables in some programming languages) and exposes its behavior through methods (functions in some programming languages).
>[Source](https://docs.oracle.com/javase/tutorial/java/concepts/object.html)

#### Using a FileMaker table to understand classes

Let's look at our `sample_data` file in FileMaker. We have a table for customers. That table is like a blueprint to create customer records. If we were to translate our customers table to a Java class we could start writing it like this:

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
Notice the comment in the first line inside the curly braces. You should be familiar with that type of single line comment. It's the same format we use in FileMaker calculations.

Below the comment, we declared the class fields. Notice that before each field, we declare their type. Just like we define the type of our FileMaker fields, for example as Text or Number, we define our class fields as Java types like String or int.

#### So how do I create customer objects?

To create Customer objects your class needs a constructor.

#### What is a constructor?

>A class contains constructors that are invoked to create objects from the class blueprint.
>[Source](https://docs.oracle.com/javase/tutorial/java/javaOO/constructors.html)
>

It's very easy to create a Constructor. They look similar to FileMaker calculation functions, or custom functions.

```java
    public Customers() {
    }
```

The main difference is that you can provide an access modifier like `public` or `private` to specify its scope. By making it public, any class in your app can call it. Also, since we don't have a Calculation dialog box, we need to declare any functionality inside the curly braces.

Let's write at a constructor that does a bit more than just creating an object.

```java
    public Customers(int id, String firstName, String lastName, String company) {
        this.id = id;
        this.firstName = firstName;
        this.lastName = lastName;
        this.company = company;
    }
```
This constructor accepts four arguments. The customer id, first name, last name, and company. In the body, we are setting the object fields with the arguments. This means that we are both creating an object and setting the fields with data.

#### Class methods

Now that we can create customer objects, how do we get and edit the customer data of an existing customer object? We can create some basic methods commonly known as getters and setters.

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

Notice the names of these methods. They are very straighforward. Some of them set data, and others get data. The set methods accept an argument for the data that you want to set. And the get methods don't have an argument because they just return the data from a particular field.

#### Private fields

Now that we have methods to access and modify the customer data, we don't need to give direct access to our fields. After creating a customer object, we can set and get its data using the getter and setter methods. By making our fields private, we provide encapsulation. We take complete control of other classes can interact with our objects.

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

### Congratulations!

We'll come back to this customer class later because we will need it in our app. But first, let's work on writing a class that will help us connect our app with the FileMaker server so we can download some data.

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-the-api-class.html">Create the API class</a>


