`[ Draft ]`

📖 Doc-Only Members
====================

Hi there developers! And hello to you normal people too. Another technical update (again).

This time I want to talk about "doc-only members": a technique to improve code documentation.
It's way to centralize and reuse the comments in your code.

- [What are XML Doc Comments?](#what-are-xml-doc-comments)
- [Declutter Your Code!](#declutter-your-code)
- [Stop Repeating Comments!](#stop-repeating-comments)
- [Inherit the Comments!](#inherit-the-comments)
- [Reuse the Comments!](#reuse-the-comments)
- [Reuse the Comments Anywhere!](#reuse-the-comments-anywhere)
- [Bad Example: Bewildering Links](#bad-example-bewildering-links)
- [Why Not XML Files and XPaths?](#why-not-xml-files-and-xpaths)
- [Conclusion](#conclusion)
- [2025-07-04 ~ Topics to Cover](#2025-07-04--topics-to-cover)
- [2025-07-04 ~ Postponed Social Post XML Doc Comments - AI Aided Draft](#2025-07-04--postponed-social-post-xml-doc-comments---ai-aided-draft)
- [Outtakes](#outtakes)


### What are XML Doc Comments?

XML doc comments are special comments, that light up when you hover the code elements.

<img alt="Tooltip with comment shows while hovering method name" src="xml-doc-comment-tooltip.png" width="450" />

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

There's a lot of other options for writing doc comments, but we'll not be covering all of those. This post focuses on a high-level way of managing these doc comments.

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

### Stop Repeating Comments!

Comments would a;so ofter get repeated.  
In the former example you can already spot some repeated comments:

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

One way is to use `<inheritdoc />`, which usually takes over the doc comments from the `base` type or `base` method. This is a shortcut to repeat the comment or not to write comment at all:

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

![Screen shot of code with tool tip showing an inherited doc](image-1.png)

But there's still repeated comments in the `Element` base class! Oh no! Now what?

### Reuse the Comments!

`<inheritdoc>` is very flexible. You can use the `cref` attribute to point at any member you want to take over its comments. This comes out handy for our constructor to brush away any repeated text:

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

![](image-2.png)

Yes, it's lazy! But efficient. But the code still has those little barriers of comments that'll stop you from reading the code.

### Reuse the Comments Anywhere!

To find our code back, we can use a trick, making things much easier:

`[ Example code: with doc-only members in one file and inheritdoc to it from the actual code. ] `

Here I define doc-only members and then `inheritdoc` from them. That way the code doesn't get cluttered with the comments, the inheritdoc cref's are simple, it's easier to reuse the same doc comment for efficiency, and still have meaningful helpful pop ups all over. The docs are now central, which makes reviews, revisions and maintenance a lot easier.

### Bad Example: Bewildering Links

These `cref` links can get wild if you're dealing with overloads and, oh boy, generics:

`[ Example code: with complicated cref to a generic overload ]`

With centralized doc comments, we've snuck by that beast completely:

`[ Example code: both generic and non-generic refer to one doc-only member with simple crefs. ]`

### Why Not XML Files and XPaths?

There is an alternative, which is putting the doc comments in XML files, but that replaces the crefs with convoluted XPaths that are also easy to break.

`[ Example XML of that. ]`

How's that going to look for generics? I don't even want to know.

### Conclusion

Eventually I settled on this doc-only member trick which has been serving me very well ever since.

### 2025-07-04 ~ Topics to Cover

- GenerateDocumentationFile
- Warnings as errors
- Squelch missing doc comment warnings temporarily.
- Several XML doc comments-related warnings.
- Centralized warning squelches.
- Deprecated members: `[Obsolete]`, compiler error option, friction with `WarningAsErrors`.
  Alternative `<b>[Deprecated]</b>` in XML doc comment.

- Converting README to doc comments manually.
- Using generalized comment for multiple elements = efficiency.
- See refs, mark up: do or do not (efficiency, quality, take your pick)
- params, etc? leave out for efficiency? Take your pick. Main descriptions = most important.

- Syntax coloring in doc comments.
- Using structs
- Naming style rule breakage.

### 2025-07-04 ~ Postponed Social Post XML Doc Comments - AI Aided Draft

<h4>What Are XML Doc Comments?</h4>

In C#, XML doc comments are those triple-slash comments you see above classes, methods, or properties. They pop up as tooltips in your IDE when you hover a symbol.

**Example:**

```csharp
/// <summary>
/// Removes accents from characters in a string.
/// </summary>
public static string RemoveAccents(this string input)
```

You can write your own to provide instant, in-code documentation for your team or yourself.

---

<h4>Why Quality Matters</h4>

Not all doc comments are useful.
A bad comment looks like this:

```csharp
/// <summary>
/// This is the text.
/// </summary>
```

Or, you see whole files so loaded with comments that the actual code gets buried.
Ever opened a file and thought: “Where’s the code? Oh, buried in the middle of all these comments.”

Repeating boilerplate across files or methods just wastes time, makes maintenance harder, and trains people to ignore documentation.

<h4>Tags: summary, inheritdoc, remarks</h4>

A good doc comment uses tags like `<summary>`, `<inheritdoc>`, or `<remarks>` to provide relevant, non-repetitive information.

<h4>The Problem With Scattered Comments</h4>

Having doc comments everywhere leads to:

* Maintenance overhead
* Outdated documentation
* Cluttered files
* Lots of repetition

<h4>Centralized Docs: `docs.cs`</h4>

The solution I use: put documentation in a central place.
I keep a `docs.cs` file with doc-only members—classes or stubs that only exist to hold documentation.

Now, when something changes, I update comments once, not all over the place.
You can check your documentation in the Object Browser and see it all in one place.

<h4>Why Not Use XML Files With XPaths?</h4>

Storing docs in separate XML files and linking them with XPath is possible, but those links are fragile and can break easily. Centralizing docs in code is stronger—one place, tracked in version control.

Downside:
You lose some automatic checking (like param-existence validation), but you gain maintainability, clarity, and easier updates.

<h4>Managing Warnings</h4>

* Set `<GenerateDocumentationFile>true>` in your `.csproj` so documentation is built.
* Use warnings as errors to enforce doc standards.
* If you’re mid-transition, squelch missing doc warnings **centrally** in your project file, not scattered through your code.

__Example:__

```xml
<PropertyGroup>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <WarningsAsErrors>$(WarningsAsErrors);1591</WarningsAsErrors>
</PropertyGroup>
```

This helps you manage all doc-related warnings in one spot.

<h4>Deprecation: `[Obsolete]` vs Doc Markers</h4>

If you mark a member as `[Obsolete]` and treat warnings as errors, you can create friction in your build process.
An alternative: add `<b>[Deprecated]</b>` in the doc comment for softer, visible warnings that don’t break the build.

<h4>Readme and Generalized Docs</h4>

You can copy key parts of your README directly into your doc comments, or use a generalized comment for several elements when they share a description.
This keeps documentation efficient.

Decide for yourself:
Do you want detailed `<param>` tags for everything, or just the essentials? Sometimes, focusing on the main description is more practical.

<h4>Syntax Coloring and Style</h4>

Use minimal formatting or even basic syntax coloring in doc comments for clarity.
Don’t be afraid to use structs or break naming rules in documentation blocks if it makes the docs easier to find and update.

<h4>Summary</h4>

Centralizing your doc comments makes your codebase:

* Easier to maintain
* Less cluttered
* More reliable for developers

The result: Better usability, efficient docs, and cleaner code.

### Outtakes

This is all fine and well, but our code may still feel cluttered with all those comments, because eventually, they've got to live somewhere. 

What shall we do...?

("Where is my code?")

Can't find the code through all the comments...
("Where'd the code go?") 

To solve the remaining hacky `crefs` 

It's hiding behind that wall.

Where'd the code go?

Centralized Doc-Only Members

Want to read more about software development techniques for .NET and C#? Here's the forever unfinished resource, where I brain-dump more of these things inside neat looking web pages. You can find explanations, reasons and code samples of all sorts of patterns and techniques both mainstream or made up:

`[ Link to JJ's Software Architecture Page ]`

- You do lose the checks on param existence. But you gain a lot of the other things discussed here.

The end result: Better usability, centralized comments, efficiently written, without repetitions or code clutter and all in all cleaner code.

This article touches on:

- XML doc comments
- Warning management
- Centralized csproj configurations
