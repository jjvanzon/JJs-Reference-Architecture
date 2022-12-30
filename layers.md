Layers
======

[back](.)

<h3>Contents</h3>

- [Introduction](#introduction)
- [Data Layer](#data-layer)
    - [Database (DB)](#database-db)
    - [ORM (NHibernate)](#orm-nhibernate)
    - [Mappings](#mappings)
    - [Entities](#entities)
    - [Repositories](#repositories)
    - [Repository Interfaces](#repository-interfaces)
    - [Platform Independence](#platform-independence)
- [Presentation Layer](#presentation-layer)
- [Business Layer](#business-layer)
- [Perpendicular Layers](#perpendicular-layers)
- [Alternatives](#alternatives)


Introduction
------------

Software might be split up into 3 layers:

<img src="images/data-business-presentation.png" width="141" />

The *presentation layer* is the visual part of a program. It is what the user sees. The screens of the system.

The *business layer* can model of the functionality of a software program, but you generally don't see it. It defines and enforces the rules of the system. It is like the internal, mechanical parts.

The *data layer*, models and stores the data. It models functionality, but more passively: it does not really do anything on its own. It does not really process the data. It just stores it.

The presentation layer builds upon the business layer with user interface technology.

The business layer uses the data layer to store the data.

Sometimes the presentation layer skips the business layer, and uses the data layer directly, where the business layer would not really add any functionality.

The data layer may be programmed with mostly fixed patterns in this architecture. The presentation layer is mostly fixed patterns too. The business layer can have patterns as well, but it gets a little more creative. If anything special needs to happen, it might be put in the business layer. It is where the magic happens, so to speak.


Data Layer
----------

`[ TODO: Links to Patterns or API's sections. ]`

A data layer can be built up of the following sub-layers:

<img src="images/data-layer.png" width="400" />

### Database (DB)

It starts with the database (DB). This can be a *relational database* like `Microsoft SQL Server`, that structuredly stores the data into tables and relationships. But the it could also be another type of data store: an [`XML`](apis.md#xml) file, flat file or even just in-memory data. It's the part where the data is stored.

### ORM (NHibernate)

The database might not be directly accessed by the rest of the code. It may go through an *object-relational mapper* (or [`ORM`](apis.md#orm)), like [`NHibernate`](apis.md#nhibernate). The [`ORM`](apis.md#orm) would translate database records to objects called [*entities*](patterns.md#entity).
    
It could also be a different data access technology, instead of [`NHibernate`](apis.md#nhibernate): a different [`ORM`](apis.md#orm), like [`Entity Framework`](apis.md#entity-framework) or [`XML` files](apis.md#xml), or perhaps [`SqlClient`](apis.md#sql) to execute raw [`SQL`](apis.md#sql) (structured query language) onto a relational database.

### Mappings

The [entity objects](patterns.md#entity) have properties, that map to columns in the database, and properties that point to related entities. [`NHibernate`](apis.md#nhibernate) needs [mappings](patterns.md#mapping), that define which *class* maps to which *table* and which *columns* map to which *properties*. `FluentNHibernate` is an `API` that can help to build up these [mappings](patterns.md#mapping).

### Entities

With all this in place, out come objects called [*entities*](patterns.md#entity), loaded from the database.

### Repositories

The [entities](patterns.md#entity) may not be directly read out of [`NHibernate`](apis.md#nhibernate) by the rest of the code, but accessed using [*repositories*](patterns.md#repository). You might see the [repositories](patterns.md#repository) as a set of queries. Each [entity](patterns.md#entity) type might have its own [repository](patterns.md#repository). Next to providing a central place to manage an optimal set of queries, the [repositories](patterns.md#repository) keep the rest of the code independent of [`NHibernate`](apis.md#nhibernate), in case you would like to switch to a different data storage technology.

### Repository Interfaces

The [repository implementations](patterns.md#repository) might not used directly, but accessed through [*interfaces*](patterns.md#repository-interfaces), so that we can indeed use a different data access technology, just by instantiating a different [repository *implementation*](patterns.md#repository). The [repository interfaces](patterns.md#repository-interfaces) are also handy for [testing](aspects.md#automated-testing), to create a [*fake*](patterns.md#mock) in-memory data store, instead of connecting to a real database. The API [`JJ.Framework.Data`](https://dev.azure.com/jjvanzon/JJs-Software/_artifacts/feed/JJs-Pre-Release-Package-Feed/NuGet/JJ.Framework.Data/) can help to abstract this data access, providing a base for these [repositories](patterns.md#repository) and [interfaces](patterns.md#repository-interfaces).

### Platform Independence

The dashed line going right through the [diagram](#data-layer), separates the *platform-specific* code from the *platform independent* code. The platform-specific code concerns itself with [`NHibernate`](apis.md#nhibernate) and `SQL Server`, while the platform independent code is agnostic of what the underlying storage technology is. You may as well stick an [`XML`](apis.md#xml) file under it and not use `SQL Server` or [`NHibernate`](apis.md#nhibernate). This allows us to program against the same model, regardless of how it is stored. This platform-independence, also allows deployment of the same code in different environment that can run `.NET`, such as a *mobile phone*, *windows* or *web*.


Presentation Layer
------------------

`[ TODO: More clarifying sentences. ]`  
`[ TODO: Sub-sections. ]`  
`[ TODO: Arrows in the diagram. ]`  

The presentation layer is built up of the following sub-layers:

<img src="images/presentation-layer.png" width="449" />

`< TODO: ToEntity is in a really odd spot if you read the diagram from top to bottom. >`

The presentation layer calls the business layer, which contains all the rules that surround the system.

The data that is exactly shown on screen is called the *view model*.

*Presenter* classes combine several responsibilities around the presentation logic.

The presenter layer forms a model of your program navigation. Each screen has its own presenter and each method in that presenter is a specific user action.

*Presenter* classes talk to the business layer.

A presenter delegates to the ToViewModel layer, to translating the data and the results of the business logic to a subset of data that is shown on screen: the view model.

A presenter delegates to the ToEntity layer, to translate user input back to [entity](patterns.md#entity) data.

The presenter then calls upon the business layer again to save, validate, side-effects and execute other logic around the user action.

Because the presenters combine several responsibilities together they are the facades / combinators of the presentation layer.

MVC is the web technology of choice we use for programming user interfaces. In our architecture the MVC layer builds on top of the presenter layer.

In MVC we use controllers, which are similar to presenters in that they group together related user actions and each user action has a specific method.

MVC will make sure that the request from the web browser will automatically make the right controller method going off. Each method in a controller represents a URL.

After the controller method is done, the view engine kicks in. It will render a piece of HTML. MVC will make sure that the view rendering automatically goes off after the controller method completes.

The view engine we use is Razor. It offers a concise syntax for programming views, in which you combine C# and HTML. Razor has tight integration with MVC. The view engine uses a view model as input, along with the view (template) and the output is a specific piece of HTML.

The dashed line going right through the diagram separates the platform-specific code from the platform independent code. The platform-specific code concerns itself with MVC, HTML and Razor, while the platform independent code is agnostic of what presentation technology we use. That means that we can use multiple presentation techniques for the same application navigation model, such as offering an application both web based as well as based on WinForms. This also provides us the flexibility we need to be able to deploy apps on mobile platforms using the same techniques as we would use for Windows or web.

Because the architecture is multi-platform, the labels in the diagram above are actually too specific:

- The *controller* is very specific to MVC and an equivalent might not even be present on other presentation platforms, even though it is advisable to have a central place to manage calls to the presenter and showing the right views depending on its result.
- The views in WinForms would be the *Forms and UserControls*. It is advised that even if a view can have 'code-behind' to only put dumb code in it and delegate the real work elsewhere.
- 'Html' can be replaced by the type of presentation output. In WinForms it is the controls you put on a form and their data. But it can also be a generated PDF, or anything that comes out of any presentation technology.


Business Layer
--------------

<img src="images/business-layer.png" width="357" />

What is business logic? Basically anything that is not presentation or data access, is business logic.

`< TODO: Layers: Say something about infrastructure, next to persistence, business and presentation. Because then you can say: everything that is not persistence, presentation or infrastructure, is business logic. >`

The business layer resides in between the data access and the presentation layer. The presentation layer calls the business layer for the most part throught the Facades. The Facades are combinators that combine multiple aspects of the business logic, by calling validators, side effects, cascadings and other things. They are 'CRUD-oriented facades'.

The business layer executes validations that verify, that the data corresponds to all the rules. Also, the business layer executes side effects when altering data, for instance storing the date time modified or setting default values when you create an [entity](patterns.md#entity), or for instance automatically generating a name. The business layer is also responsible for calculations and many other things as represented in the diagram above.

The business layer uses entities, but sometimes will call [repositories](patterns.md#repository) out of the data access layer, even though your first choice should be to just use the entities. The presentation layer uses the business layer for anything special that needs to be done. Often when something special is programmed in the presentation layer, it actually belongs in the business layer instead.

The business layer is platform independent and the code can be deployed anywhere. This does sometimes require specific API choices or using our own framework API's. These choices are inherently part of this architecture. But because most things are built on entities and repository interfaces, the business logic is very independent of everything else, which means that the magic of our software can be deployed anywhere.

`< TODO: Add 'Cloning' to big block in the diagram? It might stay too vague if you mention it there. >`

`< TODO: Consider this: `

`- Mention in the layering diagrams that Inverse Property Management is also called LinkTo and Unlink in our architecture and that Cascading is also called UnlinkRelatedEntitiesExtensions and DeleteRelatedEntitiesExtensions. Whether you should pollute the diagrams with that is an open question, because it is a really specific choice that may be broken in the future. On the other hand, the diagrams serve to clarify and are specific to this architecture already. >`


Perpendicular Layers
--------------------

The subdivision into data, business and presentation is just about the most important subdivision in software design. But there are other additional layers, called perpendicular layers:

<img src="images/perpendicular-layers.png" width="325" />

The Framework layer consists of API's that could support any aspect of software development, so could be used in any part of the layering. That is why it stretches right from Data to Presentation in the diagram.

Infrastructure is things like security, network connections and storage. The infrastructure can be seen as part at the outer end of the data layer and part at the outer end of the presentation layer, because the outer end of the data layer is actually performing the reading and writing from specific data source. However it is the presentation layer in which the final decision is made what the infrastructural context will be. The rest of the code operates independent of the infrastructure and only the top-level project determines what the context will be.

`< TODO: Encorporate this phrase: It is hard to explain what the position of infrastructure is in the architecture. One thing you can say is that the infrastructure should be loose coupled. >`

Services expose business logic through a network interface, often through the SOAP protocol. A service might also expose a presentation model to the outside world. Because it is about a specific network / communication protocol, the service layer is considered part of the infrastructure too.

Another funny thing about infrastructure, for example user right management, is that a program navigation model in the presenter layer can actually adapt itself to what rights the user has. In that respect the platform-independent presentation layer is dependent on the infrastructure, which is a paradox. The reason the presenter layer is platform-independent is that it communicates with the infrastructure using an interface, that may have a different implementation depending on the infrastructural context in which it runs.


Alternatives
------------

-  Data and Business in one layer
    - *Benefit:* Might be easier to understand
    - *Downside:* More likely for data access and business to get entangled
- No [repositories](patterns.md#repository)

[back](.)
