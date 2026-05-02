---
title: "Using Record Types in C#"
source: "https://codinghelmet.com/articles/using-record-types-in-cs"
author:
  - "[[Zoran Horvat]]"
published: 2022-05-13
created: 2026-05-02
description: "Record types were added to C# 9. In C# 10, we have got record structs as well, so now we can design a record which is either a reference or a value type. Either way, records are there, and in this article, you will learn how to use them effectively in your domain models."
tags:
  - "clippings"
---
## Defining a Record

Record types were added to C# 9. In C# 10, we have got record structs as well, so now we can design a record which is either a reference or a value type. Either way, records are there, and in this article, you will learn how to use them effectively in your domain models.

You can learn much more about the principle outlined in this article from my video course [Making Your C# Code More Object-oriented](https://codinghelmet.com/go/making-your-cs-code-more-object-oriented), published at Pluralsight.

```
public record Person(string Name, DateOnly BirthDate);

IEnumerable<Person> people = new Person[]
{
  new("Joe", new(1997, 4, 28)),
  new("Jane", new(1982, 5, 14)),
  new("Joe", new(1986, 7, 19)),
  new("Joe", new(1997, 4, 28)),
  new("Jane", new(1982, 5, 14)),
};
```

Declaring a record begins similar to what you would normally do with the class, only using the record keyword in this case. Then come the public properties: A string and the birth date in example above. Please note that this DateOnly type is new in.NET 6. We were waiting for the date only type for about 20 years or something.

That single line is the entire declaration of a record which exposes two public properties – Name and BirthDate. If you had anything more to add, you could add a body to the record the same way you do in a class declaration. Otherwise, just end the record declaration with a semicolon. That is what you cannot do with the class.

We have just completed the definition of an entire class, by only using the record keyword.

```
public record Person(string Name, DateOnly BirthDate);
```

Besides defining two public properties, this record is also implementing value-typed equality. We can instantiate the record like any other object, by calling its constructor, as you have seen in the code sample above. Compiler will generate a constructor accepting two parameters, a string and a DateOnly object, in that order.

If you printed the record object out, you would see that compiler has also generated a useful ToString() implementation.

```
Console.WriteLine(new Person("Joe", new(1997, 4, 28)));
// Person { Name = Joe, BirthDate = 4/28/1997 }
```

![](https://www.youtube.com/watch?v=3SitqWZkmD8)

## Using Records as Keys

Besides constructor, properties and ToString(), compiler is also adding value-typed equality to the record by supplying Equals() and GetHashCode() overrides. That means that you can use records as keys out of the box.

For instance, a record is a valid grouping key, so we can use records in the Distinct() LINQ operator without modification.

```
Console.WriteLine($"   Total {people.Count()}");
Console.WriteLine($"Distinct {people.Distinct().Count()}");
//    Total 5
// Distinct 3
```

The Distinct() operator is working just as one would expect, detecting a few repetitions in the sample sequence of Person record objects. The same code sample would not work correctly if Person were just a regular class. Classes implement reference-typed quality, preventing the Distinct() operator from finding objects with equal content.

You can use records as keys in indexed structures like dictionaries, hash sets and the like. Of course, the record can also be used as the key in the GroupBy() LINQ operator. For example, once we have detected duplication using the Distinct() operator, we can now isolate groups of equal record objects in the sequence.

```
var repeats = people
  .GroupBy(x => x)
  .Select(g => (g.Key, g.Count()));

foreach ((Person person, int count) in repeats)
{
  Console.WriteLine($"{count} x {person}");
}

// 2 x Person { Name = Joe, BirthDate = 4/28/1997 }
// 2 x Person { Name = Jane, BirthDate = 5/14/1982 }
// 1 x Person { Name = Joe, BirthDate = 7/19/1986 }
```

Here, you can witness that the GroupBy() operator has indeed collected together objects that have equal property values, rather than objects that are equal by reference.

But then, what is this record thing, and what is that record keyword? Let’s see what hides inside a record.

## Dissecting a Record

The record is just a class. There is nothing special in the record type, except that the compiler will take the record and populate it with members in case that you did not do it yourself. Below is the entire code behind the Person record, as the compiler would generate it for us.

```
public class Person : IEquatable<Person>
{
  public Person(string name, DateOnly birthDate)
  {
    this.Name = name;
    this.BirthDate = birthDate;
  }

  public void Deconstruct(
    out string name, out DateOnly birthDate)
  {
    name = this.Name;
    birthDate = this.BirthDate;
  }

  public string Name { get; init; }
  public DateOnly BirthDate { get; init; }

  public bool Equals(Person? other) =>
    other?.Name.Equals(this.Name) == true &&
    other?.BirthDate.Equals(this.BirthDate) == true;

  public override bool Equals(object? other) =>
    this.Equals(other as Person);

  public override int GetHashCode() =>
    HashCode.Combine(this.Name, this.BirthDate);

  public override string ToString() =>
    $"{nameof(Person)} {{ " +
    $"{nameof(Name)} = {Name}, " +
    $"{nameof(BirthDate)} = {BirthDate} }}";
}
```

The record with two parameters would expose two public read-only properties with init-only setters. Therefore, beyond construction, there will be no mutation on the object.

The class would expose a public constructor with those two parameters in the order in which they were listed within parentheses; a deconstructor, which allows you to assign a record object to a tuple with the same components as those in the record’s declaration.

Furthermore, there will be an implementation of the strongly typed IEquatable<T> interface. That means that the compiler will generate the strongly-typed Equals() method, then the weakly-typed Equals() override, and, finally the GetHashCode() override.

At the very end, there will be that nice ToString() overwrite, which is so useful in debugging.

All that code you see above, over twenty lines of code, fits into a single keyword: record. That is the principal reason why you should use records. Do not write those twenty lines of code over and over again, in every single value-typed class in your model. Use the record keyword instead.

What else can we do with record objects? There is one interesting feature they support. That is the non-destructive mutation.

![](https://www.youtube.com/watch?v=_F9jal5R0jw)

## Utilizing Non-destructive Mutation

We used to implement non-destructive mutation ourselves for a decade and more. That is the principle to construct a new object from an existing one by copying all values from the existing object, and only setting one or a few modified values to the new object.

Both objects would continue to exist side by side. The one that came later would represent a mutation of the prior one.

With the advent of records and init-only setters in C#, we can apply non-destructive mutation using the with keyword.

Here is the example. What if this person Joe had a twin brother, Jim? The only attribute they differ in is the name.

```
Person joe = new("Joe", new(1997, 4, 28));
Person jim = joe with { Name = "Jim" };
Console.WriteLine(joe);
Console.WriteLine(jim);

// Person { Name = Joe, BirthDate = 4/28/1997 }
// Person { Name = Jim, BirthDate = 4/28/1997 }
```

You can use the with expression to only change one property. That does not mean that the joe instance will change to create the jim instance. No, it will remain the same, because its type is immutable. A new instance will be created with all the properties copied, and only one of them, the Name property, changed.

The jim instance will be the same as joe, except the modified Name property. When we print both of them, you can see that the birth date has been copied, while the name was assigned anew.

You can list multiple properties in the with expression and change all of them at once. But everything else will still remain the same. The other properties will be copied from the first instance.

I will show how this becomes important when we decide to add a new property to the record’s definition.

## Maintaining Vertical Compatibility in Records

What if there were the third property in the record? The with expression will show useful, because you would not have to visit all the places where you mutated record objects – the with expression will copy the new property in all those places for you.

However, the problem with adding more properties to an existing record is with call to its constructor. By adding a new component to the record, you will break all direct calls to the constructor, which now expects the new component to be supplied as well.

```
public record Person(string Name, DateOnly BirthDate, string Nickname);

IEnumerable<Person> people = new Person[]
{
  new("Joe", new(1997, 4, 28)),  // Fails to compile
  new("Jane", new(1982, 5, 14)),
  new("Joe", new(1986, 7, 19)),
  new("Joe", new(1997, 4, 28)),
  new("Jane", new(1982, 5, 14)),
};
```

To help your future self, you may choose to avoid calling the constructor directly, by introducing a static factory function right at the beginning of the development. Instead of using the constructor, you would call this static factory function in all the places where you need the object.

```
public record Person(
  string Name, DateOnly BirthDate)
{
  public static Person Create(
    string name, DateOnly birthDate) =>
    new(name, birthDate);
}

IEnumerable<Person> people = new Person[]
{
  Person.Create("Joe", new(1997, 4, 28)),
  Person.Create ("Jane", new(1982, 5, 14)),
  Person.Create ("Joe", new(1986, 7, 19)),
  Person.Create ("Joe", new(1997, 4, 28)),
  Person.Create ("Jane", new(1982, 5, 14)),
};
```

The entire code will behave the same as when constructor was called, except that now you can transparently add a new property to the record. Compiler would complain in every place where constructor was called, but lucky for me, the factory function is the only place where I have called the constructor.

If we added the new property for the nickname to the record’s declaration, we could now introduce the third parameter to the static factory function and set it to a reasonable default.

```
public record Person(
  string Name, DateOnly BirthDate,
  string Nickname)
{
  public static Person Create(
    string name, DateOnly birthDate,
    string nickname = "") => // Default value
    new(name, birthDate, nickname);
}

IEnumerable<Person> people = new Person[]
{
  Person.Create("Joe", new(1997, 4, 28), "Pinky"),
  Person.Create ("Jane", new(1982, 5, 14)),
  Person.Create ("Joe", new(1986, 7, 19)),
  Person.Create ("Joe", new(1997, 4, 28)),
  Person.Create ("Jane", new(1982, 5, 14)),
};
```

Everything will keep working the same on the calling site, because none of the objects may know about the third parameter. But if Joe had a nickname, we could use the added parameter to set it during construction.

What is more important, the prior with expression will remain intact, but it will now pass the nickname along with other unmodified properties.

After recompiling the code, the with expression will grab the new property, Nickname, and copy it with no modification to the new object. The with expression is supporting vertical compatibility of records out of the box. Mind the additional property in the printout.

```
Person jim = joe with { Name = "Jim" };
Console.WriteLine(joe);
Console.WriteLine(jim);

// Person { Name = Joe, BirthDate = 4/28/1997, Nickname = Pinky }
// Person { Name = Jim, BirthDate = 4/28/1997, Nickname = Pinky }
```

![](https://www.youtube.com/watch?v=ESCXI65ixpk)

## Adding Custom Members

You have learned so far that record is just a regular class with a few members generated by the compiler. You can add anything to its body that you would add to a normal class. For example, you can define a pair of common public methods that are doing some age and dates calculation.

```
public record Person(
  string Name, DateOnly BirthDate,
  string Nickname)
{
  public static Person Create(
    string name, DateOnly birthDate,
    string nickname = "") => // Default value
    new(name, birthDate, nickname);

  public bool IsBeforeBirthday(int month, int day) =>
    month < this.BirthDate.Month ||
    (month == this.BirthDate.Month && day < this.BirthDate.Day);

  public int GetAge(DateOnly date) =>
    date.Year - this.BirthDate.Year +
    (this.IsBeforeBirthday(date.Month, date.Day) ? 0 : 1);
}
```

You are free to add any members you need to make the record operational and useful. Just add members like into any other class.

But keep in mind: Do not overdo that! If you need dependencies, services, let alone mutation, then the record is obviously not the right choice for you.

## Conclusion

In this article, you have learned how to define and use a record type.

When modeling any type where value-typed equality is a natural attribute, or when you are modeling Value Objects in DDD terms for example, record is just for you

Use the records! We were waiting for records for at least a decade and here they are. There is no excuse to avoid using them in production code.

If you liked what you have learned in this article, there is much more in my online course, [Making Your C# Code More Object-oriented](https://codinghelmet.com/go/making-your-cs-code-more-object-oriented), at Pluralsight.

---

### See Also:

- [Next: Understanding Covariance and Contravariance of Generic Types in C#](https://codinghelmet.com/articles/understanding-covariance-and-contravariance-of-generic-types-in-cs)
- [Previous: Unit Testing Case Study: Calculating Median](https://codinghelmet.com/articles/unit-testing-case-study-calculating-median)
- [Making Your C# Code More Object-oriented](https://codinghelmet.com/go/making-your-cs-code-more-object-oriented)

If you wish to learn more, please watch my latest video courses

[![](https://codinghelmet.com/img/beginning-oop-with-csharp.jpg)](https://codinghelmet.com/go/beginning-oop-with-csharp)

### [Beginning Object-oriented Programming with C#](https://codinghelmet.com/go/beginning-oop-with-csharp)

In this course, you will learn the basic principles of object-oriented programming, and then learn how to apply those principles to construct an operational and correct code using the C# programming language and.NET.

As the course progresses, you will learn such programming concepts as objects, method resolution, polymorphism, object composition, class inheritance, object substitution, etc., but also the basic principles of object-oriented design and even project management, such as abstraction, dependency injection, open-closed principle, tell don't ask principle, the principles of agile software development and many more.

[

More...

](https://codinghelmet.com/go/beginning-oop-with-csharp)

[![](https://codinghelmet.com/img/refactoring-to-patterns.jpg)](https://codinghelmet.com/go/design-patterns)

### [Design Patterns in C# Made Simple](https://codinghelmet.com/go/design-patterns)

In this course, you will learn how design patterns can be applied to make code better: flexible, short, readable.

You will learn how to decide when and which pattern to apply by formally analyzing the need to flex around specific axis.

[

More...

](https://codinghelmet.com/go/design-patterns)

[![](https://codinghelmet.com/img/design-patterns.png)](https://codinghelmet.com/go/refactoring-to-patterns)

### [Refactoring to Design Patterns](https://codinghelmet.com/go/refactoring-to-patterns)

This course begins with examination of a realistic application, which is poorly factored and doesn't incorporate design patterns. It is nearly impossible to maintain and develop this application further, due to its poor structure and design.

As demonstration after demonstration will unfold, we will refactor this entire application, fitting many design patterns into place almost without effort. By the end of the course, you will know how code refactoring and design patterns can operate together, and help each other create great design.

[

More...

](https://codinghelmet.com/go/refactoring-to-patterns)

[![](https://codinghelmet.com/img/PoEOOD.jpg)](https://codinghelmet.com/go/mastering-iterative-ood)

### [Mastering Iterative Object-oriented Development in C#](https://codinghelmet.com/go/mastering-iterative-ood)

In four and a half hours of this course, you will learn how to control design of classes, design of complex algorithms, and how to recognize and implement data structures.

After completing this course, you will know how to develop a large and complex domain model, which you will be able to maintain and extend further. And, not to forget, the model you develop in this way will be correct and free of bugs.

[

More...

](https://codinghelmet.com/go/mastering-iterative-ood)

#### About

![Zoran Horvat](https://codinghelmet.com/img/zh.jpg)

Zoran Horvat is the Principal Consultant at Coding Helmet, speaker and author of 100+ articles, and independent trainer on.NET technology stack. He can often be found speaking at conferences and user groups, promoting object-oriented and functional development style and clean coding practices and techniques that improve longevity of complex business applications.

#### Elsewhere

1. [Pluralsight](https://codinghelmet.com/go/pluralsight)
2. [Udemy](https://codinghelmet.com/go/udemy)
3. [Twitter](https://twitter.com/zoranh75)
4. [YouTube](https://www.youtube.com/c/zh-code)
5. [LinkedIn](https://www.linkedin.com/in/zoran-horvat/)
6. [GitHub](https://github.com/zoran-horvat)

#### Video Courses

1. [Making Your C# Code More Object-oriented](https://codinghelmet.com/go/making-your-cs-code-more-object-oriented)
2. [Beginning Object-oriented Programming with C#](https://codinghelmet.com/go/beginning-oop-with-csharp)
3. [Collections and Generics in C#](https://codinghelmet.com/go/collections-and-generics-in-cs)
4. [Design Patterns in C# Made Simple](https://codinghelmet.com/go/design-patterns)
5. [Refactoring to Design Patterns](https://codinghelmet.com/go/refactoring-to-patterns)
6. [Mastering Iterative Object-oriented Programming in C#](https://codinghelmet.com/go/mastering-iterative-ood)
7. [Making Your C# Code More Functional](https://codinghelmet.com/go/making-your-cs-code-more-functional)
8. [Making Your Java Code More Object-oriented](https://codinghelmet.com/go/more-object-oriented-java)
9. [Writing Purely Functional Code in C#](https://codinghelmet.com/go/writing-purely-functional-code-csharp-trailer)
10. [Tactical Design Patterns in.NET: Creating Objects](https://codinghelmet.com/go/tactical-design-patterns-in-net-creating-objects)
11. [Tactical Design Patterns in.NET: Control Flow](https://codinghelmet.com/go/tactical-design-patterns-in-net-control-flow)
12. [Tactical Design Patterns in.NET: Managing Responsibilities](https://codinghelmet.com/go/tactical-design-patterns-in-net-managing-responsibilities)
13. [Advanced Defensive Programming Techniques](https://codinghelmet.com/go/advanced-defensive-programming-techniques)
14. [Writing Highly Maintainable Unit Tests](https://codinghelmet.com/go/writing-highly-maintainable-unit-tests)
15. [Improving Testability Through Design](https://codinghelmet.com/go/improving-testability-through-design)

#### Articles

1. [Here is Why Calling a Virtual Function from the Constructor is a Bad Idea](https://codinghelmet.com/articles/here-is-why-calling-a-virtual-function-from-the-constructor-is-a-bad-idea)
2. [What Makes List<T> So Efficient in.NET?](https://codinghelmet.com/articles/what-makes-list-so-efficient-in-dotnet)
3. [Nondestructive Mutation and Records in C#](https://codinghelmet.com/articles/nondestructive-mutation-and-records-in-csharp)
4. [Understanding Covariance and Contravariance of Generic Types in C#](https://codinghelmet.com/articles/understanding-covariance-and-contravariance-of-generic-types-in-cs)
5. [Using Record Types in C#](https://codinghelmet.com/articles/using-record-types-in-cs)
6. [Unit Testing Case Study: Calculating Median](https://codinghelmet.com/articles/unit-testing-case-study-calculating-median)

#### Exercises

1. [Finding Maximum Value in Queue](https://codinghelmet.com/exercises/finding-maximum-value-in-queue)
2. [Counting Intersection of Two Unsorted Arrays](https://codinghelmet.com/exercises/counting-intersection-of-two-unsorted-arrays)
3. [Moving Zero Values to the End of the Array](https://codinghelmet.com/exercises/moving-zeros-to-end-of-array)
4. [Finding Minimum Weight Path Through Matrix](https://codinghelmet.com/exercises/minimum-weight-path-through-matrix)
5. [Rotating an Array](https://codinghelmet.com/exercises/rotating-array)
6. [Finding Numbers Which Appear Once in an Unsorted Array](https://codinghelmet.com/exercises/numbers-appear-once-in-unsorted-array)
7. [More...](https://codinghelmet.com/exercises)