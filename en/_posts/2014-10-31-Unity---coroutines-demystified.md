---
layout: en_post
title: Unity - coroutines demystified
---
Coroutines in Unity may seem to be a bit of a miracle, few know how they really work and what they look like from inside.

### Understanding IEnumerator

As you could guess, coroutines work with a bit of a help of `IEnumerator` interface and CLR enumeration implementation,
so we should first understand how these things work.
Initially, `IEnumerator` and `IEnumerable` are supposed to work with collections of items, like lists or arrays. Imagine a string,
which can also be an array of characters. A common way to iterate through the collection is to use a `foreach` statement:

{% highlight C# %}
foreach(char c in "coroutine")
  Debug.Log(c);
{% endhighlight %}

This neat statement works with an `IEnumerable` interface, which only knows how to provide an `IEnumerator`. 
The enumerator tells a little "story" of how to iterate through the current collection 
(it can provide the current element, and knows how to move to the next one).
Overall, it looks something like this:

{% highlight C# %}
class Enumerable // Typically implements IEnumerable or IEnumerable<T>
{
  public Enumerator GetEnumerator() {...}
}

class Enumerator // Typically implements IEnumerator or IEnumerator<T>
{
  public IteratorVariableType Current { get {...} }
  public bool MoveNext() {...}
}
{% endhighlight %}

With all that said, this is how the `foreach` statement lookes when it's compiled:

{% highlight C# %}
using (var enumerator = "coroutine".GetEnumerator())
  while (enumerator.MoveNext())
  {
  var element = enumerator.Current;
  Console.WriteLine (element);
  }
{% endhighlight %}
The `IEnumerator` also implements `IDisposable`, so it's perfectly legal to use it inside the `using` statement.

### Iterators

 This is the key topic. We're actually pretty close to the coroutines themselves.
In our example, the foreach statement is a __consumer__ of an enumeration, while iterator is a __producer__ of an enumeration. 
Take a look at the code:

{% highlight C# linenos %}
...
void Start()
{
  foreach(int fibNum in Fibs(6))
    Debug.Log(fibNum);
}

IEnumerable<int> Fibs(int fibCount)
{
   for(int i = 0, prevFib = 1, curFib = 1; i < fibCount; i++)
   {
		yield return prevFib;
	 
		int newFib = prevFib + curFib;
		prevFib = curFib;
		curFib = newFib;
   }
}
{% endhighlight %}

Take a look at line # 12. The `yield` statement, together with the `IEnumerable` return type, is what makes it all so special.
The `return` statement gives you a value that you asked the method to return while `yield return` gives you 
__the next element__ of the collection, which you asked this method to yield from the enumerator. 
Each time the yield is encountered, the control is returned to the caller, and the next time it returns to the calee, it picks up
and continues. The compiler actually transforms this method (`Fibs(...)`)
into the private classes which splices this `yield return` logic into the `MoveNext` method and `Current` property.
When you call this method, you're just __instantiating a compiler-written class__ and none of your code actually runs! Your code
will be run only when you start enumerating over this "on-the-fly" collection!

Iterators can have one or more `yield` statements and should return one of the following interfaces to bypass the compiler errors:

* `System.Collections.IEnumerable` 
* `System.Collections.Generic.IEnumerable<T>`
* `System.Collections.IEnumerator`
* `System.Collections.Generic.IEnumerator<T>`

There's also a `yield break` statement which allows to abort the iteration, so no more elements are returned from the enumerator.

### Back to Coroutines

Let's see how this enumerator logic translates to Unity coroutines. 
You may have noticed that coroutines usually `yield return` different time spans or `null` if the code needs to be continued on the next frame.
This is because Unity needs these return values to know when to call the `MoveNext()` method!
That's what you do when you write your coroutines. Take this code for example:

{% highlight C# linenos %}
void Start()
{
    WaitForSeconds delay = new WaitForSeconds(1f);
    StartCoroutine(TestCoroutine(delay));
}

IEnumerator TestCoroutine(WaitForSeconds delay)
{
    while (true)
    {
        Debug.Log("tick");
        yield return delay;
    }
}
{% endhighlight %}

As you can see, there's one `yield return` statement which returns a passed `WaitForSeconds` object, which is used for the delay.
The "tick" message will be written immediately in the console, and every second after that. Try moving the `Debug.Log(..)` call
after the `yield return` statement and see what happens.

Let's see what happens when the `TestCoroutine` method is called.

{% highlight C# linenos %}
void Start()
{
    WaitForSeconds delay = new WaitForSeconds(1f);
    TestCoroutine(delay);
}
{% endhighlight %}

Nothing seems to happen, but we're actually created an enumerator object which now lives in the heap.
You can also make something like this:

{% highlight C# linenos %}
void Start()
{
	IEnumerator rator = TestCoroutine(new WaitForSeconds(1f));
	for (int i = 0; i < 5; i++)
	{
       rator.MoveNext();
	}
}
{% endhighlight %}
You'll see that it will instantly log `"tick"` five times in a row.
See, there's no magic in coroutines at all. But they're actually pretty cool anyways.

### Kepp in mind

* Creating a coroutine is a process of creating an iterator through the imaginary collection of items.
* Iterator return values are used to determine the time of the next call of the `MoveNext` method.
* Coroutines allocate an enumerator object in the heap with each `StartCoroutine` call.

Hope this will help you to be more effective. You can also impress your colleagues with a custom coroutines implementation! 

##### A bit of info is taken from the great C# book (C# 5.0 in a Nutshell)