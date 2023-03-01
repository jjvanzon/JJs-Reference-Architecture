---
title: "🗄️ SQL"
description: "Describing the role of SQL in JJ's Software Architecture. SQL is a language for data retrieval and manipulation and other actions executed onto a database."
keywords:
  - sql
  - structured query language
  - database
  - db
  - c#
  - .net
  - coding
  - programming
  - software engineering
  - software development
  - software design
  - software architecture
---

🗄️SQL
======

[back](.)

[`SQL`](https://learn.microsoft.com/en-us/training/paths/get-started-querying-with-transact-sql) is a language for data retrieval and manipulation and other actions executed onto a *database*.

<h2>Contents</h2>

- [Introduction](#introduction)
- [Using SqlExecutor](#using-sqlexecutor)
- [With NHibernate](#with-nhibernate)
- [SQL Files](#sql-files)
- [SQL Strings](#sql-strings)
- [String Concat](#string-concat)
- [Behind Repositories](#behind-repositories)
- [Database Upgrade Scripts](#database-upgrade-scripts)

Introduction
------------

Executing queries onto a database would normally be done through [`ORM`](orm.md), but if performance is an issue, it can be combined with raw [`SQL`](https://learn.microsoft.com/en-us/training/paths/get-started-querying-with-transact-sql).

Using SqlExecutor
-----------------

Other techniques, like *stored procedures* and *views* were dismissed at one point, in favor of putting the [`SQL`](https://learn.microsoft.com/en-us/training/paths/get-started-querying-with-transact-sql) files directly the [`.NET`](misc.md#dotnet) projects, under a sub-folder named `Sql`:

![](../images/sql-sub-folder.png)

The classic way of executing [`SQL`](#-sql) in [`.NET`](misc.md#dotnet) would be to use `System.Data.SqlClient`. But in this [architecture](../index.md) the [`SqlExecutor API`](#sql-executor) might be used.

With an `API` like that, we can execute [`SQL`](#-sql) command in a strongly-typed way, often with only a single line of code.

The first choice of doing it might be to make the [`SQL`](#-sql) files embedded resources:

![](../images/sql-as-embedded-resource.png)

This deploys the [`SQL`](#-sql) together with your `EXE` or `DLL`, because compiles the [`SQL`](#-sql) file right into the assembly.

The [`SQL`](#-sql) may look as follows:

```sql
update Ingredient set Name = @name where ID = @id;
```

Then you can put an enum in the [`Sql`](#-sql) folder in your `.NET` project:

![](../images/sql-enum.png)

Add `enum` members that correspond to the file names of the [`SQL`](#-sql) files:

```cs
namespace JJ.Demos.SqlExecutor.Sql
{
    internal enum SqlEnum
    {
        Ingredient_UpdateName
    }
}
```

Then an [`SqlExecutor`](#sql-executor) can be created as follows:

```cs
ISqlExecutor sqlExecutor = SqlExecutorFactory.CreateSqlExecutor(
    SqlSourceTypeEnum.EmbeddedResource, connection, transaction);
```

We passed the `SqlConnection` and `SqlTransaction` to it.

Then you can call a method that executes the [`SQL`](#-sql):

```cs
sqlExecutor.ExecuteNonQuery(SqlEnum.Ingredient_UpdateName, new { id, name });
```

Its method names are similar to an `SqlCommand`. [`SQL`](#-sql) parameters can be passed along as an anonymous type:

```cs
new { id, name }
```

The name and type of `id` and `name` correspond to the parameters of the [`SQL`](#-sql). You do not need to use an anonymous type. You can use any object. As long as its properties correspond to the [`SQL`](#-sql) parameters:

```cs
var ingredient = new IngredientDto
{
    ID = 10,
    Name = "My ingredient"
};

sqlExecutor.ExecuteNonQuery(SqlEnum.Ingredient_UpdateName, ingredient);
```

You can also retrieve records as a collection of strongly typed objects:

```cs
IList<IngredientDto> records = sqlExecutor.ExecuteReader<IngredientDto>(SqlEnum.Ingredient_GetAll).ToArray();

foreach (IngredientDto record in records)
{
    // ...
}
```

The column names in the [`SQL`](#-sql) are *case sensitive!*

It might be an idea to let the [`SQL`](#-sql) file names begin with the [entity](../patterns/data-access.md#entities) type name, so they stay grouped together:

![](../images/sql-file-names.png)


With NHibernate
---------------

If you use [`SqlExecutor`](#sql-executor) in combination with [`NHibernate`](orm.md#nhibernate) you might want to 
use the [`NHibernateSqlExecutorFactory`](misc.md#jj-framework-data-nhibernate) instead of the default [`SqlExecutorFactory`](misc.md#sql-executor):

```cs
ISession session = ...;

ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(
    SqlSourceTypeEnum.EmbeddedResource, session);
```

This version uses an `ISession`. In order for the [`SQL`](#-sql) to run in the same transaction as [`NHibernate`](orm.md#nhibernate), we made it aware of its `ISession`.

An implementation of [`NHibernateSqlExecutorFactory`](misc.md#jj-framework-data-nhibernate) can be found in [`JJ.Framework.Data.NHibernate`](misc.md#jj-framework-data-nhibernate).


SQL Files
---------

*(This feature might not be available in the [`JJ.Framework`](misc.md#jjframework).)*

It might be a good choice to include the [`SQL`](#-sql) as an embedded resource, but you can also use loose *files:*

![](../images/sql-as-content-file.png)

Here is code to create the [`SqlExecutor`](misc.md#sql-executor) and execute the [`SQL`](#-sql) file:

```cs
ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(
    SqlSourceTypeEnum.FileName, session);

sqlExecutor.ExecuteNonQuery(@"Sql\Ingredient_Update.sql", new { id, name });
```

So the `SqlEnum` cannot be used here. You'd use a (relative) file path.


SQL Strings
-----------

*(This feature might not be available in the [`JJ.Framework`](misc.md#jjframework).)*

It is not recommended to use [`SQL`](#-sql) strings in your code. But it is possible all the same using code like this:

```cs
ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(
    SqlSourceTypeEnum.String, session);

sqlExecutor.ExecuteNonQuery("update Ingredient set Name = @name where ID = @id", new { id, name });
```

In that case no [`SQL`](#-sql) files have to be included in your project.

But it might make it harder to track down all the [`SQL`](#-sql) of your project and optimize it. Using [`SQL`](#-sql) strings may also circumvent another layer of protection against [`SQL`](#-sql) injection attacks.


String Concat
-------------

*[`SQL`](#-sql) `string` concatenation* is sort of a no-no, because it removes a layer of protection against [`SQL`](#-sql) injection attacks. `SqlClient` has `SqlParameters` from [`.NET`](misc.md#dotnet) to prevent unwanted insertion of scripting. [`SqlExecutor`](misc.md#sql-executor) from [`JJ.Framework`](misc.md#jjframework) uses `SqlParameters` under the hood, to offer the same kind of protection. This *encodes* the parameters, so that they are recognized as simple types or string values rather than additional scripting.

Here is a trick to prevent the use of `string` concatenation: When you want to filter something conditionally, depending on a parameter being filled in or not, then the following expression might be used in the [`SQL`](#-sql) script's `where` clause:

```sql
(@value is null or Value = @value)
```

But there might be exceptional cases where [`SQL`](#-sql) string concatenation would be favorable. Reasons to do so might include:

- You have a (complicated) [`SQL`](#-sql) `select` statement and wish to take the `count` of it. String concatenation may prevent rewriting the [`SQL`](#-sql) statement twice, introducing a maintenance issue. Bugs would be awaiting as you'd have to change 2 [`SQL`](#-sql) scripts simultaneously, to make a change properly, which may easily be overlooked.
- Another case where `string` concatenation might be helpful, is an [`SQL`](#-sql) script where you wish to include a *database name* or *schema name*.
- There might be other examples where [`SQL`](#-sql) string concatenation might be used as an exception to the rule.

One variation of [`SqlExecutor`](misc.md#sql-executor) included the ability to add placeholders to the [`SQL`](#-sql) files to insert additional scripting for this purpose. *(This feature might not be available in the [`JJ.Framework`](misc.md#jjframework).)* 

Behind Repositories
-------------------

The [`repository`](../patterns/data-access.md#repository) pattern is used in this [architecture](../index.md).  
The [`repository`](../patterns/data-access.md#repository) pattern can be used together with [`JJ.Framework.Data`](misc.md#jj-framework-data).  

Using [`SQL`](#-sql) combined with [`repositories`](../patterns/data-access.md#repository) can be simplified with [`SqlExecutor`](#-sql).

Here is some pseudo-code to demonstrate how it is put together:

`FilterIDs.sql`

```sql
select ID from MyEntity
where CategoryID = @categoryID
and MinStartDate >= @minStartDate
```

[`C#:`](misc.md#csharp)

```cs
enum SqlEnum
{
    FilterIDs
}

class MySqlExecutor
{
    public var FilterIDs(int categoryID, DateTime minStartDate)
    {
        return SqlExecutor.ExecuteReader<int>(
            SqlEnum.FilterIDs, new { categoryID, minStartDate });
    }
}

class MyRepository : RepositoryBase
{
    public var Filter(int categoryID, DateTime minStartDate)
    {
        var ids = MySqlExecutor.FilterIDs(categoryID, minStartDate);

        var entities = ids.Select(x => Get(x));

        return entities;
    }
}

interface IMyRepository : IRepository
{
    var Filter(int categoryID, DateTime minStartDate);
}
```

This would result in:

- Keeping all the queries of an [entity](../patterns/data-access.md#entities) together in a [`repository`](../patterns/data-access.md#repository).
- Keeping overview of all the [`SQL`](#-sql) of all the [entities](../patterns/data-access.md#entities) behind an [`SqlExecutor`](misc.md#sql-executor).
- All that data access would be hidden behind [`repository interfaces`](../patterns/data-access.md#repository-interfaces) decoupling the persistence technology.
 
It may seem overhead all the layers, but it might add up after adding more queries for more [entities](../patterns/data-access.md#entities), that are either [`SQL`](#-sql) or [`ORM`](orm.md) queries. Of course you could skip layers, but this is how it is done in some of the `JJ` projects.

You might also find split up into separate assemblies: 

- `MyProject.Data`  
- `MyProject.Data.EntityFramework`
- `MyProject.Data.SqlClient`

Separating the general things from the technology-specific things.


Database Upgrade Scripts
------------------------

[`SQL`](#-sql) executed solely for database upgrading, might not be put in the main projects, but a project on the side. Suggestions of how to organize database upgrading might be found [here](../database-conventions.md#upgrade-scripts).

[back](.)