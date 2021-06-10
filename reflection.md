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

## What happened when a *class* is initialized?

**When the program uses a *class* that has not been loaded into the memory, the system will initialize the *class* with the following steps**

1. load
   - load the *class* file into the memory, and initialize a *java.lang.Class* object for it
   - this is done by *ClassLoader*

2. Link
   - compose the binary data of this new *class* into JVM
3. Initialize
   - initializtion of the new *class* is done by JVM
   - if the super class has not been initialize, then the super class will be initialized

```java
public class Test {
  	public static void main(String[] args) {
      	A a = new A();
      	System.out.println(A.i); // print 100
    }
  	
  	/*
  			1. load data to the memory, a java.lang.Class object is created
  			2. link, m = 0 (static int type has default value 0)
  			3. initialization, the static codes will be merged as follow:
  						<clinit>() {
                  System.out.println("static code block initialized");
                  m = 300;
                  m = 100;
  						}
  	*/
  	
  	class A {
      	static {
          	System.out.println("static code block initialized");
          	m = 3;
        }
      
      	static int m = 1;
      
      	public A() {
          	System.out.println("initialize A with no parameter constructor");
        }
    }
}
```

### Memory Analysis

- stack
  - *main()* method
- Heap
  - the corresponding *java.lang.Class* object for every *class* in methods
  - *class* object instances
    - each has a link to the *Class* binary data in methods
- methods (a special heap)
  - *class* binary data
    - static variables
    - static methods
    - *final* variables
  - every *class* data will have a link to a *java.lang.Class* object in heap

## When does *class* initialization happen?

- Initiative reference to *class* (will definitely happen)
  - when JVM starts, initialize the *class* in which *main()* is
  - when creating a new instance
  - access static variables or static methods of a *class* (except for *final* variables)
  - when we initialize a *class* whose super class has not been initialised, its super class will be first initialized
  - Reflection (e.g. Class.forName())
- passive reference to *class* (not happen)
  - Access a *class*'s static field
  - access *final* variables
  - define an array

```java
package learn.java.reflection;

public class Test {
  	public static void main(String[] args) throws ClassNotFoundException {
      	
      	// initiative reference
      	// class initialization will happen
      	Son son = new Son();
      
      	// reflection causes initiative reference
      	// class initialization will happen
      Class c = Class.forName("learn.java.reflection.Father");
    }
  
  	// access to static fields
  	// will not happen
  	System.out.println(Son.a);
  
  	// define an array
  	// will not happen
  	Son[] arr = new Son[5];
  
  	// access to final variable
  	// will not happen
  	System.out.println(Son.F);
}

class Father {
  	static int a = 1;
}

class Son {
  	static int b = 2;
  	static final F = 3;
}
```

# Class loader

- loads java classes during runtime dynamically to 
- transform the static data into data structure in methods
- create a new *java.lang.Class* instance that represents this *class*
- *classes* in class loader will be cached for a while
- JVM GC will collect these *Classes*

#### Custom class loader --> system/application class loader --> extension class loader --> bootstrap (root) class loader

## Get class loader

```java
// get system/application class loader
ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();

// get extension class loader
ClassLoader extClssLoader = systemClassLoader.getParent();

// get bootstrp class loader
// bootStrapClassLoader will be null
ClassLoader bootStrapClassLoader = extClssLoader.getParent();
```

## Delegation Model

on the request to find a class or resource,a *ClassLoader* instance will delegate tbe search of the class or resource to the parent class

1. user requests to load an application class into JVM
2. system class loader delegates the loading of this class to its parent class (extension class loader)
3. extension class loader delegates the loading of this class to its parent class (bootstrap class loader)
4. if bootstrap class loader fails to load this class, extension class loader will try to load this class
5. if extension class loader fails to load this class, system class loader will try to load this class

# Get *class* structure during runtime

We can get the followings during runtime using **reflection**:

- fields
- methods
- constructors
- superclasses
- interfaces
- Annotations

```java
package learn.java.reflection;

public class User {
  	private String name;
}
```

```java
package learn.java.reflection;

public class Test {
  	public static void main(String[] args) {
      	Class c1 = User.getClass();
      
      	// get class name
      	// package name + class name
      	System.out.println(c1.getName());		// "learn.java.reflection.User"
      	System.out.println(c1.getSimpleName());		// "User"
      
      	// get public fields
      	Fields f1 = c1.getFields();
      	for (Field f : f1) {
          	System.out.println(f);
        }
      
      	// get all fields
      	f1 = c1.getDeclaredFields();
      	for (Field f : f1) {
          	System.out.println(f);
        }
      
      	// get one public field
      	// given that we know there is a such field and its name
      	Field name = c1.getField("name");
      
      	// get one field
      	name = c1.getDeclaredField("name");		// "private learn.java.reflection.User.name"
      
      	// get public methods
      	// also gets its superclass's public methods i.e. Object
      	Method[] methods = c1.getMethods();
      	// get all methods
      	// no method from its superclass
      	Method[] declaredMethods = c1.getDeclaredMethods();
      	
    }
}
```

