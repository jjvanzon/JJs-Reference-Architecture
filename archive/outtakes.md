Outtakes
========

[back](..)

Archived text that didn't make the cut for some reason.

<h3>Contents</h3>

- [API's | ORM | Meet in the Middle Queries](#apis--orm--meet-in-the-middle-queries)
- [Namespaces, Assemblies & Folders | Scrambling Technical and Functional](#namespaces-assemblies--folders--scrambling-technical-and-functional)
- [Patterns: Cascading](#patterns-cascading)
- [Patterns: ViewModels](#patterns-viewmodels)


API's | ORM | Meet in the Middle Queries
----------------------------------------

`< Archived this, because I have difficulty remembering the details about this problem. >`

`< TODO: A problem with ORM: meet-in-the-middle querties. You have two ends of a graph, you filter both ends and then want what is in the middle. >`

One time [`ORM`](../api.md#orm) almost fails a little bit, is queries where you link a bunch of [entities](patterns-data-access.md#entities) together, you want a list of [entities](patterns-data-access.md#entities) somewhere in the middle of that chain, you want to filter one end of the relationship chain and also on the other end.

In [`C#`](api.md#csharp) it seems you can filter in one direction only:

```cs
FirstList.Where(x => ...).SecondList.Where(x => ...).LastList.Where(x => ...);
```

If you want to filter the `SecondList` by stuff in the `FirstList` and in the `LastList`, it seems a [`LINQ`](api.md#linq) query won't do, while in [`SQL`](api.md#sql) it would seem so trivial. The solution in [`C#`](api.md#csharp) might be to materialize the smaller selection, which would retrieve the database rows filtered up until then. And then go filter it further down in memory.

```cs
var list = FirstList.Where(x => ...).SecondList.Where(x => ...).ToArray();

list = list.Where(x => x.SecondList.Any(x => ...).ToArray();
```

`< TODO: What was the problem again? When rows materialize? Retrieving quite a few more entities than strictly needed? It seems so specific to the use case back then. >`

Namespaces, Assemblies & Folders | Scrambling Technical and Functional
----------------------------------------------------------------------

... 
This 'scrambling' of technical and functional concerns, might be rooted in our trying to project something 2-dimensional (functional vs. technical) onto something sequential (written text).


Patterns: Cascading
--------------------

*2023-01-23*

This succession of calls might be done in a [`Facade`](#facade), whose job is to combine multiple aspects involved in an operation.

Patterns: ViewModels
---------------------

*2023-01-31*

Considerations about Inheritance:

The reason inheritance is not a go-to choice for [`ViewModels`](#viewmodels) is because that might create an unwanted n² dependency between [`Views`](#views) and the `base` [`ViewModel`](#viewmodels): *`n`* [`Views`](#views) could be dependent on `1` [`ViewModel`](#viewmodels) and *`m`* [`ViewModels`](#viewmodels) could be dependent on 1 `base` [`ViewModel`](#viewmodels), making *`n * m`* [`Views`](#views) dependent on the same `base` [`ViewModel`](#viewmodels). This means that if the `base` [`ViewModel`](#viewmodels) changes, *`n * m`* [`Views`](#views) could break, instead of just *`n`*. *`m`* is even likely to become greater than *`n`*. If multiple layers of inheritance are used, it gets even worse. That can get out of hand quickly and create a badly maintainable application. By using no inheritance, a [`ViewModel`](#viewmodels) could only break `n` [`Views`](#views) (the number of [`Views`](#views) that use that [`ViewModel`](#viewmodels)).

[back](..)
