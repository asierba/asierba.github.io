---
layout: post
title:  "Imperative vs Declarative"
---

*This is part of a [blog post series]({% post_url 2018-08-14-functional-programming %}) I am writing about functional programming.*

**Imperative** and **Declarative** programming are two different styles we can use when writing code. Let's use an example to understand each style.

We will write a function that takes an array of numbers and returns the sum all of them:

{% highlight c# %}
[Fact]
public void Test()
{
    var numbers = new[] {1, 2, 3, 4, 5};
    Assert.Equal(15, Sum(numbers));
}
{% endhighlight %}

Imperative
===
{% highlight c# %}
int Sum(int[] numbers)
{
    var result = 0;
    foreach (var number in numbers)
    {
        result += number;
    }
    return result;
}
{% endhighlight %}
In the imperative style we create a result variable initialized as 0 and then loop through the array. For each element in the array we will **mutate** the result variable appending the current element to it. At the end we return that result.

Declarative
===
{% highlight c# %}
int Sum(int[] numbers)
{
    return numbers.Aggregate(0, (result, number) => result + number);
}
{% endhighlight %}
In the declarative style we take the array of numbers and *Aggregate* then using a simple sum. To achieve this we use the Aggregate function from [C# linq](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/getting-started-with-linq).
The function takes an initial value -the seed- which is 0 in our case. I kept the same variable names as in the declarative style so we can compare them easily.

By the way, Aggregate is usually called *reduce* in other languages, it's just microsoft trying to be different..


<sub>*DISCLAIMERS: The seed in Aggregate is optional, so it could be omitted in the example since the default of int in C# is 0. Also, there is already a [Sum function in Linq](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.sum?redirectedfrom=MSDN&view=netframework-4.7.2#overloads) which could simplify this code, but this is not goal of this blogpost, I just wanted to compare both styles.*</sub>


What vs How
===
The declarative style is **more expressive** than the imperative one. The declarative code tells *WHAT* my code is doing while the imperative tells me about *HOW* my code is achieveing something. When reading the imperative code I have to infer the WHAT from it. This is a problem. I don't really care about the how, I just care about the what!

Declarative code is functional
===
Another big drawback of imperative programming is that it forces you to relay a lot on mutation and loops, two things that we won't be able to use when working with pure functional languages.

First step towards functional
===
If you want to start embracing functional programming I would say that the first step will be to start using the declarative still as much as possible. Every modern language has this capability nowadays: C# has [Linq]((https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/getting-started-with-linq)), Java has the [Streams API](http://www.oracle.com/technetwork/articles/java/ma14-java-se-8-streams-2177646.html), Ruby has the [Enumerable module](https://ruby-doc.org/core-2.5.1/Enumerable.html), Javascript since ES6 [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) has has a good bunch of declarative methods, etc. 

You can force yourself to write declarative style code by simpling following the rule of **writing no loops**.  