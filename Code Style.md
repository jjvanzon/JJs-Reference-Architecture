JJ's Reference Architecture
===========================

Code Style
----------

<h3>Contents</h3>

- [Code Style](#code-style)
    - [Introduction](#introduction)
    - [Casing (and Punctuation)](#casing-and-punctuation)
    - [Spacing (and Punctuation)](#spacing-and-punctuation)
        - [Auto-Formatting](#auto-formatting)
        - [Surplus Enters Between Braces](#surplus-enters-between-braces)
        - [Enters between Switch Cases](#enters-between-switch-cases)
        - [No Braces for Single-Line If Statements](#no-braces-for-single-line-if-statements)
        - [Loops Multi-Line](#loops-multi-line)
        - [Braces for Multi-Line Statements](#braces-for-multi-line-statements)
        - [Enters between Methods](#enters-between-methods)
        - [Properties on Separate Lines](#properties-on-separate-lines)
        - [Enters in Methods](#enters-in-methods)
        - [Variables on Separate Lines](#variables-on-separate-lines)
        - [Tabular Form Less Preferred](#tabular-form-less-preferred)
        - [Align Elements of Linq Queries](#align-elements-of-linq-queries)
        - [Indentation](#indentation)
        - [Generic Constraints on Separate Line](#generic-constraints-on-separate-line)
        - [Generic Constraints on Same Line for One-Liners](#generic-constraints-on-same-line-for-one-liners)
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
        - [Public Members for Internal Classes](#public-members-for-internal-classes)
        - [Default Switch Case at the Bottom](#default-switch-case-at-the-bottom)
        - [Prefer Value and HasValue for Nullable Types](#prefer-value-and-hasvalue-for-nullable-types)
        - [No Unused / Outcommented Code](#no-unused--outcommented-code)
        - [FileOpen, FileMode, FileAccess, FileShare](#fileopen-filemode-fileaccess-fileshare)
    - [Misc Preferences](#misc-preferences)
        - [Test Class Names to End with 'Tests'](#test-class-names-to-end-with-tests)
        - [Test Method Names](#test-method-names)
        - [Using Var](#using-var)
            - [Anonymous Types](#anonymous-types)
            - [New Statements](#new-statements)
            - [Direct Casts](#direct-casts)
            - [Code Line Quite Long and Better Readable](#code-line-quite-long-and-better-readable)
            - [View Code](#view-code)
        - [Null / Empty Strings](#null--empty-strings)
        - [String.IsNullOrEmpty](#stringisnullorempty)
        - [String.Equals](#stringequals)
        - [Activator.CreateInstance](#activatorcreateinstance)
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
        - [Prefixes and Suffixes](#prefixes-and-suffixes)
        - [Misc Names](#misc-names)

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

#### Auto-Formatting

Prefer Visual Studio’s autoformatting enabled and set to its defaults.

Reason: Less surprizing to the next developer.

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

Reason: More tidy.

#### Enters between Switch Cases

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

Reason: A bit tidier?

#### No Braces for Single-Line If Statements

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
if (condition) Bla();
```
 
</td><td>

```cs
if (condition) { Bla(); }
```

</td></tr></table>

Reason: Less visual clutter.

#### Loops Multi-Line

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

Reason: May look odd if you're not used to it.

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
    Something();
```

</td></tr></table>

Reason:  
Without braces, only the next line is looped or executed conditionally. The line after that would be outside the loop, which is sort of not obvious and might lead to error.

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

Reason: Just a bit more tidy?

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

Reason:  
Might be easy to overlook that there is another property.

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

Reason:  
Visible separation of steps inside methods.

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

Reason:  
Just a preference, when not used to it, it may be overlooked, and "Find..." may not show the results expected.

#### Tabular Form Less Preferred

Tabular form not preferred.

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

Reason:  
This tabular form might be undone by auto-formatting. It may look nice, but maybe get your eyes used to non-tabular form instead.

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

Reason: readability.

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

Reason: Just readability.

#### Generic Constraints on Separate Line

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

Reason: So they stand out.

#### Generic Constraints on Same Line for One-Liners

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

Reason:  
It might have been a one-liner for readability reasons so perhaps let's keep it that way.

### Trivial Recommendations

#### A File a Type 

Preferably give each class (or interface or enum) its own file (except nested classes).

Reason:  
One might be surprized to find types hidden away behind a single file name. It may harm the overview of the different pieces of code.

#### Members Private

Prefer keeping members private. 

```cs
private void Bla()
{
    ...
}
```

Reason:  
Other code might become dependent on publically accessible things. Managing dependencies like that seems to be quite a thing in software programming.

#### Types Internal 

Prefer to keep types `internal`.

```cs
internal class MyClass
{
    ...   
}
```

Reason:    
External things might otherwise become dependent on code, that was not meant to have so many links to it. Managing dependency between parts seems quite a concern in software programming.

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

Reason:  
One might be confused what is the default.

#### No Public Fields

Prefer not to use public fields. Use either private fields or use properties instead. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`public`***` int X `***`{ get; set; }`***

</td><td>

`public int X;`

</td></tr></table>

Reason:  
People may say the interface stability comes in jeopardy when you use public fields. The fields seem to look similar from the outside. However, frameworks may expect properties, not fields, which makes letting fields participate in reusable functions less easily. Perhaps compatibility like that is an argument.

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

Reason:  
It may not be obvious there are nested classes, unless they are put at the top.

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

Reason:  
`Exception` handling is more performance intensive than might be expected. Compared to an `if` statement. When no `exception` goes off, `exception` handling might perform well, but when an `exception` goes off, quite a few things happen, like gathering stack trace information.

#### No Inferrable Type Arguments

Prefer not to use type arguments that can be inferred. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`References(x => x.Child)`

</td><td>

`Reference`***`<Child>`***`(x => x.Child)`

</td></tr></table>

Reason: Less visual clutter.

#### Prefer Interface Types

Prefer interface types as variable types. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

***`IList`***`<int> list = new List<int>;`

</td><td>

***`List`***`<int> list = new List<int>; `

</td></tr></table>

Reason: Less refactoring when changing interface type.

#### Prefer ToArray

Prefer `ToArray` over `ToList`. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`IList<int> collection = x.`***`ToArray`***`();`

</td><td>

`IList<int> collection = x.`***`ToList`***`();`

</td></tr></table>

Reason: More performance?  
Downside: The Add method does not work for an Array.

#### Object Initializers

Using object initializers. 

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

Reason: Might be more readable.

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

Reason:  
Your comment might be valuable to see from the outside, when hovering over the name of the member.

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

Reason: English is sort of the main language in IT. Broader reach of people might be able to read your comments.

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

Reason: Less visual clutter. Gives things to read that don't seem worth it.

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

Reason:  
When using these compiling directives, a compilation might succeed, without all the code being compilable.

#### Public Members for Internal Classes

Prefer for internal classes not to have internal members.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`internal class A { `***`public`***`void B { } }`

</td><td>

`internal class A { `***`internal`***` void B { } }`

</td></tr></table>

Reason:  
The members are automatically `internal` if the class is `internal`. When you wish to make the class `public`, you would not have to manually correct the access modifiers of the methods.

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

Reason:  
The default `switch` case is often the 'last resort', so may make sense to be put 'last' too.

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

Reason:  
Changing the variable to type object, changes how the code behaves.

#### No Unused / Outcommented Code

Prefer not to leave unused (or outcommented) code around. If needed, it might be moved it to an `Archive` folder, or `Outtakes.txt`.

Reason:  
Unused code might clutter your vision or may make the suggestion that it was outcommented in error.

#### FileOpen, FileMode, FileAccess, FileShare

It is appreciated when a file stream is opened specifying all three aspects `FileMode`, `FileAccess` and `FileShare` explicitly with the most logical and most limiting values appropriate for the particular situation.

Reason:  
Otherwise these aspects may have surprizing defaults.

### Misc Preferences

#### Test Class Names to End with 'Tests'

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

Reason: Just convention.

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

Reason:  
If the test names mean to be descriptive, they might become long, and underscores to separate the 'pieces' may make it easier to read and digest.

#### Using Var

Prefer not to use `var`.

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
int x = y.X;
```

</td><td>

```cs
var x = y.X;
```

</td></tr></table>

Reason:  
It would be nice to see the variable type in the code line instead of `var`.  
There may be a few exceptions, where var may be preferred, when the type is sort of obvious, or it might be more readable.

##### Anonymous Types

<table><tr><th>Recommended</th><th>Not Preferred</th></tr><tr><td>

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

##### Code Line Quite Long and Better Readable

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
foreach (var entry in dictionary)
```

</td><td>

```cs
foreach (KeyValuePair<Canonical.ValidationMessage,  
         Tuple<NonPhysicalOrderProductList, Guid>> entry in dictionary)
```

</td></tr></table>

##### View Code

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

`<% foreach (`***`var`***` order in Model.Orders) %>`

</td><td>

`<% foreach (`***`OrderViewModel`***` order in Model.Orders) %>`

</td></tr></table>

#### Null / Empty Strings

Prefer handling both `null` and `""` the same way.

Reason: No surprises when using eithwe `null` or `""`.

#### String.IsNullOrEmpty

To check if a `string` is filled prefer `string.IsNullOrEmpty`. 

<table><tr><th>Recommended</th><th>Less Preferred</th></tr><tr><td>

```cs
String.IsNullOrEmpty(str)
```

</td><td>

```cs
str == null 
```

</td></tr></table>

Reason:  
In exceptional cases reference equality (`==`) may fail even if strings are equal.

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

Reason:  
In exceptional cases reference equality (`==`) may fail even if strings are equal.

#### Activator.CreateInstance

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

Reason:  
New statements are strongly typed and less likely to fail.

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

Reason:  
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

Reason:
For compatibility with more variations of .NET.

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

Reason: readability.

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

Reason: Readability. More ofvious what the range limits are.

#### Namespace Tips

Full namespaces in code, might make the code line difficult to read:

__Less Preferred__

```cs
JJ.Business.Cms.RepositoryInterfaces.IUserRepository userRepository =
    PersistenceHelper.CreatCmsRepository<JJ.Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

Using half a namespace might not be preferred either, because when you want to rename a namespace, it may generate more manual work.

__Less Preferred__

```cs
Business.Cms.RepositoryInterfaces.IUserRepository userRepository = 
    PersistenceHelper.CreateCmsRepository<Business.Cms.RepositoryInterfaces.IUserRepository>(cmsContext);
```

An alternative is to give a class a more unique name. Or use an alias instead:

__Recommended__

```cs
using IUserRepository_Cms = JJ.Business.Cms.RepositoryInterfaces.IUserRepository;
...

IUserRepository_Cms cmsUserRepository = PersistenceHelper.CreateCmsRepository<IUserRepository_Cms>(cmsContext);
```

Reason:  
Long visually cluttered code lines might be harder to read.

### Member Order

Try giving the members in your code file a logical order, instead of putting them in a random order. Suggested possibilities for organizing your members:

|                      | |
|----------------------|-|
| Chronological        | When one method delegates to another in a particular order, you might order the methods chronologically.
| By functional aspect | When your code file contains multiple functionalities, you might keep the members with the same function together, and put a comment line above it.
| By technical aspect  | You may choose to keep your fields together, your properties together, your members together or group them by access modifier (e.g. public or private).
| By layer             | When you can identify layers of delegation in your class you might first list the members of layer 1, then the members of layer 2, etc.

The preferred ordering of members might be chronological if applicable and otherwise by functional aspect, but there are no rights and wrongs here. Whatever's most appropriate for your code.

### Naming

See also: Casing, Punctuation and Spacing.

Reasons for naming conventions might just be knowing what kind of system elements they are really.

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

If it looks wrong to put the prefix at the beginning, maybe put it in the middle, e.g.: 

    LinesAreCopied

instead of 
    
    AreLinesCopied

Some boolean names are so common, that they might not need a prefix:

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

This architecture tends to use the `Enum` suffix for enum types e.g. `OrderStatusEnum`.

Another alternative might be the suffix `Mode`, e.g. `ConnectionMode`, but at some point the preference really went for the suffix `Enum` because it was found so important to see clearly that something is an enum.

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

Perhaps avoid event names that use two event-indications in the name. For instance `OnDragging` might be shortened to just `Dragging` or `OnDrag`. `OnMouseUp` might be shortened to just `MouseUp`, because that would be an established event name.

#### Method Names

Method names might start with verbs, e.g. `CreateOrder`. 

For clarity, perhaps not start with a verb for constructs that are not methods.

Suggestions for verbs:

| Verb        | Description |
|-------------|-------------|
| `Add`       | `List.Add(item)`<br>`ListManager.Add(list, item)`<br>In cases such as the last example, `list` may be made the first parameter.
| `Assert`    | A method throwing `Exceptions` if input is invalid.
| `Calculate` |
| `Clear`     |
| `Convert`   |
| `ConvertTo` |
| `Create`    | When a method returns a new object.
| `Delete`    |
| `Ensure`    | May set up a state if it is not set up yet.<br>If `Ensure` means throw an exception if a state is not there,<br>consider using the verb `Assert` instead.
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

#### File-Related Variable Names

Variable names that indicate parts of file paths might easily become ambiguous. Here is a list of names that might disambiguate those:

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

#### Prefixes and Suffixes

| Example                  | Description
|--------------------------|--------------------
| `source...`<br>`dest...` | In code that converts one structure to the other, it might ben clear to use the prefixes `source` and `dest` consistently in the variable names to keep track of where data comes from and where to it goes.
| `existing...`            | Denoting that something already existed (in the data store) before starting a transaction.
| `new...`                 | Denoting that the object was just newly created.
| `original...`            | Denoting that this is an original value that was (temporarily) replaced.
| `...WithRelatedEntities`<br>`...WithRelatedObjects` | Indicating that not only a single object is handled, but the object including the underlying related objects.
| `Versatile...`           | A class that might handle a multitude of types or situations.
| `...With...`             | When making a specialized class that works well for a specific situation, you could use the word `With` in the class name e.g.:<br> `CostCalculator`<br>`CostWithTaxCalculator`
| `...Polymorphic`         | Handling a multitude of differrent (derived) types, possibly each in a different way.
| `...IfNeeded`            | If something is executed conditionally. This might be a nice alternative for a possibly less pretty suffixes like `Conditionally` or a prefix `Conditional`, that might obscuring the name that comes after.
| `...Unsafe`              | When it lacks e.g. thread-safety or executes unmanaged code, or lacks some checks.
| `...Recursive`           | (Some people tend to use `Recursively` instead, probably insisting it is better grammer, but `Recursive` is shorter and not grammatically incorrect either. It is a characteristic, as in ‘Is it *recursive*?’.)
| `To...`                  | For conversion from one to another thing. Sometimes `this` is source of the conversion, for example:<br>`array.ToHashSet()`<br>Perhaps less commonly the `To` prefix is used when the `this` is not the source, for instance:<br>`MyConverter.ToHashSet(object[] array)`<br>The `Convert` or `ConvertTo` verbs might be more appropriate there:<br>`MyConverter.ConvertToHashSet(object[] array)`<br>
| `From...`                | For conversion from one to another thing. A lot like `To...` executed on the dest object instead:<br>`dest.FromSource(source)`<br>The `To...` prefix might be more common, and possibly more readable.

#### Misc Names

For number sequences these names might be used:

    ListIndex
    IndexNumber
    SortOrder
    Rank

Perhaps avoid `Index` because it is an SQL keyword.
