---
title: "What Makes List<T> So Efficient in .NET?"
source: "https://codinghelmet.com/articles/what-makes-list-so-efficient-in-dotnet"
author:
  - "[[Zoran Horvat]]"
published: 2022-09-07
created: 2026-05-02
description: "In this article, we will talk about the  List<T>  generic type – about what makes it so efficient, what makes it the most widely used collection in .NET. You will learn the working principle of  List<T>  and compare it to the working principle of the array, which is an exceptionally fast collection, but not so flexible like the  List<T>  is."
tags:
  - "clippings"
---
In this article, we will talk about the List<T> generic type – about what makes it so efficient, what makes it the most widely used collection in.NET. You will learn the working principle of List<T> and compare it to the working principle of the array, which is an exceptionally fast collection, but not so flexible like the List<T> is.

This article is related to video course which published at Pluralsight, titled Collections and Generics in C#. In that video course, you can learn the use of collections in realistic business applications, as well as watch an in-depth coverage of generics, especially on using and constructing custom covariant and contravariant types. The course ends in designing and implementing your custom collections when the existing collections fail.

So, in this article, we will cover the topic of adding objects to the list. We will analyze the performance of the List<T> class and compare it to the array, making conclusions in the end.

## Basic Operations on the List

Let us begin with code, then. What if we had some data - pretend that we don't know how many items there are. There is a sequence of items coming in, and we need to put the items into some collection for further analysis.

```
IEnumerable<int> data = ...

List<int> list = new();
foreach (int x in data) list.Add(x);
list.ForEach(x => Console.Write($"{x,3}"));
Console.WriteLine();
```

We are instantiating a list, and then traversing the data, adding every item to the list, before iterating through the list in a subsequent operation.

The question is how does this work and why does it work? We also know from experience that the list is doing this very efficiently, so the question is what magic lies behind the list that makes adding unknown number of items into it so efficient?

Of course, don't forget iterating through those data. Both operations are very efficient! In the following experiment, we will implement our own working prototype of a self-expanding collection, and then analyze its efficiency.

![](https://www.youtube.com/watch?v=4zHIzsaIPkc)

## Identifying the Shortcoming of a Common Array

To shed some light on the working principle behind the List<T> class, we might try to mimic it using just an array. Pretend that we have no list available, and we need to populate a collection with those same data.

```
int[] array = new int[16];
int count = 0;
foreach (int x in data)
{
  array[count++] = x;
}
```

We are initializing the array to some number of items. This random guess will turn out wrong, but we must start somewhere. We are also tracking how much of the array has been populated so far. Initially, the number of used elements in the array – the count variable – is zero. Then we iterate through the data, move the counter, increment it in every iteration, and set the element of the array to the incoming item.

The problem is that this implementation will fail, because it will step beyond the end of the array and cause IndexOutOfRangeException.

This is precisely the problem the List<T> is solving for us – automatically extending its underlying data structure to accept more items when its capacity is full. That is the case we must cover, and we will improve the code above in that direction.

## Implementing the Auto-expanding Array

Here is the case we must cover: When count is equal to the length of the array, then trying to add another item would cause an exception. One way to address this issue is just to double the array capacity.

We instantiate a new array with twice as many elements in it, then copy the existing content into that new array, leaving half of the new array unused, and then substitute the new array.

```
int[] array = new int[16];
int count = 0;
foreach (int x in data)
{
  if (count == array.Length)
  {
    int[] newArray = new int[array.Length * 2];
    Array.Copy(array, newArray, array.Length);
    array = newArray;
  }
  array[count++] = x;
}
```

There will be no IndexOutOfRangeException this time. When this code is run, the array with adaptive length will accept all the input elements. But now we must ask: Is this method of reallocating and copying content efficient? Wouldn't it be copying the same item many times over?

Think of the initial few items that were put into the first copy of the array. If we reallocated the array one time, two times, three times, and so on, wouldn't those initial elements be copied that many times? Yes, they will. Isn't that too much of copying? Well, before we jump to a conclusion, that is the question that requires calculation to come to a valid answer.

![](https://www.youtube.com/watch?v=B9DKv09IfT4)

## Analyzing Performance of the Self-expanding Array

If I told you that the code segment above is outlining the working principle of the List<T> class – first to store items inside a common array, and then to double its size every time when its capacity is full, then that already indicates that the cost of repeated copying is not prohibitive. That brings the next question up: How many memory writes must we make to populate a list, including all those reallocations and copying of its content?

The calculation that follows is the common step when designing a data structure. We calculate the cost of using the data structure and entail the proof of its performance.

Since we are doubling the size of the array at each step, we can assume that there are 2^n items after n expansions. We need 2^n writes just to put every item in, so we cannot escape having that many memory writes. But we had also reallocated the array and moved the existing content with every expansion that preceded.

What is the accumulated cost of all reallocations that brought us to the array of size 2^n? We had to copy one 1 to make the array of size 2, then copy 2 items to make the array of size 4 then copy 4 items, and so on.

In the end we copied 2^(n-1), after which the list has assumed its final capacity of 2^n. How many memory writes is that, then?

There is a nice little trick to derive the closed form of this sum.

![image1.png](https://codinghelmet.com/articles/what-makes-list-so-efficient-in-dotnet/img/image1.png)

The conclusion: Every location will be written two times on average.

The resizable list makes twice as many writes as the array of the same final size. That is the cost of using the List<T> versus the cost of using the plain array. On the other hand, you will get the benefit which the array cannot offer, and that is automatic resizing of the underlying collection so that you can put as many items as you like, at least until the amount of available memory is exceeded.

So, if anyone asks you what makes the list so efficient and so useful, this is the answer. it is reallocating the underlying data structure at the expense of writing every item in memory two times instead of only once.

## Summary

In this article, we have effectively reimplemented the underlying data structure of a common generic list in.NET. We have entailed its performance, concluding that, on average, every item in the list will be written two times. At times, an entire list’s content would be copied to the new array. That may cause intermittent delays when adding many items to the list using its Add() method. But on the average, list will perform exceptionally well, justifying its place as the most frequently used data collection in.NET.

---

### See Also:

- [Next: Here is Why Calling a Virtual Function from the Constructor is a Bad Idea](https://codinghelmet.com/articles/here-is-why-calling-a-virtual-function-from-the-constructor-is-a-bad-idea)
- [Previous: Nondestructive Mutation and Records in C#](https://codinghelmet.com/articles/nondestructive-mutation-and-records-in-csharp)
- [Collections and Generics in C# (Video Course)](https://codinghelmet.com/go/collections-and-generics-in-cs)
- [Beginning Object-oriented Programming with C# (Video Course)](https://codinghelmet.com/go/beginning-oop-with-csharp)

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