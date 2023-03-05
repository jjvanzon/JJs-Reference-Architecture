---
title: "📦 API's Misc"
image: "/images/api-preview.png"
---

🧱 API's Misc
==============

[back](.)

This article describes some of the technology choices in this [software architecture](../index.md).

<h3>Contents</h3>

- [Web](#web)
    - [AJAX](#ajax)
    - [JavaScript / TypeScript](#javascript--typescript)
    - [Html.BeginCollection](#htmlbegincollection)
    - [Html.BeginCollectionItem](#htmlbegincollectionitem)
- [Misc](#misc)
    - [JJ.Framework](#jjframework)
    - [Configuration](#configuration)
    - [OneToManyRelationship](#onetomanyrelationship)
    - [XML](#xml)
    - [Embedded Resources](#embedded-resources)


Web
---

### AJAX

`AJAX` is a way to load part of a web page, so the whole page does not have to be refreshed. This may make the user interaction smoother, than reloading the entire page every time.

For `AJAX'ing` such partial web content, our team programmed [wrapper](../patterns/other.md#wrapper) functions in [`JavaScript`](table.md#javascript), around calls to [`jQuery`](table.md#jquery), so we could `AJAX` with a single code line and handle both partial loads and full reloads the same way. It saved quite a few lines of [`JavaScript`](table.md#javascript) code.

Our strategy was to prefer full loads, so we could keep most logic in the [`C#`](table.md#csharp) realm. This before resorting to `AJAX` calls. See [Full Load – Partial Load – Cient-Native Code](../patterns/presentation.md#full-load--partial-load--client-native-code).

### JavaScript / TypeScript

[`JavaScript`](https://www.javascript.com/) is a programming language with a wide range of applications. Originally it was run in web browsers to optimize the user experience.

[`JavaScript`](https://www.javascript.com/) was less preferred as an architectural choice. [`JavaScript's`](https://www.javascript.com/) weak type system played a role. The strange behavior and trickiness in [`JavaScript`](https://www.javascript.com/) (part due to this weak typing) gave it less appeal.

For web, other technology was preferred in this [architecture](../index.md): The idea behind [`MVC`](table.md#mvc) was logic on the server-side. [`Views`](../patterns/presentation.md#views) were in [`Razor`](table.md#razor). Best to keep most logic [`C#`](table.md#csharp) was the idea.

[`JavaScript`](https://www.javascript.com/) would easily get bloated, getting out of hand from a maintainability perspective, was the prevailing opinion. You could refactor [`C#`](table.md#csharp) code, upon which lots of the [`JavaScript`](https://www.javascript.com/) might break unexpectedly, with an error message tucked away in some console window, instead of right in your face when compiling.

[`TypeScript`](https://www.typescriptlang.org/) may have saved the day to cover for the weak typing from [`JavaScript`](https://www.javascript.com/). But we hadn't tried that yet.

But still: logic in one place in one language ([`C#`](table.md#csharp)) felt so nice. I guess the love for [`C#`](table.md#csharp) was strong.

The idea was that a full page load was 1<sup>st</sup> choice, [`AJAX'ing`](#ajax) the 2<sup>nd</sup> choice, and last in line [`JavaScript`](https://www.javascript.com/) *only* to support the user interaction. No business logic. See also: [Full Load – Partial Load – Cient-Native Code](../patterns/presentation.md#full-load--partial-load--client-native-code).

For this last-resort [`JavaScript`](https://www.javascript.com/) we used [`jQuery`](table.md#jquery) and some home-programmed [`JavaScript`](https://www.javascript.com/) libraries: [`JJ.Framework.JavaScript`](table.md#jj-framework-javascript) which had some merit, but may have been superseded by newer tech by now.

I realize [`JavaScript`](https://www.javascript.com/) is popular with a lot of people and that this is a powerful force. I don't know how my opinion would change, if I would try a newer [`JavaScript`](https://www.javascript.com/) version, [`TypeScript`](https://www.typescriptlang.org/), newer tech and libraries. My heart says I'd rather stick with [`C#`](table.md#csharp) though.

### Html.BeginCollection

In [`MVC`](table.md#mvc) it is not so straightforward to [`HTTP` a tree structure in postdata](../aspects.md#postdata-over-http).

[`JJ.Framework.Mvc`](table.md#jj-framework-mvc) makes that easier, by offering an `HtmlHelper` extensions: [`Html.BeginCollection`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Mvc). Using that `API` you can send a [`ViewModel`](../patterns/viewmodels.md) with arbitrary nestings and collections over the line. It would be restored as a [`ViewModel`](../patterns/viewmodels.md) at the server side.

In the [`View`](../patterns/presentation.md#views) code you would wrap each nesting inside a `using` block:

```cs
@using (Html.BeginItem(() => Model.MyItem))
{
    using (Html.BeginCollection(() => Model.MyItem.MyCollection))
    {
        foreach (var x in Model.MyItem.MyCollection)
        {
            using (Html.BeginCollectionItem())
            {
                ...
            }
        }
    }
}
```

So each time you enter a level, the `HtmlHelper` is called again and the code wrapped in a `using` block.

There can be as many collections as needed, and as much nesting as you like. The nesting can even be spread around multiple partial [views](../patterns/presentation.md#views).

Input fields in a nested structure look as follows:

```cs
Html.TextBoxFor(x => x.MyProperty)
```

Or:

```cs
Html.TextBoxFor(x => Model.MyProperty)
```

But not like this:

```cs
Html.TextBoxFor(x => myLoopItem.MyItem.MyProperty)
```

Otherwise the input fields might not bind to the [`ViewModel`](../patterns/viewmodels.md). This may force you to program partial [`Views`](../patterns/presentation.md#views) sometimes. That may be good practice anyway, so might not be such a big trade-off.

### Html.BeginCollectionItem

In [`MVC`](table.md#mvc) it is not so apparent how to [send a collection as `HTTP postdata`](../aspects.md#postdata-over-http).

One alternative is the often-used [`Html.BeginCollectionItem`](https://www.nuget.org/packages/BeginCollectionItem):

```cs
@foreach (var child in Model.Children)
{
    using (Html.BeginCollectionItem("Children"))
    {
        ...
    }
}
```

This `API` has some limitations:

- It can send *one* collection over the wire, not trees.
- It takes a `string` a parameter, not an expression like: `() => Model.Children`.

To send trees and arbitrary nestings over `HTTP postdata`, consider using [`Html.BeginCollection`](table.md#htmlbegincollection) from [`JJ.Framework.Mvc`](table.md#jj-framework-mvc).


Misc
----

### JJ.Framework

[`JJ.Framework`](https://www.nuget.org/profiles/jjvanzon) are nuts, bolts and screws for software development. There were things missing in [`.NET`](table.md#dotnet), so we programmed our own. These extensions to [`.NET`](table.md#dotnet) are compact and reusable. They can be found on [NuGet](https://www.nuget.org/profiles/jjvanzon). The lesser-tested ones on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed). You can read more information of it on the [GitHub](https://github.com/jjvanzon/JJ.Framework) repository.

They were made in the spirit of in-house developing small extensions and hiding platform-specific details behind [generalized interfaces](../layers.md#loosely-coupled). They are sort part of the [software architecture](../index.md) described here.

### Configuration

[`.NET`](table.md#dotnet) code can use configuration files, often named `App.config` or `Web.config`.

To access these `configs` we might use [`JJ.Framework.Configuration`](https://www.nuget.org/packages/JJ.Framework.Configuration) quite a bit more easily than using [`.NET's`](table.md#dotnet) `System.Configuration` directly.

You might read from its [`README`](https://www.nuget.org/packages/JJ.Framework.Configuration) how it works.

It does not seem to support reading out the `connectionStrings` section yet. So here is an idea how that might work, would it ever be programmed.

<h4>ConnectionStrings</h4>

Reading out `connectionStrings` might be made similar to reading out the [`appSettings`](https://www.nuget.org/packages/JJ.Framework.Configuration#appsettingsreadert). Connection strings in the `App.config` or `Web.config` may look as follows:

```xml
<connectionStrings>
  <add name="OrderDB" connectionString="data source=192.168.XX.XX;Initial Catalog=OrderDB..." />
</connectionStrings>
```

This would be the *classic* way of reading it out:

```cs
string connectionString =
    ConfigurationManager.ConnectionStrings["OrderDB"].ConnectionString;
```

This would be the alternative in `JJ.Framework.Configuration`:

```cs
string connectionString = 
    ConnectionStrings<IConnectionStrings>.Get(x => x.OrderDB);
```

You can define an `interface` to use the strongly-typed name:

```cs
internal interface IConnectionStrings
{
    string OrderDB { get; }
}
```

### OneToManyRelationship

*Bidirectional relationship synchronization* allows for automatic synchronization of related properties in a parent-child relationship. By setting the parent property, `product.Supplier = mySupplier`, the child collection, `mySupplier.Products`, will also be updated to include `myProduct`.

This can be achieved through the use of classes such as [`ManyToOneRelationship`](https://www.nuget.org/packages/JJ.Framework.Business#onetomanyrelationship-manytoonerelationship) and [`OneToManyRelationship`](https://www.nuget.org/packages/JJ.Framework.Business#onetomanyrelationship-manytoonerelationship) from the [`JJ.Framework.Business`](table.md#jj-framework-business) package, which can be used in various models: [rich](../patterns/other.md#rich-models), [entity](../patterns/data-access.md#entities), `API` or otherwise.

There may be other options available. [`NHibernate`](orm.md#nhibernate) does not appear to do it for us automatically. However [`Entity Framework`](orm.md#entity-framework) might do this synchronization automatically. The [`LinkTo`](../patterns/business-logic.md#linkto) pattern can also be used. Or hand-writing the syncing in-place. 

### XML

`XML` is a file format for storing and transmitting data. When working with `API's` for `XML` there are a few options to consider.

In most cases, it is recommended to use `XElement` (LINQ to XML) instead of `XmlDocument`. However, a useful feature of `XmlDocument` is that it supports [`XPath`](https://www.w3schools.com/xml/xpath_intro.asp).

To handle nullability and uniqueness more gracefully, it is suggested to use `XmlHelper` methods from [`JJ.Framework.Xml`](table.md#jj-framework-xml) or [`JJ.Framework.Xml.Linq`](table.md#jj-framework-xml-linq) over using other `API's` directly.

For converting `XML` to an object graph, [`XmlToObjectConverter`](https://www.nuget.org/packages/JJ.Framework.Xml#xmltoobjectconverter) and [`ObjectToXmlConverter`](https://www.nuget.org/packages/JJ.Framework.Xml#xmltoobjectconverter) from [`JJ.Framework.Xml`](table.md#jj-framework-xml) and [`JJ.Framework.Xml.Linq`](table.md#jj-framework-data-xml-linq) might be useful. They can offer simpler solutions than other `API's`.

### Embedded Resources

*Embedded resources* might be handy, to prevent including loose files with a deployment. Instead they are compiled right into your program files' `EXE` or `DLL`. This also protects those resources a bit better against modifications.

To include a file as an embedded resource, you could set the following property in [`Visual Studio`](table.md#visual-studio):

![](../images/sql-as-embedded-resource.png)

[`JJ.Framework.Common`](https://www.nuget.org/packages/JJ.Framework.Common) contains a [`Helper`](../patterns/other.md#helper) `class` [`EmbeddedResourceReader`](table.md#embedded-resource-reader). It makes it a little bit easier to access those resources from your code:

```cs
string text = EmbeddedResourceReader.GetText(assembly, "Ingredient_UpdateName.sql");
```
[back](.)

<div style="min-height: 512px">
</div>

<h2 id="data-1">Data</h2>

<h3 id="entity-framework">Entity Framework</h3>

`[` [`Moved`](orm.md#entity-framework) `]`

<h3 id="nhibernate">NHibernate</h3>

`[` [`Moved`](orm.md#nhibernate) `]`

<h3 id="orm">ORM</h3>

`[` [`Moved`](orm.md) `]`

<h3 id="sql">SQL</h3>

`[` [`Moved`](sql.md) `]`

<div style="min-height: 512px">
</div>

[back](.)
