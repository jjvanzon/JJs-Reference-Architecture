JJ's Reference Architecture
===========================

Code Style
----------

<h3>Contents</h3>

- [Code Style](#code-style)
    - [Introduction](#introduction)
    - [Casing, Punctuation and Spacing](#casing-punctuation-and-spacing)
    - [Trivial Rules](#trivial-rules)
    - [Miscellaneous Rules](#miscellaneous-rules)
        - [Namespace Tips](#namespace-tips)
    - [Member Order](#member-order)
    - [Naming](#naming)
        - [Boolean Names](#boolean-names)
        - [Class Names](#class-names)
        - [Collection Names](#collection-names)
        - [DateTime Names](#datetime-names)
        - [Enum Names](#enum-names)
        - [Event Names / Delegate Names](#event-names--delegate-names)
        - [Method Names](#method-names)
        - [File-Related Variable Names](#file-related-variable-names)
        - [Miscellaneous Names](#miscellaneous-names)

### Introduction

This section lists trivial coding rules, that should be followed throughout the code.

Coding standards mostly conform to the Microsoft standard described in the following documents:

<http://msdn.microsoft.com/en-us/library/vstudio/ff926074.aspx>

<http://msdn.microsoft.com/en-us/library/aa260844%28v=vs.60%29.aspx>

Use Resharper. Seriously. Finetune it to automatically check your coding style. Use it to keep code clean as write code or change existing code.

### Casing, Punctuation and Spacing

| Rule                                                                | Examples                       |
|---------------------------------------------------------------------|--------------------------------|
| Properties, methods, class names and events are in pascal case.     | `MyProperty` `MyMethod`
| Local variables and parameters are in camel case.                   | `myLocalVariable` `myParameter`
| Fields are in camel case and start with underscore.                 | `_myField`
| Constants in capitals with underscores in between words.            | `MY_CONSTANT`
| No prefixes, such as `strName`.                                     |
| Avoid abbreviations.                                                |
| For long identifiers, use underscores to separate ‘the pieces’.     | `Sine_OperatorCalculator_VarFrequency_WithPhaseTracking`
| Type arguments start with the letter `T` or are just the letter `T` | `T` `TEntity` `TViewModel`
| Abbreviations of 2 letters with capitals.                           | `ID`
| Abbreviations of 3 letters or more in pascal case.                  | `Mvc`
| Start interface names with `I`.                                     | `IMyInterface`
| Partial view names in MVC should begin with an underscore           | `_MyPartialView`


| Rule | Not Recommended | Recommended |
|------|-----------------|------------ |
| Keep Visual Studio’s autoformatting enabled and set to its defaults. |
| No extra enters between braces.                | `  `}<br><br>} | `  `}<br>}
| Put enters between switch cases.               | switch (x)<br>{<br>case 1:<br>break;<br>case 2:<br>break;<br>}<br> | switch (x)<br>{<br>case 1:<br>break;<br><br>case 2:<br>break;<br>}<br>
| No braces for single-line if-statements.       | if (condition) { Bla(); }<br> | if (condition) Bla();
| Loops always on multiple lines.                | foreach (var x in list) { Bla(); } | foreach (var x in list)<br>{ <br>Bla();<br>}<br>
| Use braces for multi-line if’s and loops.      | foreach (var x in list)<br>Bla();<br><br>if (condition)<br>Bla(); | foreach (var x in list)<br>{<br>Bla();<br>}<br><br>if (condition)<br>{<br>Bla();<br>}<br>
| Put enters between methods.                    | void Bla()<br>{<br>}<br>void Bla2()<br>{<br>}<br> | void Bla()<br>{<br>}<br><br>void Bla2()<br>{<br>}<br>
| Each property at least its own line.           | int A { get; set; } int B { get; set; } | int A { get; set; } <br>int B { get; set; }<br><br>int C<br>{<br>get { ... }<br>set { ... }<br>}<br><br>int D<br>{<br>get <br>{<br>... <br>}<br>set <br>{ <br>...<br>}<br>}<br>
| Put enters inside methods between ‘pieces that do something’ (that is vague, but that is the rule). | void Bla()<br>{<br>var x = new X();<br>x.A = 10;<br>var y = new Y();<br>y.B = 20;<br>y.X = x;<br>Bla2(x, y);<br>} | void Bla()<br>{<br>var x = new X();<br>x.A = 10;<br><br>var y = new Y();<br>y.B = 20;<br>y.X = x;<br><br>Bla2(x, y);<br>}<br>
| Each variable declaration on its own line.     | int i, j; | int i;<br>int j;<br>
| Avoid ‘tabular form’. It should only rarely be used. This tabular form will often be undone by auto-formatting. It is non-standard, so it is better to get your eyes used to non-tabular form. | public int    ID       { get; set; }<br>public bool   IsActive { get; set; }<br>public string Text     { get; set; }<br>public string Answer   { get; set; }<br>public bool   IsManual { get; set; } | public int ID { get; set; }<br>public bool IsActive { get; set; }<br>public string Text { get; set; }<br>public string Answer { get; set; }<br>public bool IsManual { get; set; }
| Align the elements of linq queries as follows: | var arr = coll.Where(x => x...).<br>`   `OrderBy(x => x...).ToArray() | var arr = coll.Where(x => x...)<br>`              `.OrderBy(x => x...)<br>`              `.ToArray()<br>
| Use proper indentation                         | <TODO: Example.> | <TODO: Example.>
| Generic constraints on next line.<br>(So they stand out)| class MyGenericClass<T> where T: MyInterface<br>{<br>}<br> | class MyGenericClass<T><br>where T: MyInterface<br>{<br>}
| For one-liners, but generic constraints on same line instead.     | interface IMyInterface<br>{<br>void MyMethod(T param) <br>where T : ISomething<br>} | interface IMyInterface<br>{<br>void MyMethod(T param) where T : ISomething<br>}

### Trivial Rules

| Rule | Not Recommended | Recommended |
|------|-----------------|-------------|
| Give each class (or enum) its own file (except nested classes).
| Keep members private as much as possible. | | private void Bla()<br>{<br>}<br>
| Keep types internal as much as possible. | | internal class MyClass<br>{<br>}<br>
| Use explicit access modifiers (except for interface members). | int Bla() { ... } | __public__ int Bla() { ... }
| No public fields. Use properties instead. | public int X; | __public__ int X __{ get; set; }__<br>
| Put nested classes at the top of the parent class’s code. | internal class A<br>{<br>public int X { get; set; }<br><br>private class B<br>{<br>}<br>}<br> | internal class A<br>{<br>private class B<br>{<br>}<br><br>public int X { get; set; }<br>}<br>
| Avoid getting information by catching an exception. Prefer getting your information without using exception handling. | bool FileExists(string path)<br>{<br>try<br>{<br>File.Open(path, ...);<br>return true;<br>}<br>catch (IOException)<br>{<br>return false;<br>}<br>}<br> | bool FileExists(string path)<br>{<br>return File.Exists(path);<br>}<br>
| Do not use type arguments that can be inferred. | References__<Child>__(x => x.Child)<br> | References(x => x.Child)
| Use interface types as variable types when they are present. | __List__<int> list = new List<int>; | __IList__<int> list = new List<int>;
| Prefer ToArray over ToList. | IList<int> collection = x.__ToList__() | IList<int> collection = x.__ToArray__()
| Use object initializers for readability. | var x = new X();<br>x.A = 10;<br>x.B = 20; | var x = new X<br>{<br>A = 10,<br>B = 20<br>}<br>
| Put comment for members in <summary> tags. | // This is the x-coordinate.<br>int X { get; set; } | __/// <summary>__<br>__///__ This is the x coordinate.<br>__/// </summary>__<br>int X { get; set; }<br>
| Comment in English. | // Dit is een ding. | // This is a thing.<br>
| Do not write comment that does not add information | __// This is x__<br>int x;<br> | int x;<br>
| Avoid compiler directives<br><br>Do not use them unless you absolutely cannot run the code on a platform unless you exclude a piece of code. Otherwise use a boolean variable, a configuration setting, different concrete implementations of classes or, anything.  | __#if FEATURE\_X\_ENABLED__<br>__// ...__<br>__#endif__ | if (config.FeatureXEnabled)<br>{<br>// ...<br>}<br>
| An internal class should not have internal members.<br><br>The members are automatically internal if the class is internal. If you have to make the class public, you do not want to have to correct the access modifiers of the methods. | __internal class A__<br>__{__<br>__internal void B__<br>__{__<br>__}__<br>__}__<br> | internal class A<br>{<br>public void B<br>{<br>}<br>}<br>
| Default switch case at the bottom. | __switch (x)__<br>__{__<br>__default:__<br>__break;__<br><br>__case 0:__<br>__break;__<br><br>__case 1:__<br>__break;__<br>__}__ | switch (x)<br>{<br>case 0:<br>break;<br><br>case 1:<br>break;<br><br>__default:__<br>__break;__<br>}
| Prefer .Value and .HasValue for nullable types. | __int? number;__<br>__if (number != null)<br>{__<br>__string message = String.Format(__<br>__"Number = {0}", number);__<br>__}__ | int? number;<br>if (number.HasValue)<br>{<br>string message = String.Format(<br>"Number = {0}", number.Value);<br>}
| Do not leave unused (outcommented) around. If needed, move it to an Archive folder, or Outtakes.txt, but do not bug your coworkers with out-of-use junk lying around.
| it is appreciated when a file stream is opened specifying all three aspects FileMode, FileAccess and FileShare explicitly with the most logical and most limiting values appropriate for the particular situation.

### Miscellaneous Rules

| Description | Not Recommended | Recommended |
|-------------|-----------------|-------------|
| Test class names end with ‘Tests’. | [TestClass]<br>public class **Tests\_**Validator()<br>{<br>}<br> | [TestClass]<br>public class Validator**Tests**()<br>{<br>}<br>
| Test method names start with Test\_ and use a lot of underscores in the name because they will be long, because they will be very specific. | [TestMethod]<br>public void **Test** ()<br>{ <br>...<br>} | [TestMethod]<br>public void __Test\_Validator\_NotNullOrEmpty\_NotValid__()<br>{ <br>...<br>}<br>
| var should be avoided. The variable type should be visible in the code line instead of ‘var’. Exceptions are: | __var__ x = y.X;
| - An anonymous type is used. | __X__ q = from x in list select __new { A = x.A }__; | __var__ q = from x in list select __new { A = x.A }__;
| - The code line is a ‘new’ statement. | __X__ x = new __X__() | __var__ x = new __X__()
| - The code line is a direct cast. | __X__ x = (__X__)y; | __var__ x = (__X__)y;
| - The code line is WAAAY too long and unreadable without ‘var’. | foreach (__KeyValuePair<Canonical.ValidationMessage,  Tuple<NonPhysicalOrderProductList, Guid>>__ entry in dictionary) | foreach (__var__ entry in dictionary)
| - Use var in your __view__ code. | <% foreach (__OrderViewModel__ order in Model.Orders) %> | <% foreach (__var__ order in Model.Orders) %>
| Handle null and empty string the same way everywhere.
| To check if a string is filled use IsNullOrEmpty. | str == null | String.IsNullOrEmpty(str)
| To equate string use String.Equals. | str == "bla" | String.Equals(str, "bla")
| Avoid using Activator.CreateInstance. Prefer using the ‘new’ keyword. Using generics you can avoid some of the Activator.CreateInstance calls. A call to Activator.CreateInstance should be rare and the last choice for instantiating an object. | Activator.CreateInstance(typeof(T)) | T = new T()
| Entity equality checks are better done by ID than by reference comparison, because persistence frameworks do not always provide instance integrity, so code that compares identities is less likely to break. | if (entity1 == entity2) | if (entity1.ID == entity2.ID)<br><br>// (Also do null checks if applicable.)
| The following data types are not CLR-complient and sould be avoided | Unsigned types such as:<br>uint<br>ulong<br><br>And also:<br>sbyte<br> | int<br>long<br>byte
| Parameter order:<br>When passing infrastructure-related parameters to constructors or methods, first list the entities (or loose values), then the persistence related parameters, then the security related ones, then possibly the culture, then other settings. | | class MyPresenter<br>{<br>public MyPresenter(<br>MyEntity entity, <br>IMyRepository repository,<br>IAuthenticator authenticator,<br>string cultureName,<br>int pageSize)<br>{<br>...<br>}<br>}
| No long code lines<br><TODO: Describe better.>
| When evaluating a range in an ‘if’, mention the limits of the range and mention the start of the range first and the end of the range second. | if (x <= 100 && x >= 10)<br>if (x >= 11 && x <= 99) |if (x >= 10 && x <= 100)<br>if (x > 10 && x < 100)

#### Namespace Tips

Avoid using full namespaces in code, because that makes the code line very hard to read:

NOT RECOMMENDED:

```cs
JJ.Business.Cms.RepositoryInterfaces.IUserRepository userRepository = PersistenceHelper.CreatCmsRepository< JJ.Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

Using half a namespace is also not great, because when you need to rename a namespace, you will have a lot of manual work:

NOT RECOMMENDED:

```cs
Business.Cms.RepositoryInterfaces.IUserRepository userRepository = PersistenceHelper.CreateCmsRepository<Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

Instead, try giving a class a unique name or use aliases:

```cs
using IUserRepository_Cms = JJ.Business.Cms.RepositoryInterfaces.IUserRepository;
...

IUserRepository_Cms cmsUserRepository = PersistenceHelper.CreateCmsRepository<IUserRepository_Cms>(cmsContext);
```

### Member Order

Try giving the members in your code file a logical order, instead of mixing them all up. Suggested possibilities for organizing your members:


|                      | |
|----------------------|-|
| Chronological        | When one method delegates to another in a particular order, you might order the methods chronologically.
| By functional aspect | When your code file contains multiple functionalities, you might keep the members with the same function together, and put a comment line above it.
| By technical aspect  | You may choose to keep your fields together, your properties together, your members together or group them by access modifier (e.g. public or private).
| By layer             | When you can identify layers of delegation in your class you might first list the members of layer 1, then the members of layer 2, etc.

The preferred ordering of members might be chronological if applicable and otherwise by functional aspect, but there are no rights and wrongs here. Pick the one most appropriate for your code.

### Naming

See also: Casing, Punctuation and Spacing.

#### Boolean Names

Use common boolean variable name prefixes and suffixes:


| Prefix / Suffix | Example       | Comment |
|-----------------|---------------|---------|
| Is…             | IsDeleted     | This is the most common prefix.
| Must…           | MustDelete    |
| Can…            | CanDelete     | Usually indicates what *user* can do.
| Has…            | HasRecords    |
| Are…            | AreEqual      | For plural things.
| Not…            | NotNull       | A valid prefix, but be careful with negative names for readability’s sake. See ‘Double Negatives’.
| Include…        | IncludeHidden | Even though it is verb, it makes sense for booleans.
| Exclude…        |               | Even though it is verb, it makes sense for booleans.
| …Exists         | FileExists    |
| Always…         |               | You might use the word ‘Is’ along with it anyway.
| Never…          |               | You might use the word ‘Is’ along with it anyway.
| Only…           |               |

If it is ugly to put the prefix at the beginning, you can put it in the middle, e.g.: LinesAreCopied instead of AreLinesCopied.

Some boolean names are so common that they do not get any prefixes:

|         |
|---------|
| Visible |
| Enabled |

#### Class Names

Class names usually end with the pattern name or a verb converted to a noun, e.g.:

    Converter
    Validator
    Calculator

And they start with a term out of the domain:

    OrderConverter
    ProductValidator
    PriceCalculator

A more specialized class can get prefixes or suffixes as follows:

    OptimizedPriceCalculator
    OrderWithPriorityShippingValidator

Or alternatively:

    OrderValidatorWithPriorityShipping

Abstract classes get the preferred suffix ‘Base’:

    ProductValidatorBase

This is because it is very important to see in code whether something is a base class. Exceptions to the suffix rule can be made if it would otherwise result in less readable code. For instance, base classes in entity models might not look good with the ‘Base’ suffix.

Keep variable names similar to the class names, and end them with the pattern name.

Common ‘last names’ for classes apart form the pattern names are:

|              | |
|--------------|-|
| `Resolver`   | A class that does lookups that require complex keys or different ways of looking up depending on the situations, fuzzy lookups, etc.
| `Dispatcher` | A class that takes a canonical input, and dispatches it by calling different method depending on the input, or sending a message in a different format to a different infrastructural endpoint depending on the input.
| `Invoker`    | Something that invokes another method, probably based on input or specific conditions.
| `Provider`   | A class that provides something. It can be useful to have a separate class that provides something if there are many conditions or contextual dependencies involved in retrieving something. A provider can also be used when something has to be retrieved conditionally or if retrieval has to be postponed until later.
| `Asserter`   | <TODO: Describe>
|              | Any method verb could become a class name, by turning it into a verby noun, e.g. Convert à Converter.

#### Collection Names

Collection names are plural words, e.g.:

    Products
    Orders

Variable names for amounts of elements in the collection are named:

    Count

So avoid using plural words to denote a count and avoid plural words for things other than collections.

#### DateTime Names

A `DateTime` property should be suffixed with `Utc` or `Local`:

    StartDateLocal
    OrderDateTimeUtc

An alternative possible suffix for `DateTimes` would be `When`:

    ModifiedWhen
    OrderedWhen

But that looks less nice when you add the Local and Utc suffices again:

    ModifiedWhenUtc
    OrderedWhenLocal

#### Enum Names

Use the `Enum` suffix for enum types e.g. OrderStatus**Enum**.
Another acceptable alternative is the suffix `Mode`, e.g. Connection**Mode**, but the first choice should be the suffix `Enum`.

#### Event Names / Delegate Names

Event names and delegate names, that indicate what just happened have the following form:

    Deleted
    TransactionCompleted

Event names and delegate names, that indicate what is about to happen have the following form:

    Deleting
    TransactionCompleting

UI-related event names do not have to follow that rule:

    Click
    DoubleClick
    KeyPress

Delegate names can also have the suffix Callback or Delegate:

    ProgressInfoCallback
    AddItemDelegate

Sometimes the word ‘On’ is used:

    OnSelectedIndexChanged
    OnClick

Or the prefix Handle:

    HandleMouseDown

Or the suffix Requested, if your event looks like a method name.

    RemoveRequested

Pardon the ambiguity, but the naming above can be used for the names of events, but some of them also serve well as names for methods that fire/emulate or otherwise handle the event. The prefix ‘On’ for instance and the prefix ‘Handle’ may very well be used for the methods that actually raise the event. ‘Fire’ and ‘Do’ are also alternatives.

Avoid event names that indicate that it is an event in two different ways. For instance ‘OnDragging’ can be shortened to just ‘Dragging’, because the suffix -ing is already an indication that it is an event. ‘OnMouseUp’ can be shortened to just ‘MouseUp’, because that is an established event name.

#### Method Names

Method names start with verbs, e.g. CreateOrder. 

Names for other constructs should not start with a verb.

Common verbs:

| Verb        | Description |
|-------------|-------------|
| `Add`       | E.g.<br><br>List.Add(item)<br>ListManager.Add(list, item)<br><br>In cases such as the last example, it is best to make the list the first parameter.
| `Assert`    | A method that throws __exceptions__ if input is invalid.
| `Calculate` |
| `Clear`     |
| `Convert`   |
| `ConvertTo` |
| `Create`    | When a method returns a new object.
| `Delete`    |
| `Ensure`    | Sets up a state if it is not set up yet. If Ensure means throw an exception if a state is not there, then consider using the verb ‘Assert’ instead.
| `Execute`   |
| `Generate`  |
| `Get`       |
| `Invoke`    |
| `Parse`     |
| `Process`   |
| `Remove`    |
| `Save`      |
| `Set`       |
| `Try`       |
| `TryGet`    |
| `Validate`  | A method that generates __validation messages__ for user-input errors

#### File-Related Variable Names

Variable names that indicate parts of file paths can easily become ambiguous. Here is a list of names that can be used to disambiguate it all:

| Name                     | Value          
|--------------------------|--------------------------
| FileName                 | "MyFile.txt"
| FilePath                 | "C:\MyFolder\MyFile.txt"
| FolderPath               | "C:\MyFolder"
| SubFolder                | "MyFolder"
| RelativeFolderPath<br>(sometimes also called ‘SubFolder’ or ‘SubFolderPath’) | "MyFolder\MyFolder2"
| RelativeFilePath         | "MyFolder\MyFile.txt"
| FileNameWithoutExtension | "MyFile"
| FileExtension            | ".txt"
| AbsoluteFilePath         | "C:\MyFolder\MyFile.txt"
| AbsoluteFolderPath       | "C:\MyFolder"
| AbsoluteFileName         | DOES NOT EXIST
| FileName**Pattern**, FilePath**Pattern**, etc. | **\***.xml<br>C:\temp\BLA\_**????**.csv
| FileName**Format**, FilePath**Format**, etc. | order-__{0}__.txt<br>orders-__{0:dd-MM-yyyy}__\\*.\*

__Prefixes and Suffixes__ 

| Suffix                      | Description
|-----------------------------|--------------------
| source..<br>dest… | In code that converts one structure to the other, it is often clear to use the prefixes ‘source’ and ‘dest’ in the variable names to keep track of where data comes from and goes to.
| existing...                 | Denotes that something already existed (in the data store) before starting this transaction.
| new…                        | Denotes that the object was just newly created.
| original…                   | Denotes that this is an original value that was (temporarily) replaced.
| …WithRelatedEntities<br>…WithRelatedObjects | Indicates that not only a single object is handled, but the object including the underlying related objects.
| Versatile…                  | A class that handles a multitude of types or situations.
| …With…                      | When tou make a specialized class that works well for a specific situation, you could use the word ‘With’ in the class name like this:<br>- CostCalculator<br>- CostWithTaxCalculator
| ...Polymorphic              | Handles a multitude of differrent derived types, possibly each in a different way.
| …IfNeeded                   | If something is executed conditionally. This is a nice alternative for the less pretty suffixes ‘Conditionnally’ or a prefix ‘Conditional’, which obscures the name that comes after.
| …Unsafe                     | When it lacks e.g. thread-safety or executes unmanaged code, or lacks a lot of checks.
| …Recursive                  | (Some people tend to use ‘Recursively’ instead, probably insisting it is better grammer, but Recursive is shorter and not grammatically incorrect either. It is a characteristic, as in ‘Is it *recursive*?’.)
| To…                         | For conversion from one to another thing. Usually ‘this’ is source of the conversion, for example:<br><br>array.ToHashSet()<br><br>Less commonly the ‘To’ prefix is used when the ‘this’ is not the source, for instance:<br><br>MyConverter.ToHashSet(object[] array)<br><br>The Convert or ConvertTo verbs might be more appropriate there:<br><br>MyConverter.ConvertToHashSet(object[] array)<br>
| From…                       | For conversion from one to another thing. A lot like ‘To…’ executed on the dest object instead:<br><br>dest.FromSource(source)<br><br>The ‘To…’ prefix is more common, and usually more readable.

#### Miscellaneous Names

- For number sequences you can use names like: `ListIndex`, `IndexNumber`, `SortOrder` or `Rank`. (Perhaps avoid `Index` because it is an SQL keyword.)
