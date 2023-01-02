🏷 Namespaces, Assemblies and Folder Structure
===============================================

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
    - [Big 1st, Small 2nd](#big-1st-small-2nd)
    - [Assembly 1st, Folders 2nds](#assembly-1st-folders-2nds)
    - [Domain 1st, Layer 2nd](#domain-1st-layer-2nd)
    - [Functional-1st, Technical-2nd](#functional-1st-technical-2nd)
    - [Layer 1st, Domain 2nd](#layer-1st-domain-2nd)


Introduction
------------

This article describes how namespaces and folders might be structured in this software architecture.


Structure
---------

Solution files are put in the code root.

Assembly names, namespaces and folder structure are similar to eachother. An assembly's name is its root namespace. The folder structure also corresponds to the namespacing.

Each element of the namespacing will be described separately. But the namespace structure in a nutshell:

Assembly name is built up as follows:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test]

Internally in an assembly each pattern might get its own sub-folder:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test] [.DesignPattern]

If a project is quite small, a single sub-folder `Helpers` might be used, instead of a folder for each design pattern:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test] [.Helpers]

When a project gets bigger, a design pattern folder might again be split up into partial domains or main entities:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Test] [.DesignPattern] [.PartialDomain]

Now each element will be described separately.


Root Namespace / Company Name
-----------------------------

In this architecture the root namespace will be the company name, for instance:

    JJ


Layers
------

The 2nd level in the namespacing consists of the following parts:

|                     |                     |
|---------------------|---------------------|
| JJ.__Data__         | The data layer including the entity models and persistence.
| JJ.__Business__     | The business logic layer
| JJ.__Presentation__ | The presentation layer
| JJ.__Framework__    | Reusable code, independent from any functional domain. Any layer in the software architecture can have reusable framework code to support it.

And second in line:

|                  |                  |
|------------------|------------------|
| JJ.__Demos__     | Code for demonstration purposes or for trying things out.
| JJ.__Utilities__ | Processes that are not run very often. Utilities contains small programs. E.g. load translations, things to run for deployment.


Functional Domains
------------------

The 3rd level in the namespacing is the functional domain. Examples:

- JJ.Data.__Calendar__  
- JJ.Business.__Calendar__  
- JJ.Presentation.__Calendar__  

The 'functional domain' of the framework layer is usually a technical aspect. Examples:

- JJ.Framework.__Validation__  
- JJ.Framework.__Security__  
- JJ.Framework.__Logging__  


Technologies
------------

The 4th level in the namespacing denotes the used technology. It is kind of analogus to a file extension. You can might find two assemblies: platform-independent and one platform-specific.

- JJ.Data.Calendar  
- JJ.Data.Calendar.__NHibernate__  
- JJ.Presentation.Calendar  
- JJ.Presentation.Calendar.__Mvc__  
- JJ.Framework.Logging  
- JJ.Framework.Logging.__DebugOutput__  

This means that the platform-indepent part of the code is separate from the platform-specific code. This also means, that a portion of the code can be shared between platforms. It also means, that we can specifically choose  which technologies we want to be dependent on.


Tests
-----

Every assembly can get a `Tests` assembly, which contains automated tests. For instance:

- JJ.Business.Calendar.__Tests__  
- JJ.Presentation.Calendar.Mvc.__Tests__  


Order of the Elements
---------------------

### Scrambling Technical and Functional

In this namespacing, the technical and functional concerns seem scrambled:

- JJ.Data.Ordering.NHibernate.Mappings.Products

These are the functional (or commercial) concerns:

- __JJ__.Data.__Ordering__.NHibernate.Mappings.__Products__

These are the technical concerns:

- JJ.__Data__.Ordering.__NHibernate.Mappings__.Products

This 'scrambling' of technical and functional concerns, might be rooted in our trying to project something 2-dimensional (functional vs. technical) onto something sequential (written text).

### Big 1st, Small 2nd

`<< simplify >>`

The ordering in the namespace seems arbitrary. But it may helps us to 'scramble' the namespace elements wisely, so that it goes from one level of detail to the next. What happened here is an attempt to is organize things into bigger and smaller chunks. The split up per company may be the largest concern, while the second most important concern is the split up into main software layers (`Data`, `Business`, `Presentation`, etc.) A functional domain (`Calendar`, `Ordering`) is a larger concern than the specific technology used (e.g. `NHibernate`, `Mvc`). And a design pattern may be a level of detail even below that.

### Assembly 1st, Folders 2nds

It might make sense to put the assembly subdivision first, and the internal folder subdivision second.

- JJ.Data.Ordering.Mappings.Products

This would be the assembly:

- __JJ.Data.Ordering__.Mappings.Products

And this would be the folders in it:

- JJ.Data.Ordering.__Mappings.Products__

### Domain 1st, Layer 2nd

In other projects, a different order of the namespace elements might make more sense:

- JJ.__Calendar__.Data
- JJ.__Calendar__.Business
- JJ.__Calendar__.Presentation

But not all functional domains may have all the 3 layers like that.

### Functional-1st, Technical-2nd

We could keep functionality together, and technical things together:

- __JJ.Ordering.Products__.Data.NHibernate.Mappings
- JJ.Ordering.Products.__Data.NHibernate.Mappings__

Just looking at this, it does make a lot of sense. But this might get in the way of our plans to put the assembly subdivision first, and the internal folder subdivision second.

### Layer 1st, Domain 2nd

Putting the main layer (`Data`, `Business`, `Presentation`) before the functional domain (`Calendar`, `Ordering`) was a choice, that made sense, in the specific environment at the time.

Every software product did not have a data, business or presentation layer. Most products belonged in just one of those layers. There was a certain `n-to-n` relationship between products. A functional domain could be missing a layer, an app could use multiple functional domains, a single functional domain could have multiple front-ends. 

It made more sense there, to make the main layer the first subdivision, and drop in the functional domains from there.

[back](.)