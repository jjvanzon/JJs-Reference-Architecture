---
title: "🧱 API's Misc"
image: "/images/api-misc.png"
descriptions: "Some misc choices for API's in JJ's Reference Architecture: XML, embedded resources, configuration settings, JJ.Framework, bi-directional relationship synchronization."
keywords:
  - configuration
  - settings
  - embedded resources
  - xml
  - xpath
  - xmldocument
  - xelement
  - linq to xml
  - serializer
  - serialization
  - one to many
  - bidirectional
  - relationship
  - jj.framework
  - nuget
  - framework
  - reusability
  - c#
  - .net
  - coding
  - programming
  - software engineering
  - software development
  - software design
  - software architecture
---

🧱 API's Misc
==============

[back](.)

This article describes some misc technologies chosen in this [software architecture](../index.md).

<h2>Contents</h2>

- [JJ.Framework](#jjframework)
- [Configuration](#configuration)
- [OneToManyRelationship](#onetomanyrelationship)
- [XML](#xml)
- [Embedded Resources](#embedded-resources)

JJ.Framework
------------

[`JJ.Framework`](https://www.nuget.org/profiles/jjvanzon) are nuts, bolts and screws for software development. There were things missing in [`.NET`](table.md#dotnet), so we programmed our own. These extensions to [`.NET`](table.md#dotnet) are compact and reusable. They can be found on [NuGet](https://www.nuget.org/profiles/jjvanzon). The lesser-tested ones on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed). You can read more information of it on the [GitHub](https://github.com/jjvanzon/JJ.Framework) repository.

They were made in the spirit of in-house developing small extensions and hiding platform-specific details behind [generalized interfaces](../layers.md#loosely-coupled). They are sort part of the [software architecture](../index.md) described here.


Configuration
-------------

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


OneToManyRelationship
---------------------

*Bidirectional relationship synchronization* allows for automatic synchronization of related properties in a parent-child relationship. By setting the parent property, `product.Supplier = mySupplier`, the child collection, `mySupplier.Products`, will also be updated to include `myProduct`.

This can be achieved through the use of classes such as [`ManyToOneRelationship`](https://www.nuget.org/packages/JJ.Framework.Business#onetomanyrelationship-manytoonerelationship) and [`OneToManyRelationship`](https://www.nuget.org/packages/JJ.Framework.Business#onetomanyrelationship-manytoonerelationship) from the [`JJ.Framework.Business`](table.md#jj-framework-business) package, which can be used in various models: [rich](../patterns/other.md#rich-models), [entity](../patterns/data-access.md#entities), `API` or otherwise.

There may be other options available. [`NHibernate`](orm.md#nhibernate) does not appear to do it for us automatically. However [`Entity Framework`](orm.md#entity-framework) might do this synchronization automatically. The [`LinkTo`](../patterns/business-logic.md#linkto) pattern can also be used. Or hand-writing the syncing in-place. 


XML
---

`XML` is a file format for storing and transmitting data. When working with `API's` for `XML` there are a few options to consider.

In most cases, it is recommended to use `XElement` (LINQ to XML) instead of `XmlDocument`. However, a useful feature of `XmlDocument` is that it supports [`XPath`](https://www.w3schools.com/xml/xpath_intro.asp).

To handle nullability and uniqueness more gracefully, it is suggested to use `XmlHelper` methods from [`JJ.Framework.Xml`](table.md#jj-framework-xml) or [`JJ.Framework.Xml.Linq`](table.md#jj-framework-xml-linq) over using other `API's` directly.

For converting `XML` to an object graph, [`XmlToObjectConverter`](https://www.nuget.org/packages/JJ.Framework.Xml#xmltoobjectconverter) and [`ObjectToXmlConverter`](https://www.nuget.org/packages/JJ.Framework.Xml#xmltoobjectconverter) from [`JJ.Framework.Xml`](table.md#jj-framework-xml) and [`JJ.Framework.Xml.Linq`](table.md#jj-framework-data-xml-linq) might be useful. They can offer simpler solutions than other `API's`.


Embedded Resources
------------------

*Embedded resources* might be handy, to prevent including loose files with a deployment. Instead they are compiled right into your program files' `EXE` or `DLL`. This also protects those resources a bit better against modifications.

To include a file as an embedded resource, you could set the following property in [`Visual Studio`](table.md#visual-studio):

![](../images/sql-as-embedded-resource.png)

[`JJ.Framework.Common`](https://www.nuget.org/packages/JJ.Framework.Common) contains a [`Helper`](../patterns/other.md#helper) `class` [`EmbeddedResourceReader`](table.md#embedded-resource-reader). It makes it a little bit easier to access those resources from your code:

```cs
string text = EmbeddedResourceReader.GetText(assembly, "Ingredient_UpdateName.sql");
```

[back](.)
