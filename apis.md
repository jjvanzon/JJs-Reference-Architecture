API's | JJ's Reference Architecture
===================================

<h2>Contents</h2>

- [TODO](#todo)
- [AJAX](#ajax)
- [Configuration](#configuration)
  - [connectionStrings](#connectionstrings)
- [Embedded Resources](#embedded-resources)
- [Entity Framework 5](#entity-framework-5)
- [JavaScript / TypeScript](#javascript--typescript)
- [NHibernate](#nhibernate)
- [ORM](#orm)
- [SQL](#sql)
  - [With NHibernate](#with-nhibernate)
  - [Files instead of Embedded Resources](#files-instead-of-embedded-resources)
  - [Strings instead of Embedded Resources](#strings-instead-of-embedded-resources)
  - [TODO](#todo-1)
- [XML](#xml)
- [Keeping Bi-Directional Relationships in Sync](#keeping-bi-directional-relationships-in-sync)


TODO
----

`< TODO: API's: Make sure you mention all the important in-house API's and external API's in the documentation. >`

`< TODO: Mention JJ.Framework. >`

`< TODO: JJ.Framework documentation: Separate document for overviewing what's in JJ.Framework. Otherwise (lead) developers that use it would have to have a chrystal ball to know what's in there. >`


AJAX
----

We make our own wrapper ajax methods around ones from e.g. jQuery, so we can AJAX with a single code line and handle both partial loads and full reloads.

We choose full loads first, before resorting to AJAX. See 'First full load – then partial load – then native code'.


Configuration
-------------

For configuration we might use the [`JJ.Framework.Configuration`](https://www.nuget.org/packages/JJ.Framework.Configuration) API. That might be quite a lot easier than directly using .NET's `System.Configuration`.

You might read from the README how it works, by using the link above.

It does not seem to support reading out the `connectionStrings` section yet. So here is an idea how that might work. would it ever be programmed.

### connectionStrings

Reading out connectionStrings is similar to reading out the appSettings. Connection strings in the App.config or Web.config look as follows:

```xml
<connectionStrings>
  <add name="OrderDB" connectionString="data source=10.40.XX.XX;Initial Catalog=OrderDB..." />
</connectionStrings>
```

This is the classic way of reading it out:

string connectionString = ConfigurationManager.ConnectionStrings["OrderDB"].ConnectionString;

This is the alternative in Framework.Configuration:

```cs
string connectionString = ConnectionStrings<IConnectionStrings>.Get(x => x.OrderDB);
```

You need to define an interface to be able to use the strongly-typed name:

```cs
internal interface IConnectionStrings
{
    string OrderDB { get; }
}
```


Embedded Resources
------------------

`< TODO: Write text. >`


Entity Framework 5
------------------

`< TODO: Add story about enabling MSDTC and transactionality. '`


JavaScript / TypeScript
-----------------------

`< TODO: Describe Framework.Javascript and why you might avoid JavaScript and why you might not. Also mention TypeScript. >`


NHibernate
----------

`< TODO: Describe thgis problem with polymorphism: >`

API's, ORM: Arch: when an entity is a proxy or not a proxy, could reference comparison fail?

\> When you retrieved a polymorphic type from HNibernate using the base type it returns a Proxy of the base type instead of a proxy of the derived type, which makes reference comparisons between base proxies and derived class proxies fail. You can then unproxy both and it will return the underlying object, which is indeed of the derived class, upon which reference comparison succeeds. But if you can also get failing reference comparisons another way. If you unproxied a derived type, and retrieve another proxy of the derived type, reference comparison should also fail.

\>> This I want to test.

So always do ID comparisons, because reference comparisons can fail.

So for polymorphic entities always Unproxy before evaluating their type.>

`< TODO: describe which NHibernate methods to use and what features to avoid. Do not map binary and other serialized data fields using NHibernate, because it gives terrible performance. Use separate SQL statements for retrieving blobs. Also: always include bride entities for 1-to-n relationships, never let the two sides of the 1-to-n relationship refer to eachother directly. Always go through a bride entity. Always have surrogate keys in a bridge table, not just the composite key. Otherwise you will get problems with ORM mapping technologies crazy-complicated guarding of integrity of object graphs and passing around composite keys all the time, no-good hashing keys, ugly URL's, etc. >`

`< TODO: Describe more of the pitfalls and dos and don'ts around NHibernate and also FluentNHibernate. >`


ORM
---

Here is a ubiquitous quirk of ORM:

Many methods of IContext work with uncommitted / non-flushed entities: so things that are newly created, and not yet committed to the data store. But IContext.Query usually does the opposite: it only returns committed / flushed entities. This asymmetry is common in ORM's and doing it any other way would harm performance.

`< TODO: `

`- Reformulate the 'ubiquitous quirk of ORM': TryGet and Get MIGHT return uncommitted, non-flushed objects, but properties that lead to related entities usually return the uncommitted, non-flushed objects. IContext.Query or NHibernate's ISession.QueryOver only return flushed objects. It is still a valid point that one time you get the uncommitted objects and the other time you do not. Only the way it is formulated is not entirely accurate.`  
`- Also find a different word for 'ubiquitous'.`  
`- NHibernate FlushMode.Never or FlushMode.Commit does not prevent all intermediate automatic flushes. Flushes can be executed upon calling Save() even though the FlushMode.Commit's summary suggests otherwise. This happened when calling Save on a child object before calling it on the parent object. Internally then NHibernate asked itself the question if the child object was Transient and while doing so, it apparently wanted to get its identity, by executing an insert statement onto the data store. This caused a null exception on the 'ParentID' column of the child object.So the solution is either to create entities in a specific order: first the parent object, then the child object, or to choose a completely different identity generation scheme.`
`- ORM read-write order is relevant.`  
`- Mappings: do not solve n-to-n relationships with (NHibernate) mappings. Always use bridge entities. >`

`< TODO: A problem with ORM: meet-in-the-middle querties. You have two ends of a graph, you filter both ends and then want what is in the middle. >`


SQL
---

Executing queries onto a database is normally done through ORM, but if performance is an issue, it can be combined with SQL.

A choice was made, not to use stored procedures or views. Instead the SQL files were stored directly the .NET projects, under a sub-folder named `Sql`.

![](images/sql-sub-folder.png)

The classic way of executing SQL in .NET would be to use `System.Data.SqlClient`. But instead, the `SqlExecutor` API might be used.

A version of it is available on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.SqlClient/overview).

With an API like that, we can execute SQL in a strongly-typed way, often with only a single code line.

The first choice of doing it is to make the SQL file embedded resources:

![](images/sql-as-embedded-resource.png)

This makes the SQL be deployed together with your DLL or EXE, because compiles the SQL file right into the assembly.

The SQL may look as follows:

```sql
update Ingredient set Name = @name where ID = @id;
```

Then put an enum in the SQL folder in your .NET project:

![](images/sql-enum.png)

Add enum members that exactly correspond to the file names of the sql files:

```cs
namespace JJ.Demos.SqlExecutor.Sql
{
    internal enum SqlEnum
    {
      Ingredient_UpdateName
    }
}
```

Then an `SqlExecutor` can be created as follows:

```cs
ISqlExecutor sqlExecutor = SqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.EmbeddedResource, connection, transaction);
```

We passed the `SqlConnection` and `SqlTransaction` to it.

Then you can call a method that executes the SQL:

```cs
sqlExecutor.ExecuteNonQuery(SqlEnum.Ingredient_UpdateName, new { id, name });
```

The method names are similar to what you might be used to using `SqlCommand`. You pass SQL parameters along with the `SqlExecutor` as an anonymous type:

```cs
new { id, name }
```

The name and type of the variables `id` and `name` correspond to the parameters of the SQL. You do not need to use an anonymous type. You can use any object. As long as its properties correspond to the SQL parameters, they are applied correctly:

```cs
var ingredient = new IngredientDto
{
    ID = 10,
    Name = "My ingredient"
};

sqlExecutor.ExecuteNonQuery(SqlEnum.Ingredient_Update, ingredient);
```

You can also retrieve records as a collection of strongly typed objects:

```cs
IList<IngredientDto> records = sqlExecutor.ExecuteReader<IngredientDto>(SqlEnum.Ingredient_GetAll).ToArray();

foreach (IngredientDto record in records)
{
    // ...
}
```

The column names in the SQL are *case sensitive!*

It might be an idea to let the SQL file names begin with the entity type name, so they stay grouped together:

![](images/sql-file-names.png)

### With NHibernate

If you use `SqlExecutor` in combination with `NHibernate` you might want to 
use the `NHibernateSqlExecutorFactory` instead of the default `SqlExecutorFactory`:

```cs
ISession session = ...;

ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.EmbeddedResource, session);
```

This version uses an NHibernate `ISession`. In order for the SQL to run in the same transaction as the SQL that NHibernate executes, we make it aware of the `ISession` here.

A variation of this was implemented here: [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.NHibernate/overview).

### Files instead of Embedded Resources

*(This feature might not be available in the JJ.Framework.)*

It may usually a good choice to include the SQL as an embedded resource, but you can also use files or literal strings.

![](images/sql-as-content-file.png)

Here is code to create the `SqlExecutor` and execute an SQL file:

```cs
ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.FileName, session);

sqlExecutor.ExecuteNonQuery(@"Sql\Ingredient\_Update.sql", new { id, name });
```

So the `SqlEnum` cannot be used here. You'd use the (relative) file path here.

### Strings instead of Embedded Resources

*(This feature might not be available in the JJ.Framework.)*

It is not recommended to use SQL strings in your code. But it is possible all the same using the following:

```cs
ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.String, session);

sqlExecutor.ExecuteNonQuery("update Ingredient set Name = @name where ID = @id", new { id, name });
```

In that case no SQL files have to be included in your project.

But it might make it harder to track down all the SQL of your project, optimize it and using SQL strings also circumvents another layer of protection against SQL injection attacks.

### TODO

`< No SQL strings:`  
`    - Talk about not building up SQL strings in code.`  
`    - No parameter concatination`  
`    - Trick to prevent conditional blocks of sql. (@value is null or Value = @value)`  
`- No SQL in the code. Use SqlExecutor and .sql files.`  
`- No string concatination of sql parameters.`  
`- Hiding the SQL behind a repository.`  
`- Mention that SQL for upgrading the database structure do not belong in your project and are managed differently as described under Database Conventions.`  
`- Placeholders feature to concatinate SQL anyway, in exceptional cases. Not recommended, but sometimes it a better option.`


XML
---

Always choose `XElement` (LINQ to XML) over `XmlDocument` except when you have to use `XPath`.

Prefer the `XmlHelper` methods over using the API's directly, because the helper will handle nullability and unicity better.

`XmlToObjectConverter` and `ObjectToXmlConverter` are also acceptable XML API's.  

In `JJ.Framework.Xml` available on [NuGet](https://www.nuget.org/packages/JJ.Framework.Xml).  
Or `JJ.Framework.Xml.Linq` available on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Xml.Linq/overview).


Keeping Bi-Directional Relationships in Sync
--------------------------------------------

The classes `ManyToOneRelationship` and `OneToManyRelationship` do inverse property management more or less automatically, which you then use in your models (rich, entity, API or otherwise). More or less: You still have to program classes that derive from `ManyToOneRelationship` and `OneToManyRelationship` and use them a certain way, but the result would be in a navigation property and collection property whose ends will be kept in sync.

Package and code examples available on NuGet [here](https://www.nuget.org/packages/JJ.Framework.Business).