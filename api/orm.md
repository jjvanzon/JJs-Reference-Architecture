---
title: "📀 ORM"
description: "ORM makes it easier to focus on the logic around entity objects, while saving to a database goes automatically. This article lists issues you could encounter using ORM, and suggestions for how to deal with it."
image: "/images/orm-page.png"
keywords:
  - orm
  - object relational mapper
  - nhibernate
  - entity framework
  - c#
  - .net
  - coding
  - programming
  - software engineering
  - software development
  - software design
  - software architecture
---

📀 ORM
=======

[back](.)

An `ORM` aims to make it easier to focus on the logic around [entity](../patterns/data-access.md#entities) objects, while saving things to a database is pretty much done for you.

<h3>Contents</h3>

- [Introduction](#introduction)
- [Uncommitted Objects](#uncommitted-objects)
- [Flush](#flush)
- [Read-Write Order](#read-write-order)
- [Bridge Entities](#bridge-entities)
- [Binary Fields](#binary-fields)
- [Inheritance](#inheritance)
    - [Problem: Entity / Proxy Type Mismatch](#problem-entity--proxy-type-mismatch)
    - [Problem: Base Proxy / Derived Proxy Type Mismatch](#problem-base-proxy--derived-proxy-type-mismatch)
    - [Problem: 2 Proxies / 1 Entity](#problem-2-proxies--1-entity)
    - [Problem: Query Performance](#problem-query-performance)
    - [Alternative: Unproxy for Reference Comparison](#alternative-unproxy-for-reference-comparison)
    - [Alternative: Unproxy for Type Evaluation](#alternative-unproxy-for-type-evaluation)
    - [Alternative: ID Comparison](#alternative-id-comparison)
    - [Alternative: 1-to-1 Relationship](#alternative-1-to-1-relationship)
    - [Alternative: Interfaces](#alternative-interfaces)
    - [Alternative: No Inheritance](#alternative-no-inheritance)
- [Generic Interfaces](#generic-interfaces)
- [Entity Framework](#entity-framework)
- [NHibernate](#nhibernate)
- [Conclusion](#conclusion)


Introduction
------------

Here follow some issues you could encounter while using an `ORM`, and some suggestions for how to deal with it.

This information was gathered from experience, built up with [`NHibernate`](#nhibernate). It might be possible that other `ORM's` have similar issues, due to how `ORM's` work internally.


Uncommitted Objects
-------------------

Here is something that happens in [`ORM`](#orm) sometimes:

Some methods of data retrieval work with uncommitted / non-flushed [entities](../patterns/data-access.md#entities): so things that are newly created, and not yet committed to the data store. Other methods of data retrieval do the opposite: only returning committed / flushed [entities](../patterns/data-access.md#entities). This asymmetry might be common in [`ORM's`](#orm), since doing it another way might harm performance considerably:

| Method | Data Read |
|--------|----------|
| `IContext.Query` | committed
| `IContext.Get` | 1st committed, then uncommitted
| `IContext.TryGet` | 1st committed, then uncommitted
| Navigation properties /<br>following the object graph | 1st committed, then uncommitted

It appears to have to do with, when the [`ORM`](#orm) goes to the database to query for objects.


Flush
-----

`Flushing` in [`NHibernate`](#nhibernate) would mean that all the pending [`SQL`](misc.md#sql) statements are executed onto the database, without committing the transaction yet.

A `Flush` can help get an auto-generated `ID` from the database. Also, sometimes when [`NHibernate`](#nhibernate) is confused about the order in which to execute things, a `Flush` may help it execute things in the right order.

The trouble with `Flush` is, that it might be executed when things are not done yet, and incomplete data might go to the database, upon which database may give an error. So it is a thing to use sparsely only with a good reason, because you can expect some side-effects.

`Flushes` might also go off automatically. Sometimes [`NHibernate`](#nhibernate) wants to get a data-store generated ID. This can happen calling `Save` on an [entity](../patterns/data-access.md#entities). Unlike the documentation suggests, `FlushMode.Never` or `FlushMode.Commit` may not prevent these intermediate flushes.

Upon saving a parent object, child objects might be flushed too. Internally then [`NHibernate`](#nhibernate) asked itself the question if the child object was `Transient` and while doing so, it apparently wanted to get its identity, by executing an `insert` statement onto the data store. This once caused a `null` [`Exception`](../aspects.md#exceptions) on the child object's `ParentID` column.

It may also help to create [entities](../patterns/data-access.md#entities) in a specific order (e.g. parent object first, child objects second) or choose a identity generation scheme, that does not require flushing an [entity](../patterns/data-access.md#entities) pre-maturely.


Read-Write Order
----------------

It seems [`ORM's`](#orm) like it when you first read the data out, and then start writing to it. Not read, write some, read a little more, write some more. It may have to do with how it queries the database and handles [committed / uncommitted objects](#uncommitted-objects).


Bridge Entities
---------------

An *bridge* [entity](../patterns/data-access.md#entities) applies to `n => n` relationships and may require an additional table to make the link between the [entities](../patterns/data-access.md#entities):

<img src="../images/bridge-entity-table-with-composite-key.png" width="200"/>

Using an [`ORM`](#orm), the bridge [entity](../patterns/data-access.md#entities) might not be visible in the code, but can be managed as two collections inside the two main [entities](../patterns/data-access.md#entities):

```cs
class Question
{
    IList<Category> Categories { get; set; }
}

class Category
{
    IList<Question> Questions { get; set; }
}
```

The [`ORM`](#orm) can do quite a bit of magic under the hood, to keep these collections in sync. Perhaps a little too much for its own good. You might expect quite a few [`Exceptions`](../aspects.md#exceptions) to go off, while [`ORM`](#orm) tries to guard the integrity of the relationship.

These problems almost all go away, if you map a *bridge* [entity](../patterns/data-access.md#entities) instead. This turns the `n => n` relationship into two `1 => n` relationships which [`ORM`](#orm) can manage with less hardship. You can let both [entities](../patterns/data-access.md#entities) hold a list of *bridge* [entities](../patterns/data-access.md#entities) instead. In turn, the bridge [entity](../patterns/data-access.md#entities) would link back to the two main [entities](../patterns/data-access.md#entities):

```cs
class QuestionCategory
{
    Question Question { get; set;}
    Category Category { get; set;}
}

class Question
{
    IList<QuestionCategory> QuestionCategories { get; set; }
}

class Category
{
    IList<QuestionCategory> CategoryQuestions { get; set; }
}
```

This also has the advantage, that the [entity](../patterns/data-access.md#entities) model would not need to be refactored, if you'd want to add properties to a *combination* of things.

It might be advised, that the bridge table not rely on a *composite* key of the two `ID's`. A single *surrogate* `ID` might do better:

<img src="../images/bridge-entity-table-with-surrogate-key.png" width="200"/>

This is because it gives 1 handle to the combination of 2 thing. This gives [`ORM`](#orm) less difficulty managing things under the hood, prevents passing around composite keys, lower quality hash codes, URLs that don't look pretty, etc.


Binary Fields
-------------

You might not want to map *binary* and other *serialized data* fields using [`ORM`](#orm), because it can harm performance quite a bit.

Retrieving some loose fields of an [entity](../patterns/data-access.md#entities), would also retrieve a blob in that case. As well as saving a whole blob, when changing just a few fields. That data transmission can be quite a bottle-neck sometimes.

Using separate [`SQL`](misc.md#sql) statements for retrieving blobs might be a better alternative.

Inheritance
-----------

Particular surprises might emerge when using *inheritance* in your [entity](../patterns/data-access.md#entities) model at least while working with [`NHibernate`](#nhibernate). The main advice is to avoid inheritance at all in the [entity](../patterns/data-access.md#entities) models if you can.

### Problem: Entity / Proxy Type Mismatch

When retrieving an [entity](../patterns/data-access.md#entities) through [`ORM`](#orm), it will likely not return an instance of your [entity](../patterns/data-access.md#entities) type, but an instance of a type derived from your [entity](../patterns/data-access.md#entities), a so called `Proxy`. This `Proxy` adds to your [entity](../patterns/data-access.md#entities) a sort of connectedness to the database.

### Problem: Base Proxy / Derived Proxy Type Mismatch

When you retrieved an [entity](../patterns/data-access.md#entities) from `NHibernate` that has inheritance, using the base type it returns a `Proxy` of the base type instead of a `Proxy` of the derived type, which makes reference comparisons between base `Proxies` and derived class `Proxies` fail.

### Problem: 2 Proxies / 1 Entity

But you can also get failing reference comparisons another way. If you `Unproxied` a derived type, and retrieve another `Proxy` of the derived type, reference comparison might also fail.

### Problem: Query Performance

It can also harm performance of queries, getting a lot of `left joins`: one for each derived class' table.

### Alternative: Unproxy for Reference Comparison

You can then `Unproxy` both and it will return the underlying object, which is indeed of the derived class, upon which reference comparison succeeds.

### Alternative: Unproxy for Type Evaluation

To evaluate the *type*, you are better of `Unproxying` as well. Otherwise it will compare `Proxy` types instead of your [entity](../patterns/data-access.md#entities) type. This can be confusing.

### Alternative: ID Comparison

[ID comparison](../code-style.md#entity-equality-by-id) could avoid this problem that surrounds [entity](../patterns/data-access.md#entities) equality checks.

### Alternative: 1-to-1 Relationship

An alternative for inheritance might be, to use a `1-to-1` related object to represent the base of the [entity](../patterns/data-access.md#entities). Although, [`NHibernate`](#nhibernate) and other [`ORM's`](#orm) are  not a fan of `1 => 1` relationships either. What may save the day, is to map the relationship one-way only and not bidirectionally, so the [`ORM`](#orm) gets less confused.

### Alternative: Interfaces

Letting two [entity](../patterns/data-access.md#entities) types use a mutual `interface` might be an alternative too.

### Alternative: No Inheritance

By now maybe it may be clear, that the main advice is not to use inheritance in the first place in your [entity](../patterns/data-access.md#entities) models, if at all possible.


Generic Interfaces
------------------

Data access in this [architecture](../index.md) is favored behind generic interfaces from [`JJ.Framework.Data`](#jj-framework-data).


Entity Framework
----------------

[`Entity Framework`](https://www.nuget.org/packages/EntityFramework) is a framework for data access, a so called [`ORM`](#orm) (**O**bject **R**elational **M**apper). [`Entity Framework`](https://www.nuget.org/packages/EntityFramework) might be hidden behind abstractions using [`JJ.Framework.Data.EntityFramework`]( misc.md#jj-framework-data-entity-framework) and [repository interfaces](../patterns/data-access.md#repository-interfaces).

At one point we noticed a slow down in [`JJ.Framework.Data.EntityFramework`](misc.md#jj-framework-data-entity-framework). But it hadn't even been modified. Probably caused by an upgrade to a newer version of [`Entity Framework`](https://www.nuget.org/packages/EntityFramework). Unfortunately [`JJ.Framework.Data.EntityFramework`](misc.md#jj-framework-data-entity-framework) was not upgraded since then. The reason was most apps used [`NHibernate`](#nhibernate) instead.

When using [`Entity Framework`](https://www.nuget.org/packages/EntityFramework), transactions might not work unless you enable `MSDTC` (**M**icrosoft **D**istributed **T**ransaction **C**oordinator). That is a `Windows` service belonging to the [`SQL Server`](misc.md#sql-server) installation.


NHibernate
----------

[`NHibernate`](https://www.nuget.org/packages/NHibernate) is a technology used for data access. A so called [`ORM`](#orm) (**O**bject **R**elational **M**apper). It is comparable to [`Entity Framework`](#entity-framework).

[`NHibernate`](https://www.nuget.org/packages/NHibernate) is used in some projects, because an employer favored it, and other projects joined the club.

[`NHibernate`](https://www.nuget.org/packages/NHibernate) might be hidden behind abstractions using [`JJ.Framework.Data.NHibernate`](misc.md#jj-framework-data-nhibernate) and [repository interfaces](../patterns/data-access.md#repository).


Conclusion
----------

If all this makes you lose grip on reality and wonder whether [`ORM's`](#orm) are really worth it? Well, they can be. They allow you to program focusing on the meaning of things, rather than how to store it. Even though that is ambiguous because the story above suggests you'd still be better off knowing what it does and how it does it. You just don't need to do it yourself.

[back](.)