---
title: "💽 Patterns : Data Access"
---

💽 Patterns : Data Access
==========================

[back](README.md)

<h2>Contents</h2>

- [Entities](#entities)
- [Mapping](#mapping)
- [DTO](#dto)
- [Repository](#repository)
- [Repository Interfaces](#repository-interfaces)


Entities
--------

`Entities` are the `classes` that represent the *functional domain model*.

<h3>Contents</h3>

- [Pure Data Objects](#pure-data-objects)
- [Enums](#enums)
- [Collections](#collections)
- [Virtual Members](#virtual-members)
- [Inheritance](#inheritance)
- [Real Code](#real-code)


<h3 id="pure-data-objects">Pure Data Objects</h3>

In this [architecture](..), we aim to keep the [`Entity`](#entities) `classes` [just data](#dto) and free of logic. The [`Entities`](#entities) in this [architecture](..) have properties of simple types and references or lists to other [`Entities`](#entities).

```cs
class Supplier
{
    int ID { get; set; }
    string Name { get; set; }
    Address Address { get; set; }
    IList<Product> Products { get; set; }
}
```


<h3 id="enums">Enums</h3>

You might even want to avoid `enums` in the [`Entity`](#entities) `classes` and put those in the [business layer](../layers.md#business-layer) instead. Often the database contain [`enum`-like `Entities`](../aspects.md#enum-like-entities), which you could as [`Entities`](#entities) in your model. This to keep it a purer representaton of the data model:

```cs
class Supplier
{
    Industry Industry { get; set; }
}

class Industry
{
    int ID { get; set; }
    int Name { get; set; }
}

enum IndustryEnum
{
    Retail = 1,
    Travel = 2
}
```


<h3 id="collections">Collections</h3>

Creating collections upon initialization is recommended for [`Entity`](#entities) `classes`. [`NHibernate`](../api.md#nhibernate) does not always create the collections for us. By creating a collection we can omit some `null` checks in the code:

```cs
class Supplier
{
    var Products { get; set; } = new List<Product>();
}
```


<h3 id="virtual-members">Virtual Members</h3>

For [`Entity`](#entities) `classes`, `public` members should be `virtual`, otherwise [persistence technologies](../aspects.md#persistence) may not work. This is because [`ORM's`](../api.md#orm) want to create [`Proxy classes`](../api.md#problem-entity--proxy-type-mismatch), that tend to override all the properties.

```cs
class Supplier
{
    virtual int ID { get; set; }
    virtual int Name { get; set; }
    ...
}
```


<h3 id="inheritance">Inheritance</h3>

Generally avoid [inheritance](../api.md#inheritance) within your [`Entity`](#entities) models, because it can make using data technologies harder.


<h3 id="real-code">Real Code</h3>

The previous code examples for [`Entities`](#entities) were just illustrative pseudo-code. This might be a more realistic example:

```cs
public class Supplier
{
    public virtual int ID { get; set; }
    public virtual string Name { get; set; }
    public virtual Address Address { get; set; }
    public virtual IList<Product> Products { get; set; } = new List<Product>();
}
```


Mapping
-------

`Mappings` are `classes` programmed for a particular [persistence technology](../aspects.md#persistence), e.g. [`NHibernate`](../api.md#nhibernate), that map the [`Entity`](#entities) model to how the `objects` are stored in the data store (e.g. an [`SQL Server`](../api.md#sql-server) database). A `Mapping` defines which `class` maps to which `table` and which `column` maps to which *property*.


DTO
---

`DTO` = *data transfer object*. `DTO's` only contain data, no logic. They are used to transfer data between different parts of a system. In certain situations, where passing an  [`Entity`](#entities) is not handy or efficient, a `DTO` might be a good alternative.

For instance: A specialized, optimized [`SQL`](../api.md#sql) query may return a result with a particular record structure. You could program a `DTO` that is a strongly typed version of these records. In many cases you want to query for [`Entity`](#entities) `objects` instead, but in some cases this is not fast / efficient enough and you might resort to a `DTO`.

`DTO's` can be used for other data transfers than [`SQL`](../api.md#sql) queries as well.


Repository
----------

A `Repository` is like a set of queries. `Repositories` return or save [`Entities`](#entities) in the data store. Simple types, not [`Entities`](#entities), are preferred for parameters. The `Repository` pattern is a way to put queries in a single place. The `Repositories'` job is also to provide an optimal set of queries.

Typically, every [`Entity type`](#entities) gets its own `Repository`.

It might be best to not expose types from the underlying [persistence technology](../aspects.md#persistence), so the `Repository` abstraction stays neutral.


Repository Interfaces
---------------------

Any [`Repository type`](#repository) will get an associated `Repository interface`. This keeps our system loosely coupled from the underlying [persistence technology](../aspects.md#persistence).

The `Repository interfaces` are also handy for [testing](../aspects.md#automated-testing), to create a [fake](other.md#mock) in-memory data store, instead of connecting to a real database. The `API` [`JJ.Framework.Data`](../api.md#jj-framework-data) can help abstract this data access, providing a base for these [`Repositories`](#repository) and `interfaces`.

[back](README.md)
