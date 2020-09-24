# Table of contents

- [Object Oriented Programming](#object-oriented-programming)
  - [Object](#object)
  - [Class](#class)
  - [Principles](#principles)
    - [Encapsulation](#encapsulation)
    - [Abstraction](#abstraction)
    - [Inheritance](#inheritance)
    - [Polymorphism](#polymorphism)
- [OO Analysis and Design](#oo-analysis-and-design)

## Object Oriented Programming
Object-oriented programming (OOP) is a style of programming that focuses on using objects to design and build applications.
Contrary to procedure-oriented programming where programs are designed as blocks of statements to manipulate data,
OOP organizes the program to combine data and functionality and wrap it inside something called an “Object”.

### Object
 Objects represent a real-world entity and the basic building block of OOP. For example, an Online Shopping System will
 have objects such as shopping cart, customer, product item, etc.

### Class
Class is the prototype or blueprint of an object. It is a template definition of the attributes and methods of an object.
 For example, in the Online Shopping System, the Customer object will have attributes like shipping address, credit card, etc.,
 and methods for placing an order, canceling an order, etc.

### Principles

#### Encapsulation
Encapsulation is the mechanism of binding the data together and hiding it from the outside world.
Encapsulation is achieved when each object keeps its state private so that other objects don’t have direct access to its state.
Instead, they can access this state only through a set of public functions.
In Java, we achieve Encapsulation using access modifiers and providing public methods to access them.

#### Abstraction
Abstraction can be thought of as the natural extension of encapsulation. It means hiding all but the relevant data about
an object in order to reduce the complexity of the system.
In Java, we can achieve abstraction in two ways: abstract class and interface.

#### Inheritance
Inheritance is the mechanism of creating new classes from existing ones.

#### Polymorphism
Polymorphism is the ability of an object to take different forms and thus, depending upon the context,
to respond to the same message in different ways.
Take the example of a chess game; a chess piece can take many forms, like bishop, castle, or knight and all these pieces will respond differently to the ‘move’ message.
In Java, this is achieved using overloading and overriding.

## OO Analysis and Design
OO Analysis and Design is a structured method for analyzing and designing a system by applying object-oriented concepts.
The process of OO analysis and design can be described as:
1. Identifying the objects in a system
1. Defining relationships between objects
1. Establishing the interface of each object
1. Making a design, which can be converted to executables using OO languages.

We need a standard method/tool to document all this information; for this purpose we use UML.