---
title: "📦 API's Misc"
image: "/images/api-preview.png"
---

<style>.wrapper { max-width: 90% }</style>

🧱 API's Misc
==============

[back](.)

This article describes some of the technology choices in this [software architecture](../index.md).

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
  - [Html.BeginCollection](#htmlbegincollection)
  - [Html.BeginCollectionItem](#htmlbegincollectionitem)
- [Misc](#misc)
  - [JJ.Framework](#jjframework)
  - [Configuration](#configuration)
  - [OneToManyRelationship](#onetomanyrelationship)
  - [XML](#xml)
  - [Embedded Resources](#embedded-resources)

Introduction
------------

This article lists some of the tech used in the [`JJ`](https://github.com/jjvanzon?tab=repositories) projects. Most are listed out in the [table](#list-of-apis-and-other-tech) below.

Some technology is described in more detail, mostly [data](#data-1) technologies, but also [web](#web) technology.


List of API's (and other tech)
------------------------------

### Code

<table>

<tr id="visual-studio">
  <th>
    <a href="https://visualstudio.microsoft.com/#vs-section">
       Visual Studio</a>
  </th>
  <td>
      Programming environment used for the development of the software.
  </td>
</tr>

<tr id="vs-code">
  <th>
    <a href="https://visualstudio.microsoft.com/#vscode-section">
       VS Code</a>
  </th>
  <td>
      Used for <a href="#mark-down"><code>MarkDown</code></a> editing.
  </td>
</tr>

<tr id="dotnet">
  <th><a href="https://dotnet.microsoft.com/">.NET</a></th>
  <td>Framework from Microsoft that forms the foundation of the software.</td>
</tr>

<tr id="mono">
  <th><a href="https://www.mono-project.com/">Mono</a></th>
  <td>Version of <a href="#dotnet"><code>.NET</code></a> that worked for other platforms than <code>Windows</code>. Later versions of <a href="#dotnet"><code>.NET</code></a> itself work on more platforms out-of-the-box.</td>
</tr>

<tr id="unity-game-engine">
  <th><a href="https://unity.com/">Unity Game Engine</a></th>
  <td>The first tech at that time, that would cooperate for deploying the code on multiple mobile platforms. It used the <a href="#mono"><code>Mono</code></a> compiler.</td>
</tr>

<tr id="csharp">
  <th>
    <a href="https://dotnet.microsoft.com/en-us/languages/csharp">
       C#</a>
  </th>
  <td>
      Primary programming language.
  </td>
</tr>

<tr id="visual-basic">
  <th>
    <a href="https://learn.microsoft.com/en-us/dotnet/visual-basic/">
       Visual Basic</a>
  </th>
  <td>
      Some projects might still use this programming language.
  </td>
</tr>

<tr id="resharper">
  <th>
    <a href="https://www.jetbrains.com/resharper">
       ReSharper</a>
  </th>
  <td>
      Tool for code formatting, refactoring and code smells and such.
  </td>
</tr>

<tr id="git">
  <th><a href="https://git-scm.com/">Git</a></th>
  <td>Source control, revision history, version management of the code.</td>
</tr>

<tr id="git-hub">
  <th><a href="https://github.com/jjvanzon">GitHub</a></th>
  <td>Where the source code is hosted and shared.</td>
</tr>

<tr id ="git-for-windows">
  <th><a href="https://gitforwindows.org/">Git for Windows</a></th>
  <td><code>Windows</code> version of <a href="#git"><code>Git</code></a>.</td>
</tr>

<tr id="git-extensions">
  <th><a href="https://gitextensions.github.io/">Git Extensions</a></th>
  <td>User interface for <a href="#git"><code>Git</code></a> with many options.</td>
</tr>

<tr id="tortoise-git">
  <th><a href="https://tortoisegit.org/">TortoiseGit</a></th>
  <td><a href="#git"><code>Git</code></a> user interface that shows state in <code>File Explorer</code>.</td>
</tr>

<tr id="azure-dev-ops">
  <th>
  <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">
      Azure DevOps</a>
  </th>
  <td>
      <a href="https://dev.azure.com/jjvanzon/JJs-Software/_build">Build pipeline</a>, <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">Pre-Release-Package-Feed</a>, original <a href="https://dev.azure.com/jjvanzon/JJs-Software/_workitems/">planning boards</a>, may host a <a href="https://dev.azure.com/jjvanzon/JJs-Software/_git/"> repository</a> not migrated to <a href="#git-hub"><code>GitHub</code></a>.
  </td>
</tr>

<tr id="github-issues">
  <th>
    <a href="https://docs.github.com/en/issues/tracking-your-work-with-issues/quickstart">
       GitHub Issues</a></th>
  <td>
      Gradually used more for planning.
  </td>
</tr>

<tr id="mark-down">
  <th>
    <a href="https://www.markdownguide.org/cheat-sheet">
       MarkDown</a></th>
  <td>
      Lightweight alternative for <code>HTML</code>. Used for documentation. Compatible with various web tech.
  </td>
</tr>

<tr>
  <th>
    <a href="#jjframework">
       JJ.Framework</a></th>
  <td>
      In-house programmed extensions to the <a href="#dotnet"><code>.NET Framework</code></a>.<br/>
      <a href="https://github.com/jjvanzon/JJ.Framework">GitHub</a> / 
      <a href="https://www.nuget.org/profiles/jjvanzon">NuGet</a> /
      <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">JJs-Pre-Release-Package-Feed</a>
  </td>
</tr>

<tr id="jj-framework-conversion">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Conversion">
       JJ.Framework.Conversion</a>
  </th>
  <td>
      Makes it easier to convert simple types.
  </td>
</tr>

<tr id="jj-framework-reflection">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Reflection">
       JJ.Framework.Reflection</a>
  </th>
  <td>
      Helps and speeds up accessing code structure elements through <a href="https://learn.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/reflection">reflection</a> and <a href="https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions">lambda expressions</a>.
  </td>
</tr>


<tr id="jj-canonical">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed">
       JJ.Framework.Canonical</a>
  </th>
  <td>
      Types to share data between <code>JJ</code> projects. Small scale <a href="../service-oriented-architecture.html#canonical-model"><code>Canonical</code></a> model for generic handling of <code>Successful</code> flags, <code>Validation Messages</code> and combinations of <code>IDs</code> and <code>Names</code>.
  </td>
</tr>

</table>

### Data

<table>

<tr id="sql-server">
  <th>
    <a href="https://www.microsoft.com/en-us/sql-server">
       SQL Server</a>
  </th>
  <td>
      Primary data store technology for relational databases.
  </td>
</tr>

<tr>
  <th><a href="orm.html">ORM</a></th>
  <td>Hides most <a href="sql.html"><code>SQL</code></a>, exposing an object graph, to focus on the logic, instead of on the data storage.</td>
</tr>

<tr>
  <th><a href="sql.html">SQL</a></th>
  <td>For performance reasons <a href="sql.html"><code>SQL</code></a> is hand-programmed incidentally, combined with <a href="orm.html"><code>ORM</code></a>.</td>
</tr>

<tr>
  <th>
    <a href="orm.html#nhibernate">NHibernate</a>
  </th>
  <td>
      A type of <a href="orm.html"><code>ORM</code></a>. Chosen in several <code>JJ</code> project because an employer also so happened to use it.
  </td>
</tr>

<tr id="query-over">
  <th>
    <a href="https://nhibernate.info/doc/nhibernate-reference/queryqueryover.html">
       QueryOver</a>
  </th>
  <td>
      A strongly-typed query language like <a href="#linq"><code>LINQ</code></a>, but then the <a href="orm.html#nhibernate"><code>NHibernate</code></a> version.
  </td>
</tr>

<tr id="fluent-nhibernate">
  <th>
    <a href="https://www.nuget.org/packages/FluentNHibernate">
       FluentNHibernate</a>
  </th>
  <td>
      A way to define <a href="orm.html"><code>ORM</code></a> mappings with fluent notation.
  </td>
</tr>

<tr>
  <th>
    <a href="orm.html#entity-framework">Entity Framework</a>
  </th>
  <td>
      A type of <a href="orm.html"><code>ORM</code></a>. Chosen less in the <code>JJ</code> projects, because of more experience with <a href="orm.html#nhibernate"><code>NHibernate</code></a>. Worth considering though.
  </td>
</tr>

<tr id="linq">
  <th>
      <a href="https://learn.microsoft.com/en-us/dotnet/csharp/linq/write-linq-queries">
         LINQ</a>
  </th>
  <td>
      A query language usable in <a href="#csharp"><code>C#</code></a>. Can query several types of data store, but commonly used for in-memory collections.
  </td>
</tr>

<tr id="jj-framework-collections">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Collections">
       JJ.Framework.Collections</a>
  </th>
  <td><a href="#linq"><code>LINQ</code></a> extensions from the <a href="#jjframework"><code>JJ.Framework</code></a>.</td>
</tr>

<tr id="jj-framework-data">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data">
       JJ.Framework.Data</a>
  </th>
  <td>
      Helps hide data access behind abstractions. It does not expose whether it is <a href="#sql-server"><code>SQL Server</code></a>, <a href="sql.html"><code>SQL</code></a>, <a href="orm.html"><code>ORM</code></a>, <a href="orm.html#nhibernate"><code>NHibernate</code></a> or <a href="orm.html#entity-framework"><code>Entity Framework</code></a>. It would just offer abstracted convenient methods instead. For more information see <a href="https://github.com/jjvanzon/JJ.Framework/tree/master/Framework/Data">documentation</a>.
  </td>
</tr>

<tr id="jj-framework-data-entity-framework">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.EntityFramework">
       JJ.Framework.Data.EntityFramework</a>
  </th>
  <td>
      <a href="orm.html#entity-framework"><code>Entity Framework</code></a> extension to work with <code>interfaces</code> from <a href="#jj-framework-data"><code>JJ.Framework.Data</code></a>.
  </td>
</tr>

<tr id="jj-framework-data-memory">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.Memory">
       JJ.Framework.Data.Memory</a>
  </th>
  <td>
      Extension to the <code>interfaces</code> specified in <a href="#jj-framework-data"><code>JJ.Framework.Data</code></a> that allow working with <em>in-memory</em> data for instance to <a href="../patterns/other.html#mock">mock</a> a data store.
  </td>
</tr>

<tr id="jj-framework-data-nhibernate">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.NHibernate">
       JJ.Framework.Data.NHibernate</a>
  </th>
  <td>
      <a href="orm.html#nhibernate"><code>NHibernate</code></a> extension to work with <code>interfaces</code> from <a href="#jj-framework-data"><code>JJ.Framework.Data</code></a>.
  </td>
</tr>

<tr id="sql-executor">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.SqlClient">JJ.Framework.Data.SqlClient</a>
  </th>
  <td>
      Also know as <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.SqlClient"><strong><code>SqlExecutor</code></strong></a>. Work more easily with <a href="sql.html"><code>SqlClient</code></a> with less code.
  </td>
</tr>

<tr id="jj-framework-data-xml">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.Xml">
       JJ.Framework.Data.Xml</a>
  </th>
  <td>
      An extension to <a href="#jj-framework-data"><code>JJ.Framework.Data</code></a> for storing in <a href="#xml"><code>XML</code></a> files. <code>System.Xml</code> is used internally.
  </td>
</tr>

<tr id="jj-framework-data-xml-linq">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data.Xml.Linq">
       JJ.Framework.Data.Xml.Linq</a>
  </th>
  <td>
      Additional feature for <a href="#jj-framework-data"><code>JJ.Framework.Data</code></a> that stores data in <a href="#xml"><code>XML</code></a> files. <code>System.Xml.Linq</code> is used internally.
  </td>
</tr>

</table>

### Logic

<table>

<tr id="jj-framework-business">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Business">
       JJ.Framework.Business</a>
  </th>
  <td>
      Types for supporting a business layer and/or <code>API</code>. <a href="#onetomanyrelationship">Bidirectional relationship synchronization</a>. <code>Result</code> types to pass data, succes flags and (<a href="../patterns/business-logic.html#validators">validation</a>) messages. <a href="../patterns/business-logic.html#sideeffects"><code>ISideEffect</code></a>: Used for some polymorphism between small pieces of business logic that go off as a result of data modification.
  </td>
</tr>

<tr id="jj-framework-validation">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Validation">
       JJ.Framework.Validation</a>
  </th>
  <td>
      A nice fluent notation for <a href="../patterns/business-logic.html#validators">validations</a>.
  </td>
</tr>

<tr id="jj-framework-mathematics">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Mathematics">
       JJ.Framework.Mathematics</a>
  </th>
  <td>
      Helpers for math things.
  </td>
</tr>

</table>

### Presentation

<h4>General</h4>

<table>

<tr id="pager-view-model-factory">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Presentation">
       PagerViewModelFactory</a>
  </th>
  <td>
      Constructs a <code>PagerViewModel</code> with properties like
      <code>CanGoToFirstPage</code>, <code>CanGoToPreviousPage</code>, <code>CanGoToNextPage</code>, <code>CanGoToLastPage</code>.
  </td>
</tr>

</table>

<h4>Web</h4>

<table>

<tr id="iis">
  <th>
    <a href="https://www.iis.net/">IIS</a>
  </th>
  <td>
      Or <a href="https://www.iis.net/"><code>Internet Information Services.</code></a> For hosting web sites. 
      Some <a href="#visual-studio"><code>Visual Studio</code></a> projects like to use it upon load.
  </td>
</tr>

<tr id="mvc">
  <th>
    <a href="https://dotnet.microsoft.com/en-us/apps/aspnet/mvc">
       MVC</a>
    </th>
  <td>
      Or <a href="https://dotnet.microsoft.com/en-us/apps/aspnet/mvc"><code>Microsoft ASP.NET MVC Framework.</code></a> Web development tech. Code runs mostly server side.
  </td>
</tr>

<tr id="razor">
  <th>
    <a href="https://learn.microsoft.com/en-us/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c">
       Razor</a>
  </th>
  <td>
      A view renderer for web. Terse syntax, combining <a href="#csharp"><code>C#</code></a> and <code>HTML</code> almost seamlessly.
  </td>
</tr>

<tr id="javascript">
  <th>
    <a href="#javascript--typescript">
       JavaScript</a>
  </th>
  <td>
      Used to support UI details in web. In this <a href="../index.html">architecture</a> most (UI) logic would be handled in <a href="#csharp"><code>C#</code></a> instead.
  </td>
</tr>

<tr id="typescript">
  <th>
    <a href="#javascript--typescript">
       TypeScript</a>
  </th>
  <td>
      Might be preferred over <a href="#javascript"><code>JavaScript</code></a> in the future.
  </td>
</tr>

<tr>
  <th><a href="#ajax">AJAX</a></th>
  <td>For retrieving / posting back parts of pages to the server and back.</td>
</tr>

<tr id="jquery">
  <th>
    <a href="https://jquery.com/">jQuery</a>
  </th>
  <td>
      Used to support UI details in web. Can make some <a href="#javascript"><code>JavaScript</code></a> shorter.</td>
</tr>

<tr id="jj-framework-javascript">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.JavaScript">
       JJ.Framework.JavaScript</a>
  </th>
  <td>
      Used to support UI details in web.
      Remembering scroll position, cookie functions, URL parsing.
      Might be extended with one-line <a href="#ajax"><code>AJAX</code></a> functions one day.
  </td>
</tr>

<tr id="jj-framework-mvc">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Mvc">
       JJ.Framework.Mvc</a>
  </th>
  <td>
      Extensions to <a href="#mvc"><code>MVC</code></a> for web development.
  </td>
</tr>

<tr>
  <th>
    <a href="#htmlbegincollection">Html.BeginCollection</a>
  </th>
  <td>
      Part of <a href="#jj-framework-mvc"><code>JJ.Framework.Mvc</code></a>. Makes it possible to send tree structures over <code>HTTP</code> to the server-side <a href="#mvc"><code>MVC</code></a>.
  </td>
</tr>

<tr>
  <th>
    <a href="#htmlbegincollectionitem">Html.BeginCollectionItem</a>
  </th>
  <td>
      Alternative for <a href="#jjframework"><code>JJ.Framework</code></a>'s <a href="#htmlbegincollection"><code>Html.BeginCollection</code></a>.
      Allows sending a collection over <code>HTTP</code> to server-side <a href="#mvc"><code>MVC</code></a>, but not trees.
  </td>
</tr>

</table>

<h4>Win</h4>

<table>

<tr id="winforms">
  <th>
    <a href="https://learn.microsoft.com/en-us/dotnet/desktop/winforms/get-started/create-app-visual-studio">
       WinForms</a>
  </th>
  <td>
      Used in some projects: in small <a href="../aspects.html#utilities">utilities</a> and <a href="https://github.com/jjvanzon/JJ.Synthesizer"><code>JJ.Synthesizer</code></a> uses it as the top-most layer.
  </td>
</tr>

<tr id="simple-process-form">
  <th><a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.WinForms">SimpleProcessForm</a></th>
  <td>Part of <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.WinForms"><code>JJ.Framework.WinForms</code></a>. A base user interface for a <a href="../aspects.html#utilities">utility</a> that runs a process.</td>
</tr>

<tr id="jj-framework-vectorgraphics">
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

<tr id="mstest">
  <th>
    <a href="https://www.nuget.org/packages/MSTest.TestFramework">
       MSTest</a>
  </th>
  <td>
      For automated / unit testing. Seems a deprecated framework.
      Might upgrade, but it ain't on the top of the list.
  </td>
</tr>

<tr id="jj-framework-testing">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Testing">
       JJ.Framework.Testing</a>
  </th>
  <td>
      Extends the <code><a href="#mstest">Assert</a> class</code>, but automatically includes the tested expression in the error messages.
  </td>
</tr>

<tr id="debugger-displays">
  <th>
    <a href="../patterns/other.html#debuggerdisplays">
       DebuggerDisplays</a>
  </th>
  <td>
      A technique to quickly display helpful info in the watch screen of a programming environment.
  </td>
</tr>

<tr id="jj-framework-exceptions">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Exceptions">
       JJ.Framework.Exceptions</a>
  </th>
  <td>
      Contains <code>Exception classes</code> for basic errors.
      Clear concise error messages,
      that include tested expressions and tested values.
  </td>
</tr>

<tr id="accessor">
  <th>
    <a href="../patterns/other.html#accessor">Accessor</a>
  </th>
  <td>
      For accessing the internals of <code>types</code> for instance for testing purposes.
  </td>
</tr>

</table>

### Processing / IO

<table>

<tr id="jj-framework-text">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Text">
       JJ.Framework.Text</a>
  </th>
  <td>
      Basic helpers for working with text.
  </td>
</tr>

<tr id="jj-framework-io">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.IO">
       JJ.Framework.IO</a>
  </th>
  <td>
      Contains various file functions, functions for working with streams and working with <code>CSV's</code>.
  </td>
</tr>

<tr id="jj-framework-htmltoxml">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.HtmlToXml">
       JJ.Framework.HtmlToXml</a>
  </th>
  <td>
      <code>HtmlToXmlConverter class</code> that steals from <a href="https://www.nuget.org/packages/SgmlReaderOld/"><code>SgmlReader</code></a>. It does what its name implies.
  </td>
</tr>

<tr id="jj-framework-xml">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Xml">
       JJ.Framework.Xml</a>
  </th>
  <td>
      A convenient way to map <a href="#xml"><code>XML</code></a> to (<a href="#csharp"><code>C#</code></a>) classes.<br/>
      Access <a href="#xml"><code>XML</code></a> nodes more safely, with null and uniqueness checks.
  </td>
</tr>

<tr id="jj-framework-xml-linq">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Xml.Linq">
       JJ.Framework.Xml.Linq</a>
  </th>
  <td>
      <a href="#jj-framework-xml">"</a>
  </td>
</tr>

<tr>
  <th><a href="#embedded-resources">Embedded Resources</a></th>
  <td>Embedded resources allow compiling files and content right inside a <code>DLL</code> or <code>EXE</code>.</td>
</tr>

<tr id="embedded-resource-reader">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Common">
      EmbeddedResourceReader</a>
  </th>
  <td>
      Makes it a little easier to get <a href="#embedded-resources">embedded resource</a> <code>Streams</code>, <code>bytes</code> and <code>strings</code>.
  </td>
</tr>

</table>

### Other

<h4>Localization</h4>

<table>

<tr id="resource-strings">
  <th><a href="../patterns/resource-strings.html">Resource Strings</a></th>
  <td>For localization, <a href="../patterns/resource-strings.html"><code>resx</code></a> files can be used in <a href="#visual-studio"><code>Visual Studio</code></a>.</td>
</tr>

<tr id="jj-framework-resourcestrings">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.ResourceStrings">
       JJ.Framework.ResourceStrings</a>
  </th>
  <td>
      Reusable button texts and such in multiple languages. For now supports Dutch, US English and some broken Polish.
  </td>
</tr>

<tr>
  <th><a href="../aspects.html#localization">Localization</a></th>
  <td>More ideas about <a href="../aspects.html#localization">localization</a>.</td>
</tr>

</table>

<h4>Configuration</h4>

<table>

<tr>
  <th>
    <a href="#configuration">
       JJ.Framework.Configuration</a>
  </th>
  <td>
      For working with complex <a href="#configuration">configuration</a> files. Easier than <code>System.Configuration</code>.
  </td>
</tr>

<tr>
  <th><a href="../aspects.html#configuration">Configuration</a></th>
  <td>More info about <a href="../aspects.html#configuration">configuration</a>.</td>
</tr>

</table>

<h4>Security</h4>

<table>

<tr id="jj-framework-security">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Security">
       JJ.Framework.Security</a>
  </th>
  <td>
      A generic interfacing for <a href="../aspects.html#security">authenticating a user</a> and yet to be tested hashed salted password authentication.
  </td>
</tr>

<tr>
  <th>
    <a href="../aspects.html#security">Security</a>
  </th>
  <td>
      If more might be needed security-wise, it may be hidden behind <a href="../aspects.html#security">generic interfaces</a>, abstracting the security system.
  </td>
</tr>


</table>

<h4>Logging</h4>

<table>

<tr id="jj-framework-logging">
  <th>
    <a href="https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Logging">
       JJ.Framework.Logging</a>
  </th>
  <td>
      For now contains not much more than the <code>ExceptionHelper</code> class, which for instance converts <code>Exception</code> information to a <code>string</code>.
  </td>
</tr>

<tr>
  <th>
    <a href="../aspects.html#logging">Logging</a>
  </th>
  <td>
      More info how <a href="#jj-framework-logging"><code>JJ.Framework.Logging</code></a> might be extended to contain more code for logging.
  </td>
</tr>

</table>


More Elaborate Descriptions
---------------------------

For some of these things you can find more elaborate descriptions below: mostly about [data](#data-1) store technologies, but also some about [web](#web) technology and [others](#misc).


Web
---

### AJAX

`AJAX` is a way to load part of a web page, so the whole page does not have to be refreshed. This may make the user interaction smoother, than reloading the entire page every time.

For `AJAX'ing` such partial web content, our team programmed [wrapper](../patterns/other.md#wrapper) functions in [`JavaScript`](#javascript), around calls to [`jQuery`](#jquery), so we could `AJAX` with a single code line and handle both partial loads and full reloads the same way. It saved quite a few lines of [`JavaScript`](#javascript) code.

Our strategy was to prefer full loads, so we could keep most logic in the [`C#`](#csharp) realm. This before resorting to `AJAX` calls. See [Full Load – Partial Load – Cient-Native Code](../patterns/presentation.md#full-load--partial-load--client-native-code).

### JavaScript / TypeScript

[`JavaScript`](https://www.javascript.com/) is a programming language with a wide range of applications. Originally it was run in web browsers to optimize the user experience.

[`JavaScript`](https://www.javascript.com/) was less preferred as an architectural choice. [`JavaScript's`](https://www.javascript.com/) weak type system played a role. The strange behavior and trickiness in [`JavaScript`](https://www.javascript.com/) (part due to this weak typing) gave it less appeal.

For web, other technology was preferred in this [architecture](../index.md): The idea behind [`MVC`](#mvc) was logic on the server-side. [`Views`](../patterns/presentation.md#views) were in [`Razor`](#razor). Best to keep most logic [`C#`](#csharp) was the idea.

[`JavaScript`](https://www.javascript.com/) would easily get bloated, getting out of hand from a maintainability perspective, was the prevailing opinion. You could refactor [`C#`](#csharp) code, upon which lots of the [`JavaScript`](https://www.javascript.com/) might break unexpectedly, with an error message tucked away in some console window, instead of right in your face when compiling.

[`TypeScript`](https://www.typescriptlang.org/) may have saved the day to cover for the weak typing from [`JavaScript`](https://www.javascript.com/). But we hadn't tried that yet.

But still: logic in one place in one language ([`C#`](#csharp)) felt so nice. I guess the love for [`C#`](#csharp) was strong.

The idea was that a full page load was 1<sup>st</sup> choice, [`AJAX'ing`](#ajax) the 2<sup>nd</sup> choice, and last in line [`JavaScript`](https://www.javascript.com/) *only* to support the user interaction. No business logic. See also: [Full Load – Partial Load – Cient-Native Code](../patterns/presentation.md#full-load--partial-load--client-native-code).

For this last-resort [`JavaScript`](https://www.javascript.com/) we used [`jQuery`](#jquery) and some home-programmed [`JavaScript`](https://www.javascript.com/) libraries: [`JJ.Framework.JavaScript`](#jj-framework-javascript) which had some merit, but may have been superseded by newer tech by now.

I realize [`JavaScript`](https://www.javascript.com/) is popular with a lot of people and that this is a powerful force. I don't know how my opinion would change, if I would try a newer [`JavaScript`](https://www.javascript.com/) version, [`TypeScript`](https://www.typescriptlang.org/), newer tech and libraries. My heart says I'd rather stick with [`C#`](#csharp) though.

### Html.BeginCollection

In [`MVC`](#mvc) it is not so straightforward to [`HTTP` a tree structure in postdata](../aspects.md#postdata-over-http).

[`JJ.Framework.Mvc`](#jj-framework-mvc) makes that easier, by offering an `HtmlHelper` extensions: [`Html.BeginCollection`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Mvc). Using that `API` you can send a [`ViewModel`](../patterns/viewmodels.md) with arbitrary nestings and collections over the line. It would be restored as a [`ViewModel`](../patterns/viewmodels.md) at the server side.

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

In [`MVC`](#mvc) it is not so apparent how to [send a collection as `HTTP postdata`](../aspects.md#postdata-over-http).

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

To send trees and arbitrary nestings over `HTTP postdata`, consider using [`Html.BeginCollection`](#htmlbegincollection) from [`JJ.Framework.Mvc`](#jj-framework-mvc).


Misc
----

### JJ.Framework

[`JJ.Framework`](https://www.nuget.org/profiles/jjvanzon) are nuts, bolts and screws for software development. There were things missing in [`.NET`](#dotnet), so we programmed our own. These extensions to [`.NET`](#dotnet) are compact and reusable. They can be found on [NuGet](https://www.nuget.org/profiles/jjvanzon). The lesser-tested ones on [JJs-Pre-Release-Package-Feed](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed). You can read more information of it on the [GitHub](https://github.com/jjvanzon/JJ.Framework) repository.

They were made in the spirit of in-house developing small extensions and hiding platform-specific details behind [generalized interfaces](../layers.md#loosely-coupled). They are sort part of the [software architecture](../index.md) described here.

### Configuration

[`.NET`](#dotnet) code can use configuration files, often named `App.config` or `Web.config`.

To access these `configs` we might use [`JJ.Framework.Configuration`](https://www.nuget.org/packages/JJ.Framework.Configuration) quite a bit more easily than using [`.NET's`](#dotnet) `System.Configuration` directly.

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

This can be achieved through the use of classes such as [`ManyToOneRelationship`](https://www.nuget.org/packages/JJ.Framework.Business#onetomanyrelationship-manytoonerelationship) and [`OneToManyRelationship`](https://www.nuget.org/packages/JJ.Framework.Business#onetomanyrelationship-manytoonerelationship) from the [`JJ.Framework.Business`](#jj-framework-business) package, which can be used in various models: [rich](../patterns/other.md#rich-models), [entity](../patterns/data-access.md#entities), `API` or otherwise.

There may be other options available. [`NHibernate`](orm.md#nhibernate) does not appear to do it for us automatically. However [`Entity Framework`](orm.md#entity-framework) might do this synchronization automatically. The [`LinkTo`](../patterns/business-logic.md#linkto) pattern can also be used. Or hand-writing the syncing in-place. 

### XML

`XML` is a file format for storing and transmitting data. When working with `API's` for `XML` there are a few options to consider.

In most cases, it is recommended to use `XElement` (LINQ to XML) instead of `XmlDocument`. However, a useful feature of `XmlDocument` is that it supports [`XPath`](https://www.w3schools.com/xml/xpath_intro.asp).

To handle nullability and uniqueness more gracefully, it is suggested to use `XmlHelper` methods from [`JJ.Framework.Xml`](#jj-framework-xml) or [`JJ.Framework.Xml.Linq`](#jj-framework-xml-linq) over using other `API's` directly.

For converting `XML` to an object graph, [`XmlToObjectConverter`](https://www.nuget.org/packages/JJ.Framework.Xml#xmltoobjectconverter) and [`ObjectToXmlConverter`](https://www.nuget.org/packages/JJ.Framework.Xml#xmltoobjectconverter) from [`JJ.Framework.Xml`](#jj-framework-xml) and [`JJ.Framework.Xml.Linq`](#jj-framework-data-xml-linq) might be useful. They can offer simpler solutions than other `API's`.

### Embedded Resources

*Embedded resources* might be handy, to prevent including loose files with a deployment. Instead they are compiled right into your program files' `EXE` or `DLL`. This also protects those resources a bit better against modifications.

To include a file as an embedded resource, you could set the following property in [`Visual Studio`](#visual-studio):

![](../images/sql-as-embedded-resource.png)

[`JJ.Framework.Common`](https://www.nuget.org/packages/JJ.Framework.Common) contains a [`Helper`](../patterns/other.md#helper) `class` [`EmbeddedResourceReader`](#embedded-resource-reader). It makes it a little bit easier to access those resources from your code:

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