<style>section, .wrapper { max-width: 90% }</style>

🎁 API's
=========

[back](.)

This article describes some of the API and technology choices in this software architecture.

<h3>Contents</h3>

- [Introduction](#introduction)
- [List of API's (and other tech)](#list-of-apis-and-other-tech)
  - [Code](#code)
  - [Data](#data)
  - [Logic](#logic)
  - [Presentation](#presentation)
  - [Debugging / Testing](#debugging--testing)
  - [Processing / IO](#processing--io)
  - [Other](#other)
- [More Elaborate Descriptions](#more-elaborate-descriptions)
- [Web](#web)
  - [AJAX](#ajax)
  - [JavaScript / TypeScript](#javascript--typescript)
- [Misc](#misc)
  - [JJ.Framework](#jjframework)
  - [Configuration](#configuration)
    - [ConnectionStrings](#connectionstrings)
  - [OneToManyRelationship](#onetomanyrelationship)
  - [XML](#xml)
  - [Embedded Resources](#embedded-resources)
- [Data](#data-1)
  - [Entity Framework](#entity-framework)
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
    - [SQL String Concatenation](#sql-string-concatenation)
    - [Hiding SQL behind Repositories](#hiding-sql-behind-repositories)
    - [Database Upgrade Scripts](#database-upgrade-scripts)


Introduction
------------

This article lists some of the tech used in `JJ` projects. Most are listed out in a table.

Some technology is described in more detail, mostly data store technologies but also about web technology.


List of API's (and other tech)
------------------------------

### Code

<table>

<tr>
  <th>Visual Studio</th>
  <td>Used for the development of the code.</td>
</tr>

<tr>
  <th>VS Code</th>
  <td>Used for MarkDown editing.</td>
</tr>

<tr>
  <th>.NET</th>
  <td>Framework from Microsoft that forms a base for the programming.</td>
</tr>

<tr>
  <th>C#</th>
  <td>Primary programming language.</td>
</tr>

<tr>
  <th>VB.NET</th>
  <td>Some projects might still use this programming language.</td>
</tr>

<tr>
  <th>ReSharper</th>
  <td>Tool for code formatting, refactoring and code smells and such.</td>
</tr>

<tr>
  <th>git</th>
  <td>Source control, revision history, version management for the code.</td>
</tr>

<tr>
  <th><a href="https://github.com/jjvanzon">GitHub</a></th>
  <td>Where the source code is hosted and shared.</td>
</tr>

<tr>
  <th>
  <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">
      Azure DevOps</a>
  </th>
  <td>
      Build pipeline. Pre-release package feed. Original planning boards. Might still host one project not migrated to GitHub.
  </td>
</tr>

<tr>
  <th>GitHub Issues</th>
  <td>Gradually using GitHub Issues more for planning.</td>
</tr>

<tr>
  <th>JJ.Framework</th>
  <td>
      In-house programmed extensions to the .NET Framework can be found on
      <a href="https://github.com/jjvanzon/JJ.Framework">GitHub</a> / 
      <a href="https://www.nuget.org/profiles/jjvanzon">NuGet</a> /
      <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">JJs-Pre-Release-Package-Feed</a>
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Conversion">
       JJ.Framework.Conversion</a>
  </th>
  <td>
      Makes it easier to convert simple types.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Reflection">
       JJ.Framework.Reflection</a>
  </th>
  <td>
      Helps with and speeds up accessing code structure elements through reflection and lambda expressions.
  </td>
</tr>

</table>

### Data

<table>

<tr>
  <th>SQL Server</th>
  <td>Primary data store technology for relational databases.</td>
</tr>

<tr>
  <th>ORM</th>
  <td>Hides most SQL, exposing an object graph, to focus on the logic, instead of on the data storage.</td>
</tr>

<tr>
  <th>SQL</th>
  <td>For performance reasons SQL is hand-programmed incidentally, combined with ORM.</td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/NHibernate">
       NHibernate</a>
  </th>
  <td>
      A type of ORM. Chosen in several JJ project because an employer also so happened to use it.
  </td>
</tr>

<tr>
  <th>
    <a href="https://nhibernate.info/doc/nhibernate-reference/queryqueryover.html">
       QueryOver</a>
  </th>
  <td>
      A strongly-typed query language like LINQ, but then the NHibernate variation.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/FluentNHibernate">
       FluentNHibernate</a>
  </th>
  <td>
      A way to define ORM mappings, using fluent notation.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/EntityFramework">
       EntityFramework</a>
  </th>
  <td>
      A type of ORM. Chosen less in the JJ projects, because of more experience with NHibernate. Worth considering though.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">
       JJ.Framework.Data</a>
  </th>
  <td>
      Helps hide data access behind abstractions. It does notexpose whether it is SQL Server, SQL, ORM, NHibernate. There would just be abstracted convenient methods instead.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.SqlClient">
      SqlExecutor</a>
  </th>
  <td>
      Helps execute SQL with less code lines, and more type save than using SqlClient directly.
  </td>
</tr>

<tr>
  <th>LINQ</th>
  <td>
      A query language usable in C#. Can be used to query several types of data store, but used commonly for in-memory collections.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Collections">
        JJ.Framework.Collections</a>
  </th>
  <td>JJ extensions to LINQ.</td>
</tr>

</table>

### Logic

<table>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Business">
       JJ.Framework.Business</a>
  </th>
  <td>
      Types for supporting a business layer and/or API. Bidirectional relationship sync. Result types to pass data, succes flags and (validation) messages.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Validation">
       JJ.Framework.Validation</a>
  </th>
  <td>
      A nice fluent notation for validations.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Mathematics">
       JJ.Framework.Mathematics</a>
  </th>
  <td>
      Helpers for math things. See link.
  </td>
</tr>

</table>

### Presentation

<h4>General</h4>

<table>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Presentation">
       PagerViewModelFactory</a>
  </th>
  <td>
      Can construct a pager view model with properties like
      CanGoToFirstPage, CanGoToPreviousPage, CanGoToNextPage, CanGoToLastPage.
  </td>
</tr>

</table>

<h4>Web</h4>

<table>

<tr>
  <th>IIS</th>
  <td>
      (Internet Information Services.) For hosting web sites. 
      Some Visual Studio projects wish to use it upon load.
  </td>
</tr>

<tr>
  <th>MVC</th>
  <td>A web development tech in the .NET Framework. Code runs mostly server side.</td>
</tr>

<tr>
  <th>Razor</th>
  <td>A view renderer for web. Give terse syntax, combining C# and HTML almost seemlessly.</td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Mvc">
       Html.BeginCollection</a>
  </th>
  <td>
      Makes it possible to send tree structures over HTTP to the server-side MVC.
  </td>
</tr>

<tr>
  <th><a href="#javascript--typescript">JavaScript</a></th>
  <td>Used to support UI details in web. In this architecture most (UI) logic would be handled in C#.</td>
</tr>

<tr>
  <th><a href="#javascript--typescript">TypeScript</a></th>
  <td>Might be preferred over JavaScript in the future. Has not been applied</td>
</tr>

<tr>
  <th><a href="#ajax">AJAX</a></th>
  <td>For retrieving / posting back partial views to the server and back.</td>
</tr>

<tr>
  <th>jQuery</th>
  <td>Used to support UI details in web. Can make some JavaScript shorter.</td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.JavaScript">
       JJ.Framework.JavaScript</a>
  </th>
  <td>
      Used to support UI details in web.
      Remembering scroll position, cookie functions, url parsing.
      Might be extended with one-line AJAX functions once.
  </td>
</tr>

</table>

<h4>Win</h4>

<table>

<tr>
  <th>WinForms</th>
  <td>
      Used in some projects. Small utilities and JJ.Synthesizer uses it as the top-most layer.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.WinForms">
       SimpleProcessForm</a>
  </th>
  <td>
      A base form for a utility that runs a process.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.VectorGraphics">
       JJ.Framework.VectorGraphics</a>
  </th>
  <td>
      A custom-programmed vector graphics model. Can be used for component-based user interfaces.
  </td>
</tr>

</table>

### Debugging / Testing

<table>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/MSTest.TestFramework">
       MSTest</a>
  </th>
  <td>
      For automated / unit testing. Seems a deprecated framework.
      Might upgrade, but it ain't on the top of the list.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Testing">
       JJ.Framework.Testing</a>
  </th>
  <td>
      Extends the Assert class, but automatically includes the tested expression in the error messages.
  </td>
</tr>

<tr>
  <th>DebuggerDisplays</th>
  <td>A technique to quickly display helpful info in the watch screen.</td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Exceptions">
       JJ.Framework.Exceptions</a>
  </th>
  <td>
      Contains exception classes for basic errors.
      Clear concise error messages,
      including tested expressions and tested values.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Reflection#accessor">
       Accessor</a>
  </th>
  <td>
      For accessing the internals of types for instance for testing purposes.
  </td>
</tr>

</table>

### Processing / IO

<table>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Text">
       JJ.Framework.Text</a>
  </th>
  <td>
      Basic helpers for working with text.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.IO">
       JJ.Framework.IO</a>
  </th>
  <td>
      Contains various file functions, functions for working with streams and working with CSV's.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.HtmlToXml">
       JJ.Framework.HtmlToXml</a>
  </th>
  <td>
      The HtmlToXmlConverter class steals from SgmlReader. It does what it says.
  </td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Xml">
       JJ.Framework.Xml</a>
  </th>
  <td>
      A convenient way to map XML to (C#) classes.<br/>
      Access XML nodes more safely, with null and unicity checks.
  </td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Xml.Linq">
       JJ.Framework.Xml.Linq</a>
  </th>
  <td>
      "
  </td>
</tr>

<tr>
  <th><a href="#embedded-resources">Embedded Resources</a></th>
  <td>Embedded resources allow compiling files and content right inside a DLL or EXE.</td>
</tr>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Common">
      EmbeddedResourceReader</a>
  </th>
  <td>
      Make it a little easier to get embedded resource Streams, bytes and strings.
  </td>
</tr>

</table>

### Other

<h4>Localization</h4>

<table>

<tr>
  <th><a href="patterns.md#resource-strings">Resource Strings</a></th>
  <td>For localization, resx files can be used in Visual Studio.</td>
</tr>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.ResourceStrings">
       JJ.Framework.ResourceStrings</a>
  </th>
  <td>
      Reusable button texts and such in multiple languages. For now supports Dutch, US English and some broken Polish.
  </td>
</tr>

<tr>
  <th><a href="aspects.md#localization">Localization</a></th>
  <td>More ideas about localization.</td>
</tr>

</table>

<h4>Configuration</h4>

<table>

<tr>
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Configuration">
       JJ.Framework.Configuration</a>
  </th>
  <td>
      For working with complex configuration files, esier than System.Configuration.
  </td>
</tr>

<tr>
  <th><a href="aspects.md#configuration">Configuration</a></th>
  <td>More info about configuration.</td>
</tr>

</table>

<h4>Security</h4>

<table>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Security">
       JJ.Framework.Security</a>
  </th>
  <td>
      A generic interfacing for authenticating a user and yet to be tested hashed salted password authentication.
  </td>
</tr>

<tr>
  <th>
    <a href="aspects.md#security">Security</a>
  </th>
  <td>
      If more might be needed security-wise, it may be hidden behind generic interfaces, abstracting the security system.
  </td>
</tr>


</table>

<h4>Logging</h4>

<table>

<tr>
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Logging">
       JJ.Framework.Logging</a>
  </th>
  <td>
      For now might only contains the ExceptionHelper class, which for instance converts exception information to a string.
  </td>
</tr>

<tr>
  <th>
    <a href="aspects.md#logging">Logging</a>
  </th>
  <td>
      Described how it might be extended to contain more other code to do with logging.
  </td>
</tr>


</table>


More Elaborate Descriptions
---------------------------

For some of these things you can find more elaborate descriptions below: mostly about data store technologies, but also some about web technology.

Web
---

### AJAX

For `AJAX`'ing partial web content, our team made our own wrapper `AJAX` methods, around calls to `jQuery`, so we could `AJAX` with a single code line and handle both partial loads and full reloads the same way. Saved quite a few lines of JavaScript code.

Our strategy was to prefer full loads, so we could keep most logic in the `C#` realm. This before resorting to `AJAX` calls. See [First Full Load – Then Partial Load – Then Native Code](patterns.md#first-full-load--then-partial-load--then-native-code).

### JavaScript / TypeScript

`JavaScript` was less preferred as an architectural choice. `JavaScript's` weak type system played a role. The strange behavior and trickiness in `JavaScript` (part due to this weak typing) gave it less appeal.

The idea behind `MVC` was logic on the server-side. Views were in `Razor`. Best to keep most logic `C#` was the idea.

`JavaScript` would easily get bloated, getting out of hand from a maintainability perspective, was the opinion. In `C#` you could refactor, upon which lots of the `JavaScript` might break unexpectedly, with an error message tucked away in some console window, instead of right in your face.

`TypeScript` may have saved the day to cover for the weak typing from `JavaScript`. But it wasn't tried yet.

But still: logic in one place in one language (`C#`) felt so nice. I guess the love for `C#` is strong.

The idea was that a full page load was 1st choice, 2nd choice `AJAX'ing`, and last in line `JavaScript` only to support the user interaction. No business logic. See also: [First Full Load – Then Partial Load – Then Native Code](patterns.md#first-full-load--then-partial-load--then-native-code).

For this last-resort `JavaScript` we used `jQuery` and some home-programmed `JavaScript` libraries [`JJ.Framework.JavaScript`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.JavaScript) which had some merit, but may have been superseded by newer tech by now.

I realize `JavaScript` is popuplar with a lot of people and that this is a powerful force. I don't know how my opinion would change, if I would try a newer `JavaScript` version, `TypeScript`, newer tech and libraries. My heart says I'd rather stick to `C#` though.


Misc
-----

### JJ.Framework

`JJ.Framework` are extensions to `.NET`. They are compact and reusable. They can be found on [NuGet](https://www.nuget.org/profiles/jjvanzon). The lesser-tested ones on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed). You can read more information of it on the [GitHub](https://github.com/jjvanzon/JJ.Framework) repository.

They were made in the spirit of in-house developing small extensions and hiding platform-specific details behind generalize interfaces. They are sort part of the software architecture described here.

### Configuration

For configuration we might use the [`JJ.Framework.Configuration`](https://www.nuget.org/packages/JJ.Framework.Configuration) API, which is quite a bit easier than using .NET's `System.Configuration` directly.

You might read from its `README` how it works.

It does not seem to support reading out the `connectionStrings` section yet. So here is an idea how that might work. would it ever be programmed.

#### ConnectionStrings

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

### OneToManyRelationship

The classes `ManyToOneRelationship` and `OneToManyRelationship` can keep bidirectional relationships in sync more or less automatically, which you then use in your models (rich, entity, API or otherwise). More or less: You still have to program classes that derive from `ManyToOneRelationship` and `OneToManyRelationship` and use them a certain way, but the result would be in a navigation property and collection property whose ends will be kept in sync.

Package and code examples available on NuGet [here](https://www.nuget.org/packages/JJ.Framework.Business).

There might be other ways to do this. `Entity Framework` might do it automatically. `NHibernate` does not appear to do it for us. A [`LinkTo`](patterns.md#linkto) pattern might be used in certain projects. Or hand-writing the syncing code whereever.


### XML

Preference for `XElement` (`LINQ to XML`) over `XmlDocument` except when you want to use `XPath`.

Perhaps prefer the `XmlHelper` methods (from [`JJ.Framework.Xml`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Xml) or [`JJ.Framework.Xml.Linq`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.Xml.Linq)) over using other API's directly, because the helper will handle nullability and unicity more grafully.

`XmlToObjectConverter` and `ObjectToXmlConverter` might also be used. (Also in [`JJ.Framework.Xml`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Xml) and [`JJ.Framework.Xml.Linq`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.Xml.Linq)). That might be a simpler way to convert XML to an object graph than other API's.

### Embedded Resources

Embedded resources might be handy to prevent having to include loose files with a deployment, but instead compiling the loose files right into the assembly DLL or EXE. It also protects those resources a little bit better against modifications.

To include a file as an embedded resource, you could set the following property:

![](images/sql-as-embedded-resource.png)

[`JJ.Framework.Common`](https://www.nuget.org/packages/JJ.Framework.Common) contains a helper class `EmbeddedResourceReader` which make it a little bit easier to access those resources in your code:

```cs
string text = EmbeddedResourceReader.GetText(
  assembly, "Ingredient_UpdateName.sql");
```


Data
----

### Entity Framework

`Entity Framework` is a framework for data access, a so called `ORM` (__O__bject __R__elational __M__apper). `Entity Framework` might be hidden behind abstractions using [`JJ.Framework.Data.EntityFramework`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.EntityFramework) and [repository interfaces](patterns.md#repository-interfaces).

`JJ.Framework.Data.EntityFramework` at one point seemed to become quite slow, without modifying it. It was not upgraded since then, because most of the apps used `NHibernate` instead.

It may be required to enable `MSDTC`. That would be a service belonging to an `SQL Server` installation that might have to be enabled. Otherwise transactions might not work.

### NHibernate

`NHibernate` is a technology, used for data access. A so called `ORM` (__O__bject __R__elational __M__apper). It is comparable to `Entity Framework`.

`NHibernate` is used in some projects, because an employer favored it, and some other projects joined the club.

`NHibernate` might be hidden behind abstractions using [`JJ.Framework.Data.NHibernate`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.NHibernate) and through [repository interfaces](patterns.md#repository).

### ORM

An `ORM` aims to make it easier to focus on the logic around an entity model, while saving things to a database is pretty much done for you.

Here follow some issues you could encounter while using an `ORM`, and some suggestions for dealing with it.

This information was gathered from experience built up with `NHibernate`. Also having experienced `NPersist`, it might be possible that the `Entity Framework` has similar issues, due to how `ORM's` work. 

#### Generic Interfaces

Data access in this architecture is favored behind generic interfaces using [`JJ.Framework.Data`](https://github.com/jjvanzon/JJ.Framework/tree/master/Framework/Data).

#### Committed / Uncommitted Objects

Here is something that happens in `ORM` sometimes:

Some methods of data retrieval work with uncommitted / non-flushed entities: so things that are newly created, and not yet committed to the data store. Other methods of data retrieval do the opposite: only returning committed / flushed entities. This asymmetry might be common in `ORM's`, since doing it another way might harm performance.

| Method | Data |
|--------|------|
| `IContext.Query` | uncommitted / non-flushed
| `IContext.Get` | 1st committed / flushed, then uncommitted / non-flushed
| `IContext.TryGet` | 1st committed / flushed, then uncommitted / non-flushed
| Navigation properties /<br>following the object graph | 1st committed / flushed, then uncommitted / non-flushed

It appears to have to do with when the `ORM` goes to the database to query or save objects.

#### Flush

`Flushing` in `NHibernate` would mean that all the pending `SQL` statements are executed onto the database, without committing the transaction yet.

A `Flush` can help get an auto-generated `ID` from the database. Also when `NHibernate` is confused about the order in which to execute things, a `Flush` may help it execute things in the right order sometimes.

The trouble with `Flush` is that, it might be executed when things are not done yet, and incomplete data might go to the database, upon which database may give an error. So it is a thing to use sparsely only with a good reason, because you can expect some side-effects.

`Flushes` might also go off automatically. Sometimes `NHibernate` wants to get a data-store generated ID. This can happen calling `Save` an entity. Unlike the documentation might suggest, `FlushMode.Never` or `FlushMode.Commit` may not prevent thse intermediate flushes.

Upon saving a parent object, child objects might be flushed too. Internally then `NHibernate` asked itself the question if the child object was `Transient` and while doing so, it apparently wanted to get its identity, by executing an `insert` statement onto the data store. This caused a `null` exception on the `ParentID` column of the child object.

It may also help to create entities in a specific order (e.g. parent object first, then the child objects) or choose a identity generation scheme, that does not require flushing an entity pre-maturely.

#### Read-Write Order

It seems `ORM's` like it when you first read the data out, and then start writing to it. Not read, write some, read a little more, write some more. It may have to do with how it queries the database and handles committed / uncommited objects as described earlier above.

#### Bridge Entities 

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

#### Binary Fields

You might not want to map binary and other serialized data fields using `NHibernate`, because it can harm performance quite a bit.

Retrieving some loose fields of an entity, would also retrieve a blob in that case. As wel as saving a whole blob, when changing just a few fields. That data transmission can be quite a bottle-neck sometimes.

Using separate `SQL` statements for retrieving blobs might be a better alternative.

#### Inheritance

Particular surprises might emerge when using inheritance in your entity model at least while working with `NHibernate`. The main advance is to avoid inheritance at all in the entity models if you can.

When retrieving an entity through ORM, it is likely it will not return an instance of your entity type, but an instance of a type derived from your entity type, a so called ***proxy***. This proxy adds to your entity type a sort of connectedness to the database.

When you retrieved an entity from `HNibernate` that has inheritance, using the base type it returns a proxy of the base type instead of a proxy of the derived type, which makes reference comparisons between base proxies and derived class proxies fail.

You can then *unproxy* both and it will return the underlying object, which is indeed of the derived class, upon which reference comparison succeeds.

But if you can also get failing reference comparisons another way. If you unproxied a derived type, and retrieve another proxy of the derived type, reference comparison might also fail.

[ID comparison](code-style.md#entity-equality-by-id) could avoid this problem with entity equality checks.

This also means that to evaluate the *type*, you are better of unproxying, or it will return the proxy type instead of your entity type, which can be confusing.

By now maybe it may be clear, why the main advice is not to use inheritance in the first place in your entity models, if at all possible.

An alternative for inheritance might be to use a `1-to-1` related object to represent the base of the entity. Although, `NHibernate` and other `ORM's` are  not a fan of `1 => 1` relationships either. Oh well, all in a day's work. Letting two entity types use a mutual `interface` might be an alternative too.

#### Conclusion

If this makes you lose grip on reality and wonder whether `ORM's` are worth it? Well, they might still be worth it. They might allow you to program focusing on the meaning of things, rather than how to store it. Even though that is ambiguous because the story above suggests you'd still be better off knowing what it does and when it does it. You just don't need to do it yourself, or see much of it in the code.

### SQL

Executing queries onto a database is normally done through `ORM`, but if performance is an issue, it can be combined with raw `SQL`.

The choice to use stored procedures and views was dismissed at one point, in favor of putting the `SQL` files directly the `.NET` projects, under a sub-folder named `Sql`.

![](images/sql-sub-folder.png)

The classic way of executing `SQL` in `.NET` would be to use `System.Data.SqlClient`. But instead, the `SqlExecutor API` might be used.

A version of it is available on [`JJs-Pre-Release-Package-Feed`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.SqlClient).

With an `API` like that, we can execute `SQL` command in a strongly-typed way, often with only a single code line.

The first choice of doing it might be to make the `SQL` file embedded resources:

![](images/sql-as-embedded-resource.png)

This makes the `SQL` be deployed together with your `DLL` or `EXE`, because compiles the `SQL` file right into the assembly.

The `SQL` may look as follows:

```sql
update Ingredient set Name = @name where ID = @id;
```

Then put an enum in the `SQL` folder in your `.NET` project:

![](images/sql-enum.png)

Add `enum` members that exactly correspond to the file names of the `sql` files:

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

Then you can call a method that executes the `SQL`:

```cs
sqlExecutor.ExecuteNonQuery(SqlEnum.Ingredient_UpdateName, new { id, name });
```

The method names are similar to what you might be used to using `SqlCommand`. You pass `SQL` parameters along with the `SqlExecutor` as an anonymous type:

```cs
new { id, name }
```

The name and type of the variables `id` and `name` correspond to the parameters of the `SQL`. You do not need to use an anonymous type. You can use any object. As long as its properties correspond to the `SQL` parameters, they are applied correctly:

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

The column names in the `SQL` are *case sensitive!*

It might be an idea to let the `SQL` file names begin with the entity type name, so they stay grouped together:

![](images/sql-file-names.png)

#### With NHibernate

If you use `SqlExecutor` in combination with `NHibernate` you might want to 
use the `NHibernateSqlExecutorFactory` instead of the default `SqlExecutorFactory`:

```cs
ISession session = ...;

ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.EmbeddedResource, session);
```

This version uses an `NHibernate` `ISession`. In order for the SQL to run in the same transaction as the `SQL` that `NHibernate` executes, we make it aware of the `ISession` here.

A variation of this was implemented here: [`JJs-Pre-Release-Package-Feed`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.NHibernate).

#### Files instead of Embedded Resources

*(This feature might not be available in the JJ.Framework.)*

It may usually a good choice to include the SQL as an embedded resource, but you can also use files or literal strings.

![](images/sql-as-content-file.png)

Here is code to create the `SqlExecutor` and execute an SQL file:

```cs
ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.FileName, session);

sqlExecutor.ExecuteNonQuery(@"Sql\Ingredient\_Update.sql", new { id, name });
```

So the `SqlEnum` cannot be used here. You'd use the (relative) file path here.

#### Strings instead of Embedded Resources

*(This feature might not be available in the JJ.Framework.)*

It is not recommended to use SQL strings in your code. But it is possible all the same using the following:

```cs
ISqlExecutor sqlExecutor = NHibernateSqlExecutorFactory.CreateSqlExecutor(SqlSourceTypeEnum.String, session);

sqlExecutor.ExecuteNonQuery("update Ingredient set Name = @name where ID = @id", new { id, name });
```

In that case no SQL files have to be included in your project.

But it might make it harder to track down all the SQL of your project, optimize it and using SQL strings also circumvents another layer of protection against SQL injection attacks.

#### SQL String Concatenation

*`SQL` `string` concatenation* is sort of a no-no, because it removes a layer of protection against `SQL` injection attacks. `SqlClient` has `SqlParameters` from `.NET` to prevent unwanted insertion of scripting. `SqlExecutor` from `JJ.Framework` uses `SqlParameters` under the hood, to offer the same kind of protection. This encodes the parameters, so that they are recognized as simple types or string values rather than additional scripting.

Here is a trick to potentially prevent using string concatenation as an option: When you want to filter something conditionally, depending on a parameter being filled in or not then the following expression might be used in the `SQL` script's `where` clause

```sql
(@value is null or Value = @value)
```

But there might be exceptional cases where `SQL` string concatenation could be favorable. Reasons to do so might include:

- If you have a (complicated) `SQL` `select` statement and wish to take the `count` of it, concatenation may prevent rewritng the `SQL` statement twice, introducing a maintenance issue. Bugs would be awaiting as you'd have to change 2 SQL scripts simultaneously, to make change properly, which may easily be overlooked.
- Another case where `string` concatenation might be helpful, is an `SQL` script where you wish to include a *database name* or *schema name* not known beforehand.
- There might be other examples where SQL string concatenation might be used as an exception to the rule not to.

One variation of `SqlExecutor` included the ability to add placeholders to the `SQL` files to insert additional scripting for this purpose. *(This feature might not be available in the JJ.Framework.)* 

#### Hiding SQL behind Repositories

The `repository` pattern is used in this architecture. The pattern is roughly described [here](patterns.md#repository).

The repository pattern can be used together with `JJ.Framework.Data`, documentation [here](https://github.com/jjvanzon/JJ.Framework/tree/master/Framework/Data).

For SQL executing in cooperation with repositories using `SqlExecutor` there is a way described here [here](#sql).

Here is some pseudo-code to demonstrate how it is put together:

`SQL:`

```sql
select ID from MyEntity
where CategoryID = @categoryID
and MinStartDate >= @minStartDate
```

`C#:`

```cs
enum SqlEnum
{
    MyEntity_FilterIDs
}

class MySqlExecutor
{
    public var MyEntity_FilterIDs(int categoryID, DateTime minStartDate)
    {
        return SqlExecutor.ExecuteReader<int>(
            SqlEnum.MyEntity_FilterIDs, new { categoryID, minStartDate });
    }
}

class MyRepository : RepositoryBase
{
    public var Filter(int categoryID, DateTime minStartDate)
    {
        var ids = MySqlExecutor.MyEntity_FilterIDs(categoryID, minStartDate);

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

- Keeping all the queries of an entity together in a `repository`.
- Keeping overview of all the SQL of all the entities behind an `SqlExecutor`.
- All that data access would be hidden `repository interfaces` decoupling the persistence technology.
 
It may seem overhead all the layers, but it might add up after adding more queries for more entities, that are either SQL or ORM queries. Of couse you could skip layers, but this is how it is done in some of the `JJ` projects.

In `JJ` projects you might also find split up into separate assemblies: 

- `MyProject.Data`  
- `MyProject.Data.EntityFramework`
- `MyProject.Data.SqlClient`

Separating the general things from the technology-specific things.

#### Database Upgrade Scripts

`SQL` executed solely for database upgrading, might not be put in the main projects, but a project/folder on the side. Suggestions of how to organize database upgrading might be found [here](database-conventions.md#upgrade-scripts).

[back](.)