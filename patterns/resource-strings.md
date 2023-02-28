---
title: "🌍 Resource Strings"
image: "/images/resource-string-editor.png"
---

🌍 Resource Strings
====================

[back](README.md)

<h2 id="">Contents</h2>

- [Introduction](#resource-strings-introduction)
- [Visual Studio Editor](#resource-strings-visual-studio-editor)
- [File Names](#resource-string-file-names)
- [Descriptive Names](#resource-strings-descriptive-names)
- [ResourceFormatter](#resourceformatter)
- [ResourceFormatterHelper](#resourceformatterhelper)
- [Reusability](#resource-strings-reusability)
- [Use the Business Layer](#resource-strings-use-the-business-layer)
- [For More Information](#resource-strings-more-information)


<h4 id="resource-strings-introduction">Introduction</h4>

To store `Button Texts` and translations of [domain](data-access.md#entities) terminology in [`.NET`](../api.md#dotnet) projects, you could use `resx` files.


<h4 id="resource-strings-visual-studio-editor">Visual Studio Editor</h4>

Here's what the `Resource strings` editor looks like in [`Visual Studio`](../api.md#visual-studio):

![String Resource Editor](../images/resource-string-editor.png)


<h4 id="resource-string-file-names">File Names</h4>

[`.NET`](../api.md#dotnet) returns the translations in the right language (of the `CurrentCulture`) if you name your `Resource` files like this:

    Resources.resx
    Resources.nl-NL.resx
    Resources.de-DE.resx

[`CultureNames`](https://www.csharp-examples.net/culture-names/) like `nl-NL` and `de-DE` are commonly used within [`.NET`](../api.md#dotnet).

The *culture-independent* `Resources.resx` might be used for the language `US English`.


<h4 id="resource-strings-descriptive-names">Descriptive Names</h4>

For clarity it's recommended to keep the [`Resource Name`](#resource-strings-visual-studio-editor) descriptive of the text it represents:

    Name: Save
    Value: "Save"

    Name: Save_WithName
    Value: "Save {0}"


<h4 id="resourceformatter">ResourceFormatter</h4>

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


<h4 id="resourceformatterhelper">ResourceFormatterHelper</h4>

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

This eliminates the need to repeat the [`Resource Name`](#resource-strings-visual-studio-editor) in the code above. It also encourages consistency by forcing the method names to match the [`Resource Names`](#resource-strings-visual-studio-editor).


<h4 id="resource-strings-reusability">Reusability</h4>

[`JJ.Framework.ResourceStrings`](../api.md#jj-framework-resourcestrings) goes even further than that. It provides reusable [`Resources`](#resource-strings) for common phrases like `Delete`, `Edit`, `Save`, and more. No more typing out the same messages over and over again!


<h4 id="resource-strings-use-the-business-layer">Use the Business Layer</h4>

[`Resource strings`](#resource-strings) may play a role beyond just presentation. They're also commonly used in the [business layer](../layers.md#business-layer). Keeping the `DisplayNames` for [model](data-access.md#entities) properties in the [`business layer`](../layers.md#business-layer) makes it possible to reuse them from multiple places.


<h4 id="resource-strings-more-information">For More Information</h4>

For extra information in Dutch about how to structure the [`Resource` files](#resource-string-file-names), see [Appendix B](../appendices.md#appendix-b-knopteksten-en-berichtteksten-in-applicaties-resource-strings--dutch-).

[back](README.md)
