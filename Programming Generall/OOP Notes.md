#programming #oop #architecture #notes

A **default parameter's constructor** is <u>not equivalent</u> to the **default constructor**.

## Different Type of inheritance:
- <font color="#ffc000">Single Inheritance</font> is where a derived class inherits properties and behaviour from a single base class. Example: Class A → Class B.
- <font color="#ffc000"> Hierarchical Inheritance</font> is where more than one derived class is created from a **single base class**. Example: Class A → Class B → Class C.
- <font color="#ffc000">Multiple Inheritance</font> is for deriving a class from **multiple base classe**s. Here, the child objects programmers create will have combined aspects of characteristics and features from multiple parent classes. These objects do follow their hierarchies of base classes.
- <font color="#ffc000">Multilevel Inheritance</font> is where a child class is derived from **another derived class**. This feature carries combined aspects of multiple classes and follows their hierarchies.
- <font color="#ffc000">Hybrid Inheritance</font> is a heterogeneous feature of using <font color="#00b050">multiple inheritances</font>. Here a child class is derived from one or more combinations of single, hierarchical, and multilevel inheritances. This inheritance is adopted for programs to mix different types of inheritance; for example, when mixing a single inheritance with multiple inheritances or maybe a situation when multiple inheritances are mixed within a single program.
![[Pasted image 20230126131833.png]]
## why use <font color="#92d050">geeter</font> and <font color="#92d050">setters</font> ? 
- Getters and setters provide **encapsulation of behavior**.
- Getters and setters provide a **debugging point** for when a property changes at runtime.
- Getters and setters permit **different access levels**.

## In context of OOP, what is <font color="#00b0f0">association</font>?
Association is a relationship where all objects have their own life cycle and there is **no owner**.

## What are <font color="#00b0f0">CRC</font> Cards?
**Class Responsibility collaboration** cards are a brainstorming tool used in the *design of oop software*.

## What is the relationship between <font color="#ffff00">abstraction</font> and <font color="#ffc000">encapsulation</font>?
**Abstraction** is about *making relevant information visible,* while **encapsulation** enables a programmer to *implement the desired level of abstraction*.

## What is a <font color="#00b050">copy constructor</font>?
It is a unique constructor for creating a <u>new object as a copy of an object that already exists</u>. There will always be only one copy constructor that can be either defined by the user or the system.

## What is the function of a <font color="#95b3d7">finalizer</font> or <font color="#95b3d7">destructor</font>?
To relinquish **resources that are no longer needed**.

## What is the difference between <font color="#b2a2c7">early binding</font> and l<font color="#b2a2c7">ate binding</font>?
Early binding is when a variable is assigned its value at **compile time**. Late binding is when a variable is assigned a value at **run time**.


## <font color="#ffff00">Static and dynamic binding</font>
**Static binding uses Type information for binding while Dynamic binding uses Objects to resolve binding**. Overloaded methods are resolved (deciding which method to be called when there are multiple methods with same name) using static binding while overridden methods using dynamic binding, i.e, at run time.
![[Pasted image 20230126124020.png]]

## <font color="#00b050">Is-A relationship</font>:
**Whenever one class inherits another class**, it is called an IS-A relationship.
![[Pasted image 20230126123613.png]]

## <font color="#f79646">Overridding vs Overloading </font>
**Overriding** occurs when the<u> method signature is the same</u> in the superclass and the child class. **Overloading** occurs when two or more methods in the same class have the same name but <u>different parameters</u>.

## <font color="#4bacc6">virtual method</font>
A virtual method is **a declared class method that allows *overriding* by a method with the same derived class signature**.Abstract Method.

## <font color="#1f497d">composition</font>
Composition is **the design technique in object-oriented programming to implement has-a relationship between objects**.
an object is composed of severall object like a car is compose of severall components.


## <font color="#c0504d">friend</font> 
In some circumstances, <u>it's useful for a class to grant member-level access to functions that aren't members of the class</u>, or to all members in a separate class. These free functions and classes are known as _friends_.