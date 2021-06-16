# Class Design

- make data *private*
  - or make data *public* and *final*
- make getter + setter methods for data
- always initialize data

## Attributes VS Classes

**Should it be an attribute or a class?**

- if a concept can not represented by a numbe or a string, most likely it is a class

## Principle#1: Law of Demeter (Principle of Least Knowledge)

**Talk Only to Your Friends**

A method of an object should only invoke methods of

- the object itself
- the objects passed at a parameter
- objects instantiated inside this method
- **not** objects returned by a method

```java
// ! incorrect
obj.get(name).get(thing).remove(something)
// correct
obj.remove(something)
```

## Principle#2: Liskov Substitution Principle

**Well-designed inheritance**

- a class should be substituted by its subclass

### What wrong with Square-is-rectangle relationship?

TODO add example

```java
public class Reactangle {
  	protected int width;
  	protected int length;
  	public int getArea() { return this.width * this.length; }
  	public Reactangle(int width, int length) {...}
  	public void setWidth(int width) {...}
  	public void setLength(int length) {...}
}

class Square extends Rectangle {
  	@Override
  	public int getArea() {return this}
}
```

#### The Correct Way

- use composition instead of inheritance

```java
public class Rectangle {...}
```

```java
public class Square {
  	private Rectangle rectangle;
}
```

## Rules of Method Overriding

- the argument list should be same as that of the overriden method
- the access level **cannot** be more restrictive than the overriden method
  - if the overriden method is *public*, then the overriding one cannot be *private* or *protected*
- *final* methods **cannot** be overriden
- constructor methods **cannot** be overriden
- the returned type of the overrding method should be the **same** or **sub-type** of  the overriden one

### Static Methods

- static method can be defined in subclass
  - the super method will be hidden by the drived class

## Refactoring

- No change in external behaviour
- Test-driven
- before u start refactoring, test cases should be ready

# Common Bad Codes Smells

- duplicated code
- long method
- Long parameter list
- large class
- divergent change
  - when a class is commonly changed in different ways for different reasons
  - this class must be doing too many things
- Shotgun surgery
  - when u have to make a lot of little changes to a lot of different classes

# Dependency Relationships

## Inheritance Relationship

"Is a" relationship

- *extends*

```java
public class Person {
  	public static void main(String[] args) {}
}

// we say there is an inheritance relationship between Person and Man
// a Man "is a" Person
class Man extends Person {
  	private static String gender;
  	public Man() {
      	this.gender = "male";
    }
}
```



# Association Relationships

- "Has a" relationship
- can be "one-to-one", "one-to-many" or "many-to-many"

## Aggregation Relationship

- if the parent does not exist anymore, the children will still exist
  - if Person disappears, the Bag will still exist
- most of the time we use aggregation for association relationship
- 0 or many
  - a Person can have or many Bags

```java
public class Person {
  	private Bag bag;
    public static void main(String[] args) {
      	Person Peter = new Person();
    }
  
  	public Person() {
      	this.bag = new Bag();
    }
}

// we say there is an association relationship between 
// Person and Bag
// a Person "has a" Bag
// a Person can "have" many Bags
class Bag {
    private List<String> things;
  	public Bag() {
      	this.things = new ArrayList<String>();
    }
}
```

## Composition Relationship

- the contained class is integral to the container class
- the contained class does not exist outside of the container
- if the parent does not exist anymore, the children will not exist either
  - if a Person dies, his/her memory will not exist anymore
- 1 or many
  - a Person has 1 or many memories

```java
public class Person {
  	private List<Memory> memory;
  	public Person() {
      	this.memory = new ArrayList<Memory>();
    }	
  
  	class Memory {
      	private String time;
      	private String description;
      	public Memory(String when, String what) {
          	this.time = when;
          	this.description = what;
        }
    }
}
```



# Visibility Modifiers

- *protected* - visible to packages and all subclasses
- *private* - visible to class only
- no modifier - visiable to the package



# Interfaces

- variables in an interface must be *static* and *final*
- a class **must** implement all of the abstract methods of the interface it implements

- a class can implement more than one interfaces
- an interface can extend more than one interfaces

```java
public interface Cook {
  	public void crackEgg();
  	public void fryRice();
  	default public void wash() {
      	System.out.println("Wash potatos!");
    }
}

public class Chef implements Cook {
  	// implemnetation of methods
  	public void crackEgg() {...}
  	public void fryRice() {...}
}
```



# Method Overriding (Polymorphism)

- *super* refers the superclass object
- *super()* refers to the superclass constructor

```java
public class Parent {
  	public void greet() {
      	System.out.println("Hello how are you?");
    }
}

// Child1 overrides the method 'greet' method in Parent
class Child1 extends Parent {
  	@Override
  	public void greet() {
      	System.out.println("Hi how have you been?");
    }
}

// we can also evoke superclass's old method in the overriding method
class Child2 extends Parent {
  	@Override
  	public void greet() {
      	super.greet();
      	System.out.println("Today's weather is good!");
    }
}
```



# Method Overloading

## Constructors

- if no constructor is defined in the class
  - a no-argument constructor is implicitly inserted
    - this no-parameter constructor will invoke superclass's no-argument constructor
  - if the superclass does not have a visible no-argument constructor, compilation error
- If the first statement in a constructor is not a call to *super()* or *this()*, a call to *super()*  is implicitly inserted.
- if there is a constructor with one or more arguments, no-argument constructor will not be inserted
- constructors can be overloading

```java
public class Test {
  	public int a;
  	public int b;
  	public int c;
  	public int d;
  	
  	// constructors overloading
  	public Test() {
      	// super() is implicitly inserted
    }
  	public Test(int a) {
      	this();
      	this.a = a;
    }
  	public Test(int a, int b) {
      	this(a);
      	this.b = b;
    }
  	public Test(int a, int b, int c) {
      	this(a, b);
      	this.c = c;
    }
  	public Test(int a, int b, int c, int d) {
      	this(a, b, c);
      	this.d = d;
    }
}
```



# Method Forwarding

- a class *A* can access all or some methods of another class *B* by including a *B* instance in *A*
  - no inheritance
  - use a "has-a" association relationship

```java
class Car {
  	public void fastMove {...}
}

/**
	Before having a car:
			a Person can "move"
  After having a car:
  		a Person can "fast move" like a Car
*/
public class Person {
  	private Car car;
  	public void move {...}
  	public static void main(String[] args) {
      	this.car = new Car();
      	this.car.fastMove();
    }
}
```



