JJ's Reference Architecture
===========================

Code Style
----------

<h3>Contents</h3>

- [Code Style](#code-style)
    - [Introduction](#introduction)
    - [Casing (and Punctuation)](#casing-and-punctuation)
    - [Spacing (and Punctuation)](#spacing-and-punctuation)
        - [Default Auto-Formatting](#default-auto-formatting)
        - [Surplus Enters Between Braces](#surplus-enters-between-braces)
        - [Enters between switch Cases](#enters-between-switch-cases)
        - [No Braces for Single-Line if Statements](#no-braces-for-single-line-if-statements)
        - [Loops on Multiple Lines](#loops-on-multiple-lines)
        - [Braces for Multi-Line Statements](#braces-for-multi-line-statements)
        - [Enters between Methods](#enters-between-methods)
        - [Properties on Separate Lines](#properties-on-separate-lines)
        - [Enters in Methods](#enters-in-methods)
        - [Variables on Separate Lines](#variables-on-separate-lines)
        - ['Tabular Form' Less Preferred](#tabular-form-less-preferred)
        - [Align Elements of Linq Queries](#align-elements-of-linq-queries)
        - [Indentation](#indentation)
        - [Generic Constraints on Separate Line](#generic-constraints-on-separate-line)
        - [Generic Constraints for One-Liners on Same Line](#generic-constraints-for-one-liners-on-same-line)
    - [Trivial Recommendations](#trivial-recommendations)
        - [A File a Type](#a-file-a-type)
        - [Members Private](#members-private)
        - [Types Internal](#types-internal)
        - [Explicit Access Modifiers](#explicit-access-modifiers)
        - [No Public Fields](#no-public-fields)
        - [Nested Class on Top](#nested-class-on-top)
        - [No Decisions from Exceptions](#no-decisions-from-exceptions)
        - [No Inferrable Type Arguments](#no-inferrable-type-arguments)
        - [Prefer Interface Types](#prefer-interface-types)
        - [Prefer ToArray](#prefer-toarray)
        - [Object Initializers](#object-initializers)
        - [Comments in Summaries](#comments-in-summaries)
        - [Comments in English](#comments-in-english)
        - [No Comments without Info](#no-comments-without-info)
        - [Avoiding Compiler Directives](#avoiding-compiler-directives)
        - [No Internal Members for Internal Classes](#no-internal-members-for-internal-classes)
        - [Default Switch Case at the Bottom](#default-switch-case-at-the-bottom)
        - [Prefer Value and HasValue for Nullable Types](#prefer-value-and-hasvalue-for-nullable-types)
        - [No Unused / Outcommented Code](#no-unused--outcommented-code)
        - [FileOpen, FileMode, FileAccess and FileShare](#fileopen-filemode-fileaccess-and-fileshare)
    - [Misc Preferences](#misc-preferences)
        - [Test Class Names Ending With 'Tests'](#test-class-names-ending-with-tests)
        - [Test Method Names](#test-method-names)
        - [Using var](#using-var)
            - [Anonymous Types](#anonymous-types)
            - [New Statements](#new-statements)
            - [Direct Casts](#direct-casts)
            - [Code Line Quite Long and Better Readable with var](#code-line-quite-long-and-better-readable-with-var)
            - [View Code](#view-code)
        - [Null / Empty Strings](#null--empty-strings)
        - [String.IsNullOrEmpty](#stringisnullorempty)
        - [String.Equals](#stringequals)
        - [Avoiding Activator.CreateInstance](#avoiding-activatorcreateinstance)
        - [Entity Equality by ID](#entity-equality-by-id)
        - [CLR Data Types](#clr-data-types)
        - [Parameter Order](#parameter-order)
        - [Long Code Lines](#long-code-lines)
        - [Ordered If Range](#ordered-if-range)
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

This article lists trivial coding style preferenced, that might be followed in certain projects.

Coding standards mostly conform to the Microsoft standard described in the following documents:

<http://msdn.microsoft.com/en-us/library/vstudio/ff926074.aspx>

<http://msdn.microsoft.com/en-us/library/aa260844%28v=vs.60%29.aspx>

Using a tool like ReSharper may help. It's settings can be finetuned to closely match your preferences. It then checks the check code style and can auto-format. It can be used to keep code clean as you write and change your code.

### Casing (and Punctuation)

| Suggestion                                                      | Examples                       |
|-----------------------------------------------------------------|--------------------------------|
| Properties, methods, class names and events in pascal case      | `MyProperty` `MyMethod`
| Local variables and parameters in camel case                    | `myLocalVariable` `myParameter`
| Fields in camel case starting with underscore                   | `_myField`
| Constants in capitals with underscores between words            | `MY_CONSTANT`
| Type arguments just the letter `T` or start with the letter `T` | `T` `TEntity` `TViewModel`
| Interface names start with `I`.                                 | `IMyInterface`
| Abbreviations not preferred                                     |
| Abbreviations of 2 letters with capitals.                       | `ID`
| Abbreviations of 3 letters or more in pascal case.              | `Mvc`
| MVC partial view names in pascal case, starting with underscore | `_MyPartialView`
| For long identifiers, underscores to separate ‘the pieces’      | `Sine_OperatorCalculator_VarFrequency`
| No prefixes, such as `strName`                                  |

### Spacing (and Punctuation)

#### Default Auto-Formatting

Prefer Visual Studio’s autoformatting enabled and set to its defaults.

#### Surplus Enters Between Braces

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
    }
}
```

</td><td>

```cs
    }
    
}
```

</td></tr></table>

#### Enters between switch Cases           

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
switch (x)
{
    case 1:
        break;

    case 2:
        break;
}
```

</td><td>

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

#### No Braces for Single-Line if Statements

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
if (condition) Bla();
```
 
</td><td>

```cs
if (condition) { Bla(); }
```

</td></tr></table>

#### Loops on Multiple Lines

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
foreach (var x in list)
{ 
    Bla();
}
```

</td><td>

```cs
foreach (var x in list) { Bla(); } 
```

</td></tr></table>

#### Braces for Multi-Line Statements

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

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

</td><td>

```cs
foreach (var x in list)
    Bla();

if (condition)
    Bla(); 
```

</td></tr></table>

#### Enters between Methods

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
void Bla()
{
   
}

void Bla2()
{
   
}
```

</td><td>

```cs
void Bla()
{
    
}
void Bla2()
{
    
}
```

</td></tr></table>

#### Properties on Separate Lines

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

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

</td><td>

```cs
int A { get; set; } int B { get; set; } 
```

</td></tr></table>

#### Enters in Methods

Putting enters inside methods between ‘Pieces that do Something’.  

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

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

</td><td>

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

#### Variables on Separate Lines

Putting variable declarations on separate lines.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
int i;
int j;
```

</td><td>

```cs
int i, j; 
```

</td></tr></table>

#### 'Tabular Form' Less Preferred

Tabular form not preferred. This tabular form might be undone by auto-formatting. It may look nice, but maybe get your eyes used to non-tabular form instead.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
public int ID { get; set; }
public bool IsActive { get; set; }
public string Text { get; set; }
public string Answer { get; set; }
public bool IsManual { get; set; }
```

</td><td>

```cs
public int    ID       { get; set; }
public bool   IsActive { get; set; }
public string Text     { get; set; }
public string Answer   { get; set; }
public bool   IsManual { get; set; } 
```

</td></tr></table>

#### Align Elements of Linq Queries

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
var arr = coll.Where(x => x...)
              .OrderBy(x => x...)
              .ToArray()
```

</td><td>

```cs
var arr = coll.Where(x => x...).
   OrderBy(x => x...).ToArray() 
```

</td></tr></table>

#### Indentation

Use proper indentation.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
<TODO: Example.> 
```

</td><td>

```cs
<TODO: Example.>
```

</td></tr></table>

#### Generic Constraints on Separate Line

So they stand out.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
class MyGenericClass<T>
    where T: MyInterface
{
    ...
}
```

</td><td>

```cs
class MyGenericClass<T> where T: MyInterface
{
    ...
}
```

</td></tr></table>

#### Generic Constraints for One-Liners on Same Line

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
interface IMyInterface
{
    void MyMethod(T param) where T : ISomething
}
```

</td><td>

```cs
interface IMyInterface
{
    void MyMethod(T param) 
        where T : ISomething
} 
```

</td></tr></table>

### Trivial Recommendations

#### A File a Type 

Preferably give each class (or interface or enum) its own file (except nested classes).

#### Members Private

Prefer keeping members private. 

```cs
private void Bla()
{
    ...
}
```

#### Types Internal 

Prefer to keep types internal 

```cs
internal class MyClass
{
    ...   
}
```

#### Explicit Access Modifiers

Using explicit access modifiers (except for interface members). 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
public int Bla() { ... }
```

</td><td>

```cs
int Bla() { ... } 
```

</td></tr></table>

#### No Public Fields

Prefer not to use public fields. Use either private fields or use properties instead. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`public`***` int X `***`{ get; set; }`***

</td><td>

`public int X;`

</td></tr></table>

#### Nested Class on Top

Putting nested classes at the top of the parent class' code. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
internal class A
{
    private class B
    {
        
    }
    
    public int X { get; set; }
}
```

</td><td>

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

#### No Decisions from Exceptions

Avoid getting information by catching an exception. Prefer getting your information without using exception handling. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
bool FileExists(string path)
{
    return File.Exists(path);
}
```

</td><td>

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

#### No Inferrable Type Arguments

Prefer not to use type arguments that can be inferred. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`References(x => x.Child)`

</td><td>

`Reference`***`<Child>`***`(x => x.Child)`

</td></tr></table>

#### Prefer Interface Types

Prefer interface types as variable types. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`IList`***`<int> list = new List<int>;`

</td><td>

***`List`***`<int> list = new List<int>; `

</td></tr></table>

#### Prefer ToArray

Prefer ToArray over ToList. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`IList<int> collection = x.`***`ToArray`***`();`

</td><td>

`IList<int> collection = x.`***`ToList`***`();`

</td></tr></table>

#### Object Initializers

Using object initializers for readability. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
var x = new X
{
    A = 10,
    B = 20
}
```

</td><td>

```cs
var x = new X();
x.A = 10;
x.B = 20; 
```

</td></tr></table>

#### Comments in Summaries

Putting comment for members in `<summary>` tags. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
/// <summary> This is the x coordinate. </summary>
int X { get; set; }
```

</td><td>

```cs
// This is the x-coordinate.
int X { get; set; } 
```

</td></tr></table>

#### Comments in English

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
// This is a thing.
```

</td><td>

```cs
// Dit is een ding. 
```

</td></tr></table>

#### No Comments without Info

No comments that do not add information.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
int x;
```

</td><td>

```cs
// This is x
int x;`
```

</td></tr></table>

#### Avoiding Compiler Directives

Prefer not to use them, unless the code cannot run on a platform without excluding that piece of code. Otherwise a boolean variable might be preferred, a configuration setting or different concrete implementations of classes.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
if (config.FeatureXEnabled)
{
    // ...
}
```

</td><td>

```cs
#if FEATURE_X_ENABLED
    // ...
#endif
```

</td></tr></table>

__Reason:__

When using these compiling directives, a compilation might succeed, without all the code being compilable.

#### No Internal Members for Internal Classes

Prefer for internal classes not to have internal members.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`internal class A { `***`public`***`void B { } }`

</td><td>

`internal class A { `***`internal`***` void B { } }`

</td></tr></table>

__Reason:__

The members are automatically `internal` if the class is `internal`. When you wish to make the class `public`, you would not have to correct the access modifiers of the methods.

#### Default Switch Case at the Bottom

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

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

</td><td>

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

__Reason:__

The default switch case is often the 'last resort', so may make sense to be put 'last' too.

#### Prefer Value and HasValue for Nullable Types

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
int? number;
if (number.HasValue)
{
    string message = string.Format(
        "Number = {0}", number.Value);
}
```

</td><td>

```cs
int? number;
if (number != null)
{
    string message = string.Format(
        "Number = {0}", number);
} 
```

</td></tr></table>

#### No Unused / Outcommented Code

Prefer not to leave unused (or outcommented) code around. If needed, it might be moved it to an Archive folder, or Outtakes.txt.

__Reason:__

Unused code might clutter your vision or may make the suggestion that it was outcommented in error.

#### FileOpen, FileMode, FileAccess and FileShare

It is appreciated when a file stream is opened specifying all three aspects FileMode, FileAccess and FileShare explicitly with the most logical and most limiting values appropriate for the particular situation. Otherwise these aspects may use surprizing defaults.

### Misc Preferences

#### Test Class Names Ending With 'Tests'

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
[TestClass]
public class ValidatorTests()
{
    
}
```

</td><td>

```cs
[TestClass]
public class Tests_Validator()
{
    
}
```

</td></tr></table>

#### Test Method Names

Prefer to start test method names with `Test_` and do not hold back on underscores in the name.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
[TestMethod]
public void Test_Validator_NotNullOrEmpty_NotValid()
{ 
    ...
}
```

</td><td>

```cs
[TestMethod]
public void Test()
{ 
    ...
} 
```

</td></tr></table>

__Reason:__

If the test names mean to be descriptive, they might become long, and underscores to separate the 'pieces' may make it easier to read and digest.

#### Using var

Prefer not to use var.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
int x = y.X;
```

</td><td>

```cs
var x = y.X;
```

</td></tr></table>

__Reason:__

It would be nice to see the variable type in the code line instead of `var`. 

There may be a few exceptions, where var may be preferred

##### Anonymous Types

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`var`***` q = from x in list select new { A = x.A };`

</td><td>

***`X`***`q = from x in list select new { A = x.A };`

</td></tr></table>

##### New Statements

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`var`***` x = new X();`

</td><td>

***`X`***` x = new X();`

</td></tr></table>

##### Direct Casts

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`var`***` x = (X)y;`

</td><td>

***`X`***` x = (X)y;`

</td></tr></table>

##### Code Line Quite Long and Better Readable with var

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`foreach (`***`var`***` entry in dictionary)`

</td><td>

`foreach (`***`KeyValuePair<Canonical.ValidationMessage,  Tuple<NonPhysicalOrderProductList, Guid>>`***` entry in dictionary)`

</td></tr></table>

##### View Code

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`<% foreach (`***`var`***` order in Model.Orders) %>`

</td><td>

`<% foreach (`***`OrderViewModel`***` order in Model.Orders) %>`

</td></tr></table>

#### Null / Empty Strings

Prefer handling both null and empty string the same way.

#### String.IsNullOrEmpty

To check if a string is filled prefer `string.IsNullOrEmpty`. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
String.IsNullOrEmpty(str)
```

</td><td>

```cs
str == null 
```

</td></tr></table>

__Reason:__

In exceptional cases reference equality (`==`) can fail even if `strings` are equal.

#### String.Equals

To equate string prefer `string.Equals`. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
string.Equals(str, "bla")
```

</td><td>

```cs
str == "bla"
```

</td></tr></table>

__Reason:__

In exceptional cases reference equality (`==`) can fail even if `strings` are equal.

#### Avoiding Activator.CreateInstance

Prefer using the `new` keyword instead of `Activator.CreateInstance`. Using generics' `new` constraint  might avoid some of the `Activator.CreateInstance` calls.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
T = new T()
```

</td><td>

```cs
Activator.CreateInstance(typeof(T)) 
```

</td></tr></table>

A call to `Activator.CreateInstance` might be the last choice for instantiating an object.

#### Entity Equality by ID

Entity equality checks might be better done by ID than by reference.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
if (entity1.ID == entity2.ID)
// (Also do null checks if applicable.)
```

</td><td>

```cs
if (entity1 == entity2) 
```

</td></tr></table>

__Reason:__

Persistence frameworks do not always provide instance integrity, so code that compares identities may be less likely to break. 

#### CLR Data Types

Prefer using CLR-complient data types. Some aren't CLR-complient.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
int
long
byte
```

</td><td>

```cs
// Unsigned types such as:
uint
ulong
// And also:
sbyte
```

</td></tr></table>

#### Parameter Order

An idea for passing infrastructure-related parameters to constructors or methods, is to first list the entities (or loose values), then the persistence related parameters, then the security related ones, then possibly the culture, then other settings. 

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

#### Long Code Lines

It might be an idea to avoid long code lines.

#### Ordered If Range 

When evaluating a range in an `if`, it may be a good idea to mention the limits of the range and mention the start of the range first and the end of the range second.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
if (x >= 10 && x <= 100)
if (x > 10 && x < 100)
```

</td><td>

```cs
if (x <= 100 && x >= 10)
if (x >= 11 && x <= 99) 
```

</td></tr></table>

#### Namespace Tips

Full namespaces in code, might make the code line difficult to read:

__Less Preferred:__

```cs
JJ.Business.Cms.RepositoryInterfaces.IUserRepository userRepository =
    PersistenceHelper.CreatCmsRepository<JJ.Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

Using half a namespace might not be preferred either, because when you want to rename a namespace, it may generate more manual work.

__Less Preferred:__

```cs
Business.Cms.RepositoryInterfaces.IUserRepository userRepository = 
    PersistenceHelper.CreateCmsRepository<Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

An alternative is to give a class a more unique name. Or use an alias instead:

__Recommended:__

```cs
using IUserRepository_Cms = JJ.Business.Cms.RepositoryInterfaces.IUserRepository;
...

IUserRepository_Cms cmsUserRepository = PersistenceHelper.CreateCmsRepository<IUserRepository_Cms>(cmsContext);
```

### Member Order

Try giving the members in your code file a logical order, instead of putting them in a random order. Suggested possibilities for organizing your members:

|                      | |
|----------------------|-|
| Chronological        | When one method delegates to another in a particular order, you might order the methods chronologically.
| By functional aspect | When your code file contains multiple functionalities, you might keep the members with the same function together, and put a comment line above it.
| By technical aspect  | You may choose to keep your fields together, your properties together, your members together or group them by access modifier (e.g. public or private).
| By layer             | When you can identify layers of delegation in your class you might first list the members of layer 1, then the members of layer 2, etc.

The preferred ordering of members might be chronological if applicable and otherwise by functional aspect, but there are no rights and wrongs here. Whatever's  most appropriate for your code.

### Naming

See also: Casing, Punctuation and Spacing.

#### Boolean Names

Suggestions for boolean variable name prefixes and suffixes:

| Prefix / Suffix | Example         | Comment |
|-----------------|-----------------|---------|
| `Is...`         | `IsDeleted`     | Might be the most common prefix.
| `Must...`       | `MustDelete`    |
| `Can...`        | `CanDelete`     | Might indicate what *user* can do.
| `Has...`        | `HasRecords`    |
| `Are...`        | `AreEqual`      | For plural things.
| `Not...`        | `NotNull`       | A nice prefix, but perhaps be careful with negative names for readability’s sake. See ‘Double Negatives’.
| `Include...`    | `IncludeHidden` | Even though it is verb, it may make sense for booleans.
| `Exclude...`    |                 | "
| `...Exists`     | `FileExists`    |
| `Always...`     |                 |
| `Never...`      |                 |
| `Only...`       |                 |

If it looks wrong to put the prefix at the beginning, maybe put it in the middle, e.g.: `LinesAreCopied` instead of `AreLinesCopied`.

Some boolean names are so common, that they do not really need a prefix:

    Visible
    Enabled

#### Class Names

In this architecture, class names may end with the pattern name or a verb converted to a noun, e.g.:

    Converter
    Validator
    Calculator

And they may start with a term out of the domain (like `Order`, `Product` or `Price`):

    OrderConverter
    ProductValidator
    PriceCalculator

More specialized classes might get prefixes or suffixes (like `Optimized` or `WithPriorityShipping`):

    OptimizedPriceCalculator
    OrderValidatorWithPriorityShipping

Abstract classes might prefer the suffix `Base`:

    ProductValidatorBase

This is because it might be quite important to see in code whether something is a base class. Exceptions to the suffix idea might be made, if it would otherwise result in less readable code. For instance, base classes in entity models might not look good with the `Base` suffix.

Variables might be kept similar to the class names and include the prefixes and suffixes, so it stays clear what they are.

Some other suggested ‘last names’ for classes apart form the pattern names might be:

|              | |
|--------------|-|
| `Resolver`   | A class that does lookups that require complex keys or different ways of looking up depending on the situations, fuzzy lookups, etc.
| `Dispatcher` | A class that takes a canonical input, and dispatches it by calling different method depending on the input, or sending a message in a different format to a different infrastructural endpoint depending on the input.
| `Invoker`    | Something that invokes another method, possibly based on input or specific conditions.
| `Provider`   | A class that provides something. It can be useful to have a separate class that provides something if there are many conditions or contextual dependencies involved in retrieving something. A provider might also be used when something has to be retrieved conditionally or if retrieval has to be postponed until later.
| `Asserter`   | A class meant to throw exceptions under certain conditions.
|              | Any method verb might become a class name, by turning it into a verby noun, e.g. `Convert` => `Converter`.

#### Collection Names

Plural words are preferred for collections:

    Products
    Orders

Variable names for amounts of elements in the collection might be named:

    Count

So perhaps avoid plural words to denote a count and avoid plural words for things other than collections.

#### DateTime Names

A `DateTime` property might be suffixed with `Utc` or `Local`:

    StartDateLocal
    OrderDateTimeUtc

An alternative suffix for `DateTimes` could be `When`:

    ModifiedWhen
    OrderedWhen

But that might look less nice when you add the Local and Utc suffices again:

    ModifiedWhenUtc
    OrderedWhenLocal

#### Enum Names

This architecture tends to use the `Enum` suffix for enum types e.g. `OrderStatus`***`Enum`***.

Another alternative might be the suffix `Mode`, e.g. `Connection`***`Mode`***, but at some point the preference really went for the suffix `Enum` because it was found so important to see clearly that something is an enum.

#### Event Names / Delegate Names

Event names and delegate names, that indicate what just happened might have the following form:

    Deleted
    TransactionCompleted

Event names and delegate names, that indicate what is about to happen might have the following form:

    Deleting
    TransactionCompleting

User-initiated events might not follow that pattern:

    Click
    DoubleClick
    KeyPress

Delegate names might also have the suffix `Callback` or `Delegate`:

    ProgressInfoCallback
    AddItemDelegate

Sometimes the word `On` might beused:

    OnSelectedIndexChanged
    OnClick

Or the prefix `Handle`:

    HandleMouseDown

Or the suffix `Requested`, if your event looks like a method name.

    RemoveRequested

Pardon the ambiguity, but the naming above can be used for the names of events, but some of them also serve well as names for methods that fire/emulate or otherwise handle the event. The prefix `On` for instance and the prefix `Handle` may very well be used for the methods that actually raise the event. `Fire` and `Do` might also be alternatives.

Perhaps avoid event names that use two event-indications in the name. For instance `OnDragging` might be shortened to just `Dragging` or `OnDrag`, because `ing` and `On` both already indicate that it's an event. `OnMouseUp` might be shortened to just `MouseUp`, because that is an established event name.

#### Method Names

Method names might start with verbs, e.g. ***`Create`***`Order`. 

For clarity, perhaps not start with a verb for constructs that are not methods.

Suggestions for verbs:

| Verb        | Description |
|-------------|-------------|
| `Add`       | E.g.<br>`List.Add(item)`<br>`ListManager.Add(list, item)`<br>In cases such as the last example, `list` may be made the first parameter.
| `Assert`    | A method throwing `Exceptions` if input is invalid.
| `Calculate` |
| `Clear`     |
| `Convert`   |
| `ConvertTo` |
| `Create`    | When a method returns a new object.
| `Delete`    |
| `Ensure`    | May set up a state if it is not set up yet.<br>If `Ensure` means throw an exception if a state is not there, consider using the verb `Assert` instead.
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
| `Validate`  | A method for generating *validation messages* for user-input errors.

[ ... ]

#### File-Related Variable Names

Variable names that indicate parts of file paths might easily become ambiguous. Here is a list of names that might disambiguate it all:

| Name                     | Value          
|--------------------------|--------------------------
| `FileName`                 | `"MyFile.txt"`
| `FilePath`                 | `"C:\MyFolder\MyFile.txt"`
| `FolderPath`               | `"C:\MyFolder"`
| `SubFolder`                | `"MyFolder"`
| `RelativeFolderPath` or<br>`SubFolder` or<br>`SubFolderPath`) | `"MyFolder\MyFolder2"`
| `RelativeFilePath`         | `"MyFolder\MyFile.txt"`
| `FileNameWithoutExtension` | `"MyFile"`
| `FileExtension`            | `".txt"`
| `AbsoluteFilePath`         | `"C:\MyFolder\MyFile.txt"`
| `AbsoluteFolderPath`       | `"C:\MyFolder"`
| `AbsoluteFileName`         | DOES NOT EXIST
| `FileNamePattern`,<br>`FilePathPattern`, etc.<br>With wildcards like * and ? | `*.xml`<br>`C:\temp\BLA_????.csv`
| `FileNameFormat`,<br>`FilePathFormat`, etc.<br>With placeholders like {0} and {0:dd-MM-yyyy} | `order-{0}.txt`<br>`orders-{0:dd-MM-yyyy}\*.*`

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
