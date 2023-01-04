👨‍💻 Code Style
==============

[back](.)

<style>
section, .wrapper { max-width: 90% }
table td { vertical-align: top; }
</style>

<h3>Contents</h3>

- [Introduction](#introduction)
- [Casing](#casing)
- [Naming](#naming)
    - [Method Names](#method-names)
    - [Class Names](#class-names)
    - [Boolean Names](#boolean-names)
    - [Collection Names](#collection-names)
    - [Enum Names](#enum-names)
    - [DateTime Names](#datetime-names)
    - [Sort Order](#sort-order)
    - [File-Related Variable Names](#file-related-variable-names)
    - [Prefixes and Suffixes](#prefixes-and-suffixes)
    - [Event Names / Delegate Names](#event-names--delegate-names)
    - [Test Class Names End with "Tests"](#test-class-names-end-with-tests)
    - [Test Method Names](#test-method-names)
    - ['Hungarian' Notation](#hungarian-notation)
- [Enters](#enters)
    - [Properties on Separate Lines](#properties-on-separate-lines)
    - [Variables on Separate Lines](#variables-on-separate-lines)
    - [Enters between Methods](#enters-between-methods)
    - [Enters in Methods](#enters-in-methods)
    - [Loops Multi-Line](#loops-multi-line)
    - [Enters between Switch Cases](#enters-between-switch-cases)
    - [Surplus Enters Between Braces](#surplus-enters-between-braces)
    - [Generic Constraints on Separate Line](#generic-constraints-on-separate-line)
    - [Generic Constraints on Same Line for One-Liners](#generic-constraints-on-same-line-for-one-liners)
- [Spaces and Braces](#spaces-and-braces)
    - [Auto-Formatting](#auto-formatting)
    - [Indentation](#indentation)
    - [No Braces for Single-Line If Statements](#no-braces-for-single-line-if-statements)
    - [Braces for Multi-Line Statements](#braces-for-multi-line-statements)
    - [Tabular Form Less Preferred](#tabular-form-less-preferred)
    - [Align Elements of Linq Queries](#align-elements-of-linq-queries)
- [Encapsulation](#encapsulation)
    - [Members Private](#members-private)
    - [Types Internal](#types-internal)
    - [Explicit Access Modifiers](#explicit-access-modifiers)
    - [No Public Fields](#no-public-fields)
    - [Public Members for Internal Classes](#public-members-for-internal-classes)
    - [Prefer Interface Types](#prefer-interface-types)
    - [Nested Class on Top](#nested-class-on-top)
    - [1 Type 1 File](#1-type-1-file)
    - [No Lone Classes](#no-lone-classes)
- [Comments](#comments)
    - [Comments in Summaries](#comments-in-summaries)
    - [Comments in English](#comments-in-english)
    - [No Comments without Info](#no-comments-without-info)
    - [No Unused / Outcommented Code](#no-unused--outcommented-code)
- [More Notation](#more-notation)
    - [Object Initializers](#object-initializers)
    - [Default Switch Case at the Bottom](#default-switch-case-at-the-bottom)
    - [Long Code Lines](#long-code-lines)
    - [Var](#var)
        - [Anonymous Types](#anonymous-types)
        - [New Statements](#new-statements)
        - [Direct Casts](#direct-casts)
        - [Code Line Quite Long and Better Readable](#code-line-quite-long-and-better-readable)
        - [View Code](#view-code)
    - [No Inferrable Type Arguments](#no-inferrable-type-arguments)
    - [FileOpen, FileMode, FileAccess, FileShare](#fileopen-filemode-fileaccess-fileshare)
    - [Ordered If Range](#ordered-if-range)
    - [Parameter Order](#parameter-order)
    - [Namespace Tips](#namespace-tips)
- [Member Order](#member-order)
- [Misc Preferences](#misc-preferences)
    - [Null / Empty Strings](#null--empty-strings)
    - [String.IsNullOrEmpty](#stringisnullorempty)
    - [String.Equals](#stringequals)
    - [Prefer Value and HasValue](#prefer-value-and-hasvalue)
    - [Prefer ToArray](#prefer-toarray)
    - [CLR Data Types](#clr-data-types)
    - [No Decisions from Exceptions](#no-decisions-from-exceptions)
    - [Entity Equality by ID](#entity-equality-by-id)
    - [Avoiding Compiler Directives](#avoiding-compiler-directives)
    - [Activator.CreateInstance](#activatorcreateinstance)


Introduction
------------

This article lists coding style preferences, that might be followed in the `JJ`  projects.

It mostly conforms to the `Microsoft` standard described in the following documents:

<http://msdn.microsoft.com/en-us/library/vstudio/ff926074.aspx>  
<http://msdn.microsoft.com/en-us/library/aa260844%28v=vs.60%29.aspx>

Using a tool like `ReSharper` may help. Its settings can be finetuned to closely match the preferences. It then checks the check code style and may auto-format code for you.


Casing
------

| Suggestion                                                      | Examples                       |
|-----------------------------------------------------------------|--------------------------------|
| Pascal case for properties, methods, class names and events     | `MyProperty` `MyMethod`
| Camel case for local variables and parameters                   | `myLocalVariable` `myParameter`
| Fields in camel case starting with underscore                   | `_myField`
| Constants in capitals with underscores between words            | `MY_CONSTANT`
| Type arguments just the letter `T` or start with the letter `T` | `T` `TEntity` `TViewModel`
| Interface names start with `I`.                                 | `IMyInterface`
| Abbreviations not preferred                                     |
| Abbreviations of 2 letters with capitals.                       | `ID`
| Abbreviations of 3 letters or more in pascal case.              | `Mvc`
| MVC partial view names in pascal case, starting with underscore | `_MyPartialView`
| For long identifiers, underscores to separate 'the pieces'      | `Sine_OperatorCalculator_VarFrequency`


Naming
------

Reasons for naming conventions might generally be, just knowing what kind of system elements they are.

### Method Names

Method names commonly start with verbs, e.g. `CreateOrder`.  
For clarity, avoid starting with a verb, for other constructs than methods.  
Suggestions for verbs:

| Verb        | Description |
|-------------|-------------|
| `Add`       | `List.Add(item)`<br>`ListManager.Add(list, item)`<br>(List can be the `this` argument or the first argument.)
| `Assert`    | Throws `Exceptions` if input is invalid.
| `Calculate` |
| `Clear`     | Clearing a list.
| `Convert`   |
| `ConvertTo` |
| `Create`    | Returns a new object.
| `Delete`    |
| `Ensure`    | Setting up a state, if not set up yet.<br>If `Ensure` means throw an exception,<br>consider using the `Assert` instead.
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
| `Validate`  | Generating *validation messages* for user-input errors.

### Class Names

In this architecture, *class* names may end with the *pattern* name or a *verb* converted to a *noun*, e.g.:

    Converter
    Validator
    Calculator

And they may start with a term out of the *domain* (like `Order`, `Product` or `Price`):

    OrderConverter
    ProductValidator
    PriceCalculator

More *specialized* classes might get prefixes or suffixes (like `Optimized` or `WithPriorityShipping`):

    OptimizedPriceCalculator
    OrderValidatorWithPriorityShipping

Abstract classes might prefer the suffix `Base`:

    ProductValidatorBase

It might be quite important to see in code, whether something is a base class. Exceptions to the `Base` suffix might be made, if it would make the code more readable. For instance, base classes in entity models might look better without the `Base` suffix.

*Variables* might be kept similar to the *class* names and include the prefixes and suffixes, so it stays clear what they are.

Other suggested 'last names' for classes apart form the pattern names:

|              | |
|--------------|-|
| `Resolver`   | A class that does lookups that require complex keys or different ways of looking up depending on the situations, fuzzy lookups, etc.
| `Dispatcher` | A class that takes a canonical input, and dispatches it by calling different method depending on the input, or sending a message in a different format to a different infrastructural endpoint depending on the input.
| `Invoker`    | Something that invokes another method, possibly based on input or specific conditions.
| `Provider`   | A class that provides something. It can be useful to have a separate class that provides something if there are many conditions or contextual dependencies involved in retrieving something. A `Provider` might also be used when something has to be retrieved conditionally or if retrieval has to be postponed until later.
| `Asserter`   | A class meant to throw exceptions under certain conditions.
|              | Any method verb might become a class name, by turning it into a verby noun, e.g. `Convert` => `Converter`.

### Boolean Names

Suggestions for *boolean* variable name prefixes and suffixes:

| Prefix / Suffix | Example               | Comment |
|-----------------|-----------------------|---------|
| `Is...`         | `IsDeleted`           | Might be the most common.
| `Must...`       | `MustDelete`          |
| `Can...`        | `CanDelete`           | Might indicate what *user* can do.
| `Has...`        | `HasRecords`          |
| `Are...`        | `AreEqual`            | For plural things.
| `Not...`        | `NotNull`             | A nice prefix, but perhaps be careful with negative names for readability's sake. See [Double Negatives](practices-and-principles.md#double-negatives-)
| `Include...`    | `IncludeHidden`       | Verb are usually be for methods, but these may make sense for booleans.
| `Exclude...`    | `ExcludeSpecialChars` | "
| `...Exists`     | `FileExists`          |
| `Always...`     |                       |
| `Never...`      |                       |
| `Only...`       |                       |

If it looks wrong to put the prefix at the beginning, maybe put it in the middle, e.g.: 

    LinesAreCopied

instead of:
    
    AreLinesCopied

Some boolean names are so common, that they might not need a prefix:

    Visible
    Enabled

### Collection Names

Plural words are preferred for collections:

    Products
    Orders

Variable names for amounts of elements in the collection might be named:

    Count

So perhaps avoid plural words to denote a count and avoid plural words for things other than collections.

### Enum Names

This architecture tends to use the `Enum` suffix for `enum` types e.g. `OrderStatusEnum`.

Another alternative might be the suffix `Mode`, e.g. `ConnectionMode`, but at some point the preference became `Enum` because it was found so important to see clearly that something is an `enum`.

### DateTime Names

A `DateTime` property might be suffixed with `Utc` or `Local`:

<pre class="green">
StartDateLocal
OrderDateTimeUtc
</pre>

An alternative suffix for `DateTimes` could be `When`:

<pre class="green">
ModifiedWhen
OrderedWhen
</pre>

But that might look less nice when you add the `Local` and `Utc` suffices again:

<pre class="red">
ModifiedWhenUtc
OrderedWhenLocal
</pre>

### Sort Order

For number sequences these names might be used:

    ListIndex
    IndexNumber
    SortOrder
    Rank

Perhaps avoid `Index` because it is an `SQL` keyword.

### File-Related Variable Names

Variable names that indicate parts of *file paths* might easily become ambiguous. Here are suggestions to disambiguate them:

| Name                     | Value          
|--------------------------|--------------------------
| `FileName`                 | `"MyFile.txt"`
| `FilePath`                 | `"C:\MyFolder\MyFile.txt"`
| `FolderPath`               | `"C:\MyFolder"`
| `SubFolder`                | `"MyFolder"`
| `RelativeFolderPath` /<br>`SubFolder` /<br>`SubFolderPath` | `"MyFolder\MyFolder2"`
| `RelativeFilePath`         | `"MyFolder\MyFile.txt"`
| `FileNameWithoutExtension` | `"MyFile"`
| `FileExtension`            | `".txt"`
| `AbsoluteFilePath`         | `"C:\MyFolder\MyFile.txt"`
| `AbsoluteFolderPath`       | `"C:\MyFolder"`
| `AbsoluteFileName`         | DOES NOT EXIST
| `FileNamePattern` /<br>`FilePathPattern` / etc.<br>wildcards like `*` and `?` | `*.xml`<br>`C:\temp\BLA_???.csv`
| `FileNameFormat` /<br>`FilePathFormat` / etc.<br>placeholders like `{0}` and `{0:dd-MM-yyyy}` | `order-{0}.txt`<br>`orders-{0:dd-MM-yyyy}\*.*`

### Prefixes and Suffixes

| Example                  | Description
|--------------------------|--------------------
| `source...`<br>`dest...` | In code that converts one structure to the other, it might be clear to use the prefixes `source` and `dest` consistently in the variable names to keep track of where data comes from and where it goes.
| `existing...`            | Denoting that something already existed (in the data store) before starting.
| `new...`                 | Denoting that the object was just newly created.
| `original...`            | May denote that this is an original value that was (temporarily) replaced.
| `...WithRelatedEntities`<br>`...WithRelatedObjects` | Indicating that not only a single object is handled, but the object including the underlying related objects.
| `Versatile...`           | Might handle a multitude of types or situations.
| `...With...`             | When making a specialized class that works well for a specific situation, you could use the word `With` in the class name e.g.:<br> `CostCalculator`<br>`CostWithTaxCalculator`
| `...Polymorphic`         | Handling a multitude of differrent (derived) types, possibly each in a different way.
| `...IfNeeded`            | If something is executed conditionally. This might be a nice alternative for a possibly less easy to read suffixes like `Conditionally` or a prefix `Conditional`.
| `...Unsafe`              | When it lacks e.g. thread-safety or executes unmanaged code, or lacks some checks.
| `...Recursive`           | `< TODO: Make description >`
| `To...`                  | For conversion from one thing to another. Sometimes the `this` is source of the conversion, for example:<br>`array.ToHashSet()`<br>Perhaps less commonly the `To` prefix is used when the `this` is not the source, for instance:<br>`MyConverter.ToHashSet(object[] array)`<br>The `Convert` or `ConvertTo` verbs might also be used as an alternative:<br>`MyConverter.ConvertToHashSet(object[] array)`<br>
| `From...`                | For conversion from one thing to another. A lot like `To...`, but executed on the `dest` object:<br>`dest.FromSource(source)`<br>The `To...` prefix might be more common, and possibly more readable.

### Event Names / Delegate Names

*Event* names and *delegate* names, that indicate what *just happened* might have the following form:

    Deleted
    TransactionCompleted

Event names and delegate names, that indicate what is *about to happen* might have the following form:

    Deleting
    TransactionCompleting

*User-initiated* events might not follow that pattern:

    Click
    DoubleClick
    KeyPress

*Delegate* names might also have the suffix `Callback` or `Delegate`:

    ProgressInfoCallback
    AddItemDelegate

Sometimes the word `On` may be used:

    OnSelectedIndexChanged
    OnClick

Or the prefix `Handle`:

    HandleMouseDown

Or the suffix `Requested`, if your event looks like a method name.

    RemoveRequested

Pardon the ambiguity, but the naming above can be used for the names of *events*, but some of them also serve well as names for methods that *fire/emulate* or otherwise handle the event. The prefix `On` for instance and the prefix `Handle` may very well be used for the methods that actually raise the event. `Fire` and `Do` might also be alternatives for methods that raise events.

Perhaps avoid event names that use *two* event-indications in the same name. For instance `OnDragging` might be shortened to just `Dragging` or `OnDrag`. `OnMouseUp` might be shortened to just `MouseUp`, because that would be a well established event name.

### Test Class Names End with "Tests"

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```csharp
[TestClass]
public class ValidatorTests()
{
    
}
```

</td><td markdown="1" class="red">

```csharp
[TestClass]
public class Tests_Validator()
{
    
}
```

</td></tr></table>

Reason: Just convention.

### Test Method Names

Prefer to start test method names with `Test_` and maybe don't hold back on underscores in the name.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
[TestMethod]
public void Test_Validator_NotNullOrEmpty_Valid()
{ }

[TestMethod]
public void Test_Validator_NotNullOrEmpty_NotValid()
{ }
```

</td><td markdown="1" class="red">

```cs
[TestMethod]
public void Test1()
{ }

public void Test2()
{ }
```

</td></tr></table>

Reason:  
If the test names mean to be descriptive, they might become long, and underscores to separate the 'pieces' may make it easier to read.

### 'Hungarian' Notation

Avoid prefixes such as `strName`.


Enters
------

### Properties on Separate Lines

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
int A { get; set; } 
int B { get; set; }

int C
{
    get { ... }
    set { ... }
}

int D
{
    get 
    {
        ... 
    }
    set 
    { 
        ...
    }
}
```

</td><td markdown="1" class="red">

```cs
int A { get; set; } int B { get; set; } 
```

</td></tr></table>

Reason:  
Might be easy to overlook that there is another property.

### Variables on Separate Lines

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
int i;
int j;
```

</td><td markdown="1" class="red">

```cs
int i, j; 
```

</td></tr></table>

Reason:  
Just a preference, when not used to it, it may be overlooked, doing a Search through the code, may not show the results expected.

### Enters between Methods

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
void Bla()
{
   
}

void Bla2()
{
   
}
```

</td><td markdown="1" class="red">

```cs
void Bla()
{
    
}
void Bla2()
{
    
}
```

</td></tr></table>

Reason: Just a bit more tidy.

### Enters in Methods

Putting enters inside methods between 'pieces that do something'.  

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
void Bla()
{
    var x = new X();
    x.A = 10;
    
    var y = new Y();
    y.B = 20;
    y.X = x;
    
    Bla2(x, y);
}
   
```

</td><td markdown="1" class="red">

```cs
void Bla()
{
    var x = new X();
    x.A = 10;
    var y = new Y();
    y.B = 20;
    y.X = x;
    Bla2(x, y);
} 
```

</td></tr></table>

Reason:  
Visible separation of steps inside methods.

### Loops Multi-Line

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
foreach (var x in list)
{ 
    Bla();
}
```

</td><td markdown="1" class="red">

```cs
foreach (var x in list) { Bla(); } 
```

</td></tr></table>

Reason: May look odd if you're not used to it.

### Enters between Switch Cases

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
switch (x)
{
    case 1:
        break;

    case 2:
        break;
}
```

</td><td markdown="1" class="red">

```cs
switch (x)
{
    case 1:
        break;
    case 2:
        break;
}
```

</td></tr></table>

Reason: A bit tidier.

### Surplus Enters Between Braces

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
    }
}
```

</td><td markdown="1" class="red">

```cs
    }
    
}
```

</td></tr></table>

Reason: More tidy.

### Generic Constraints on Separate Line

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
class MyGenericClass<T>
    where T: MyInterface
{
    ...
}
```

</td><td markdown="1" class="red">

```cs
class MyGenericClass<T> where T: MyInterface
{
    ...
}
```

</td></tr></table>

Reason: So they stand out.

### Generic Constraints on Same Line for One-Liners

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
interface IMyInterface
{
    void MyMethod(T param) where T : ISomething
}
```

</td><td markdown="1" class="red">

```cs
interface IMyInterface
{
    void MyMethod(T param) 
        where T : ISomething
} 
```

</td></tr></table>

Reason:  
It might have been a one-liner for readability reasons, so perhaps we keep it in that style.


Spaces and Braces
-----------------

### Auto-Formatting

Prefer Visual Studio's autoformatting enabled and set to its defaults.  
Reason: Less surprising to the next developer.

### Indentation

Apply indentation properly.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
object ParseValue(string input, Type type)
{
    if (type.IsNullableType())
    {
        if (string.IsNullOrEmpty(input))
        {
            return null;
        }

        type = type.GetUnderlyingNullableTypeFast();
    }

    if (type.IsEnum)
    {
        return Enum.Parse(type, input);
    }

    if (type == typeof(TimeSpan))
    {
        return TimeSpan.Parse(input);
    }

    if (type == typeof(Guid))
    {
        return new Guid(input);
    }

    return Convert.ChangeType(input, type);
}
```

</td><td markdown="1" class="red">

```cs
        object ParseValue(string input,
Type type 
            )
{
        if (type.IsNullableType()) 
        {
    if (string.IsNullOrEmpty(input)) {
    return null;
}

type = type.GetUnderlyingNullableTypeFast();
    }

        if (type.IsEnum) 
    {
return Enum.Parse(type, input);
    }

            if (type == 
    typeof(TimeSpan))
{
                return TimeSpan.Parse(input);
}

            if (type == typeof(Guid))
            { 
    return new Guid(input);
            }


        return 
        Convert.ChangeType(
   input, 

        type);
                }
```

</td></tr></table>

Reason: More readable.

### No Braces for Single-Line If Statements

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
if (condition) Bla();
```
 
</td><td markdown="1" class="red">

```cs
if (condition) { Bla(); }
```

</td></tr></table>

Reason: Less clutter.

### Braces for Multi-Line Statements

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
foreach (var x in list)
{
    Bla();
}

if (condition)
{
    Bla();
}
```

</td><td markdown="1" class="red">

```cs
foreach (var x in list)
    Bla();

if (condition)
    Bla(); 
    Something();
```

</td></tr></table>

Reason:  
Without braces, only the next line is looped or executed conditionally. The line after that would be outside the loop, which is sort of not obvious and might lead to error.

### Tabular Form Less Preferred

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
public int ID { get; set; }
public bool IsActive { get; set; }
public string Text { get; set; }
public string Answer { get; set; }
public bool IsManual { get; set; }
```

</td><td markdown="1" class="red">

```cs
public int    ID       { get; set; }
public bool   IsActive { get; set; }
public string Text     { get; set; }
public string Answer   { get; set; }
public bool   IsManual { get; set; } 
```

</td></tr></table>

Reason:  
Tabular form might be undone by auto-formatting. It may look nice, but maybe get your eyes used to non-tabular form instead.

### Align Elements of Linq Queries

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
var arr = coll.Where(x => x...)
              .OrderBy(x => x...)
              .ToArray()
```

</td><td markdown="1" class="red">

```cs
var arr = coll.Where(x => x...).
   OrderBy(x => x...).ToArray() 
```

</td></tr></table>

Reason: readability.


Encapsulation
-------------

### Members Private

Prefer keeping members private. 

```cs
private void Bla()
{
    ...
}
```

Reason:  
Other code might become dependent on publically accessible things. Managing dependencies like that seems quite a thing in software development.

### Types Internal 

Prefer to keep types `internal`.

```cs
internal class MyClass
{
    ...   
}
```

Reason:    
External things might otherwise become dependent on code, that was not meant to have so many links to it. Managing dependency between parts seems quite a concern in software.

### Explicit Access Modifiers

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
public int Bla() { ... }
```

</td><td markdown="1" class="red">

```cs
int Bla() { ... } 
```

</td></tr></table>

Reason:  
Avoiding confusion about the defaults.

Exception:  
Interface members have no access modifiers.

### No Public Fields

Prefer not to use public fields. Use either private fields or use properties. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

`public int X `***`{ get; set; }`***

</td><td markdown="1" class="red">

`public int X;`

</td></tr></table>

Reason:  
People may say the interface stability comes in jeopardy when you use public fields. The fields may look similar from the outside. However, frameworks may expect properties, not fields, which makes letting fields participate in reusable functions less easily. Perhaps compatibility like that is an argument.

### Public Members for Internal Classes

Internal classes are preferred not to have internal members.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

`internal class A { `***`public`***`void B { } }`

</td><td markdown="1" class="red">

`internal class A { `***`internal`***` void B { } }`

</td></tr></table>

Reason:  
The members are automatically `internal` if the class is `internal`. When you wish to make the class `public`, you would not have to manually correct the access modifiers of the methods, creating an opportunity for error.

### Prefer Interface Types

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

***`IList`***`<int> list = new List<int>;`

</td><td markdown="1" class="red">

***`List`***`<int> list = new List<int>; `

</td></tr></table>

Reason:  
Less refactoring when changing the type. Less dependency on specific implementation allowing you to switch more easily to another class.

### Nested Class on Top

Putting nested classes at the top of the parent class' code. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
internal class A
{
    private class B
    {
        
    }
    
    public int X { get; set; }
}
```

</td><td markdown="1" class="red">

```cs
internal class A
{
    public int X { get; set; }
    
    private class B
    {
        
    }
}
```

</td></tr></table>

Reason:  
It may not be obvious there are nested classes, unless they are put at the top.

### 1 Type 1 File

Preferably give each class (or interface or enum) its own file.

Reason:  
One might be surprised to find types hidden away behind a single file name. It may harm the overview of the different pieces of code.

Exceptions:  
It does not count for nested classes. Also a single class can be spread among files, if they are partial classes. This guideline might also be ignored if the amount of classes really becomes big. 

### No Lone Classes

It might not be handy to have a lot of folders just containing one or very few classes. Consider moving those classes into other folders. Another solution could be to put them all together, in a `Helpers` folder for instance.


Comments
--------

### Comments in Summaries

You might want to put comment for members in `<summary>` tags. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
/// <summary> This is the x coordinate. </summary>
int X { get; set; }
```

</td><td markdown="1" class="red">

```cs
// This is the x-coordinate.
int X { get; set; } 
```

</td></tr></table>

Reason:  
Your comment might be valuable on the outside for others to see. The `<summary>` would show up when hovering over a member.

### Comments in English

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
// This is a thing.
```

</td><td markdown="1" class="red">

```cs
// Dit is een ding. 
```

</td></tr></table>

Reason: English is sort of the main language in IT. Broader reach of people might be able to read your comments.

### No Comments without Info

Avoid comments that do not add information.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
int x;
```

</td><td markdown="1" class="red">

```cs
// This is x
int x;
```

</td></tr></table>

Reason:  
Less visual clutter. Reading it mightr not be worth the time.

### No Unused / Outcommented Code

Prefer not to leave unused (or outcommented) code around. If needed, it might be moved it to an `Archive` folder.

Reason:  
Unused code might clutter your vision or may make the suggestion that it was outcommented in error.


More Notation
-------------

### Object Initializers

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
var x = new X
{
    A = 10,
    B = 20
}
```

</td><td markdown="1" class="red">

```cs
var x = new X();
x.A = 10;
x.B = 20; 
```

</td></tr></table>

Reason: More readable.

### Default Switch Case at the Bottom

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
switch (x)
{
    case 0:
        break;
    
    case 1:
        break;
    
    default:
        break;
}
```

</td><td markdown="1" class="red">

```cs
switch (x)
{
    default:
        break;

    case 0:
        break;

    case 1:
        break;
} 
```

</td></tr></table>

Reason:  
The default `switch` case is often the "last resort" case, so it may make sense to put it last in the list.

### Long Code Lines

Avoid long code lines.  
Reason: readability.

### Var

Prefer not to use `var`.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
int x = y.X;
```

</td><td markdown="1" class="red">

```cs
var x = y.X;
```

</td></tr></table>

Reason:  
It would be nice to see the variable type in the code line instead of `var`.

There may be a few exceptions, where var may be preferred. For instance when the type is sort of obvious, or it might be more readable.

#### Anonymous Types

<table><tr><th class="green">Recommended</th><th class="red">Not Preferred</th></tr><tr><td markdown="1" class="green">

***`var`***` q = from x in list select new { A = x.A };`

</td><td markdown="1" class="red">

***`X`***`q = from x in list select new { A = x.A };`

</td></tr></table>

#### New Statements

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

***`var`***` x = new X();`

</td><td markdown="1" class="red">

***`X`***` x = new X();`

</td></tr></table>

#### Direct Casts

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

***`var`***` x = (X)y;`

</td><td markdown="1" class="red">

***`X`***` x = (X)y;`

</td></tr></table>

#### Code Line Quite Long and Better Readable

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
foreach (var entry in dictionary)
```

</td><td markdown="1" class="red">

```cs
foreach (KeyValuePair<Canonical.ValidationMessage,  
         Tuple<NonPhysicalOrderProductList, Guid>> entry in dictionary)
```

</td></tr></table>

#### View Code

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

`<% foreach (`***`var`***` order in Model.Orders) %>`

</td><td markdown="1" class="red">

`<% foreach (`***`OrderViewModel`***` order in Model.Orders) %>`

</td></tr></table>

### No Inferrable Type Arguments

Prefer not to use type arguments that can be inferred. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

`References(x => x.Child)`

</td><td markdown="1" class="red">

`Reference`***`<Child>`***`(x => x.Child)`

</td></tr></table>

Reason: Less visual clutter.

### FileOpen, FileMode, FileAccess, FileShare

It is appreciated when a file stream is opened specifying all three aspects `FileMode`, `FileAccess` and `FileShare` explicitly with the most logical and most limiting values appropriate for the particular situation.

Reason:  
Otherwise these aspects may have surprising defaults.

### Ordered If Range 

When evaluating a range in an `if`, it may be a good idea to mention the *limits* of the range and mention the *start* of the range 1st and the *end* of the range second.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
if (x >= 10 && x <= 100)
if (x > 10 && x < 100)
```

</td><td markdown="1" class="red">

```cs
if (x <= 100 && x >= 10)
if (x >= 11 && x <= 99) 
```

</td></tr></table>

Reason: Readability. More obvious what the range limits are.

### Parameter Order

An idea for passing infrastructure-related parameters to constructors or methods, is to first list:

- *entities* (or loose values) then
- *persistence* related parameters then
- *security* related ones then
- *culture* (optionally) then
- *other* things. 

```cs
class MyPresenter
{
    public MyPresenter(
        MyEntity entity, 
        IMyRepository repository,
        IAuthenticator authenticator,
        string cultureName,
        int pageSize)
    {
        ...
    }
}
```

### Namespace Tips

*Full* namespaces in code, might make the code harder to read:

__Less Preferred__

```cs
JJ.Business.Cms.RepositoryInterfaces.IUserRepository userRepository =
    PersistenceHelper.CreatCmsRepository<JJ.Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

Using *half* a namespace might not be preferred either, because when you want to rename a namespace, it may generate more manual work.

__Less Preferred__

```cs
Business.Cms.RepositoryInterfaces.IUserRepository userRepository = 
    PersistenceHelper.CreateCmsRepository<Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

An alternative is to give a class a more *unique* name. Or use an *alias* instead:

__Recommended__

```cs
using IUserRepository_Cms = JJ.Business.Cms.RepositoryInterfaces.IUserRepository;
...

IUserRepository_Cms cmsUserRepository = 
    PersistenceHelper.CreateCmsRepository<IUserRepository_Cms>(cmsContext);
```

Reason:  
Long, visually cluttered code lines might be harder to read.


Member Order
------------

Try giving the members in your code file a logical order, instead of putting them in am arbitrary order. Suggestions for organizing members:

|                      |   |
|----------------------|---|
| *Chronological*        | When one method delegates to another in a particular order, you might order the methods chronologically.
| *By functional aspect* | When your code file contains multiple functionalities, you might keep the members with the same function together, and put a comment line above it.
| *By technical aspect*  | You may choose to keep your fields together, your properties together, your members together or group them by access modifier (e.g. public or private).
| *By layer*             | When you can identify layers of delegation in your class you might first list the members of layer 1, then the members of layer 2, etc.

The preferred ordering of members might be chronological if applicable and otherwise by functional aspect, but there are no rights and wrongs here. Whatever seems most appropriate for your code.


Misc Preferences
----------------

### Null / Empty Strings

Prefer handling both `null` and `""` the same way.  
Reason: No surprises when using either `null` or `""`.

### String.IsNullOrEmpty

Prefer `string.IsNullOrEmpty` to check if a `string` is filled in. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
String.IsNullOrEmpty(str)
```

</td><td markdown="1" class="red">

```cs
str == null 
```

</td></tr></table>

Reason:  
In exceptional cases reference equality (`==`) may fail even when `strings` are equal.

### String.Equals

Prefer `string.Equals` to check the equality of `string`. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
string.Equals(str, "bla")
```

</td><td markdown="1" class="red">

```cs
str == "bla"
```

</td></tr></table>

Reason:  
In exceptional cases reference equality (`==`) may fail even if `strings` are equal.

### Prefer Value and HasValue

For nullable types.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
int? number;
if (number.HasValue)
{
    string message = string.Format(
        "Number = {0}", number.Value);
}
```

</td><td markdown="1" class="red">

```cs
int? number;
if (number != null)
{
    string message = string.Format(
        "Number = {0}", number);
} 
```

</td></tr></table>

Reason:  
Changing the variable to type `object`, would change the behavior of the code considerably.

### Prefer ToArray

Prefer `ToArray` over `ToList`. 

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

`IList<int> collection = x.`***`ToArray`***`();`

</td><td markdown="1" class="red">

`IList<int> collection = x.`***`ToList`***`();`

</td></tr></table>

Reason: More performance.  
Downside: The `Add` method may throw an exception for an `Array`.

### CLR Data Types

Prefer using `CLR`- complient data types. Some aren't `CLR`- complient.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
int
long
byte
```

</td><td markdown="1" class="red">

```cs
// Unsigned types:
uint
ulong
// And also:
sbyte
```

</td></tr></table>

Reason:  
For compatibility with more variations of `.NET`.

### No Decisions from Exceptions

Avoid getting information by catching an exception. Prefer getting your information without using exception handling.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
bool FileExists(string path)
{
    return File.Exists(path);
}
```

</td><td markdown="1" class="red">

```cs
bool FileExists(string path)
{
    try
    {
        File.Open(path, ...);
        return true;
    }
    catch (IOException)
    {
        return false;
    }
}
```

</td></tr></table>

Reason:  
`Exception` handling is more performance intensive than might be expected, compared to an `if` statement. When no exception goes off, exception handling might perform well, but when an exception does go off, quite a few things happen, like gathering stack trace information.

### Entity Equality by ID

Entity equality checks might be better done by ID than by reference.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
if (entity1.ID == entity2.ID)
```

</td><td markdown="1" class="red">

```cs
if (entity1 == entity2) 
```

</td></tr></table>

Reason:  
Persistence frameworks do not always provide *instance* integrity, so code that compares *identities* may be less likely to break. 

(Also consider doing *null* checks on the entities if applicable.)

### Avoiding Compiler Directives

Prefer not to use compiler directives, unless the code cannot run on a platform without excluding that piece of code. Otherwise a *boolean* variable might be preferred, a *configuration* setting or different *concrete implementations* of classes.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
if (config.FeatureXEnabled)
{
    // ...
}
```

</td><td markdown="1" class="red">

```cs
#if FEATURE_X_ENABLED
    // ...
#endif
```

</td></tr></table>

Reason:  
When using these compiling directives, a compilation might succeed, without all the code being compilable.

### Activator.CreateInstance

Prefer using the `new` keyword instead of `Activator.CreateInstance`. Using generics' `new` constraint  might avoid some of the `Activator.CreateInstance` calls.

<table><tr><th class="green">Recommended</th><th class="red">Less Preferred</th></tr><tr><td markdown="1" class="green">

```cs
T = new T()
```

</td><td markdown="1" class="red">

```cs
Activator.CreateInstance(typeof(T)) 
```

</td></tr></table>

A call to `Activator.CreateInstance` might be the last choice for instantiating an object.

Reason:  
New statements are strongly typed and less likely to break.

[back](.)