---
title: "🕸️ Patterns : Presentation (MVC)"
---

`[ Draft ]`

🕸️ Patterns : Presentation (MVC)
================================

[back](README.md)

<h2>Contents</h2>

- [Controller](#controller)
- [Post-Redirect-Get](#post-redirect-get)
- [ValidationMessages in ModelState](#validationmessages-in-modelstate)
- [Polymorphic RedirectToAction / View()](#polymorphic-redirecttoaction--view)
- [For Loops for Lists in HTTP Postdata](#for-loops-for-lists-in-http-postdata)
- [Return URL's](#return-urls)
- [Back Buttons](#back-buttons)
- [TODO](#todo)


Controller
----------

In an [`ASP.NET MVC`](../api/table.md#mvc) application a [`Controller`](presentation-mvc.md#controller) has a lot of responsibilities, but in this [architecture](..) most of the responsibility is delegated to [`Presenters`](presenters.md#-presenters). The responsibilities that are left for the [`MVC`](../api/table.md#mvc) [`Controllers`](#controller) are the URL routing, the HTTP verbs, redirections, setting up infrastructural context and miscellaneous [`MVC`](../api/table.md#mvc) quirks.

The [`Controller`](#controller) may use multiple [`Presenters`](presenters.md#-presenters) and [`ViewModels`](viewmodels.md), since it is about multiple screens.

[`Entity`](data-access.md#entities) names put in [`Controller`](#controller) are plural by convention. So Customer**s**Controller not `CustomerController`.


Post-Redirect-Get
-----------------

This is a quirk intrinsic to [`ASP.NET MVC`](../api/table.md#mvc). We must conform to the Post-Redirect-Get pattern to make sure the page navigation works as expected.

At the end of a post action, you must call `RedirectToAction()` to redirect to a Get action.

Before you do so, you must store the [`ViewModel`](viewmodels.md) in the `TempData` dictionary. In the Get action that you redirect to, you have to check if the [`ViewModel`](viewmodels.md) is in the TempData dictionary. If the [`ViewModel`](viewmodels.md) exist in the TempData, you must use that [`ViewModel`](viewmodels.md), otherwise you must create a new [`ViewModel`](viewmodels.md).

Here is simplified pseudo-code in which the pattern is applied.

```cs
public ActionResult Edit(int id)
{
    object viewModel;
    if (!TempData.TryGetValue(TempDataKeys.ViewModel, out viewModel))
    {
      // TODO: Call Presenter
    }
    return View(viewModel);
}

[HttpPost]
public ActionResult Edit(EditViewModel viewModel)
{
    // TODO: Call Presenter
    TempData[TempDataKeys.ViewModel] = viewModel2;
    return RedirectToAction(ActionNames.Details);
}
```

There might be an exception to the rule to always `RedirectToAction` at the end of a Post. When you would redirect to a page that you can never go to directly, you might return `View()` instead, because there is no `Get` method. This may be the case for a `NotFoundViewModel` or a `DeleteConfirmedViewModel`.

`< TODO:`

`- Mention that return View in case of validation messages is the way to go, because otherwise MVC will not remember un-mappable wrong input values, like Guids and dates entered as strings. (In one case this lead to the browser asking for resending postdata upon clicking the back button, so check whether this is actually a good idea.)`  
`- Not using return View() in a post action makes old values not be remembered. >`

<h4>Considerations</h4>

If you do not conform to the Post-Redirect-Get pattern in [`MVC`](../api/table.md#mvc), you may get to see ugly URL's. When you hit the back button, you might go to an unexpected page, or get an error. You may see original values that you changed re-appear in the user interface. You may also see that [`MVC`](../api/table.md#mvc) keeps complaining about [validation](business-logic.md#validators) errors, that you already resolved. So conform to the Post-Redirect-Get pattern to stay out of trouble.


ValidationMessages in ModelState
--------------------------------

For the architecture to integrate well with [`MVC`](../api/table.md#mvc), you have to make [`MVC`](../api/table.md#mvc) aware that there are [validation](business-logic.md#validators) messages, after you have gotten a [`ViewModel`](viewmodels.md) from a [`Presenter`](presenters.md#-presenters). If you do not do this, you will get strange application navigation in case of [validation](business-logic.md#validators) errors.

You do this in an [`MVC`](../api/table.md#mvc) HTTP GET action method.

The way we do it here is as follows:

```cs
if (viewModel.ValidationMessages.Any())
{
    ModelState.AddModelError(
        ControllerHelper.DEFAULT_ERROR_KEY,
        ControllerHelper.GENERIC_ERROR_MESSAGE);
}
```

In theory we could communicate all [validation](business-logic.md#validators) messages to [`MVC`](../api/table.md#mvc) instead of just communicating a single generic error message. In theory [`MVC`](../api/table.md#mvc) could be used to color the right input fields red automatically, but in practice this breaks easily without an obvious explanation. So instead we manage it ourselves. If we want a [validation](business-logic.md#validators) summary, we simply render all the [validation messages from the [`ViewModel`](viewmodels.md) ourselves and not use the `Html.ValidationSummary()` method at all. If we want to change the appearance of input fields if they have [validation](business-logic.md#validators) errors, then the [`ViewModel`](viewmodels.md) should give the information that the appearance of the field should be different. Our [`View's`](presentation.md#views) content is totally managed by the [`ViewModel`](viewmodels.md).


Polymorphic RedirectToAction / View()
-------------------------------------

A [`Presenter`](presenters.md#-presenters) action method may return different types of [`ViewModels`](viewmodels.md).

This means that in the [`MVC`](../api/table.md#mvc) [`Controller`](#controller) action methods, the [`Presenter`](presenters.md#-presenters) returns `object` and you should do polymorphic type checks to determine which [`View`](presentation.md#views) to go to.

Here is simplified code for how you can do this in a post method:

```cs
var editViewModel = viewModel as EditViewModel;
if (editViewModel != null)
{
    return RedirectToAction(ActionNames.Edit, new { id = editViewModel.Question.ID });
}

var detailsViewModel = viewModel as DetailsViewModel;
if (detailsViewModel != null)
{
    return RedirectToAction(ActionNames.Details, new { id = viewModel.Question.ID });
}
```

At the end throw the following exception (from [`JJ.Framework.Exceptions`](../api/table.md#jj-framework-exceptions)):

```cs
throw new UnexpectedTypeException(() => viewModel);
```

To prevent repeating this code for each [`Controller`](#controller) action, you could program a generalized method that returns the right ActionResult depending on the [`ViewModel`](viewmodels.md) type. Do consider the performance penalty that it may impose and it is worth saying that such a method is not very easy code.


For Loops for Lists in HTTP Postdata
------------------------------------

An alternative to for [posting collections](../aspects.md#postdata-over-http) is using for-loops.

```cs
@Html.TextBoxFor(x => x.MyItem.MyProperty)

@for (int i = 0; i < Model.MyItem.MyCollection.Count; i++)
{
    @Html.TextBoxFor(x => x.MyItem.MyCollection[i].MyProperty)
}
```

This solution only works if the expressions you pass to the `Html` helpers contain the full path to a [`ViewModel`](viewmodels.md) property (or hack the `HtmlHelper.ViewData.TemplateInfo.HtmlFieldPrefix`) and therefore it does not work if you want to split up your [`View`](presentation.md#views) code into partials.


Return URL's
------------

- Return URL's indicate what page to go back to when you are done in another page.
- It is used when you are redirected to a login screen, so it knows what page to go back to after you login.
- Return URL's are encoded into a URL parameter, called 'ret' e.g.:
  `http://www.mysite.com/Login?`__`ret=%2FMenu%2FIndex`__

The ret parameter is the following value encoded:  `/Menu/Index`  
That is the URL you will go back to after you log in.

The Login action can redirect to the ret URL like this:

```cs
[HttpPost]
public ActionResult Login(... string ret = null)
{
    ...
    return Redirect(ret);
    ...
}
```

ASSIGN DIFFERENT RET FOR FULL PAGE LOAD OR [`AJAX`](../api/web.md#ajax) CALL.

- For full page loads, the ret parameter must be set to:

  ```cs
  Request.RawUrl
  ```

- For [`AJAX`](../api/web.md#ajax) calls the ret parameter must be set to:

  ```cs
  Url.Action(ActionNames.Index)
  ```

- The ret parameter is set in a [`Controller`](#controller) action method, when you return the ActionResult. Example:

  EXAMPLE WORKS FOR FULL PAGE LOAD ONLY!!!

  ```cs
  return RedirectToAction(
    ActionNames.Login,
    ControllerNames.Account,
    new { ret = Request.RawUrl });
  ```

- A return URL should always be optional, otherwise you could never serparately debug a [`View`](presentation.md#views).
- That way you have an easily codeable, well maintainable solution.
- Do not use RefferrerUrl, because that only works for HttpPost, not HttpGet. Use Request.RawUrl instead.

- There is a built-in error proneness in return URLs'. If you pass the same return URL along multiple HTTP requests, only one action has to forget to pass along the return URL and a back or close button is broken and you will find out very late that it is, because it is not an obvious thing to test. The same error-proneness is there for return actions with return actions with return actions, or with bread-crumb like structures with multiple return actions built in.

`< TODO: Incorporate this: Ret parameters can be done with new { ret = Request.RawUrl } for full load, and for AJAX this works: { ret = Url.Action(ActionNames.Index) } if you always make sure you have an Index action in your Controller, which is advisable. >`


Back Buttons
------------

There is a pitfall in builing back buttons. If you mix back buttons being handled at the server side, compared to window.history.back() at the client-side, you run the risk that the back button at one point keeps flipping back and foreward between pages.


TODO
----

`< TODO: Mention ModelState.ClearErrors. >`

`< TODO: Mention: Using Request.UrlReferrer in Http Get actions crashes. Use Request.RawUrl. >`

[back](README.md)

