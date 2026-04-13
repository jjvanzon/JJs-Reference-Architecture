`[ Draft ]`

📔 Doc-Only Members
====================


A way to centralize and reuse comments: a technique to improve code documentation.

- [What are XML Doc Comments?](#what-are-xml-doc-comments)
- [Declutter Your Code!](#declutter-your-code)
- [Stop Repeating the Comments!](#stop-repeating-the-comments)
- [Inherit the Comments!](#inherit-the-comments)
- [Reuse the Comments!](#reuse-the-comments)
- [Reuse the Comments Anywhere!](#reuse-the-comments-anywhere)
- [Say "No" to Bewildering Links](#say-no-to-bewildering-links)
- [Say "No" to XPaths](#say-no-to-xpaths)
- [Unobtrusive Doc-Only Members](#unobtrusive-doc-only-members)
    - [Docs Namespace](#docs-namespace)
    - [Public](#public)
    - [Object Browser](#object-browser)
    - [Structs](#structs)
    - [Naming Style](#naming-style)
- [Make 'Em Play Nicely](#make-em-play-nicely)
    - [Shipping](#shipping)
    - [Warnings as Errors](#warnings-as-errors)
    - [Naming Rules](#naming-rules)
    - [Namespace != Folder](#namespace--folder)
    - [Param Tag Mismatch](#param-tag-mismatch)
- [Conclusion](#conclusion)
- [~ Drafts](#-drafts)
    - [~ AI Aided](#-ai-aided)
    - [~ Missing XML Comments](#-missing-xml-comments)
    - [~ Deprecated Members](#-deprecated-members)
    - [~ Efficiency](#-efficiency)
    - [~ For Social Post](#-for-social-post)
    - [~ Snippets](#-snippets)


### What are XML Doc Comments?

XML doc comments are those special comments, that pop up when you hover classes, methods or properties in your code:

<img alt="Tooltip with comment shows while hovering method name" src="xml-doc-comment-tooltip.png" width="500" />

You can add them to your own code, so instant documentation pops up while you program.

Here's an example method:

```cs
string StartWithCap(string input)
{
    if (input.Length == 0) return input;
    return input.Left(1).ToUpper() + input.CutLeft(1);
}
```

Do you know what it does? Great, but this might help:  
Here's an XML doc comment added to it:

```cs
/// <summary>
/// Changes the first character to upper case:
/// <code>
/// StartWithCap("test") = "Test"
/// </code>
/// </summary>
string StartWithCap(string input)
{
    if (input.Length == 0) return input;
    return input.Left(1).ToUpper() + input.CutLeft(1);
}
```

This helps you remember what it's for. Even if you have not much to write about it, a slight addition of detail can make the reader get an "aha" moment of recognition.

There's a lot of other options for writing doc comments, we'll be covering in this article.

### Declutter Your Code!

See, the doc comments are placed with the code, and it can get quite crowded with them.

I couldn't even give a good example of that: it'd overpower this very post as much as it'd overpower your code. Here's a bad example anyway:

```cs
/// <summary>
/// Returns the left part of a string.
/// Can return less characters than
/// the length provided if string is shorter.
/// </summary>
string TakeStart(string input, int length)
{
    if (length > input.Length) length = input.Length;
    return input.Left(length);
}

/// <summary>
/// Takes the part of a string until the specified delimiter. 
/// Excludes the delimiter itself.
/// </summary>
string TakeStartUntil(string input, string until)
{
    if (until == null) throw new ArgumentNullException(nameof(until));
    int index = input.IndexOf(until, StringComparison.Ordinal);
    if (index == -1) return "";
    string output = input.Left(index);
    return output;
}

/// <summary>
/// Takes the part of a string until the specified delimiter. 
/// Excludes the delimiter itself.
/// </summary>
string TakeStartUntil(string input, char until)
{
    return TakeStartUntil(input, until.ToString());
}
```

It can happen though that you can hardly see the code itself through all the comment.

### Stop Repeating the Comments!

Comments would also ofter get repeated.  
In the former example you can already spot some:

```cs
/// <summary>
/// Takes the part of a string until the specified delimiter.
/// Excludes the delimiter itself.
/// </summary>
string TakeStartUntil(string input, string until);

/// <summary>
/// Takes the part of a string until the specified delimiter.
/// Excludes the delimiter itself.
/// </summary>
string TakeStartUntil(string input, char until);
```

They have exactly the same comment! But there are ways to avoid the clutter.

### Inherit the Comments!

One way is to use `<inheritdoc />`, which takes over the doc comment from the `base` class or method. This is a shortcut to avoid repeating the comment:

```cs
/// <inheritdoc />
class Rectangle : Element
{
    /// <inheritdoc />
    Rectangle(Element parent) : base(parent) { }
}
```

It makes the `Rectangle` class inherit comments of its base class  `Element`. Here it is with its comments, that we didn't need to repeat in the `Rectangle` class:

```cs
/// <summary>
/// VectorGraphics element that can contain
/// VectorGraphics child elements.
/// </summary>
class Element
{
    /// <summary>
    /// VectorGraphics element that can contain
    /// VectorGraphics child elements.
    /// </summary>
    /// <param name="parent">
    /// When in doubt, use Diagram.Background.
    /// </param>
    public Element(Element parent) { }
}
```

Here's a resulting IntelliSense tool tip:

<img alt="Screen shot of code with tool tip showing an inherited doc" src="image-1.png" width="500" />

But there's still repeated comments in the `Element` base class! Oh no! Now what?

### Reuse the Comments!

`<inheritdoc>` is very flexible. You can use the `cref` attribute to point at any member you want to take. This takes over its comments. This comes out handy for our constructor to brush away any repeated text:

```cs
/// <summary>
/// VectorGraphics element that can contain
/// VectorGraphics child elements.
/// </summary>
/// <param name="parent">
/// When in doubt, use Diagram.Background.
/// </param>
class Element
{
    /// <inheritdoc cref="Element" />
    public Element(Element parent) { }
}
```

There the inner constructor `inherits` the doc from the `Element class`, because we added the `cref` attribute pointing to it. Here's a resulting tool tip:

<img src="image-2.png" width="500" />

Yes, it's lazy! But efficient. But it's still quite cluttered with comment. Half the code above is still comment.

### Reuse the Comments Anywhere!

To find our code back, we can use a trick, making things much easier. I like to put my comments in a file called `docs.cs`:

```cs
/// <summary>
/// VectorGraphics element that can contain
/// VectorGraphics child elements.
/// </summary>
/// <param name="parent">
/// When in doubt, use Diagram.Background.
/// </param>
struct _element;
```

Those are our __Doc-Only Members__. Here they are referenced with `inheritdoc`:

```cs
/// <inheritdoc cref="_element" />
class Rectangle : Element
{
    /// <inheritdoc cref="_element" />
    Rectangle(Element parent) : base(parent) { }
}

/// <inheritdoc cref="_element" />
class Element
{
    /// <inheritdoc cref="_element" />
    public Element(Element parent) { }
}
```

Now each documentation is just a single unobtrusive line with an `inheritdoc` tag.

This way the code doesn't get cluttered with comments, the `inheritdoc` `cref`'s are simple, it's easier to reuse the same doc comment for efficiency, and have meaningful, helpful pop ups all over the place. The docs are now central, which makes it a lot easier to review, revisise and maintenance them.

It's my preferred way of doing it now.

<img src="image-3.png" width="500" />

### Say "No" to Bewildering Links

These `cref` links can get wild if you're dealing with overloads and, oh boy, generics:

`[ Example code: with complicated cref to a generic overload ]`

With centralized doc comments, we've snuck by that beast completely:

`[ Example code: both generic and non-generic refer to one doc-only member with simple crefs. ]`

### Say "No" to XPaths

There is an alternative: Storing docs in separate XML files:

`[ Example XML of that. ]`

You can then link to that XML with XPath

`[ Links to XML file from code ]`

But those links are convoluted and can break easily. How's that going to look for generics? I don't even want to know. Centralizing docs in code seems a stronger alternative.

### Unobtrusive Doc-Only Members

#### Docs Namespace

I like to give the docs their own sub-namespace `.docs`

```cs
namespace JJ.Demos.Architecture.docs;

/// <summary>...</summary>
struct _element;
```

This keeps them from cluttering the main namespace.

To use the `docs` you'd add a `using` statement to `GlobalUsings.cs`:

```cs
global using JJ.Demos.Architecture.docs;
```

Or to each code file, like `Element.cs`, you can just add `using docs`:

```cs
namespace JJ.Demos.Architecture;

using docs;

/// <inheritdoc cref="_element" />
class Element;
```
#### Public

I actually like to make the docs-structs `public`:

```cs
/// <summary>...</summary>
public struct _element;
```

That way a `docs` namespace gives an overview of the documentation accessible from outside your assembly.

#### Object Browser

For instance you can inspect them in the `Object Browser` as a whole, and other people can too:

`[Screen shot]`

#### Structs

The choice to use `structs` is for camouflage. They are usually displayed in an unassuming green, making the `<inheritdoc>'s` blend in the background, so the code itself pops out.

`[ TODO: Screen shot ]`

This in contrast to crefs to actual code elements, which might be colored like that code element, which makes them very confusing to me:

`[ TODO: Screen shot ]`

#### Naming Style

The naming format is also specifically chosen to make the `<inheritdoc/>` tags as unobtrusive as possible.

Consider the follow docs only member called `_mydoc`:

```cs
/// <summary> ... </summary>
struct _myprop;
```

Not only do the lower case letters not stand out as much. This and the underscore prevent the name from colliding with the actual code elements:

<img src="image-7.png" width="200" />

### Make 'Em Play Nicely

#### Shipping

To ship the docs along with your NuGet package you can add this to your csproj file:

```xml
<GenerateDocumentationFile>True</GenerateDocumentationFile>
```
To actually generate the package you add: 

```xml
<GeneratePackageOnBuild>True</GeneratePackageOnBuild>`
```

#### Warnings as Errors

You may get some warnings you might need to deal with. I like to make things worse, by treating all warnings as errors. You can do this too by adding the following to your `.csproj`:

```xml
<TreatWarningsAsErrors>True</TreatWarningsAsErrors>
```

Now you can be sure, that it won't compile, until we've solved our self-imposed problems.

#### Naming Rules

Then the system might start bickering about things, like naming rule violations:

<img src="image-5.png" width="500" />

See the [Naming Style](#naming-style) section explains why we use names like that. There's no way to configure a naming rule specifically for doc-only elements, so I like to squelch that warning at the top of the file:

`#pragma warning disable IDE1006 // naming rule`

#### Namespace != Folder

The namespace where we put the docs is violating another naming rule. We  put a `docs` namespace in the `JJ.Demos.Architecture` folder, which does not include the `docs` sub-folder, which can get you another nag from the compiler:

<img src="image-4.png" width="500" />

Squelch as follows:

`#pragma warning disable IDE0130 // namespace != folder`

Since we only use one docs file per project, at least we can just squelch these pesky warnings with a single line each.

#### Param Tag Mismatch

Another warning you can get, has to do with the docs-only members not actually having the parameters you defined:

<img src="image-6.png" width="500" />

I'd just squelch that warning at the top of the code file:

```cs
#pragma warning disable CS1572 // param tag mismatch
```

The members you use it for will still use the `param` doc anyway.

Downside: You do lose the checks on param existence this way. That's a trade-off. You get a whole lot back for it though, like centralized reusable docs, maintainability and clarity of the code.

### Conclusion

Eventually I settled on this doc-only member trick, which has been serving me very well ever since. I hope you can use it too. The end result: centralized comments, efficiently written, without repetitions or code clutter.

-----

### ~ Drafts

#### ~ AI Aided

<h4>Missing Doc Lenience</h4>

* If you’re mid-transition, you can squelch missing doc warnings in your project file:

```xml
<PropertyGroup>
  <WarningsAsErrors>$(WarningsAsErrors);1591</WarningsAsErrors>
</PropertyGroup>
```

#### ~ Missing XML Comments

Compiler Nags. Squelch missing doc comment warnings temporarily.

#### ~ Deprecated Members

`[Obsolete]`, compiler error option, friction with `WarningAsErrors`. Alternative `<b>[Deprecated]</b>` in XML doc comment.

If you mark a member as `[Obsolete]` and treat warnings as errors, you can create friction in your build process.
An alternative: add `<b>[Deprecated]</b>` in the doc comment for softer, visible warnings that don’t break the build.

#### ~ Efficiency

- [ ] Converting README to doc comments manually.
- [ ] Using generalized comment for multiple elements = efficiency.
- [ ] See refs, mark up: do or do not (efficiency, quality, take your pick)
- [ ] params, etc? leave out for efficiency? Take your pick. Main descriptions = most important.

You can copy key parts of your README directly into your doc comments, or use a generalized comment for several elements when they share a description.
This keeps documentation efficient.

Decide for yourself:
Do you want detailed `<param>` tags for everything, or just the essentials? Sometimes, focusing on the main description is more practical.

#### ~ For Social Post

Hi there developers! And hello to you normal people too. Another technical update (again). Ever opened a file and thought: "Where’s the code? Oh, buried in the middle of all these comments."

This time I want to talk about "doc-only members": a pattern to improve code documentation.

I had way too much to say about it, so I put together an article with all the details.

👉 XML Doc Comments: `[ TODO: Link ]`

#### ~ Snippets

You can check your documentation in the Object Browser and see it all in one place.

This is all fine and well, but our code may still feel cluttered with all those comments, because eventually, they've got to live somewhere. 

What shall we do...?

("Where is my code?")

Can't find the code through all the comments...
("Where'd the code go?") 

To solve the remaining hacky `crefs` 

It's hiding behind that wall.

Where'd the code go?

Want to read more about software development techniques for .NET and C#? Here's the forever unfinished resource, where I brain-dump more of these things inside neat looking web pages. You can find explanations, reasons and code samples of all sorts of patterns and techniques both mainstream or made up:

`[ Link to JJ's Software Architecture Page ]`


`[ This fits better in the Comments section in patterns/other.md ]`

Not all doc comments are useful.
A bad comment looks like this:

```csharp
/// <summary>
/// This is the text.
/// </summary>
```
