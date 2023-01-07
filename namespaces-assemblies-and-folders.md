🍱 Namespaces, Assemblies & Folders
====================================

[back](.)

<h3>Contents</h3>

- [Introduction](#introduction)
- [Structure](#structure)
- [Root Namespace / Company Name](#root-namespace--company-name)
- [Layers](#layers)
- [Functional Domains](#functional-domains)
- [Technologies](#technologies)
- [Tests](#tests)
- [Order of the Elements](#order-of-the-elements)
    - [Scrambling Technical and Functional](#scrambling-technical-and-functional)
    - [1st Big then Small](#1st-big-then-small)
    - [1st Assembly then Folders](#1st-assembly-then-folders)
    - [1st Domain then Layer](#1st-domain-then-layer)
    - [1st Functional then Technical](#1st-functional-then-technical)
    - [1st Layer then Domain](#1st-layer-then-domain)


Introduction
------------

This article describes how namespaces and folders might be structured in this [software architecture](index.md).


Structure
---------

Solution files are put in the repository root.

Assembly names, namespaces and folder structure are similar to eachother.

An assembly's name is its root namespace.  
The folder structure also corresponds to the namespacing.

Details will follow, but in a nutshell:

Assembly name is built up as follows:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test]

Internally in an assembly each design pattern might get its own sub-folder:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test]
        [.DesignPattern]

If a project is quite small, a single sub-folder `Helpers` might be used, instead of a folder for each design pattern:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test]
        [.Helpers]

When a project gets bigger, a design pattern folder might again be split up into partial domains or main entities:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test]
        [.DesignPattern] [.PartialDomain]

Now each element will be described separately.


Root Namespace / Company Name
-----------------------------

In this [architecture](index.md) the *root* namespace is the company name, for instance:

    JJ


Layers
------

The 2nd level in the namespacing may consist of the following parts:

|                     |                     |
|---------------------|---------------------|
| JJ.__Data__         | The data layer including the entity models and persistence.
| JJ.__Business__     | The business logic layer
| JJ.__Presentation__ | The [presentation layer](layers.md#presentation-layer)
| JJ.__Framework__    | Reusable code, independent from any functional domain. Any layer in the [software architecture](index.md) can have reusable code to support it.

And second in line:

|                  |                  |
|------------------|------------------|
| JJ.__Demos__     | Code for demonstration purposes or trying things out.
| JJ.__Utilities__ | Small programs, e.g. load translations, things to run for deployment.


Functional Domains
------------------

The 3rd level in the namespacing is the *functional domain*. Examples:

- JJ.Data.__Calendar__  
- JJ.Business.__Calendar__  
- JJ.Presentation.__Calendar__  

The 'functional domain' of the *framework* layer is usually a technical aspect. Examples:

- JJ.Framework.__Validation__  
- JJ.Framework.__Security__  
- JJ.Framework.__Logging__  


Technologies
------------

The 4th level in the namespacing denotes the used *technology*. It is sort of analogous to a file extension. You might find two assemblies: one platform-independent and one platform-specific.

- JJ.Data.Calendar  
- JJ.Data.Calendar.__NHibernate__  
- JJ.Presentation.Calendar  
- JJ.Presentation.Calendar.__Mvc__  
- JJ.Framework.Logging  
- JJ.Framework.Logging.__DebugOutput__  

This means that the platform-indepent part of the code is separate from the platform-specific code. This also means, that quite a portion of the code can be shared between platforms. It also means, that we can specifically choose  which technologies we want to be dependent on.


Tests
-----

Every assembly can get a `Tests` assembly containing automated tests. For instance:

- JJ.Business.Calendar.__Tests__  
- JJ.Presentation.Calendar.Mvc.__Tests__  


Order of the Elements
---------------------

### Scrambling Technical and Functional

In this namespacing, the technical and functional concerns seem scrambled.

There are functional (or commercial) concerns:

- __JJ__.Data.__Ordering__.NHibernate.Mappings.__Products__

And these are the technical concerns:

- JJ.__Data__.Ordering.__NHibernate.Mappings__.Products

This 'scrambling' of technical and functional concerns, might be rooted in our trying to project something 2-dimensional (functional vs. technical) onto something sequential (written text).

### 1st Big then Small

The ordering in the namespace may seem arbitrary. But what happened here is an attempt to organize things into *bigger and smaller* chunks.

The split up per *company* may be the largest concern, while of secondary importance is the split up into *main layers* (`Data`, `Business`, [Presentation](layers.md#presentation-layer)) A *functional domain* (`Calendar`, `Ordering`) is considered a larger concern than the specific *technology* used (e.g. `NHibernate`, `Mvc`). And a *design pattern* may be a level of detail even below that.

### 1st Assembly then Folders

What's also done here is putting the *assembly* subdivision first, and then the *internal folder* subdivision.

This would be the assembly:

- __JJ.Data.Ordering__.Mappings.Products

And this would be the folders in it:

- JJ.Data.Ordering.__Mappings.Products__

### 1st Domain then Layer

In other projects, a different order of the namespace elements might make more sense. First the *domain* then the *layer:*

- JJ.__Calendar__.Data
- JJ.__Calendar__.Business
- JJ.__Calendar__.Presentation

But not all functional domains might have all 3 layers like that.

### 1st Functional then Technical

We could keep functionality together, and technical things together:

- __JJ.Ordering.Products__.Data.NHibernate.Mappings
- JJ.Ordering.Products.__Data.NHibernate.Mappings__

Just looking at this, it does make a lot of sense.

But this might get in the way of our plans to put the assembly subdivision first, and the internal folder subdivision second, depending on how we organize things.

### 1st Layer then Domain

Putting the *main layer* (`Data`, `Business`, [`Presentation`](layers.md#presentation-layer) before the *functional domain* was a choice, that made sense at the time in a specific environment:

- JJ.Data.__MainEntities__
- JJ.Business.__Magic__
- JJ.Business.__GreatCalculator__
- JJ.Presentation.__LandingPage__
- JJ.Presentation.__ProprietarySite__
- JJ.Presentation.__InternalManager__
- JJ.Presentation.__CoolHub__

Not every software had a data, business or [presentation layer](layers.md#presentation-layer). Most products just had *one* of those layers.

There was a certain *n-to-n* relationship between products. A functional domain could be *missing* a layer, an app could use *multiple* functional domains, a single functional domain could have multiple front-ends. 

It made more sense there, to make the *main layer* the first subdivision, and drop in the *functional domains* there.

[back](.)