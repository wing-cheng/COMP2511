# Class Design

- make data *private*
- make getter + setter methods for data
- always initialize data



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

