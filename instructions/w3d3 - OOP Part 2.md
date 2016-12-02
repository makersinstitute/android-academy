![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 3 - Day 3

## <a name="lab1"></a>1. The Rectangle Class and Block Class

**1.** Create a class called `Rectangle` contains:
- Two private instance variables: `width` (of the type `double`) and `height` (of the type `double`), with default value of `1.0` and `1.0`.

- Two overloaded constructors - a default constructor with no argument and a constructor with two arguments for width and height.

- Two public methods:  `getArea()` and `getCircumferense()`, which return the area (of the type `double`) and circumference of the rectangle (of the type `double`). 

**2.** Add `getter` and `setter` method for width and height to `Rectangle` class.

**3.** Create `Block` class that inherit from `Rectangle` class.  

**4.** Add instance private variable `length` (of the type `double`) with default value of `1.0` to `Block` Class.

**5.** Add `getter` and `setter` method for `length` to `Block` class. 

**6.** Add two overload constructors - a default constructor with no argument and a constructor with tree arguments for width, height, and length. In countructor with three argument use keyword `super` for access a parent contructor (`Rectangle` class) for width and height. 

*Example*

```
public Block(double width,double height,double length){
    super(width,height);
    mLength = length;
}
```

**7.** Add method `getVolume()` which return volume of block. 

**8.** Add Overidding method : `getArea()` which return the area of block(of type the `double`).

**9.** Create object from `Rectangle` class and object from `Block` class. Print area of rectangle and area of block.  

---

## <a name="lab2"></a>2. Animal class, Sheep class , Dog class, and Cat class

**1.** Create a abstract class called `Animal` contains:
- Two private instance variables: `weight` (of the type `double`) and `tall` (of the type `double`), with default value of `1.0` and `1.0`.
- Two overloaded constructors - a default constructor with no argument and a constructors with two arguments for weight and tall
- One public methods: `showWeightAndTall()` with no argument for show a tall and weight
*Expected output from `showWeightAndTall()` method*
```
The Height of Animal is 1.0 and The Tall of ANimal is 1.4 m
```
- One abstract method : `sound()` with no argument to print sound of animal.

**2.** Add `getter` and `setter` method for height and tall to `Animal` class.

**3.** Create `Sheep` class, `Dog` class, `Cat` class that inherit from `Animal` class.  

**4.** Declare a `sound()` for each class but with different output. 

*Expected output from method sound in Dog class*
```
Gog gog
```

*Example output from method sound in Sheep class* 
```
Embe embe....
```
---

## <a name="lab3"></a>3. Person class, Teacher class, Student class, and  Activity interface

This time you're given `Person` class and `Activity` interface. You're task is to create `Teacher` class and `Student` class inherited from `Person` class and implements `Activity` interface. 

Here is the `Person` class.
```
public class Person {

    private String mName;
    private int mAge;

    public Person(String name, int age) {
        mName = name;
        mAge = age;
    }

    public String getName() {
        return mName;
    }

    public void setName(String name) {
        mName = name;
    }

    public int getAge() {
        return mAge;
    }

    public void setAge(int age) {
        mAge = age;
    }
}
```

Here is `Activity` interface  
```
public interface Activity  {
    public abstract void myActivity();
}
```

For `Teacher` class, you have to add *private* field `subject` type **string**, and add *getter* and *setter* method for `subject`. Declare `myActivity()` method from *Activity* interface that print like this **"`name` is teaching `subject` right now."**. `name` and `subject` are fields from this class. 

And for `Student` class, you have to add *private* field `subject` and *private* field `score`. `subject` is type **string** and `score` is type **integer**. Declare `myActivity()` method from *Activity* intereface that print like this **"My name is `name`. I have `subject` homework and get a score of `score`. `name` ,`score` and `subject` are fields from this class

---
## <a name="lab4"></a>4. Random class
First go to [here](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html). On that website there is `Random` Class. Your task is to create an object from `Random` class and use it for random number.
