
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
