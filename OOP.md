# Class Design

- make data *private*
- make getter + setter method for data
- always initialize data

# Inheritance Relationship

- "Is a" relationship
- *extends*

```java
public class Person {
  	public static void main(String[] args) {}
}

// we say there is an inheritance relationship between
// Person and Man
// a Man "is a" Person
class Man extends Person {
  	private static String gender;
  	public Man() {
      	this.gender = "male";
    }
}
```

# Association Relationship

- "Has a" relationship
- a class object contains an object of another class

```java
public class Person {
    public static void main(String[] args) {}
	
  	// we say there is an association relationship between 
    // Person and Bag
  	// a Person "has a" Bag
    class Bag {
        private String[] things;
        public Bag(String[] things) {
            this.things = new String[things.length];
            this.things = things;
        }
    }
}
```

# Visibility Modifiers

- protected - visible to packages and all subclasses
- Private - visible to class only
- no modifier - visiable to the package

