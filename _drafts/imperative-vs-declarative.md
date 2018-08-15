---
layout: post
title:  "Imperative vs Declarative"
---

*This is part of a [blog post series]({% post_url 2018-08-14-functional-programming %}) I am writing about functional programming.*

**Imperative** and **Declarative** programming are two different styles we can use when writing code. I would use a simple example to show the differences: we will write a function that takes an array of numbers and returns the sum all of them:

{% highlight c# %}
[Fact]
public void Test()
{
    var numbers = new[] {1, 2, 3, 4, 5};
    Assert.Equal(15, Sum(numbers));
}
{% endhighlight %}

Imperative:
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
In the imperative style we will create a result variable initialized as 0 and then we will loop through the array. For each element in the array we will **mutate** the result variable appending the current element to it. At the end we return that result.

Declarative:
{% highlight c# %}
int Sum(int[] numbers)
{
    return numbers.Aggregate(0, (result, number) => result + number);
}
{% endhighlight %}
In the declarative style we take the array of numbers and *Aggregate* then using a simple sum. To achieve this we use the Aggregate function from [C# linq](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/getting-started-with-linq). The function takes an initial value, the seed, which is 0 in our case. 
I kept the same variable names as in the declarative style so we can compare then easily.

I find the declarative style **more expressive** than the imperative one. The declarative code tels *WHAT* my code is doing while the imperative tells me about the *HOW* is it doing it and then I have to infer the WHAT from there. I don't really care about the how, I just care about the what!
Another big drawback of imperative programming is that it forces you to relay a lot on mutation and loops, two things that we won't be able to use when working with pure functional languages.

When I started programming I wrote just write imperative programming: whiles, do-whiles, fors, foreachs every where. That's how I learned to program! Nowadays I try to write 0 loops, I really like using C# linq or all the function on top of array from ES6.

Before we leave, let's dig in into that Sum function...

Actually that seed is optional and we can omit it, because int's default value is 0 in C#.
{% highlight c# %}
int Sum(int[] numbers)
{
    return numbers.Aggregate((result, number) => result + number);
}
{% endhighlight %}

{% highlight c# %}
int Sum(int[] numbers)
{
    return numbers.Aggregate(Sum);
}

private static int Sum(int left, int right)
{
    return left + right;
}
{% endhighlight %}




TODO: 
- link from functional-programing to imperative-vs-declarative
- links from imperative-vs-declarative to mutate
- links from imperative-vs-declarative to no loops