---
title: "🌍 Resource Strings"
image: "/images/resource-string-editor.png"
---

🌍 Resource Strings
====================

[back](README.md)

<h2>Contents</h2>

- [Introduction](#introduction)
- [Visual Studio Editor](#visual-studio-editor)
- [File Names](#file-names)
- [Descriptive Names](#descriptive-names)
- [ResourceFormatters](#resourceformatters)
- [ResourceFormatterHelper](#resourceformatterhelper)
- [Reusability](#reusability)
- [Use the Business Layer](#use-the-business-layer)
- [For More Information](#for-more-information)


Introduction
------------

To store `Button Texts` and translations of [domain](data-access.md#entities) terminology in [`.NET`](../api.md#dotnet) projects, you could use `resx` files.


Visual Studio Editor
--------------------

Here's what the `Resource strings` editor looks like in [`Visual Studio`](../api.md#visual-studio):

![String Resource Editor](../images/resource-string-editor.png)


File Names
----------

[`.NET`](../api.md#dotnet) returns the translations in the right language (of the `CurrentCulture`) if you name your `Resource` files like this:

    Resources.resx
    Resources.nl-NL.resx
    Resources.de-DE.resx

[`CultureNames`](https://www.csharp-examples.net/culture-names/) like `nl-NL` and `de-DE` are commonly used within [`.NET`](../api.md#dotnet).

The *culture-independent* `Resources.resx` might be used for the language `US English`.


Descriptive Names
-----------------

For clarity it's recommended to keep the [`Resource Name`](#visual-studio-editor) descriptive of the text it represents:

    Name: Save
    Value: "Save"

    Name: Save_WithName
    Value: "Save {0}"


ResourceFormatters
------------------

You could use `ResourceFormatters` to add the correct values to the `{0}` placeholders:

```cs
public static class ResourceFormatter
{
    public static string Save_WithName(string name) 
        => string.Format(Resources.Save_WithName, name);
}
```

By using `ResourceFormatters`, you can ensure the safe usage of placeholders in the code:

```cs
ResourceFormatter.Save_WithName("Document");
```

Returning:

```cs
"Save Document"
```


ResourceFormatterHelper
-----------------------

You can streamline your code and minimize the risk of typos by using the `ResourceFormatterHelper` from the [`JJ.Framework`](../api.md#jj-framework-resourcestrings):

```cs
public static class ResourceFormatter
{
    private static readonly ResourceFormatterHelper _helper 
        = new (Resources.ResourceManager);

    public static string Save => _helper.GetText();

    public static string Save_WithName(string name) 
        => _helper.GetText_WithOnePlaceHolder(name);
}
```

This eliminates the need to repeat the [`Resource Name`](#visual-studio-editor) in the code above. It also encourages consistency by forcing the method names to match the [`Resource Names`](#visual-studio-editor).


Reusability
-----------

[`JJ.Framework.ResourceStrings`](../api.md#jj-framework-resourcestrings) goes even further than that. It provides reusable [`Resources`](#resource-strings) for common phrases like `Delete`, `Edit`, `Save`, and more. No more typing out the same messages over and over again!


Use the Business Layer
----------------------

[`Resource strings`](#resource-strings) may play a role beyond just presentation. They're also commonly used in the [business layer](../layers.md#business-layer). Keeping the `DisplayNames` for [model](data-access.md#entities) properties in the [`business layer`](../layers.md#business-layer) makes it possible to reuse them from multiple places.


For More Information
--------------------

For extra information in Dutch about how to structure the [`Resource` files](#file-names), see [Appendix B](../appendices.md#appendix-b-knopteksten-en-berichtteksten-in-applicaties-resource-strings--dutch-).


[back](README.md)
