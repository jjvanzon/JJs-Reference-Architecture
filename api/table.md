---
title: "📜 Table of API's"
description: "A comprehensive list of API's and other tech choices inside JJ's Reference Architecture."
image: "/images/api-table.png"
keywords:
  - code
  - data
  - logic
  - presentation
  - debugging
  - testing
  - processing
  - io
  - localization
  - globalization
  - settings
  - configuration
  - logging
  - security
  - api
  - framework
  - software stack
  - c#
  - .net
  - coding
  - programming
  - software engineering
  - software development
  - software design
  - software architecture
---

<style>.wrapper { max-width: 90% }</style>

📜 Table of API's
==================

[back](.)

This article lists some of the tech used in the [`JJ`](https://github.com/jjvanzon?tab=repositories) projects.

<h2>Contents</h2>

- [Code](#code)
- [Data](#data)
- [Logic](#logic)
- [Presentation](#presentation)
- [Debugging / Testing](#debugging--testing)
- [Processing / IO](#processing--io)
- [Other](#other)


Code
----

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
    <a href="misc.html#jjframework">
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


Data
----

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


Logic
-----

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


Presentation
------------

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
    <a href="web.html#javascript--typescript">
       JavaScript</a>
  </th>
  <td>
      Used to support UI details in web. In this <a href="../index.html">architecture</a> most (UI) logic would be handled in <a href="#csharp"><code>C#</code></a> instead.
  </td>
</tr>

<tr id="typescript">
  <th>
    <a href="web.html#javascript--typescript">
       TypeScript</a>
  </th>
  <td>
      Might be preferred over <a href="#javascript"><code>JavaScript</code></a> in the future.
  </td>
</tr>

<tr>
  <th><a href="web.html#ajax">AJAX</a></th>
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
      Might be extended with one-line <a href="web.html#ajax"><code>AJAX</code></a> functions one day.
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
    <a href="web.html#htmlbegincollection">Html.BeginCollection</a>
  </th>
  <td>
      Part of <a href="#jj-framework-mvc"><code>JJ.Framework.Mvc</code></a>. Makes it possible to send tree structures over <code>HTTP</code> to the server-side <a href="#mvc"><code>MVC</code></a>.
  </td>
</tr>

<tr>
  <th>
    <a href="web.html#htmlbegincollectionitem">Html.BeginCollectionItem</a>
  </th>
  <td>
      Alternative for <a href="#jjframework"><code>JJ.Framework</code></a>'s <a href="web.html#htmlbegincollection"><code>Html.BeginCollection</code></a>.
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


Debugging / Testing
-------------------

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


Processing / IO
---------------

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
      A convenient way to map <a href="misc.html#xml"><code>XML</code></a> to (<a href="#csharp"><code>C#</code></a>) classes.<br/>
      Access <a href="misc.html#xml"><code>XML</code></a> nodes more safely, with null and uniqueness checks.
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
  <th><a href="misc.html#embedded-resources">Embedded Resources</a></th>
  <td>Embedded resources allow compiling files and content right inside a <code>DLL</code> or <code>EXE</code>.</td>
</tr>

<tr id="embedded-resource-reader">
  <th>
    <a href="https://www.nuget.org/packages/JJ.Framework.Common">
      EmbeddedResourceReader</a>
  </th>
  <td>
      Makes it a little easier to get <a href="misc.html#embedded-resources">embedded resource</a> <code>Streams</code>, <code>bytes</code> and <code>strings</code>.
  </td>
</tr>

</table>


Other
-----

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
    <a href="misc.html#configuration">
       JJ.Framework.Configuration</a>
  </th>
  <td>
      For working with complex <a href="misc.html#configuration">configuration</a> files. Easier than <code>System.Configuration</code>.
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

[back](.)
