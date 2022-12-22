API's | JJ's Reference Architecture
===================================

This article describes some of the API choices in this architecture.

<h2>Contents</h2>

- [AJAX](#ajax)
- [Configuration](#configuration)
  - [ConnectionStrings](#connectionstrings)
- [Embedded Resources](#embedded-resources)
- [Entity Framework](#entity-framework)
- [JavaScript / TypeScript](#javascript--typescript)
- [JJ.Framework](#jjframework)
- [Keeping Bi-Directional Relationships in Sync](#keeping-bi-directional-relationships-in-sync)
- [NHibernate](#nhibernate)
- [ORM](#orm)
  - [Generic Interfaces](#generic-interfaces)
  - [Committed / Uncommitted Objects](#committed--uncommitted-objects)
  - [Flush](#flush)
  - [Read-Write Order](#read-write-order)
  - [Bridge Entities](#bridge-entities)
  - [Binary Fields](#binary-fields)
  - [Inheritance](#inheritance)
  - [Conclusion](#conclusion)
- [SQL](#sql)
  - [With NHibernate](#with-nhibernate)
  - [Files instead of Embedded Resources](#files-instead-of-embedded-resources)
  - [Strings instead of Embedded Resources](#strings-instead-of-embedded-resources)
  - [TODO](#todo)
- [XML](#xml)
- [TODO](#todo-1)


AJAX
----

For AJAX'ing partial web content, our team made our own wrapper AJAX methods, around calls to jQuery, so we could AJAX with a single code line and handle both partial loads and full reloads the same way. Saved quite a few lines of JavaScript code.

Our strategy was to prefer full loads, so we could keep most logic in the C# realm. This before resorting to AJAX calls. See [First Full Load – Then Partial Load – Then Native Code](patterns.md#first-full-load--then-partial-load--then-native-code).


Configuration
-------------

For configuration we might use the [`JJ.Framework.Configuration`](https://www.nuget.org/packages/JJ.Framework.Configuration) API, which might be quite a bit easier than using .NET's `System.Configuration` directly.

You might read from its `README` how it works, by following the link above.

It does not seem to support reading out the `connectionStrings` section yet. So here is an idea how that might work. would it ever be programmed.

### ConnectionStrings

Reading out `connectionStrings` might be made similar to reading out the `appSettings`. Connection strings in the `App.config` or `Web.config` may look as follows:

```xml
<connectionStrings>
  <add name="OrderDB" connectionString="data source=192.168.XX.XX;Initial Catalog=OrderDB..." />
</connectionStrings>
```

This would be the *classic* way of reading it out:

```cs
string connectionString = ConfigurationManager.ConnectionStrings["OrderDB"].ConnectionString;
```

This could the alternative in `JJ.Framework.Configuration`:

```cs
string connectionString = ConnectionStrings<IConnectionStrings>.Get(x => x.OrderDB);
```

You may then define an interface to be able to use the strongly-typed name:

```cs
internal interface IConnectionStrings
{
    string OrderDB { get; }
}
```

Embedded Resources
------------------

Embedded resources might be handy to prevent having to include loose files with a deployment, but instead compiling the loose files right into the assembly DLL or EXE. It also protects those resources a little bit better against modifications.

To include a file as an embedded resource, you could set the following property:

![](images/sql-as-embedded-resource.png)

[`JJ.Framework.Common`](https://www.nuget.org/packages/JJ.Framework.Common) contains a helper class `EmbeddedResourceReader` which make it a little bit easier to access those resources in your code:

```cs
string text = EmbeddedResourceReader.GetText(
  assembly, "Ingredient_UpdateName.sql");
```


Entity Framework
----------------

Entity Framework is a framework for data access. It might be hidden behind abstractions using [`JJ.Framework.Data.EntityFramework`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.EntityFramework/overview) and [repository interfaces](patterns.md#repository-interfaces).

`JJ.Framework.Data.EntityFramework` at one point seemed to become quite slow, without modifying it. It was not upgraded since then, because most of the apps used `NHibernate` instead.

It may be required to enable `MSDTC`. That would be a service belonging to an `SQL Server` installation that might have to be enabled. Otherwise transactions might not work.


JavaScript / TypeScript
-----------------------

`JavaScript` was less preferred as an architectural choice. `JavaScript's` weak type system played a role. The strange behavior and trickiness in `JavaScript` (part due to this weak typing) gave it less appeal.

The idea behind `MVC` was logic on the server-side. Views were in `Razor`. Best to keep most logic `C#` was the idea.

`JavaScript` would easily get bloated, getting out of hand from a maintainability perspective, was the opinion. In `C#` you could refactor, upon which lots of the `JavaScript` might break unexpectedly, with an error message tucked away in some console window, instead of right in your face.

`TypeScript` may have saved the day to cover for the weak typing from `JavaScript`. But it wasn't tried yet.

But still: logic in one place in one language (`C#`) felt so nice. I guess the love for `C#` is strong.

The idea was that a full page load was 1st choice, 2nd choice `AJAX'ing`, and last in line `JavaScript` only to support the user interaction. No business logic. See also: [First Full Load – Then Partial Load – Then Native Code](patterns.md#first-full-load--then-partial-load--then-native-code).

For this last-resort `JavaScript` we used `jQuery` and some home-programmed `JavaScript` libraries [`JJ.Framework.JavaScript`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.JavaScript/overview/1.7.7817.43032) which had some merit, but may have been superseded by newer tech by now.

I realize `JavaScript` is popuplar with a lot of people and that this is a powerful force. I don't know how my opinion would change, if I would try a newer `JavaScript` version, `TypeScript`, newer tech and libraries. My heart says I'd rather stick to `C#` though.


JJ.Framework
------------

`JJ.Framework` are extensions to `.NET`. They are compact and reusable. They can be found on [NuGet](https://www.nuget.org/profiles/jjvanzon). The lesser-tested ones on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed). You can read more information of it on the [GitHub](https://github.com/jjvanzon/JJ.Framework) repository.

They were made in the spirit of in-house developing small extensions and hiding platform-specific details behind generalize interfaces. They are sort part of the software architecture described here.


Keeping Bi-Directional Relationships in Sync
--------------------------------------------

The classes `ManyToOneRelationship` and `OneToManyRelationship` do inverse property management more or less automatically, which you then use in your models (rich, entity, API or otherwise). More or less: You still have to program classes that derive from `ManyToOneRelationship` and `OneToManyRelationship` and use them a certain way, but the result would be in a navigation property and collection property whose ends will be kept in sync.

Package and code examples available on NuGet [here](https://www.nuget.org/packages/JJ.Framework.Business).

There might be other ways to do this. `Entity Framework` might do it automatically. `NHibernate` appears not to do it for us. A [`LinkTo`](patterns.md#linkto) pattern might be used in certain projects. Or hand-writing the syncing code whereever.


NHibernate
----------

`NHibernate` is a technology, used for data access. A so called `ORM`. It is comparable to `Entity Framework`.

`NHibernate` is used in some projects, because an employer favored it, and some other projects joined the club.

It might be hidden behind abstractions using [`JJ.Framework.Data.NHibernate`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.NHibernate/overview) and through [repository interfaces](patterns.md#repository).


ORM
---

An `ORM` aims to make it easier to focus on the logic around an entity model, while saving things to a database is pretty much done for you.

Here follow some issues you could encounter while using one, and some suggestions for resolving them.

This information was gathered while building experience with `NHibernate`. Also having experienced `NPersist`, it might be possible that the `Entity Framework` has similar issues, due to how `ORM's` seem to work. 

### Generic Interfaces

Data access in this architecture is favored behind generic interfaces using [`JJ.Framework.Data`](https://github.com/jjvanzon/JJ.Framework/tree/master/Framework/Data).

### Committed / Uncommitted Objects

Here is something that happens n `ORM` sometimes:

Some methods of data retrieval work with uncommitted / non-flushed entities: so things that are newly created, and not yet committed to the data store. Other methods of data retrieval do the opposite: only returning committed / flushed entities. This asymmetry might be common in `ORM's`, since doing it any other way might harm performance.

| Method | Data |
|--------|------|
| `IContext.Query` | uncommitted / non-flushed
| `IContext.Get` | 1st committed / flushed, then uncommitted / non-flushed
| `IContext.TryGet` | 1st committed / flushed, then uncommitted / non-flushed
| Navigation properties /<br>following the object graph | 1st committed / flushed, then uncommitted / non-flushed

It appears to have to do with when the `ORM` goes to the database to query or save objects.

### Flush

`Flushing` in `NHibernate` would mean that all the pending `SQL` statements are executed onto the database, without committing the transaction yet.

A `Flush` can help get an auto-generated `ID` from the database. Also when `NHibernate` is confused about the order in which to execute things, a `Flush` may help it execute things in the right order sometimes.

The trouble with `Flush` is that, it might be executed when things are not done yet, and incomplete data might go to the database, upon which database may give an error. So it is a thing to use sparsely only with a good reason, because you can expect some side-effects.

`Flushes` might also go off automatically. Sometimes `NHibernate` wants to get a data-store generated ID. This can happen calling `Save` an entity. Unlike the documentation might suggest, `FlushMode.Never` or `FlushMode.Commit` may not prevent thse intermediate flushes.

Upon saving a parent object, child objects might be flushed too. Internally then `NHibernate` asked itself the question if the child object was `Transient` and while doing so, it apparently wanted to get its identity, by executing an `insert` statement onto the data store. This caused a `null` exception on the `ParentID` column of the child object.

It may also help to create entities in a specific order (e.g. parent object first, then the child objects) or choose a identity generation scheme, that does not require flushing an entity pre-maturely.

### Read-Write Order

It seems `ORM's` like it when you first read the data out, and then start writing to it. Not read, write some, read a little more, write some more. It may have to do with how it queries the database and handles committed / uncommited objects as described earlier above.

### Bridge Entities 

An *bridge* entity is for `n => n` relationhips, usually requiring an additional table to make the link between the entities.

<img src="images/bridge-entity-table-with-composite-key.png" width="200"/>

Using an `ORM`, the bridge entity might not be visible in the code, but can be managed as two collections inside the two related entity types.

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

The `ORM` might do quite a bit of magic under the hood, to keep these collections in sync. Perhaps a little too much for its own good. You can expect quite a few exceptions to go off unfortunately, while `ORM` tries to guard the integrity of the relationship.

These problems almost all go away, if you map to a *bridge entity* instead. Instead of putting collections of the other entity in an entity, you let both entities hold a list of *bridge entities* instead, and the bridge entity would link to 1 entity of each type. This turns the `n => n` relationship into two `1 => n` relationships which ORM can manage with less hardship.

```cs
class QuestionCategory
{
    Question Question { get; set;}
    Question Category { get; set;}
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

It might be is advised that the bridge table not to rely on a composite key of the two `ID's` of the related entities. A single surrogate `ID` field might do better.

<img src="images/bridge-entity-table-with-surrogate-key.png" width="200"/>

This is because it gives 1 handle to the combination of 2 thing, giving `ORM` less difficulty managing things under the hood, prevents passing around composite keys all the time, less good hashing keys, URL's that look more difficult, etc.

`[ ... ]`

### Binary Fields

You might not want to map binary and other serialized data fields using `NHibernate`, because it can harm performance quite a bit.

Retrieving some loose fields of an entity, would also retrieve a blob in that case. As wel as saving a whole blob, when changing just a few fields. That data transmission can be quite a bottle-neck sometimes.

Using separate `SQL` statements for retrieving blobs might be a better alternative.

### Inheritance

Particular surprizes might emerge when using inheritance in your entity model at least while working with `NHibernate`. The main advance is to avoid inheritance at all in the entity models if you can.

When retrieving an entity through ORM, it is likely it will not return an instance of your entity type, but an instance of a type derived from your entity type, a so called ***proxy***. This proxy adds to your entity type a sort of connectedness to the database.

When you retrieved an entity from `HNibernate` that has inheritance, using the base type it returns a proxy of the base type instead of a proxy of the derived type, which makes reference comparisons between base proxies and derived class proxies fail.

You can then *unproxy* both and it will return the underlying object, which is indeed of the derived class, upon which reference comparison succeeds.

But if you can also get failing reference comparisons another way. If you unproxied a derived type, and retrieve another proxy of the derived type, reference comparison might also fail.

[ID comparison](code-style.md#entity-equality-by-id) could avoid this problem with entity equality checks.

This also means that to evaluate the *type*, you are better of unproxying, or it will return the proxy type instead of your entity type, which can be confusing.

By now maybe it may be clear, why the main advice is not to use inheritance in the first place in your entity models, if at all possible.

An alternative for inheritance might be to use a `1-to-1` related object to represent the base of the entity. Although, `NHibernate` and other `ORM's` are  not a fan of `1 => 1` relationships either. Oh well, all in a day's work. Letting two entity types use a mutual `interface` might be an alternative too.

### Conclusion

If this makes you lose grip on reality and wonder whether `ORM's` are worth it? Well, they might still be worth it. They might allow you to program focusing on the meaning of things, rather than how to store it. Even though that is ambiguous because the story above suggests you'd still be better off knowing what it does and when it does it. You just don't need to do it yourself, or see much of it in the code.


SQL
---

Executing queries onto a database is normally done through ORM, but if performance is an issue, it can be combined with SQL.

A choice was made, not to use stored procedures or views. Instead the SQL files were stored directly the .NET projects, under a sub-folder named `Sql`.

![](images/sql-sub-folder.png)

The classic way of executing SQL in .NET would be to use `System.Data.SqlClient`. But instead, the `SqlExecutor` API might be used.

A version of it is available on [`JJs-Pre-Release-Package-Feed`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.SqlClient/overview).

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

A variation of this was implemented here: [`JJs-Pre-Release-Package-Feed`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.NHibernate/overview).

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

`XmlToObjectConverter` and `ObjectToXmlConverter` are also acceptable XML API's in `JJ.Framework.Xml` available on [NuGet](https://www.nuget.org/packages/JJ.Framework.Xml) or `JJ.Framework.Xml.Linq` available on [`JJs-Pre-Release-Package-Feed`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Xml.Linq/overview).


TODO
----

`<Maybe mention some chosen tech from the architectural layering.>`