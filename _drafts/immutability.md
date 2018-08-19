---
layout: post
---

*This is part of a [blog post series]({% post_url 2018-08-14-functional-programming %}) I am writing about functional programming.*

Why is mutability a terrible idea
===
Many programming languages allow mutable data structures by default.

{% highlight js %}
var number = 1;

// something happens

number = 5;

// more stuff happens

if (some_method()) {
    number = 3;
} else {
    number = 4;
}

console.log(number); // what's the value of number?
{% endhighlight %}

The problem with mutation is that is really hard to track down the value of a variable. If you allow mutation of data it means that that a variable could be reassigned to a new value anywhere in the code at execution time. The code is going to be harder to reason about and it's going to be harder to find bugs.

Now, mutation of a variable inside a small function could be not a big deal, but what about mutation of a value inside and object which gets shared between different objects? That could get really hard to track down.

Another problem of mutability is multi-threading. If you have a multi-threaded program how do you ensure that two threads are not accessing and changing the same data at the same data? You have to implement locks which are really hard to implement. 
What if we just made everything immutable?

Functional languages don't allow mutability
===

side effects

Loops don't make sense with immutability
===
If we don't allow immutability then how do we implement loops?

How do you implement a for loop? 
[For loop]

How do you implement a while loop? 
[while loop]

The simple answer is *you don't*. 

Declarative programming + recursion plays well with immutability
===
You use declarative programming or recursion instead.

Immutable objects and collections
===