---
title: "🤖 Patterns : Business Logic"
---

🤖 Patterns : Business Logic
============================

[back](patterns.md)

<h2>Contents</h2>

- [Introduction](patterns.md)
- [RepositoryWrappers](#repositorywrappers)
- [Validators](#validators)
- [SideEffects](#sideeffects)
- [LinkTo](#linkto)
    - [Unlink](#unlink)
    - [NewLinkTo](#newlinkto)
- [Cascading](#cascading)
- [Facade](#facade)
- [Visitor](#visitor)
- [Resource Strings](#resource-strings)


Introduction
------------

[Presentation](layers.md#presentation-layer), [`Entity` model](patterns-data-access.md#entities) and [persistence](aspects.md#persistence) should be straightforward [pattern-wise](patterns.md). If anything 'special' needs to happen it belongs in the [business layer](layers.md#business-layer). Any number of different [patterns](patterns.md) can be used. But also things, that do not follow any standard [pattern](patterns.md).

The [business layer](layers.md#business-layer) externally speaks a language of [`Entities`](patterns-data-access.md#entities) or sometimes [`DTO's`](patterns-data-access.md#dto). Internally it can talk to [`Repository interfaces`](patterns-data-access.md#repository-interfaces) for [data access](aspects.md#persistence).

It is preferred that [business logic](layers.md#business-layer) hooks up with  [`Entity`](patterns-data-access.md#entities) `classes` rather than [`Repositories`](patterns-data-access.md#repository). But there is a large gray area. Using [`Entities`](patterns-data-access.md#entities) improves testability, limits queries and limits interdependence, dependency on a data source and passing around a lot of [`Repository`](patterns-data-access.md#repository) variables.


RepositoryWrappers
------------------

Passing around lots of [`Repositories`](patterns-data-access.md#repository) can create long lists of parameters, prone to change. To prevent that phenomenon, sets of [`Repositories`](patterns-data-access.md#repository) could be combined into [`RepositoryWrappers`](#repositorywrappers). Those can then be passed around instead. This keeps the parameter lists shorter.

You can make a single [`RepositoryWrapper`](#repositorywrappers) with all the [`Repositories`](patterns-data-access.md#repository) out of a [functional domains](namespaces-assemblies-and-folders.md#functional-domains) in it.

Some logic might use [`Repositories`](patterns-data-access.md#repository) out of multiple [domains](namespaces-assemblies-and-folders.md#functional-domains). You could choose to pass around multiple [`RepositoryWrappers`](#repositorywrappers): one for each [domain model](namespaces-assemblies-and-folders.md#functional-domains). But you could also make a custom [`RepositoryWrapper`](#repositorywrappers) with [`Repositories`](patterns-data-access.md#repository) from multiple [functional domains](namespaces-assemblies-and-folders.md#functional-domains).

You may also want to more limited [`RepositoryWrappers`](#repositorywrappers). For instance one for each [partial domain](namespaces-assemblies-and-folders.md#partial-domains). This keeps the width of dependency more narrow, so logic that has nothing to do with certain [`Repositories`](patterns-data-access.md#repository), would not accidentally become dependent on them.

An alternative to [`RepositoryWrappers`](#repositorywrappers) might be [dependency injection](practices-and-principles.md#dependency-injection). Under this [link](practices-and-principles.md#dependency-injection) you can find some criticism about the techique, but that might be due to not using a very safe [dependency injection](practices-and-principles.md#dependency-injection) `API`. [`RepositoryWrappers`](#repositorywrappers) and [dependency injection](practices-and-principles.md#dependency-injection) might also go hand in hand in combination with each other.


Validators
----------

Separate [`Validator`](api.md#jj-framework-validation) `classes` could be used for [validation](aspects.md#validation). Specialized `classes` can be derived from [`VersatileValidator`](api.md#jj-framework-validation) from the [`JJ.Framework`](api.md#jjframework).

It is recommended to keep [`Validators`](api.md#jj-framework-validation) independent from each other.

If multiple [`Validators`](api.md#jj-framework-validation) should go off, you might call them individually one by one.

For complex [`Validator`](api.md#jj-framework-validation), it is suggested to add a [prefix or suffix](code-style.md#prefixes-and-suffixes) to the name such as [`Recursive`](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities) or `Versatile` to make it clear that it is more than a simple [`Validator`](api.md#jj-framework-validation).

Next to [`Validators`](api.md#jj-framework-validation) deciding whether user input is valid, [`Validators`](api.md#jj-framework-validation) could also be used to generate *warnings*, that are not blocking, but help the user work with an app.

[`Validators`](api.md#jj-framework-validation) might also be used for *delete constraints*. For instance when an [`Entity`](patterns-data-access.md#entities) is still in use, you might not be able to delete it.


SideEffects
-----------

The [business layer](layers.md#business-layer) can execute [`SideEffects`](api.md#jj-framework-business) while altering data, for instance to record a *date time modified*, set [default values](aspects.md#defaults), or automatically generate a *name*.

We could implement an `interface` [`ISideEffect`](api.md#jj-framework-business) for each of these. It has only one method: `Execute`. This gives us some polymorphism over [`SideEffects`](api.md#jj-framework-business) so it is easier to handle them *generically* and for instance `Execute` multiple in a row.

Using separate `classes` for [`SideEffects`](api.md#jj-framework-business) can create overview over pieces of logic, creative in nature, and prevent things from getting entangled.

[`SideEffects`](api.md#jj-framework-business) might evaluate conditions internally. The caller of the [`SideEffect`](api.md#jj-framework-business) `class` would not know what conditions there are. A [`SideEffect`](api.md#jj-framework-business) could skip over its own execution, when it wouldn't apply. This makes the [`SideEffect`](api.md#jj-framework-business) fully responsible for what happens. What a [`SideEffect`](api.md#jj-framework-business) does can also depend on [status flagging](aspects.md#entity-status-management).


LinkTo
------

This pattern is about [bidirectional relationship synchronization](aspects.md#bidirectional-relationship-synchronization). That means that if a parent property is set: `myProduct.Supplier = mySupplier`, automatically the product is added to the child collection too: `mySupplier.Products.Add(myProduct)`.

To manage [bidirectional relationships](aspects.md#bidirectional-relationship-synchronization), even when the underlying [persistence technology](aspects.md#persistence) doesn't, we could link [`Entities`](patterns-data-access.md#entities) together using [`LinkTo`](#linkto) extension methods. By calling [`LinkTo`](#linkto), both ends of the relationship are updated. Here is a template for a [`LinkTo`](#linkto) method that works for an `1-to-n` relationship:

```cs
public static void LinkTo(this Child child, Parent parent)
{
    if (child == null) throw new NullException(() => child);

    if (child.Parent != null)
    {
        if (child.Parent.Children.Contains(child))
        {
            child.Parent.Children.Remove(child);
        }
    }

    child.Parent = parent;

    if (child.Parent != null)
    {
        if (!child.Parent.Children.Contains(child))
        {
            child.Parent.Children.Add(child);
        }
    }
}
```

Beware that all the checks and list operations can come with a performance penalty.

You could put the [`LinkTo`](#linkto) methods together in a `class` named `LinkToExtensions`. You might put it in a [`LinkTo`](#linkto) [`namespace`](namespaces-assemblies-and-folders.md#patterns) in your project.

If a [`LinkTo`](#linkto) method name becomes ambiguous, you could suffix it, for instance:

    LinkToParentDocument

### Unlink

Next to [`LinkTo`](#linkto) methods, you might also add [`Unlink`](#unlink) methods in an `UnlinkExtensions class`:

```cs
public static void UnlinkParent(this Child child)
{
    if (child == null) throw new NullException(() => child);
    child.LinkTo((Parent)null);
}
```

### NewLinkTo

If you are linking `objects` together, that you *know are new*, you may use better-performing variations for [`LinkTo`](#linkto), called [`NewLinkTo`](#newlinkto), that omit the expensive checks:

```cs
public static void NewLinkTo(this Child child, Parent parent)
{
    if (child == null) throw new NullException(() => child);
    child.Parent = parent;
    parent.Children.Add(child);
}
```

But beware that [`LinkTo`](#linkto) might be a better choice, because executing [`NewLinkTo`](#newlinkto) onto *existing* `objects` may corrupt the `object` graph.


Cascading
---------

[`Cascading`](aspects.md#cascading) means that upon `Deleting` a main [`Entity`](patterns-data-access.md#entities), *child-*[`Entities`](patterns-data-access.md#entities) are `Deleted` too. But if they are not inherently part of the main [`Entity`](patterns-data-access.md#entities), they would be [`Unlinked`](#unlink) instead.

This can be implemented as a pattern in [`C#`](api.md#csharp). A reason to do it in [`C#`](api.md#csharp), is to explicitly see in the code, that the other `Deletions` take place. It may be important not to hide this from view.

One way to implement [`Cascading`](aspects.md#cascading), is through extension methods:  
`DeleteRelatedEntities` and `UnlinkRelatedEntities`.

Contents:

- [Code Files](#cascading-code-files)
- [DeleteRelatedEntities](#deleterelatedentities)
- [UnlinkRelatedEntities](#unlinkrelatedentities)
- [Delete Main Entity](#cascading-delete-main-entity)
- [Cascading & Repositories](#cascading-and-repositories)
- [Nuance](#cascading-nuance)
- [Conclusion](#cascading-conclusion)


<h4 id="cascading-code-files">Code Files</h4>

Here is a suggestion for how to organize the [`Cascading`](#cascading) code.

In the `csproj` of the [`Business` layer](layers.md#business-layer), you could put a [sub-folder](namespaces-assemblies-and-folders.md#patterns) called [`Cascading`](#cascading) and put two code files in it:

```
JJ.Ordering.Business.csproj
    |
    |- Cascading
        |
        |- DeleteRelatedEntitiesExtensions.cs
        |- UnlinkRelatedEntitiesExtensions.cs
```


<h4 id="deleterelatedentities">DeleteRelatedEntities</h4>

Here is how  `DeleteRelatedEntitiesExtensions.cs` might look internally:

```cs
/// <summary>
/// Deletes child entities inherently part of the main entity.
/// </summary>
public static class DeleteRelatedEntitiesExtensions
{
    public static void DeleteRelatedEntities(this Order order)
    {
        ...
    }
}
```

In there, child [`Entities`](patterns-data-access.md#entities) are successively `Deleted`:

```cs
/// <summary>
/// Deletes child entities inherently part of the main entity.
/// </summary>
public static class DeleteRelatedEntitiesExtensions
{
    public static void DeleteRelatedEntities(this Order order)
    {
        // Delete child entities.
        foreach (var orderLine in order.OrderLines.ToArray())
        {
            _repository.Delete(orderLine);
        }
    }
```

(Note: The `ToArray` can prevent an `Exception` about the loop collection being modified.)

Before an extension method `Deletes` a child [`Entity`](patterns-data-access.md#entities), it might call [`Cascading`](#cascading) upon the child [`Entity`](patterns-data-access.md#entities) too!

```cs
public static void DeleteRelatedEntities(this Order order)
{
    // Delete child entities.
    foreach (var orderLine in order.OrderLines.ToArray())
    {
        // Call cascading on the child entity too!
        orderLine.UnlinkRelatedEntities(); 

        _repository.Delete(orderLine);
    }
}
```


<h4 id="unlinkrelatedentities">UnlinkRelatedEntities</h4>

`UnlinkRelatedEntities` might be a little bit easier. It neither requires [`Repositories`](patterns-data-access.md#repository) not does it do much recursion:

```cs
/// <summary>
/// Unlinks related entities, not inherently part of the main entity.
/// </summary>
public static class UnlinkRelatedEntitiesExtensions
{
    public static void UnlinkRelatedEntities(this OrderLine orderLine)
    {
        orderLine.UnlinkOrder();
        orderLine.UnlinkProduct();
    }
}
```

Note that it uses the [Unlink](#unlink) pattern discussed earlier.


<h4 id="cascading-delete-main-entity">Delete Main Entity</h4>

The [`Cascading`](#cascading) extension methods delete *related* [`Entities`](patterns-data-access.md#entities), not the *main* [`Entity`](patterns-data-access.md#entities). The idea behind that is: Where a main [`Entity`](patterns-data-access.md#entities) is `Deleted`, we could call the [`Cascading`](#cascading) methods first:

```cs
entity.DeleteRelatedEntities();
entity.UnlinkRelatedEntities();

// Delete main entity separately.
_repository.Delete(entity);
```

That way we can see explicitly that more `Deletions` take place.


<h4 id="cascading-and-repositories">Cascading & Repositories</h4>

The [`DeleteRelatedEntities`](#deleterelatedentities) methods might need [`Repositories`](patterns-data-access.md#repository) to perform the `Delete` operations.

You could pass these [`Repositories`](patterns-data-access.md#repository) as *parameters:*

```cs
public static void DeleteRelatedEntities(
    this Order order,
    /* Repository parameter */
    IOrderLineRepository repository)
{
    foreach (var orderLine in order.OrderLines.ToArray())
    {
        orderLine.UnlinkRelatedEntities();
        repository.Delete(orderLine);
    }
}
```

Or you might make [repositories](patterns-data-access.md#repository) available through a technique called [dependency injection](practices-and-principles.md#dependency-injection).
 
It's up to you. The choice to use *extension* methods was also a matter of preference.


<h4 id="cascading-nuance">Nuance</h4>

Sometimes an [`Entity`](patterns-data-access.md#entities) does have related [`Entities`](patterns-data-access.md#entities) to [`Cascadedly`](#cascading) [`Unlink`](#unlink) or `Delete`, but sometimes it doesn't, creating subtleties in the implementation.


<h4 id="cascading-conclusion">Conclusion</h4>

Hopefully this introduced a way to build up [`Cascading`](#cascading) code by just using a pattern.


Facade
------

A [`Facade`](#facade) combines several related (usually [`CRUD`](layers.md#crud)) operations into one `class` that also performs additional [business logic](layers.md#business-layer) and [`Validation`](#validators), [`SideEffects`](#sideeffects), integrity constraints, [conversions](aspects.md#conversion), etc. It delegates to other `classes` to do the work. If you do something using a [`Facade`](#facade) you should be able to count on it that integrity is maintained.

It is a combinator `class`: a [`Facade`](#facade) combines other (smaller) parts of the [business layer](layers.md#business-layer) into one, offering a single entry point for a lot of related operations. A [`Facade`](#facade) can be about a [partial functional domain](namespaces-assemblies-and-folders.md#partial-domains), so managing a *set* of [`Entity`](patterns-data-access.md#entities) `types` together.

<h4>Repositories instead of Facades</h4>

[`Facades`](#facade) may typically contain [`CRUD`](layers.md#crud) operations, that could be used as an entry point for all your [business logic](layers.md#business-layer) and [data access](layers.md#crud) needs. But in some cases, it may be more appropriate to use the [data access layer](layers.md#data-layer) directly.

For example, a simple `Get` by `ID` may be better going through a [`Repository`](patterns-data-access.md#repository). There could be other cases where using [`Repositories`](patterns-data-access.md#repository) directly is a better choice. For instance in the [`ToEntity`](patterns-presentation.md#toentity) and [`ToViewModel`](patterns-presentation.md#toviewmodel) code, which is usually straightforward [data conversion](aspects.md#conversion).

The reason is, that a [`Facade`](#facade) could create an excessive amount of dependency and high degree of coupling. Because simple operations executed frequently, would require a reference to a [`Facade`](#facade), a [combinator](#facade) `class`, naturally dependent on many other `objects`. So, for a simple `Get` it may be better to use a [`Repository`](patterns-data-access.md#repository), to limit the interdependence between things.


Visitor
-------

<h4>Contents</h4>

- [Introduction](#visitor-introduction)
- [Visit Methods](#visit-methods)
- [Base Visitor](#base-visitor)
- [Specialized Visitors](#specialized-visitors)
- [Optimization](#visitor-optimization)
- [Entry Points](#visitor-entry-points)
- [Using Fields](#visitor-using-fields)
- [Polymorphic Visitation](#polymorphic-visitation)
- [Change the Sequence](#visitor-change-the-sequence)
- [Accept Methods](#accept-methods)
- [Alternatives](#visitor-alternatives)
- [Conclusion](#visitor-conclusion)


<h4 id="visitor-introduction">Introduction</h4>

A [`Visitor`](#visitor) `class` processes a recursive tree structure that might involve many `objects` and multiple `types` of `objects`. Usually a [`Visitor`](#visitor) translates a complex structure into something else. Examples are calculating total costs over a recursive structure, or filtering down a whole `object` graph by complex criteria. [`Visitors`](#visitor) can result in well performing code.

Whenever a whole recursive structure needs to be processed, the [`Visitor`](#visitor) pattern may be a good way to go.


<h4 id="visit-methods">Visit Methods</h4>

A [`Visitor`](#visitor) `class` has a set of [`Visit`](#visit-methods) methods, e.g. `VisitOrder`, `VisitProduct`, typically one for every `type`:

```cs
class Visitor
{
    void VisitOrder(Order order) { }
    void VisitOrderLine(OrderLine orderLine) { }
    void VisitProduct(Product product) { }
}
```

It can also have separate [`Visit`](#visit-methods) methods for `collections`:

```cs
void VisitOrderLines(IList<OrderLine> orderLines) { }
```

And there might be [`Visit`](#visit-methods) methods for special cases:

```cs
void VisitPhysicalProduct(Product product) { }
void VisitDigitalProduct(Product product) { }
```


<h4 id="base-visitor">Base Visitor</h4>

A `base` [`Visitor`](#visitor) might simply follow the whole recursive structure, and has a [`Visit`](#visit-methods) method for each node. Here is an example where an `Order` structure is `Visited`:

```cs
class OrderVisitorBase
{
    /// <summary>
    /// VisitOrder processes the child objects:
    /// Customer, Supplier and OrderLines.
    /// </summary>
    protected virtual void VisitOrder(Order order)
    {
        VisitCustomer(order.Customer);
        VisitSupplier(order.Supplier);

        foreach (var orderLine in order.OrderLines.ToArray())
        {
            VisitOrderLine(orderLine);
        }
    }

    /// <summary>
    /// VisitOrderLine also processes its child object: Product.
    /// </summary>
    protected virtual void VisitOrderLine(OrderLine orderLine)
        => VisitProduct(orderLine.Product);

    protected virtual void VisitCustomer(Customer customer) { }
    protected virtual void VisitSupplier(Supplier supplier) { }
    protected virtual void VisitProduct(Product product) { }
}
```

The ones with *child objects* also call [`Visit`](#visit-methods) on their children. Those without children have empty implementations.


<h4 id="specialized-visitors">Specialized Visitors</h4>

You can make *specialized* [`Visitor`](#visitor) classes, by overriding the [`Visit`](#visit-methods) methods.

If you only want to process certain `types` of `objects`, you can override [`Visit`](#visit-methods) methods for those `types` only:

```cs
/// <summary>
/// This specialized Visitor only processes
/// OrderLines and Products,
/// so the respective Visit methods are overridden.
/// </summary>
class OrderSummaryVisitor : OrderVisitorBase
{
    protected override void VisitOrderLine(OrderLine orderLine)
        => base.VisitOrderLine(orderLine);

    protected override void VisitProduct(Product product) 
        => base.VisitProduct(product);
}
```

They call their `base` methods. Keep those calls in there, so the `base` will process the rest of the recursive structure!

The aim for this simple new [`Visitor`](#visitor) is to create a text, that summarizes the `Order`. 
Here is the code that uses a `StringBuilder` for this:

```cs
/// <summary>
/// Here the Visit methods are extended,
/// creating a text that summarizes the Order.
/// </summary>
class OrderSummaryVisitor : OrderVisitorBase
{
    StringBuilder _sb = new();

    protected override void VisitOrderLine(OrderLine orderLine)
    {
        _sb.Append($"{orderLine.Quantity} x ");

        base.VisitOrderLine(orderLine);
    }

    protected override void VisitProduct(Product product)
    {
        _sb.Append($"{product.Name}");
        _sb.AppendLine();

        base.VisitProduct(product);
    }
}
```

The result of the process might be a text like this:

```
1 x Cool Gadget
2 x Fidget Thing
```

Don't be underwhelmed. This is just a simple example. The more complicated the structures: this is where the [`Visitor`](#visitor) pattern really starts to shine.


<h4 id="visitor-optimization">Optimization</h4>

You can make the performance better by `overriding` [`Visit`](#visit-methods) methods for skipping parts of the recursive structure that don't you don't need:

```cs
/// <summary>
/// This Visitor aims to optimize the recursive process.
/// </summary>
class OrderSummaryVisitor : OrderVisitorBase
{
    /// <summary>
    /// Override VisitOrder and leave out part of the recursion.
    /// </summary>
    protected override void VisitOrder(Order order)
    {
        // Customer and Supplier are skipped here for optimization.

        foreach (var orderLine in order.OrderLines.ToArray())
        {
            VisitOrderLine(orderLine);
        }

        // Don't call base here. This method replaced it.
    }
}
```

However, be mindful of the trade-off between performance and completeness, as skipping parts of the structure also means missing out on the deeper objects.


<h4 id="visitor-entry-points">Entry Points</h4>

`Public` methods can show us the starting point of the recursion, making it easier to understand where the process begins:

```cs
class OrderSummaryVisitor : OrderVisitorBase
{
    StringBuilder _sb = new();

    /// <summary>
    /// This Execute method is the only one that's public.
    /// This makes it clear where the process starts.
    /// The Visit methods are kept protected
    /// for internal processing.
    /// </summary>
    public string Execute(Order order)
    {
        VisitOrder(order);
        return _sb.ToString();
    }

    ...
}
```

Here is the complete code sample of our derived `Visitor`:

```cs
class OrderSummaryVisitor : OrderVisitorBase
{
    StringBuilder _sb = new();

    public string Execute(Order order)
    {
        VisitOrder(order);
        return _sb.ToString();
    }

    protected override void VisitOrderLine(OrderLine orderLine)
    {
        _sb.Append($"{orderLine.Quantity} x ");

        base.VisitOrderLine(orderLine);
    }

    protected override void VisitProduct(Product product)
    {
        _sb.Append($"{product.Name}");
        _sb.AppendLine();

        base.VisitProduct(product);
    }
}
```


<h4 id="visitor-using-fields">Using Fields</h4>

The result of a [`Visitor's`](#visitor) operation is typically stored in *fields* and used across multiple [`Visit`](#visit-methods) methods. In the examples above, we are talking about the `StringBuilder` field. The result structure might not have a straightforward, 1-to-1 relationship with the source structure. This makes fields the better choice over parameters and return values. It makes our `base` [`Visitors`](#visitor) more reusable as well.


<h4 id="polymorphic-visitation">Polymorphic Visitation</h4>

A `Customer` and `Supplier` might both derive from a `Party base` type:

```cs
class Supplier : Party { }
class Customer : Party { }
```

Sometimes there is a [`Visit`](#visit-methods) method for each concrete `type`. A [`Visitor`](#visitor) `class` might allow tapping into different levels of the abstraction like this:

```cs
protected virtual void VisitPartyPolymorphic(Party party)
{
    switch (party)
    {
        case Supplier supplier:
            VisitSupplier(supplier);
            break;

        case Customer customer:
            VisitCustomer(customer);
            break;
    }
}

protected virtual void VisitSupplier(Supplier supplier)
    => VisitPartyBase(supplier);

protected virtual void VisitCustomer(Customer customer)
    => VisitPartyBase(customer);

protected virtual void VisitPartyBase(Party party) { }
```

This way you can separately `override` a [`Visit`](#visit-methods) method for `Supplier` or `Customer`.

But you could also `override` `VisitPartyBase` instead, where you wish to handle both `Parties` the same way.

The `VisitPartyPolymorphic` method is best used for switching between different types. It might not be the first choice for `overriding`. However, it's still the best method to *call*, as it ensures that all specialized [`Visit`](#visit-methods) methods are called.

You need all those methods delegating in the right order, for the visitation to work properly.

Here is another example of polymorphic visitation, where we don't `switch` on an `object type`, but on an `enum` instead:

```cs
protected virtual void VisitProductPolymorphic(Product product)
{
    var productTypeEnum = product.GetProductTypeEnum();
    switch (productTypeEnum)
    {
        case ProductTypeEnum.Physical:
            VisitPhysicalProduct(product);
            break;

        case ProductTypeEnum.Digital:
            VisitDigitalProduct(product);
            break;
    }
}

protected virtual void VisitPhysicalProduct(Product product)
    => VisitProductBase(product);

protected virtual void VisitDigitalProduct(Product product)
    => VisitProductBase(product);

protected virtual void VisitProductBase(Product product) { }
```

This way we can create [`Visit`](#visit-methods) methods for specific cases if needed.

Here is a full example of a [`Visitor`](#visitor) `base class` with polymorphic [`Visit`](#visit-methods) methods:

```cs
class PolymorphicVisitorBase
{
    protected virtual void VisitOrder(Order order)
    {
        VisitPartyPolymorphic(order.Customer);
        VisitPartyPolymorphic(order.Supplier);
        VisitOrderLines(order.OrderLines);
    }

    protected virtual void VisitPartyPolymorphic(Party party)
    {
        switch (party)
        {
            case Supplier supplier:
                VisitSupplier(supplier);
                break;

            case Customer customer:
                VisitCustomer(customer);
                break;
        }
    }

    protected virtual void VisitSupplier(Supplier supplier)
        => VisitPartyBase(supplier);

    protected virtual void VisitCustomer(Customer customer)
        => VisitPartyBase(customer);

    protected virtual void VisitPartyBase(Party party) { }

    protected virtual void VisitOrderLines(IList<OrderLine> orderLines)
    {
        foreach (OrderLine orderLine in orderLines)
        {
            VisitOrderLine(orderLine);
        }
    }

    protected virtual void VisitOrderLine(OrderLine orderLine)
        => VisitProductPolymorphic(orderLine.Product);

    protected virtual void VisitProductPolymorphic(Product product)
    {
        var productTypeEnum = product.GetProductTypeEnum();
        switch (productTypeEnum)
        {
            case ProductTypeEnum.Physical:
                VisitPhysicalProduct(product);
                break;

            case ProductTypeEnum.Digital:
                VisitDigitalProduct(product);
                break;
        }
    }

    protected virtual void VisitPhysicalProduct(Product product)
        => VisitProductBase(product);

    protected virtual void VisitDigitalProduct(Product product)
        => VisitProductBase(product);

    protected virtual void VisitProductBase(Product product) { }
}
```

This may seem like a lot of code, but note that the `base class` is set up with fixed patterns, and is written only once, so that the specialized [`Visitor`](#visitor) `classes` can be made much simpler.


<h4 id="visitor-change-the-sequence">Change the Sequence</h4>

You might also `override` a [`Visit`](#visit-methods) method to change the order in which things are processed:

```cs
/// <summary>
/// This Visitor changes the order of processing.
/// </summary>
class ReversedOrderVisitor : OrderVisitorBase
{
    protected override void VisitOrder(Order order)
    {
        foreach (var orderLine in order.OrderLines.ToArray())
        {
            VisitOrderLine(orderLine);
        }

        // Visit Customer and Supplier last instead of first.
        VisitCustomer(order.Customer);
        VisitSupplier(order.Supplier);
    }
}
```


<h4 id="accept-methods">Accept Methods</h4>

The *classic* [`Visitor`](#visitor) pattern has a bit of a drawback in my opinion. It requires that `classes` *used by* the [`Visitor`](#visitor) have to be *adapted*. `Accept` methods would be added to them. I think this is adapting the wrong `classes`. My advice would be not to do that, and leave out these `Accept` methods.

This would keep the [`Visitor`](#visitor) `classes` self-sufficient and separated from the rest of the code.

However, `Accept` methods could be useful for specialized use-cases for instance to prevent the [polymorphic visitation](#polymorphic-visitation) proposed earlier.


<h4 id="visitor-alternatives">Alternatives</h4>

However, there are also alternatives for the [`Visitor`](#visitor) pattern.

For instance, [`JJ.Framework.Collections`](api.md#jj-framework-collections), which has a method for [`LINQ`](api.md#linq)-style processing of recursive structures: [`.SelectRecursive`](https://www.nuget.org/packages/JJ.Framework.Collections#recursive-collection-extensions), which might work for simpler scenarios.

You could also skip the [`base Visitor`](#base-visitor) and program a (recursive) [converter](aspects.md#conversion) instead, if you're only interested in a specific part of the structure.

But the [`Visitor`](#visitor) pattern might be more ideal, when the structure is quite complicated, or when you want to process the same structure in many different ways.

<h4 id="visitor-conclusion">Conclusion</h4>

By creating a `base` [`Visitor`](#visitor) and multiple specialized [`Visitors`](#visitor), you can create short and powerful code for processing recursive structures. A coding error is easily made, and can break calculations easily. However, it is the best and fastest choice for complicated processes that involve complex recursive structures.

Another good example of a [`Visitor`](#visitor) `class` is [`.NET's`](api.md#dotnet) own [`ExpressionVisitor`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expressionvisitor). However, the style of the [`Visitors`](#visitor) might be different here. It can still be called a [`Visitor`](#visitor) if it operates by slightly different rules.


Resource Strings
----------------

<h4 id="">Contents</h4>

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

To store `Button Texts` and translations of [domain](patterns-data-access.md#entities) terminology in [`.NET`](api.md#dotnet) projects, you could use `resx` files.


<h4 id="resource-strings-visual-studio-editor">Visual Studio Editor</h4>

Here's what the `Resource strings` editor looks like in [`Visual Studio`](api.md#visual-studio):

![String Resource Editor](images/resource-string-editor.png)


<h4 id="resource-string-file-names">File Names</h4>

[`.NET`](api.md#dotnet) returns the translations in the right language (of the `CurrentCulture`) if you name your `Resource` files like this:

    Resources.resx
    Resources.nl-NL.resx
    Resources.de-DE.resx

[`CultureNames`](https://www.csharp-examples.net/culture-names/) like `nl-NL` and `de-DE` are commonly used within [`.NET`](api.md#dotnet).

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

You can streamline your code and minimize the risk of typos by using the `ResourceFormatterHelper` from the [`JJ.Framework`](api.md#jj-framework-resourcestrings):

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

[`JJ.Framework.ResourceStrings`](api.md#jj-framework-resourcestrings) goes even further than that. It provides reusable [`Resources`](#resource-strings) for common phrases like `Delete`, `Edit`, `Save`, and more. No more typing out the same messages over and over again!


<h4 id="resource-strings-use-the-business-layer">Use the Business Layer</h4>

[`Resource strings`](#resource-strings) may play a role beyond just presentation. They're also commonly used in the [business layer](layers.md#business-layer). Keeping the `DisplayNames` for [model](patterns-data-access.md#entities) properties in the [`business layer`](layers.md#business-layer) makes it possible to reuse them from multiple places.


<h4 id="resource-strings-more-information">For More Information</h4>

For extra information in Dutch about how to structure the [`Resource` files](#resource-string-file-names), see [Appendix B](appendices.md#appendix-b-knopteksten-en-berichtteksten-in-applicaties-resource-strings--dutch-).

[back](patterns.md)
