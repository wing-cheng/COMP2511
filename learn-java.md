# Class Design

- make data *private*
- make getter + setter method for data
- always initialize data

# Inheritance Relationship

- "Is a" relationship
- *extends*

# Association Relationship

- "Has a" relationship
- a class object contains an object of another class

# Visibility Modifiers

- protected - visible to packages and all subclasses
- Private - visible to class only
- no modifier - visiable to the package

# Reflection

- with reflection, we can get a class during runtime using *Reflection API* and manipulate its properties and method
- after a *class* is loaded, a *Class* object is created in heap memory
- every *class* has only one *Class* object, the entire structure of *class* will be encapsulated in the *Class* object

```java
Class class = Class.forName("java.lang.String");
```

### Normally how we get a *class*

Import package -> initialize an instance -> get the object instance

### How we get a *class* through reflection

initialize an instance -> *getClass()* -> get the complete package name

## Usage of Reflection

### During Runtime ...

- Determine the *class* of an object
- Create a new instance of a *class*
- Determine all variables and methods of a *class*
- get info about the generic type
- get access to a variable or methods of an object
- deal with annotation

## Advantages

- dynamically create objects and compile, flexible

## Disadvantages

- slow down the performance
- Always slower than direct operation of the same type

## Methods to get *Class*

### Class.forName()

- may throw *ClassNotFoundException*

```java
package learn.java.reflection;

public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
        Class c1 = Class.forName("learn.java.reflection.User");
        Class c2 = Class.forName("learn.java.reflection.User");
        Class c3 = Class.forName("learn.java.reflection.User");
    		
        // they will all print the same thing
        // as every class has only one Class object in heap memory
      	System.out.println(c1.hashCode());
        System.out.println(c2.hashCode());
      	System.out.println(c3.hashCode());
    }
}

class User {
    private String name;
    private int age;
    private int id;
  
    public User() {}
  
  	public User(String name, int age, int id {
        this.name = name;
        this.age = age;
        this.id = id;
    }
  
    public String getName() {
        return this.name
    }
}
```

### .getClass()

```java
public final Class getClass()
```

```java
public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
      	User a = new User();
        Class c1 = a.getClass();
      	System.out.println(c1);
}

class User {
    private String name;
    private int age;
    private int id;
  
    public User() {}
  
  	public User(String name, int age, int id {
        this.name = name;
        this.age = age;
        this.id = id;
    }
  
    public String getName() {
        return this.name
    }
}
```

### .class

- the most secure and reliable

```java
public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
      	User a = new User();
        Class c1 = a.class;
      	System.out.println(c1);
}

class User {
    private String name;
    private int age;
    private int id;
  
    public User() {}
  
  	public User(String name, int age, int id {
        this.name = name;
        this.age = age;
        this.id = id;
    }
  
    public String getName() {
        return this.name
    }
}
```

### .TYPE

```java
Class c1 = Integer.TYPE;
```

### .getSuperClass()

```java
public class Test {
  	public static void main(String[] args) {
      	Class c1 = Student.class;
      	// we can get Class of super class
      	Class c2 = c1.getSuperClass();
    }
}

class Person {
  	public Person() {}
}

class Student extends Person {
  	public Student() {
      	this.name = "student";
    }
}
```

## *Class* object of data types

- void
- annotation
- interface
- class
- String[]
- Enum
- ...

### Same *Class* when the same data type and the same dimension

```java
public class Test {
  	public static void main(String[] args) {
      	int[] a1 = new int[10];
      	int[] a2 = new int[100];
      	System.out.println(a1.getClass().hashCode());
      	System.out.println(a2.getClass().hashdCode());	// same hashCode
    }
}
```

## Class Loader

**When the program uses a *class* that has not been loaded into the memory, the system will initialize the *class* with the following steps**

1. load
   - load the *class* file into the memory, and initialize a *java.lang.Class* object for it
   - this is done by *ClassLoader*

2. Link
   - compose the binary data of this new *class* into JVM
3. Initialize
   - initializtion of the new *class* is done by JVM
   - if the super class has not been initialize, then the super class will be initialized

### Memory Analysis

- stack
  - *main()* method
- Heap
  - the corresponding *java.lang.Class* object for every *class* in methods
  - *class* object instances
- methods (a special heap)
  - *class* binary data
    - static variables
    - static methods
    - *final* variables
  - every *class* data will have a link to a *java.lang.Class* object in heap
