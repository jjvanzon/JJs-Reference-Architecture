---
title: "🕺 Presenters"
image: "/images/presenter-code-sample-test7.png"
redirect_from:
  - /patterns-presenters.md
description: "Learn how to improve your user interfaces by using Presenters: a software design pattern for modeling user interaction. This article provides practical tips using them in your own projects, helping you make your user interfaces more flexible and reliable."
---

<style type="text/css" rel="stylesheet">td:first-child { white-space: nowrap } thead { display: none; } </style>

🕺 Presenters
==============

[back](.)

[`Presenters`](#-presenters) model the user interactions. A non-visual blue-print of the user interface.
This section describes how they are implemented in [this architecture](../JJs-Reference-Architecture).

<h2>Contents</h2>

- [The Role of the Presenter](#the-role-of-the-presenter)
- [Working with ViewModels](#working-with-viewmodels)
- [Infrastructure and Configuration](#infrastructure-and-configuration)
- [Internal Implementation](#internal-implementation)
- [Delegating ViewModel Creation](#delegating-viewmodel-creation)
- [Delegating More Responsibilities](#delegating-more-responsibilities)
- [Complete Example](#complete-example)
- [Overhead](#overhead)
- [Using ViewModels Directly](#using-viewmodels-directly)
- [Conclusion](#conclusion)


The Role of the Presenter
-------------------------

Unlike the [`ViewModels`](presentation.md#viewmodels), which model the *data* shown on a screen, the [`Presenters`](#-presenters) model the *actions* a user can perform on the screen.

Each [`View`](presentation.md#views) can get its own [`Presenter`](#-presenters), for instance:

```cs
class LoginPresenter { }
class ProductListPresenter { }
class ProductEditPresenter { }
class ProductDetailsPresenter { }
class CategorySelectionPresenter { }
```

Each *user action* would be represented by a *method*, like these:

```cs
class ProductEditPresenter
{
    void Show() { }
    void Save() { }
}
```


Working with ViewModels
-----------------------

Our action methods of the [`Presenter`](#-presenters) work by a [`ViewModel-in`](presentation.md#viewmodels), [`ViewModel-out`](presentation.md#viewmodels) principle. An action method returns a [`ViewModel`](presentation.md#viewmodels) that contains the data to display on screen:

```cs
class ProductEditPresenter
{
    ProductEditViewModel Show() { }
}
```

Action methods can also *receive* a [`ViewModel`](presentation.md#viewmodels) parameter containing the data the user has edited:

```cs
class ProductEditPresenter
{
    void Save(ProductEditViewModel userInput) { }
}
```

Other action method parameters are also things the user has chosen:

```cs
class ProductEditPresenter
{
    void Show(string productNumber) { }
}
```

The [`Presenter's`](#-presenters) action methods are not limited to returning a single type of [`ViewModel`](presentation.md#viewmodels). It is possible to return different [`ViewModel`](presentation.md#viewmodels) types as well:

```cs
class ProductEditPresenter
{
    object Save(ProductEditViewModel userInput)
    {
        if (!successful)
        {
            // Stay in the edit view
            return new ProductEditViewModel();
        }

        // Redirect to list
        return new ProductListViewModel();
    }
}
```

Following those guidelines, the [`Presenters`](#-presenters) form a structured model for what the user can do with the application.


Infrastructure and Configuration
--------------------------------

Sometimes you also pass [infra and config](../layers.md#infrastructure) parameters to an action method:

```cs
class ProductEditPresenter
{
    object Save(
        ProductEditViewModel userInput, 
        IAuthenticator authenticator) // Infra-related parameter
    {
        // ...
    }
}
```

But it is preferred that the main chunk of the [infra and settings](../layers.md#infrastructure) is passed to the [`Presenter's`](#-presenters) constructor:

```cs
class ProductEditPresenter
{
    /// <summary>
    /// Passing infra-related parameters to the constructor.
    /// </summary>
    public ProductEditPresenter(
        IRepository repository,
        IAuthenticator authenticator,
        string cultureName)
    {
        // ...
    }
}
```

Internal Implementation
-----------------------

Internally a [`Presenter`](#-presenters) can use [business logic](../layers.md#business-layer) and [`Repositories`](data-access.md#repository) to access the domain model.


Delegating ViewModel Creation
-----------------------------

It is preferred that [`ViewModel`](presentation.md#viewmodels) creation is delegated to the [`ToViewModel`](presentation.md#toviewmodel) layer (instead of creating them directly within the [`Presenters`](#-presenters)):

```cs
public ProductEditViewModel Show(int id)
{
    // Delegate to ToViewModel layer
    var viewModel = entity.ToEditViewModel();
}
```

This way, [`ViewModel` creation](presentation.md#viewmodels) is in a centralized spot, so that changes only have to be made in one place.


Delegating More Responsibilities
--------------------------------

The responsibility of the [`Presenter`](#-presenters) is not limited to [`ViewModel`](presentation.md#viewmodels) creation alone, as you can see here:

```cs
/// <summary>
/// An action method with multiple responsibilities.
/// </summary>
public ProductEditViewModel Save(ProductEditViewModel userInput)
{
    // ToEntity
    Product entity = userInput.ToEntity(_repository);

    // Business Logic
    new SideEffect_SetDateModified(entity).Execute();

    // Save
    _repository.Commit();

    // ToViewModel
    var viewModel = entity.ToEditViewModel();
    return viewModel;
}
```

This way the [`Presenter`](#-presenters) layer is also responsible for [retrieving data](../layers.md#data-layer), calling [business logic](../layers.md#business-layer) and converting [`ViewModels`](presentation.md#viewmodels) back to [`Entities`](data-access.md#entities). So it isn't a needless [pass-through layer](../practices-and-principles.md#pass-through-layers-) just delegating [`ViewModel`](presentation.md#viewmodels) creation. It is a [combinator `class`](business-logic.md#facade), in that it combines multiple smaller aspects of the logic, by delegating work to various parts of the system.

A [`Presenter`](#-presenters) action method can have the following steps:

|   |   |
|---|---|
| [`Security` checks](../aspects.md#security) | Executing the necessary [security checks](../aspects.md#security) to prevent unauthorized access.
| [`ViewModel`](presentation.md#viewmodels) [`Validation`](business-logic.md#validators) | If possible, [`Entity`](data-access.md#entities) [`Validation`](business-logic.md#validators) is preferred. But sometimes the data can't be converted to an [`Entity`](data-access.md#entities) yet. [`Validating`](business-logic.md#validators) a [`ViewModel`](presentation.md#viewmodels) might be useful in that case. 
| [`ToEntity`](presentation.md#toentity) | Converting [ViewModel](presentation.md#viewmodels) data into [`Entity`](data-access.md#entities) data.
| [`GetEntities`](data-access.md#repository) | Retrieving [`Entity`](data-access.md#entities) data from the database.    
| [`Business Logic`](../layers.md#business-layer) | Executing the necessary [logic](../layers.md#business-layer) for [data `Validation`](business-logic.md#validators), [`Calculations`](../aspects.md#calculation), and decisions based on the data.
| `Commit` | Saving changes made to the [`Entities`](data-access.md#entities) to the database.
| [`ToViewModel`](presentation.md#toviewmodel) | Mapping [`Entity`](data-access.md#entities) data to the corresponding [`ViewModel`](presentation.md#viewmodels) `properties`, to prepare it for the [`view`](presentation.md#views).
| `NonPersisted Data` | Copying data that does not need to be stored, from the old [`ViewModel`](presentation.md#viewmodels) to the new, such as selections or search criteria.
| `Redirect` | Returning a different [`ViewModel`](presentation.md#viewmodels), to trigger the UI to go to the appropriate page like a success message or the home screen.

This seems a bit of a throw-together of concepts, but that's how it is for a [combinator class](business-logic.md#facade), like our [`Presenters`](#-presenters). Separating these steps is recommended, so that they do not get intermixed or entangled.

Not all of the steps are needed. [`ToEntity`](presentation.md#toentity) / [`Business Logic`](../layers.md#business-layer) / [`ToViewModel`](presentation.md#toviewmodel) might be the typical steps. Slight variations in the order of the steps are also possible.


Complete Example
----------------

Here is a code sample with most of the discussed steps in it:

```cs
public class ProductEditPresenter
{
    public object Save(ProductEditViewModel userInput)
    {
        // Security
        SecurityAsserter.AssertLogIn();

        // ToEntity
        Product entity = userInput.ToEntity(_repository);

        // Business Logic
        IValidator validator = new ProductValidator(entity);
        if (validator.IsValid)
        {
            new SideEffect_SetDateModified(entity).Execute();

            // Save
            _repository.Commit();

            // Redirect
            return new ProductListViewModel();
        }

        // ToViewModel
        var viewModel = entity.ToEditViewModel();

        // Non-Persisted Data  
        viewModel.Validation.Messages = validator.Messages;

        return viewModel;
    }
}
```


Overhead
--------

Even though the actual call to the [business logic](../layers.md#business-layer) might be trivial, it may still be necessary to convert from [`Entity`](data-access.md#entities) to [`ViewModel`](presentation.md#viewmodels) and back.

One reason might be the stateless nature of the web. It requires restoring state from the [`View`](presentation.md#views) to the [`Entity`](data-access.md#entities) model in between requests. This is because the [`ViewModel`](presentation.md#viewmodels) sent to the server may be incomplete, only containing the editable parts of the page. Restoration of [`Entity`](data-access.md#entities) state is also needed to delegate responsibilities to the right parts of the system, like delegate to the [business layer](../layers.md#business-layer), that expects [`Entities`](data-access.md#entities).

You might save the server some work by doing *partial loads* instead of *full loads* or maybe execute client-native code. For more info, see: [First Full Load – Then Partial Load – Then Client-Native Code](presentation.md#first-full-load--then-partial-load--then-client-native-code).


Using ViewModels Directly
-------------------------

Some actions might also operate onto [`ViewModels`](presentation.md#viewmodels) directly instead:

```cs
public void ExpandNode(TreeViewModel viewModel, int id)
{
    var node = viewModel.Nodes.Single(x => x.ID == id);
    node.IsExpanded = true;
}
```

This may not be the first option to consider, but sometimes it makes sense.


Conclusion
----------

The [`Presenter`](#-presenters) pattern is a commonly used design pattern for modeling user interactions in an application. By creating a [`Presenter`](#-presenters) for each [`View`](presentation.md#views) and working with [`ViewModels`](presentation.md#viewmodels), we can achieve a clear modularization of our [presentation logic](../layers.md#presentation-layer) and we ensure that each component has a specific responsibility. Delegating [`ViewModel`](presentation.md#viewmodels) creation to the [`ToViewModel`](presentation.md#toviewmodel) layer enables separation of concerns and allows the [`Presenter`](#-presenters) to focus on its primary responsibility of modeling user interaction and delegating work to the various parts of the system.

The [`Presenters`](#-presenters) form a [platform-independent](../layers.md#platform-independence-1) layer below the actual front-end technology. All logic is hidden under a shell of [`ViewModels`](presentation.md#viewmodels) and user actions. This makes it possible to swap out the front-end while leaving the underlying system intact.

[back](.)

