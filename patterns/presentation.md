﻿---
title: "🖥️ Patterns : Presentation"
image: "/images/view-model-code-sample.png"
redirect_from:
  - /patterns-presentation.md
---

<style type="text/css" rel="stylesheet">td:first-child { white-space: nowrap } thead { display: none; } </style>

`[ Draft ]`

🖥️ Patterns : Presentation
===========================

[back](README.md)

<h3>Contents</h3>

<ul>
<li><a href="#viewmodels">ViewModels</a></li>
<li><a href="https://jjvanzon.github.io/JJs-Reference-Architecture/patterns/presenters.html">Presenters</a></li>
<li><a href="#toviewmodel">ToViewModel</a></li>
<li><a href="#toentity">ToEntity</a></li>
<li><a href="#views">Views</a></li>
<li><a href="#lookup-lists">Lookup Lists</a></li>
<li><a href="#first-full-load--then-partial-load--then-client-native-code">First Full Load – Then Partial Load – Then Client-Native Code</a></li>
<li><a href="#stateless-and-stateful">Stateless and Stateful</a></li>
<li><a href="#nullcoalesce-viewmodels">NullCoalesce (ViewModels)</a></li>
<li><a href="#temporary-ids">Temporary ID's</a></li>
<li><a href="#considerations">Considerations</a></li>
</ul>

ViewModels
----------

[`ViewModels`](#viewmodels) are as simple as they are invaluable in [this architecture](../JJs-Reference-Architecture).  
A [`ViewModel`](#viewmodels) provides a simplified and organized representation of the data to display on screen.


<h3>Contents</h3>

- [Only Data](#view-models-only-data)  
- [Screen ViewModels](#screen-viewmodels)  
- [Entity ViewModels](#entity-viewmodels)  
- [Partial ViewModels](#partial-viewmodels)  
- [ListItem ViewModels](#listitem-viewmodels)  
- [Lookup ViewModels](#lookup-viewmodels)  
- [How to Model](#view-models-how-to-model)  
- ["What", not "How"](#view-models-what-not-how)  
- ["What", not "Why"](#view-models-what-not-why)  
- [Keeping It Clean](#view-models-keeping-it-clean)  
- [No Entities](#view-models-no-entities)
- [Avoid ViewModel to ViewModel Conversion](#avoid-viewmodel-to-viewmodel-conversion)  
- [Avoid Inheritance](#view-models-avoid-inheritance)  
- [Composition](#view-models-composition)
- [Realistic Example](#view-models-realistic-example)
- [Conclusion](#view-models-conclusion)  


<h3 id="view-models-only-data">
Only Data
</h3>

In [this architecture](../JJs-Reference-Architecture) a [`ViewModel`](#viewmodels) is meant to be a pure [data object](data-access.md#dto). It's recommended that [`ViewModels`](#viewmodels) only have `public` properties, *no* methods, *no* constructors, *no* member initialization and *no* list instantiation. This to insist that the code *handling* the [`ViewModels`](#viewmodels) takes full responsibility for the data. This also makes it better possible to integrate with different types of technology. Here is an example of a simple [`ViewModel`](#viewmodels):

```cs
public class ProductViewModel
{ 
    public int ID { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
}
```


<h3 id="screen-viewmodels">
Screen ViewModels
</h3>

Every screen can get a [`ViewModel`](#viewmodels). Here are some [`Screen ViewModels`](#screen-viewmodels) you might find in an application: 

    ProductDetailsViewModel
    ProductListViewModel
    ProductEditViewModel
    ProductDeleteViewModel

These names are built up from parts:

1. Starting with the [`Entity`](data-access.md#entities) name:  
   `Product`, `Category`
2. Then something [`CRUD`](../layers.md#crud)-related:  
   `Details`, `List`, `Edit` or `Delete`
3. And it ends with:  
   [`ViewModel`](#viewmodels)

Instead of [`CRUD`](../layers.md#crud) actions, you could also use terms like `Overview`, `Selector`, `NotFound`, or `Login`:

    ProductOverviewViewModel
    CategorySelectorViewModel
    NotFoundViewModel
    LoginViewModel

Here is a code example of a [`Screen ViewModel`](#screen-viewmodels):

```cs
public class ProductEditViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public string Category { get; set; }
    public string ProductType { get; set; }
    public IList<string> ValidationMessages { get; set; }
    public bool CanDelete { get; set; }
}
```


<h3 id="entity-viewmodels">
Entity ViewModels
</h3>

You can also reuse [`ViewModels`](#viewmodels) that represent single [`Entities`](data-access.md#entities), e.g.:

    ProductViewModel
    CategoryViewModel

For instance:

```cs
public class CategoryViewModel 
{ 
    public int ID { get; set; }
    public string Name { get; set; }
}
```

[`Entity ViewModels`](#entity-viewmodels) might be reused among different [`Screen ViewModels`](#screen-viewmodels), for instance:

```cs
/// <summary>
/// An Edit ViewModel using several Entity ViewModels.
/// </summary>
public class ProductEditViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }

    // Using Entity ViewModels
    public CategoryViewModel Category { get; set; }
    public ProductTypeViewModel ProductType { get; set; }
}
```

[`Entity ViewModels`](#entity-viewmodels) are a kind of `Item ViewModel`.


<h3 id="partial-viewmodels">
Partial ViewModels
</h3>

[`Partial ViewModels`](#partial-viewmodels) describe *parts* of a screen, to keep overview of its sections, like:

    LoginPartialViewModel
    ButtonBarViewModel
    MenuViewModel
    PagerViewModel
    
They may or may not have the word `Partial` in their name. Here is a code sample of a `ButtonBarViewModel`:

```cs
/// <summary>
/// Partial ViewModel representing a ButtonBar.
/// </summary>
public class ButtonBarViewModel 
{ 
    public bool CanSave { get; set; }
    public bool CanDelete { get; set; }
    public bool CanCreate { get; set; }
    public bool CanShowList { get; set; }
}
```

Each property in there says whether you can use a certain button or not.

The [`Partial ViewModels`](#partial-viewmodels) can be used in [`Screen ViewModels`](#screen-viewmodels). Here some [`Partials`](#partial-viewmodels) are used in the `ProductEditViewModel`:

```cs
/// <summary>
/// Edit ViewModel with Partials
/// </summary>
public class ProductEditViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }

    // Partials:
    public ButtonBarViewModel Buttons { get; set; }
    public LoginPartialViewModel Login { get; set; }
}
```


<h3 id="listitem-viewmodels">
ListItem ViewModels
</h3>

[`ListItem ViewModels`](#listitem-viewmodels) are similar to the [`Entity ViewModels`](#entity-viewmodels) but instead they might represent a row in *list* or *grid*. Here are some names they might get:

    ProductListItemViewModel
    CategoryItemViewModel

A `CategoryItemViewModel` could look as follows:

```cs
public class CategoryItemViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }
    public string UsedBy { get; set; }
    public bool CanDelete { get; set; }
}
```

So they can be different from the [`Entity ViewModels`](#entity-viewmodels).  
[`ListItem ViewModels`](#listitem-viewmodels) can be used inside a `ListViewModel`:

```cs
/// <summary>
/// Example of a ViewModel using ListItem ViewModels.
/// </summary>
public class CategoryListViewModel
{
    // Here, a ListItem ViewModel is used.
    public IList<CategoryItemViewModel> Categories { get; set; }
}
```

Some list views only need an [`IDAndName`](../api.md#jj-canonical) [`DTO`](data-access.md#dto), a version of which can be found in the [`JJ.Canonical`](../api.md#jj-canonical) project:

```cs
namespace JJ.Data.Canonical
{
    public class IDAndName
    {
        public int ID { get; set; }
        public string Name { get; set; }
    }
}
```

Here you can find [`IDAndName`](../api.md#jj-canonical) objects used in a `ListViewModel`:

```cs
/// <summary>
/// Example of a List ViewModel that uses IDAndName as the item type.
/// </summary>
public class CategoryListViewModel
{
    // Here, IDAndName is used as a list item.
    public IList<IDAndName> Categories { get; set; }
}
```


<h3 id="lookup-viewmodels">
Lookup ViewModels
</h3>

A *lookup* list can provide the data for drop-down select boxes or other controls, e.g.:

```cs
IList<IDAndName> ProductTypeLookup { get; set; }
```

It might be used in a [`Screen ViewModel`](#screen-viewmodels) like this:

```cs
/// <summary>
/// Edit ViewModel with a Lookup List in it.
/// </summary>
public class ProductEditViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }
    public IDAndName ProductType { get; set; }

    // Here is the Lookup ViewModel.
    IList<IDAndName> ProductTypeLookup { get; set; }
}
```


<h3 id="view-models-how-to-model">
How to Model
</h3>

A [`ViewModel`](#viewmodels) is an abstract representation of what is shown on screen. The idea for how to model them is:

> *A [`ViewModel`](#viewmodels) says __what__ is shown on screen, not __how__ or __why__.*


<h3 id="view-models-what-not-how">
"What", not "How"
</h3>

A [`ViewModel`](#viewmodels) says ***what*** is shown on screen, not ***how:***

Therefore `CanDelete` may be a better name than `DeleteButtonVisible`. Whether it is a `Button` or a hyperlink or `Visible` or `Enabled`, should be up to the [`View`](#views) instead.


<h3 id="view-models-what-not-why">
"What", not "Why"
</h3>

A [`ViewModel`](#viewmodels) should say ***what*** is shown on screen, not ***why:***  

For instance: if the business logic tells us that an [`Entity`](data-access.md#entities) is a very special [`Entity`](data-access.md#entities), therefore it should be displayed read-only, the [`ViewModel`](#viewmodels) might contain a property `IsReadOnly` or `CanEdit`, not a property named `ThisIsAVerySpecialEntity`. 
The *reason* for displaying data read-only should not be a concern for a [`ViewModel`](#viewmodels) or a [view](#views).


<h3 id="view-models-keeping-it-clean">
Keeping It Clean
</h3>

[`ViewModels`](#viewmodels) might only use *simple* `types` and references to other [`ViewModels`](#viewmodels). This keeps the [`ViewModel`](#views) layer completely self-contained.


<h3 id="view-models-no-entities">
No Entities
</h3>

For instance, a [`ViewModel`](#viewmodels) in [this architecture](../JJs-Reference-Architecture) isn't supposed to reference any [`Entities`](data-access.md#entities). This is because it would potentially connect the [`ViewModel`](#viewmodels) to a database, which is not always desired or even possible.

Even when the [`ViewModel`](data-access.md#entities) looks almost exactly the same as the [`Entity`](data-access.md#entities), we tend to not use [`Entities`](data-access.md#entities) directly. 

[`Entity`](data-access.md#entities):

```cs
public class Product
{
    public virtual int ID { get; set; }
    public virtual string Name { get; set; }
    public virtual Category Caterogy { get; set; }
}
```

[`ViewModel`](#viewmodels):

```cs
public class ProductViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }
    public CategoryViewModel Category { get; set; }
}
```

It is worth noting that linking to an [`Entity`](data-access.md#entities) can result in the availability of other related [`Entities`](data-access.md#entities), which may broaden the scope of the [view](#views) beyond our intentions:

```cs
/// <summary>
/// This ViewModel references an Entity, which is not recommended.
/// </summary>
public class ProductViewModel
{
    public int ID { get; set; }
    public string Name { get; set; }

    // Not recommended: Referencing an Entity from a ViewModel.
    public Category Category { get; set; }
}

// Unintentionally, many customers' data
// is available in the Product view, 
// because we referenced an Entity.
var customers =
    productViewModel.Category.Products
                    .SelectMany(x => x.Orders)
                    .Select(x => x.Customer);
```

An added benefit of decoupling the [`ViewModels`](#viewmodels) from [`Entities`](data-access.md#entities), is that it makes it possible to change a [`ViewModel`](#viewmodels) without affecting the [data access layer](../layers.md#data-layer) or the [business logic](../layers.md#business-layer):

[`ViewModel`](#viewmodels):

```cs
public class CustomerViewModel
{
    public string CustomerNumber { get; set; }
    public string FirstName { get; set; }
    public string CouponCode { get; set; }
}
```

[`Entity`](data-access.md#entities):

```cs
public class Customer
{
    public virtual int ID { get; set; }
    public virtual string Name { get; set; }
    public virtual string CustomerNumber { get; set; }
}
```

Here the [`Entity`](data-access.md#entities) and [`ViewModel`](#viewmodels) look completely different, which is totally fine.


<h3 id="avoid-viewmodel-to-viewmodel-conversion">
Avoid ViewModel to ViewModel Conversion
</h3>

It is not advised to convert [`ViewModels`](#viewmodels) to other [`ViewModels`](#viewmodels) directly:

```cs
/// <summary>
/// Conversion from ViewModel to ViewModel is not recommended.
/// </summary>
public static void Convert(
    EditViewModel userInput, ProductViewModel viewModel)
{
    if (viewModel.HasVat)
    {
        viewModel.Price = userInput.Price * userInput.Vat;
    }
    else
    {
        viewModel.Price = userInput.Price / userInput.Vat;
    }
}
```

What we're trying to prevent here is too much interdependence between [`ViewModels`](#viewmodels). Prefer converting from [`Entities`](data-access.md#entities) to [`ViewModel`](#viewmodels) and back:

```cs
Product entity = _repository.Get(id);

decimal price = entity.PriceWithoutVat * _taxCalculator.VatFactor;

var viewModel = new EditViewModel { Price = price };
```

This gives us finer control over where the data comes from and goes. But there might be exceptions. There could be cases, where it makes more sense to operate directly on the [`ViewModels`](#viewmodels) instead:

```cs
public void ExpandNode(TreeViewModel viewModel, int id)
{
    var node = viewModel.Nodes.Single(x => x.ID == id);
    node.IsExpanded = true;
}
```

It may be about properties, that aren't intended for permanent storage. You can also pass other non-persisted properties between [`ViewModel`](#viewmodels) like this:

```cs
public QuizViewModel Answer(QuizViewModel userInput)
{
    var viewModel = new QuizViewModel();

    // Yield over non-persisted properties.
    viewModel.SelectedOption = userInput.SelectedOption;
    viewModel.AnswerVisible = userInput.AnswerVisible;

    return viewModel;
}
```


<h3 id="view-models-avoid-inheritance">
Avoid Inheritance
</h3>

Inheritance is not the go-to choice for [`ViewModels`](#viewmodels).

Using inheritance creating a `base` [`ViewModel`](#viewmodels) can lead to a high number of interdependencies between the [`Views`](#views) and the [`ViewModels`](#viewmodels). If the `base` [`ViewModel`](#viewmodels) changes, it can potentially break many [`Views`](#views), making the application harder to maintain:

```cs
public abstract class ScreenViewModel
{
    public string Name { get; set; }
    public string Description { get; set; }
}

public class HomePageViewModel : ScreenViewModel
{
    // ...
}

public class ProductEditViewModel : ScreenViewModel
{
    // ...
}
```

Here the `BaseViewModel` contains the properties `Name` and `Description`. These properties might potentially mean something different for the `HomePage` and `ProductEdit` [views](#viewmodels). If we decide to rename the `base` properties to be more specific, or to change their purpose, we could be breaking multiple views, because we used inheritance.

By avoiding inheritance, a [`ViewModel`](#viewmodels) can only break things that directly depend on it, reducing the potential impact of changes:

```cs
public class HomePageViewModel
{
    public string PageTitle { get; set; }
    public string UserDisplayName { get; set; }
}

public class ProductEditViewModel
{
    public int ProductID { get; set; }
    public int ProductName { get; set; }
    public int ProductDescription { get; set; }
}
```

In these examples, each [`ViewModel`](#viewmodels) is self-sufficient and does not affect the other.

To really 'seal' the deal, you could make the [`ViewModel`](#viewmodels) `classes` `sealed` to prevent inheritance at all:

```cs
public sealed class HomePageViewModel
{
    public string PageTitle { get; set; }
    public string UserDisplayName { get; set; }
}
```

Though no hard rules here. It doesn't mean that inheritance should always be avoided. It may still be possible to use inheritance in a way that is maintainable by applying it carefully:

```cs
public abstract class ScreenViewModel
{
    public bool Visible { get; set; }
    public bool Successful { get; set; }
    public IList<string> ValidationMessages { get; set; }
}
```

By keeping the members in the base class very general, and not applicable to specific situations, it would be less likely to break as the software evolves.

<h3 id="view-models-composition">
Composition
</h3>

As an alternative, you could also choose to use other design patterns, such as [composition](#view-models-composition), to reduce the impact of changes:

```cs
public class HomePageViewModel
{
    public ScreenViewModel Screen { get; set; }
    public LoginPartialViewModel Login { get; set; }
}

public class ProductEditViewModel
{
    public ScreenViewModel Screen { get; set; }
    public ValidationViewModel Validation { get; set; }
}
```

The `HomePage` uses common properties and has a `Login`. The `ProductEdit` [view](#views) uses common properties and has `Validation`.

`ScreenViewModel` and the `ValidationViewModel` are reused. `ScreenViewModel` defines common properties for any screen or page. The `ValidationViewModel` has properties for data validation, like `ValidationMessages` and a `Successful` flag.

By using [composition](#view-models-composition), changes to a child object can still have an impact on multiple [views](#views). But the modular nature of [composition](#view-models-composition) allows for a potentially smaller scope of dependency than [inheritance](#view-models-avoid-inheritance).

<h3 id="view-models-realistic-example">
Realistic Example
</h3>

Finally, here is a realistic example of a `ProductEdit` [`ViewModel`](#viewmodels):

```cs
// Screen ViewModel
public class ProductEditViewModel
{
    // Entity ViewModel
    public ProductViewModel Product { get; set; }

    // Partial ViewModels
    public ButtonBarViewModel Buttons { get; set; }
    public LoginPartialViewModel Login { get; set; }

    // Lookup ViewModels
    public IList<IDAndName> CategoryLookup { get; set; }
    public IList<IDAndName> ProductTypeLookup { get; set; }

    // Composition
    public ValidationViewModel Validation { get; set; }
}
```

<h3 id="view-models-conclusion">
Conclusion
</h3>

Hopefully this gave a good impression of how [`ViewModels`](#viewmodels) might be structured. They can enable technology independence, preventing hard coupling to [business logic](../layers.md#business-layer) and [data access](../layers.md#data-layer), offering a flexible way to model the user interaction. In the coming sections, more patterns will be introduced, to illustrate how these [`ViewModels`](#viewmodels) might be used in practice. To see where they come from and go.


<h2 id="presenters">Presenters</h2>

See the [`Presenters`](presenters.md) article.


ToViewModel
-----------

An extension method that converts an [`Entity`](data-access.md#entities) to a [`ViewModel`](#viewmodels). You can make simple `ToViewModel` methods per [`Entity`](data-access.md#entities), converting it to a simple [`ViewModel`](#viewmodels) that represents the [`Entity`](data-access.md#entities). You can also have methods returning more complex [`ViewModels`](#viewmodels), such as `ToDetailsViewModel()` or `ToCategoryTreeEditViewModel()`.

You may pass [`Repositories`](data-access.md#repository) to the `ToViewModel` methods if required.

Sometimes you cannot appoint one [`Entity type`](data-access.md#entities) as the source of a [`ViewModel`](#viewmodels). In that case you cannot logically make it an extension method, but you make it a [`Helper`](other.md#helper) method in the `static ViewModelHelpers class`.

The `ToViewModel classes` should be put in the sub-folder / sub-namespace `ToViewModel` in your csproj. For an app with many [`Views`](#views) a split it up into the following files may be a good plan:

    ToIDAndNameExtensions.cs
    ToItemViewModelExtensions.cs
    ToListItemViewModelExtensions.cs
    ToPartialViewModelExtensions.cs
    ToScreenViewModelExtensions.cs
    ToViewModelHelper.cs
    ToViewModelHelper.EmptyViewModels.cs
    ToViewModelHelper.Values.cs
    ToViewModelHelper.Items.cs
    ToViewModelHelper.ListItems.cs
    ToViewModelHelper.Lookups.cs
    ToViewModelHelper.Partials.cs
    ToViewModelHelper.Screens.cs

For clarity: the `ViewModelHelper` files are all `ViewModelHelper partial classes`. The other files have a `class` that has the same name as the file.

Inside the `classes`, the methods should be sorted by source [entity](data-access.md#entities) or application section alphabetically and each section should be headed by a comment line, e.g.:

```cs
// Orders

public static OrderListViewModel ToListViewModel(this IList<Order> orders) { ... }
public static OrderEditPopupViewModel ToEditViewModel(this Order order) { ... }
public static OrderDeletePopupViewModel ToDeleteViewModel(this IList<Order> orders) { ... }
```

Some [`ViewModels`](#viewmodels) do not take one primary [`Entity`](data-access.md#entities) as input. So it does not make sense to turn it into an extension method, because you cannot decide which [`Entity`](data-access.md#entities) is the this argument. In that case we put it in a `ViewModelHelper class` with `static classes` without this arguments. `ViewModelHelper` is also part of the `ToViewModel` layer.


ToEntity
--------

Extension methods that convert a [`ViewModel`](#viewmodels) to an [`Entity`](data-access.md#entities).

You typically pass [`Repositories`](data-access.md#repository) to the method. A simple `ToEntity` method might look up an existing [`Entity`](data-access.md#entities), if it exists, it would be updated, if it does not, it would be created.

A more complex `ToEntity` method might also update related [`Entities`](data-access.md#entities). In that case related [`Entities`](data-access.md#entities) might be inserted, updated and deleted, depending on whether the [`Entity`](data-access.md#entities) still exists in the [`ViewModel`](#viewmodels) or in the data store.

A `ToEntity` method takes on much of the resposibility of a Save action.

`< TODO: Describe the organization of the ToEntity extensions. >`


Views
-----

A *template* for rendering the `View`.

It might be `HTML`.

In `WebForms` this would be an `aspx`.

In [`MVC`](../api.md#mvc) it can be an `aspx` or `cshtml`.

Any code used in the [`View`](#views) should be simple. That is: most tasks should be done by the [`Presenter`](presenters.md#-presenters), which produces the [`ViewModel`](#viewmodels), which is simply shown on screen. The [`View`](#views) should not contain [business logic](../layers.md#business-layer).


Lookup Lists
------------

In a stateless environment, lookup lists in [`Views`](#views) can be resource-intensive. For instance a drop down list in each row of a grid in which you choose from 1000 items may easily bloat your `HTML`. You might repeat the same list of 1000 items for each grid row. There are multiple ways to combat this problem.

For small lookup lists you might include a copy of the list in each [Item ViewModel](#entity-viewmodels) and repeat the same lookup list in `HTML`.

Reusing the same list instance in multiple [ViewModels](#viewmodels) may seem to save you some memory, but a message formatter may actually repeat the list when sending a [`ViewModel`](#viewmodels) over the line.

For lookup lists up until say 100 items you might want to have a single list in an [`Edit ViewModel`](#screen-viewmodels). A central list may save some memory but, but when you still repeat the HTML multiple times, you did not gain much. You may use the HTML5 `<datalist>` tag to let a `<select>` / drop down list reuse the same data. You might also use a [`jQuery`](../api.md#jquery) trick to populate a drop down just before you slide it open.

For big lookup list a viable option seems to [`AJAX`](../api.md#ajax) the list and show a popup that provides some search functionality, and not retrieve the full list in a single request. Once [`AJAX'ed`](../api.md#ajax) you might *cache* the popup to be reused each time you need to select something from it.


First Full Load – Then Partial Load – Then Client-Native Code
-------------------------------------------------------------

You might save the server some work by doing [partial loads instead of full loads](../api.md#ajax) or maybe even do [`JavaScript`](../api.md#javascript) or other client-native code.

[Partial loads](../api.md#ajax) load part of a web page, intead of the whole page, so the whole page does not have to be refreshed every time. It also might save work for the server that has to do less processing then.

[`JavaScript`](../api.md#javascript) is client-native code, that could omit server code altoghether, but potentially with a penalty on maintainability, because the client-code may be written in a way that is more dependent on specific [view](#views) details, which might easily change as the server-code evolves.

You could also call it: first choice full load.

In web technology you could also call it:

Full postback - [`AJAX`](../api.md#ajax) - [`JavaScript`](../api.md#javascript)

When programming page navigation, the first choice for showing content is a *full page load* in this [architecture](../JJs-Reference-Architecture). Only if there is a very good reason, we might use [`AJAX`](../api.md#ajax) to do a *partial load*. Only with a very good reason, we might start programming user interaction in [`JavaScript`](../api.md#javascript).

But it was always the first choice to do full postbacks.

The reason for this choice is *maintainability*: programming the application navigation in [`C#`](../api.md#csharp) using [`Presenters`](presenters.md#-presenters) is more maintainable than a whole lot of [`JavaScript`](../api.md#javascript). Also: when you do not use [`AJAX`](../api.md#ajax), the [`Presenter`](presenters.md#-presenters) keeps full control over the application navigation, and you do not have to let the web layer be aware of page navigation details.

Furthermore [`AJAX'ing`](../api.md#ajax) comes with extra difficulties. For instance that [`MVC`](../api.md#mvc) `<input>` tag ID's vary depending on the context and must be preserved after an [`AJAX`](../api.md#ajax) call, big code blocks of [`JavaScript`](../api.md#javascript) for doing [`AJAX`](../api.md#ajax) posts, managing when you do a full redirect or just an update of a div. Keeping overview over the multitude of formats with which you can get and post partial content. The added complexity of sometimes returning a row, sometimes returning a partial, sometimes returning a full [`View`](#views). Things like managing the redirection to a full [`View`](#views) from a partial action. Info from a parent [`ViewModel`](#viewmodels) e.g. a lookup list that is passed to the generation of a child [`ViewModel`](#viewmodels) is not available when you generate a partial [`View`](#views). `Request.RawUrl` cannot be used as a return URL in links anymore. Related info in other panels is not updated when info from one panel changes. A lot of times the data on screen is so intricately related to eachother, updating one panel just does not cut it. The server just does not get a chance to change the [`View`](#views) depending on the outcome of the business logic. Sometimes an [`AJAX`](../api.md#ajax) call's result should be put in a different target element, depending on the type you get returned, which adds more complexity.

Some of the difficulties with [`AJAX`](../api.md#ajax) have been solved by employing a specific way of working, as described under [`AJAX`](../api.md#ajax) in the Aspects section.


Stateless and Stateful
----------------------

The [presentation patterns](#️-patterns-presentation) may differ slightly if used in a stateful environment, but most of it stays in tact. For instance that [`Presenters`](presenters.md#-presenters) have action methods that take a [`ViewModel`](#viewmodels) and output a new [`ViewModel`](#viewmodels) is still useful in that setting. In a stateless environment such as web, it is needed, because the input [`ViewModel`](#viewmodels) only contains the user input, not the data that is only displayed and also not the lookup lists for drop down list boxes, etc. So in a stateless environment a new [`ViewModel`](#viewmodels) has to be created. You cannot just return the user input [`ViewModel`](#viewmodels). You would think that in a stateful environment, such as a `Windows` application, this would not be necessary anymore, because the read-only [`View`](#views) data does not get lost between user actions. However, creating a new [`ViewModel`](#viewmodels) is still useful, because it creates a kind of transaction, so that when something fails in the action, the original [`ViewModel`](#viewmodels) remains untouched.

You would be making assumptions in your [`Presenter`](presenters.md#-presenters) code when you program a stateful or stateful application. Some things in a stateful environment environment would not work in a stateless environment and you might make some `objects` long-lived in a stateful environment, such as `Context`, [`Repositories`](data-access.md#repository) and [`Presenters`](presenters.md#-presenters). But even if you build code around those assumptions, then when switching to a stateless environment –  if that would ever happen – the code is still so close to what's needed for stateless, that it might not come with any insurmountable problems. I would not beforehand worry about 'will this work in stateless', because then you would write a lot of logic and waste a lot of energy programming something that might probably never be used. And programming something for no reason at all, handling edge cases that would never occur, is a really counter-intuitive, unproductive way of working.

`< TODO: Added benefit: Transactional. >`


NullCoalesce (ViewModels)
-------------------------

When you user input back as a [`ViewModel`](#viewmodels) from your presentation framework of choice, for instance [`MVC`](../api.md#mvc), you might encounter null-lists in it, for lists that do not have any items. To prevent other code from doing `NullCoalescing` or instead tripping over the nulls, you can centralize the `NullCoalescing` of pieces of [`ViewModel`](#viewmodels) and call it in the [`Presenter`](presenters.md#-presenters).

`< TODO: Better description. Also incorporate:`

`- Also add a code example.`  
`- Consider making a separate pattern description for NullCoalesce methods in general and move it to the Other Patterns section to which you then refer from this section NullCoalesce (ViewModels).`
`- NullCoalesce. Applied to viewmodels that are passed to Presenters. The choice is made here to only NullCoalesce things that a View / client technology might leave out. Theoretically it might be better to NullCoalesce everything in the ViewModel, but this does take full traversal of the tree, which comes with a (small) performance penalty. Also: the NullCoalesce procedures take some typing time for the programmer, and requires maintenance when the structure changes. That is why the choice is made to only NullCoalesce a select set of things, that is adapted to our specific needs, rather than something that will always work. >`



Temporary ID's
--------------

When you edit a list, and between actions you do not commit you may need to generate ID's for the rows that are not committed, otherwise you cannot identify them individually to for instance delete a specific uncommitted row. For this you can add a TemporaryID to the [`ViewModel`](#viewmodels), that are typically Guids.

The TemporaryID's can be really temporary and can be regenerated every time you create a new [`ViewModel`](#viewmodels).

The TemporaryID concept breaks down, as soon as you need to use it to refer to something from multiple places in the [`ViewModel`](#viewmodels).

An alternative is to let a data store generate the ID's by flushing pendings statements to the data store, which might give you data-store-generated ID's. But this method fails when the data violates database constraints. Since the data does not have to be valid until we press save, this is usually not a viable option, not to speak of that switching to another persistence technology might not give you data-store-generated ID's upon flushing at all.

Another alternative is a different ID generation scheme. You may use an [`SQL`](../api.md#sql) Sequence, or use GUID's, which you assign from your code. Switching from int ID's to GUID's is a high impact change though, and does come with performance and storage penalties.


Considerations
--------------

### ToEntity / ToViewModel

`< TODO: Explain the argument that ViewModel, ToEntity and ToViewModel does require programming a lot of conversion code, but gives you complete freedom over your program navigation, but the alternative, a framework prevents writing this conversion code for each application, but has the downside that you are stuck with what the framework offers and loose the complete freedom over your how your program navigation works. >`

[back](README.md)
