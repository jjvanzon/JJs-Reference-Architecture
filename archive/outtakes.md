Outtakes
========

[back](..)

Archived text that didn't make the cut for some reason.

<h3>Contents</h3>

- [API's | ORM | Meet in the Middle Queries](#apis--orm--meet-in-the-middle-queries)
- [Namespaces, Assemblies & Folders | Scrambling Technical and Functional](#namespaces-assemblies--folders--scrambling-technical-and-functional)


API's | ORM | Meet in the Middle Queries
----------------------------------------

`< Archived this, because I have difficulty remembering the details about this problem. >`

`< TODO: A problem with ORM: meet-in-the-middle querties. You have two ends of a graph, you filter both ends and then want what is in the middle. >`

One time [`ORM`](../api.md#orm) almost fails a little bit, is queries where you link a bunch of [entities](#entity) together, you want a list of [entities](#entity) somewhere in the middle of that chain, you want to filter one end of the relationship chain and also on the other end.

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

[back](..)
