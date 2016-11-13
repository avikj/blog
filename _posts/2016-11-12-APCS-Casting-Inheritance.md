---
layout: post
title: APCS Guide&#58; Casting and Inheritance Legality
categories: discovery
---

If you're taking APCS (especially at Monta Vista), it's likely that you have seen questions asking about the legality of different casting expressions. In my experience, understanding how to do these questions is one of the things APCS students struggle with most. I've spent multiple hours explaining how to determine which expresions are legal, so I figured I might as well write it all down (stuff on interfaces coming soon).

First, let's review some Java basics. A class defines a blueprint for an object, which can represent a real world item. In Java, when an object is assigned to a variable, that variable is a *reference* to the object. I usually visualize a reference as a label with an arrow pointing to a circle which represents an object. For example, the following line would create an object (circle) holding data about me, and the reference "me" points to that circle. Also note that the `=` (equals) sign denotes assigning a reference.

```java
Person me = new Student("Avik Jain", 15); // Student is a subclass of Person
```

With this example, let's also define some terms. 

- **Reference type** -  this is the type (class) of the reference. Whenever the reference is used, the Java compiler will know that it holds something of this type. For the reference `me`, the type is `Person`.
- **Value type** - this is the type of the actual object. It must be the same as or a subclass of the reference type. At any given point in the code, the Java compiler will not know the value type, only the reference type. This is why casting becomes important. For the object which `me` points to, the value type is `Student`, since it is defined as a new Student.

Questions about casting generally come in 2 forms: determining which statements compile, and which run without errors.
## "Which of the following statements would cause a syntax error?"

This means the same thing as "which of the following would not compile?".

The options given will usually look something like this:
```java
SomeClass someReference = (SomeOtherClass) someOtherReference;
```
When determining if something like this compiles, there are two questions you need to ask yourself.
1. Is the casting legal?
2. Is the assignment legal?
We will look at each of these.

### Is the casting legal?

At compile-time, references can be casted up or down. That means that the class in parentheses can be a superclass or a subclass of (or the same as) the type of the reference.
```java
Person p1 = new Person("Hugh Mungus", 42);
Student s1 = new Student("Avik Jain", 15);
// both of the following casts would be legal at compile-time
... = (Person) s1 // casting up from Student to Person
... = (Student) p1 // casting down from Person to Student (compiles but may give runtime error)
// the following would give an error at compile-time
... = (String) p1 // String is not a subclass or superclass of Person, so this fails
```
If you determine that the cast is legal, then the reference of the expression is successfully changed. For example, the compiler will think `((Student) p1)` is a `Student`, since casting changes its reference type.

*Note: while these rules apply at compile-time some of these statements may still cause run-time errors. Keep reading.*

### Is the assignment legal?

Assignment is essentially making a reference point at an object. The syntax is 
```java
myRef = otherRef; // this makes myRef point to the object which otherRef points to
```
Assignment is legal at compile time if the reference type to the right of the equal sign is a subclass or the same as the reference type on the left. For example, 
```java
Person p2 = new Student(...); // works since the reference type on the right is Student, which is a subclass of the reference type on the left (Person).
Person p3 = new Person(...); // works since the reference type on the right is Person, which is the same as the reference type on the left.

Student s2 = new Person(...); // does NOT work, since Person is not a subclass of Student
Person p4 = "HARAMBE"; // does NOT work, since String is not a subclass of Person
```

### Putting it together

To determine if a statement compiles, check if the casting is legal, and then check if the assignment is legal.
Remember that casting changes the reference type of the right side of an expression.

```
Student s1 = new Student(...);
Person p1 = new Person(...);

// Which of the following compile successfully? 
```
a. `Student newStud = (Person) s1;`

b. `Person newPer = (Person) s1;`

c. `Person newPer = s1;`

d. `Student newStud = (Student) s1;`

e. `Student newStud = p1;`

f. `Student newStud = (Student) p1;`

Scroll down for answers.

.

.

.

.

.

.

.

.

.

.

.

a. **NO, it does not compile**. While it is legal to cast `s1` to a `Person`, that changes the reference type of the right side to `Person`. Since `Person` is not a subclass of `Student`, the *assignment* is not legal.

b. **YES, it compiles**. Casting `s1` as a `Person` is legal, since `Person` is a superclass of `Student`. The reference type is then `Person` on both sides, so assignment is legal.

c. **YES, it compiles**. The reference type on the right is `Student`, and the reference type on the left is `Person`. Since `Student` is a subclass of `Person`, assignment is legal.

d. **YES, it compiles**. While casting `s1` to a `Student` doesn't change anything, it is legal. Since the reference type is `Student` on each side, assignment is legal.

e. **NO, it does not compile**. The reference type on the right is `Person`, while the reference type on the left is `Student`. `Person` is NOT a subclass of `Student`, so the assignment is not legal.

f. **YES, it compiles**. Since `Student` is a subclass of `Person`, casting `p1` to `Student` works at compile-time. Assignment works since it's `Student` on both sides.

**Hopefully that made sense, because we're not done yet.**

## "Which of the following statements would run without causing an error?"
Since you can't run Java code without compiling it, first check if the statement would successfully compile. Hopefully you haven't forgotten how to do that.

There's only really one case in which such a statement would compile but produce a run-time error.
### The ClassCastException
This error occurs when casting is legal at compile-time, but turns out to be impossible at run-time.
Ex:
```java
Object obj = new Object;
String str = (String) obj; // compiles since Object is a superclass of String
```
While this code compiles, at run-time the computer determines that the cast is not possible. But you're not a computer. Here's how you can determine whether casting causes an error.

*Casting causes an error when the **object** being casted is not an instance of the type which it is being casted to.*

This is where the difference between the value type and the reference type is important. Let's look at another example.
```java
Person p = new Student(...);
Student s = (Student) p; // THIS RUNS!
Person p2 = new Person(...);
Student s2 = (Student) p2; // THIS DOESN'T RUN!
```

The first one works because the **object** that `p` points to is actually a student. Even though the reference type of `p` is `Person`, the value type is `Student`. Therefore, casting `p` to a `Student` is legal, and it changes the reference type of the expression to `Student`.
The second compiles, but produces a `ClassCastException` when it runs. The value type of `p2` is `Person`, since it's instantiated as a new Person. Casting fails when the *value type* isn't the same as or a subclass of the type to which it is being casted.

## Practice

```java
Student s1 = new Student(...);
Person p1 = new Person(...);
```
For each of the following, determine if the statement compiles and runs, compiles but doesn't run, or doesn't compile.

a. `Student newStud = (Person) s1;`

b. `Person newPer = (Person) s1;`

c. `Person newPer = s1;`

d. `Student newStud = (Student) s1;`

e. `Student newStud = p1;`

f. `Student newStud = (Student) p1;`

Scroll down for answers. 
.

.

.

.

.

.

.

.

.

.

.

a. Doesn't compile.

b. Compiles and runs.

c. Compiles and runs.

d. Compiles and runs.

e. Doesn't compile.

f. Compiles but doesn't run.

## Conclusion

Hopefully this improved your understanding of casting and inheritance, or at least didn't make it worse. If you're still having trouble, try rereading a couple times (it does take some time to understand) and leave any questions or comments below. Also, I haven't mentioned interfaces at all in this post. Hopefully I'll get around to doing that in the near future.
