<style>thead{display:none;}</style>

🌐 Service Oriented Architecture
=================================

[back](.)

<h3>Contents</h3>

- [Introduction](#introduction)
- [The ESB Concept](#the-esb-concept)
- [Canonical Model](#canonical-model)
- [Less Integration Code](#less-integration-code)
- [Clearer Integration Code](#clearer-integration-code)
- [In Practice](#in-practice)
- [Memory](#memory)
- [Standard ESB vs Custom ESB](#standard-esb-vs-custom-esb)
- [ESB Model](#esb-model)
    - [Enterprises](#enterprises)
    - [ConnectionTypes](#connectiontypes)
    - [Connections](#connections)
    - [KeyMappings](#keymappings)
    - [Transmissions](#transmissions)
- [Service Implementations](#service-implementations)
- [Multi-Dispatch](#multi-dispatch)
- [Namespaces](#namespaces)
- [Service-Related Patterns](#service-related-patterns)
    - [IsSupported](#issupported)
    - [Facade](#facade)
    - [Hidden Infrastructure](#hidden-infrastructure)
    - [Tag Model](#tag-model)
    - [Canonical KeyMapping](#canonical-keymapping)


Introduction
------------

What has been described so far is the [application architecture](introduction.md#application-architecture-vs-service-architecture). Another part of this [software architecture](index.md) is the *service oriented architecture.* It is mainly about linking systems together. Currently the main technology used is `WCF`.


The ESB Concept
---------------

`ESB` stands for *Enterprise Service Bus*, which is a system for exchanging data between different systems of different organizations in different formats with different protocols. Central components are used to make integration between these systems more manageable. One relevant concept is a [`Canonical`](#canonical-model) model.


Canonical Model
---------------

A `Canonical` model helps us exchange data between systems. Data can be retrieved from multiple systems and is converted to a `Canonical` form, so that the same code may be reused for data that comes from various systems. The aim is for the `Canonical` model to be as pure and general as possible, so indeed information of any system can fit into it with few modifications.


Less Integration Code
---------------------

Say you have `4` systems: `A`, `B`, `C` and `D` and you want to connect all `4` of them together. Theoretically you would have to write `12` different message conversions. See alls the arrows in the diagram below:

<img src="images/no-esb.png" width="133"/>

By connecting a system to the `ESB`, instead of connecting individual systems together, you have to implement only `8` different message conversions. The number of arrows below is reduced:

<img src="images/esb.png" width="150"/>

You just saved yourself __33%__ of the work!

With every added system it gets better. You can see this from the numbers below, that indicate the amount of message conversions.

<img src="images/esb-connection-counts.png" width="325" />

The first integration between 2 systems you actually program more message conversions by using an `ESB`. But the next system it's already a tie between `ESB` and no `ESB`. The 4th integration you introduce, you will have saved 33% of the overall work.

It gets better with each system you introduce to your `ESB`. When messages from one system are converted to and from the [`Canonical`](#canonical-model) model, you can automatically connect it to all the other systems.


Clearer Integration Code
------------------------

But it gets better. You save yourself even more work. The code to convert a message to [`Canonical`](#canonical-model) model is often easier than converting from one system's format to the other system's format. Instead of converting from one quirky format to another quirky format, which is quite difficult to do, you convert from one quirky format to a more straightforward format, which is quite a lot easier to program.


In Practice
-----------

In practice not every system tends to send every type of message back and forth to every other system. And sometimes the messaging is not bidirectional but one-way only. But the benefits of an `ESB` still hold and systems would be linked with less code and less effort than custom programming every integration.


Memory
------

An added benefit to the `Canonical` model is that it tends to live in memory. Thit means that changes to it, do not require any data migrations. You would just refactor the conversion code. This makes it lower impact and more flexible.


Standard ESB vs Custom ESB
--------------------------

There is standard `Enterprise Service Bus` software. Yet, you might choose to build a *custom* one. The concepts might be easier to implement than you think. And standard `ESB's` are complex and have a steep learning curve, require training, specialists. This all while you are going to have to custom program much of the message conversion yourself anyway, and design your own [`Canonical`](#canonical-model) model, which is basically all of the work. Therefore building it yourself might be a viable option.


ESB Model
---------

On top of a [`Canonical`](#canonical-model) model, we might need more facilities. The `ESB` [model](patterns.md#entity) could offer a model for administrating [`Connection`](#connections) settings and register [`Enterprises`](#enterprises) that can log in to our system to get access to our services.

Next: the main [entities](patterns.md#entity) of an `ESB` model.

### Enterprises

Every `Enterprise` involved in this service architecture would be registered in the `ESB` database. Some of these `Enterprises` will actually log into our system. Those will get an associated `User` [entity](patterns.md#entity) with (encrypted) credentials stored in it.

### ConnectionTypes

Every type of [`Connection`](#connections) between systems might be registered in a table of `ConnectionTypes`. Each `ConnectionType` is meant to be a very specific way of integrating with a system, with a specific messaging protocol, message format and implementation.

### Connections

Every individual `Connection` between two [`Enterprises`](#enterprises) would be registered in the `Connection` table with the `Connection` settings stored with it. Each `Connection` has an associated [`ConnectionType`](#connectiontypes) that indicates what type of integration it is. Note that some `Connections` might not be *between* [`Enterprises`](#enterprises), but involve only *one* [`Enterprise`](#enterprises). `Connections` do not have to be complete messaging implementations. Sometimes they are simply a database connection or even the path of a network folder.

### KeyMappings

Systems often have different identifiers for e.g. `Orders`, `Customers` or other [entities](patterns.md#entity). It may be needed to map a reference number from one system to the reference number of another system. An `ESB` model could have [entities](patterns.md#entity) and logic to manage those kinds of `KeyMappings`.

### Transmissions

Optionally you can log the transferred messages that went over a [`Connection`](#connections). Do note that logging all messages can impact performance and storage requirements so perhaps use it sparsely.


Service Implementations
-----------------------

The implementation of a service would involve mostly *message transformation* and *transmission*. Data is received through some communication protocol, the message format is parsed and then converted to a [`Canonical`](#canonical-model) model. After that, the [`Canonical`](#canonical-model) model is converted to another message format and sent over another communication protocol.


Multi-Dispatch
--------------

The content of a [`Canonical`](#canonical-model) model might determine what service it is sent to. For instance, one [`Canonical`](#canonical-model) `Order` has to be sent to one `Supplier` using their own specific integration protocol, another order might simply be emailed to the `Supplier`. This service architecture enables you to retrieve a message from one system, for instance an `Order`, and then send that message to an arbitrary other system. That is part of the power of the [`Canonical`](#canonical-model) model. Multiple systems' messages are converted to [`Canonical`](#canonical-model) models, enabling all those systems to communicate with each other.


Namespaces
----------

These are [namespaces](namespaces-assemblies-and-folders.md) use a hypothetical `Ordering` system. The main [layers](layers.md) and [namespaces](namespaces-assemblies-and-folders.md): [`JJ.Data`](layers.md#data-layer), [`JJ.Business`](layers.md#business-layer) and `JJ.Services` can be seen in there.

|                                                 |     |
|-------------------------------------------------|-----|
| __`JJ.Services`__                               | Root `namespace` for web services / `WCF` services.
| __`JJ.LocalServices`__                          | Root `namespace` for `Windows` services. (Not part of this service architecture, but this is where that other type of *service* goes.)
| __`JJ.Data.Canonical`__                         | Where are [`Canonical`](#canonical-model) [entity](patterns.md#entity) models are defined.
| __`JJ.Data.Esb`__                               | [Model](#esb-model) that stores [`Enterprises`](#enterprises), `Users`, [`ConnectionTypes`](#connectiontypes), [`Connections`](#connections), etc. Basically, the configuration settings of the architecture.
| __`JJ.Data.Esb.NHibernate`__                    | Stores the [`Esb` model](#esb-model) using [`NHibernate`](api.md#nhibernate).
| __`JJ.Data.Esb.SqlClient`__                     | [`SQL`](api.md#sql) queries for working with the [`Esb` entities](#esb-model).
| __`JJ.Business.Canonical`__                     | Some shared logic that operates on [`Canonical`](#canonical-model) models.
| __`JJ.Business.Esb`__                           | [Business logic](layers.md#business-layer) for managing the [`Esb` model](#esb-model).
| __`JJ.Services.Ordering.Interface`__            | Defines [`interfaces`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface) (the [`C#`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface) kind) that `abstract` the way messages are sent between different `Ordering` systems. These interfaces use the [`Canonical`](#canonical-model) models.
| __`JJ.Services.Ordering.Dispatcher`__           | Makes sure messages (orders, price updates) are received from and sent to the [right system](#multi-dispatch) depending on message content.
| __`JJ.Services.Ordering.Email`__                | A specific implementation of an `Ordering` `interface`, where we send the order by email.
| __`JJ.Services.Ordering.SuperAwesomeProtocol`__ | Implementation of an `Ordering` `interface`, behind which we implement the hypothetical `SuperAwesomeProtocol` for sending `Orders`.
| __`JJ.Services.Ordering.Wcf`__                  | A `WCF` service that allows you to communicate with the [multi-dispatch](#multi-dispatch) ordering system.
| __`JJ.Services.Ordering.Wcf.Interface`__        | Defines the `interface` of the `WCF` service. The service `interface` can be used by both server and client.
| __`JJ.Services.Ordering.Wcf.Client`__           | Allows code to connect to the `WCF` service using the strongly typed service `interface`.
| __`JJ.Services.Ordering.JsonRest`__             | Exposes the [multi-dispatch](#multi-dispatch) `Ordering` service using the `Json` and `Rest` protocols.
| __`JJ.Services.Ordering.WebApi`__               | There is no reason `Web API` should not be involved in this service architecture, in fact, the idea of `WCF` being the default for services, might not be a very long-lived.
| __`JJ.Presentation.Shop.AppService.Wcf`__       | A special kind of service is an `AppService`, that exposes [presentation logic](layers.md#presentation-layer) instead of [business logic](layers.md#business-layer) and returns [`ViewModels`](patterns.md#viewmodel).


Service-Related Patterns
------------------------

### IsSupported

A service environment may hold the same `interface` for accessing multiple systems. But not every system is able to support the same features. You could solve it by creating a lot of different `interfaces`, but that might make it more difficult to know which interface to use. Instead, you could also add `IsSupported` properties to the `interface`. Then an implementation can communicate back if it supports a feature at all:

```cs
Product GetProducts();
bool GetProductsIsSupported { get; }
```

Then when for instance running price updates, you can simply skip the systems that do not support it. Possibly a different mechanism is used for keeping prices up-to-date, possibly there is another reason why price updates are irrelevant. It does not matter. The `IsSupported` booleans keeps complexity at bay, instead of the confusion that comes, handling a large number of `interfaces`.

### Facade

A [`Facade`](patterns.md#facade) is an `interface` behind which a lot of other `interfaces` and `classes` are used. Its goal is to simplify working with these systems.

This concept is used in this [architecture](#index.md) to give a service an even simpler `interface` than the underlying business. It may hide interactions with multiple systems, and hide infrastructural setup.

### Hidden Infrastructure

Not so much a pattern, but a difference in handling infrastructure setup between a possible [application architecture](introduction.md#application-architecture-vs-service-architecture) and this kind of service architecture.

In the [application architecture](introduction.md#application-architecture-vs-service-architecture) the [infrastructural context](layers.md#infrastructure) may be determined by the top-level project and passed down to the deeper layers as for instance [`Repository interfaces`](patterns.md#repository) or `interfaces` on [security](aspects.md#security). 

While in the *service architecture* the [infrastructural context](layers.md#infrastructure) might be determined by the bottom-level project. At least in the case of multi-dispatch this seems necessary. A bottom-level project, for instance `JJ.Services.Ordering.Email` does not expose that there will be `SMTP` server setup. You cannot see that from the constructor or the `interface`. The service would handle all of that internally.

### Tag Model

The [`Canonical`](#canonical-model) model should focus on data, that plays a logical role in your company. But another system may need data that is not relevant to you. To avoid cluttering the [`Canonical`](#canonical-model) model with unnecessary data structure, you could use `Tag` collections. You might use those `Tags` to domain models too, to add data, that none of your own business concerns itself with. But this data can still be sent along to another system, when *it* needs it.

Here are some examples for `Tag` models:

    Order { Tags[] }
    Tag { Name, Value }

You might also like culture-specific `Tags`:

    Tag { Name, Value, CultureName }

Or you might loosely link the `Tags` to [entities](patterns.md#entity):

    Tag { Name, Value, EntityTypeName, EntityID }

### Canonical KeyMapping

[`KeyMapping`](#keymappings) is an idea that maps `ReferenceNumbers` from one system to `ReferenceNumbers` of another. For example, the same `Order` could have a different `OrderNumber` depending on which party it is sent to.

If the amount of systems becomes larger, the amount of [`KeyMappings`](#keymappings) might go up exponentially.

You might get many `IDs` in your model:

    Order
    {
        InternalID,
        CustomerOrderNumber,
        SupplierOrderNumber,
        ManufacturerOrderNumber,
        IntermediaryOrderNumber
    }

And the jeopardy of getting many [`KeyMappings`](#keymappings) in the `ESB` database arises:

    KeyA <=> KeyB
    KeyA <=> KeyC
    KeyA <=> KeyD
    KeyB <=> KeyC
    KeyB <=> KeyD
    KeyC <=> KeyD

This might become difficult to manage. You could make it a bit more generic like this:

    Order
    {
        IDs[] { System, Number }
    }

So it becomes an array of `IDs` for different `Systems`.

But there's a *trick*, that requires only 2 key fields in your [`Canonical`](#canonical-model) models, but no more!

    Order
    {
        InternalID
        ExternalID
    }

What you could do is map `ExternalIDs` from one system *only* to `InternalIDs` in the `ESB`, so that the `InternalID` can in turn be mapped to an `ID` from another system:

    { SystemA, ExternalID } => InternalID
    { SystemB, ExternalID } => InternalID
    { SystemC, ExternalID } => InternalID 
    { SystemD, ExternalID } => InternalID

This way, when a new system is added, only one [`KeyMapping`](#keymappings) is needed, to map it to all the other systems.

As messages are sent back and forth between systems, the keys in the [`Canonical`](#canonical-model) model are translated from `ExternalID` to `InternalID`. Then the `ExternalID` property is overwritten by the `ID` from the next system.

It does all depend on the specific design of your system. But hopefully this demonstrated a few options how to handle [`KeyMappings`](#keymappings), `IDs` and reference numbers in a [Service Oriented Architecture](#-service-oriented-architecture).

[back](.)
