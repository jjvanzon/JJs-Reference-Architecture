<style>thead{display:none;}</style>

🍱 Namespaces, Assemblies & Folders
====================================
 
[back](.)

<h3>Contents</h3>

- [Introduction](#introduction)
- [Company Name / Root Namespace](#company-name--root-namespace)
- [Layers](#layers)
- [Functional Domains](#functional-domains)
- [Technologies](#technologies)
- [Patterns](#patterns)
- [Partial Domains](#partial-domains)
- [Tests](#tests)
- [Order of the Elements](#order-of-the-elements)
    - [1st Layer then Domain](#1st-layer-then-domain)
    - [1st Domain then Layer](#1st-domain-then-layer)
    - [Scrambling Functional and Technical](#scrambling-functional-and-technical)
    - [1st Big then Small](#1st-big-then-small)
    - [1st Assembly then Folders](#1st-assembly-then-folders)
    - [1st Functional then Technical](#1st-functional-then-technical)
- [Summary](#summary)
    - [Guidelines](#guidelines)
    - [Example](#example)
    - [Structure](#structure)


Introduction
------------

This article describes how namespaces and folders might be structured in this [software architecture](index.md).


Company Name / Root Namespace
-----------------------------

In this [architecture](index.md) the root namespace is the company name, for instance:

    JJ


Layers
------

The 2<sup>nd</sup> level in the namespacing may consist of the following parts:

|             |             |
|-------------|-------------|
| JJ.__Data__ | The [`Data`](layers.md#data-layer) layer including the entity models and storage of data.
| JJ.__Business__ | The [`Business`](layers.md#business-layer) logic layer: guarding the rules of the system.
| JJ.__Presentation__ | The [`Presentation`](layers.md#presentation-layer) layer: the visual part of a program.
| [JJ.__Framework__](api.md#jjframework) | Reusable code, independent from any functional domain. Any layer in the [software architecture](index.md) can have reusable code to support it.

And second in line:

|              |              |
|--------------|--------------|
| [JJ.__Utilities__](aspects.md#utilities) | Small programs, e.g. load translations, things to run for deployment.
| JJ.__Demos__ | Code for demonstration purposes or trying things out.


Functional Domains
------------------

The 3<sup>rd</sup> level in the namespacing is the *functional domain:*

- JJ.Data.__Calendar__  
- JJ.Business.__Calendar__  
- JJ.Presentation.__Calendar__  

The 'functional domain' of the [framework layer](api.md#jjframework) is usually a technical [aspect](aspects.md):

- [JJ.Framework.__Validation__](patterns.md#validators)
- [JJ.Framework.__Security__](aspects.md#security)
- [JJ.Framework.__Logging__](aspects.md#logging)


Technologies
------------

The 4<sup>th</sup> level in the namespacing denotes what [technology](api.md) is used. It is sort of analogous to a file extension. You might find two assemblies: one platform-independent and one platform-specific.

- JJ.Data.Calendar  
- JJ.Data.Calendar.[__NHibernate__](api.md#nhibernate)
- JJ.Presentation.Calendar  
- JJ.Presentation.Calendar.[__Mvc__](https://dotnet.microsoft.com/en-us/apps/aspnet/mvc)
- JJ.Framework.Logging  
- JJ.Framework.Logging.__DebugOutput__  

This means that the *platform-independent* part of the code is separate from the *platform-specific* part. This also means, that quite a portion of the code can be shared between platforms. It also means, that we can specifically choose which [technologies](api.md) we want to depend on.


Patterns
--------

The next level in the namespacing might a [design pattern](patterns.md). It can be a sub-folder inside an assembly:

- JJ.Data.Calendar.NHibernate.__Mappings__
- JJ.Business.Calendar.__Validators__
- JJ.Presentation.Ordering.Mvc.__Controllers__


Partial Domains
---------------

For bigger projects, a [design pattern](patterns.md) folder may split up into sub-folders for partial domains or main [entities](patterns.md#entity):

- JJ.Business.Synthesizer.Validation.__Documents__
- JJ.Business.Synthesizer.Validation.__Operators__
- JJ.Business.Synthesizer.Validation.__Scales__


Tests
-----

Every assembly can get a `Tests` assembly with [automated tests](aspects.md#automated-testing):

- JJ.Business.Calendar.__Tests__  
- JJ.Presentation.Calendar.Mvc.__Tests__  


Order of the Elements
---------------------

### 1st Layer then Domain

Putting the [main layers](layers.md) ([data](layers.md#data-layer), [business](layers.md#business-layer), [presentation](layers.md#presentation-layer)) before the *functional domain* was a choice, that made sense at the time in a specific environment:

- JJ.Data.__MainEntities__
- JJ.Business.__Magic__
- JJ.Business.__GreatCalculator__
- JJ.Presentation.__LandingPage__
- JJ.Presentation.__ProprietarySite__
- JJ.Presentation.__InternalManager__
- JJ.Presentation.__CoolHub__

Not every software product had a [data](layers.md#data-layer), [business](layers.md#business-layer) or [presentation](layers.md#presentation-layer) layer. Most products just had *one* of those layers.

There was a certain *n-to-n* relationship between products. A functional domain could be *missing* a layer, an app could use *multiple* functional domains, a single functional domain could have multiple front-ends. 

It made more sense there, to make the [main layer](layers.md) the first subdivision, and drop in the functional domains from there.

### 1st Domain then Layer

In other projects, putting the functional domain 1<sup>st</sup> and the [layer](layers.md) 2<sup>nd</sup> might make more sense:

- JJ.__Calendar__.Data
- JJ.__Calendar__.Business
- JJ.__Calendar__.Presentation

But maybe not all functional domains have all [3 layers](layers.md#3-layers) like that.

### Scrambling Functional and Technical

In this namespacing, the functional and technical concerns seem scrambled.

There are functional (or commercial) concerns:

- __JJ__.Data.__Ordering__.NHibernate.Mappings.__Products__

And there are technical concerns:

- JJ.__Data__.Ordering.__NHibernate.Mappings__.Products

### 1st Big then Small

But what happened here is an attempt to organize things into *bigger and smaller* chunks.

The split up per *company* may be the largest concern, while of secondary importance is the split up into [main layers](layers.md) ([data](layers.md#data-layer), [business](layers.md#business-layer), [presentation](layers.md#presentation-layer))

A functional domain (`Calendar`, `Ordering`) is considered a larger concern than the specific [technology](api.md) used (e.g. [`NHibernate`](api.md#nhibernate), [`MVC`](https://dotnet.microsoft.com/en-us/apps/aspnet/mvc)).

And a [design pattern](patterns.md) may be a level of detail even below that.

### 1st Assembly then Folders

What's also done here is putting the *assembly* subdivision first, and the *folder* subdivision after that.

This would be the assembly:

- __JJ.Data.Ordering__.Mappings.Products

And this would be the folders in it:

- JJ.Data.Ordering.__Mappings.Products__

### 1st Functional then Technical

We could keep functionality together, and technical things together:

Functional things:

- __JJ.Ordering.Products__.Data.NHibernate.Mappings

Technical things:

- JJ.Ordering.Products.__Data.NHibernate.Mappings__

Just looking at this, it does make a lot of sense.

But this might get in the way of our plans to put the [assembly 1<sup>st</sup>, and the folder 2<sup>nd</sup>](#1st-assembly-then-folders), depending on how we organize things.


Summary
-------

### Guidelines

- Solution files in repository root
- Assembly name = root namespace
- Assembly names, namespaces and folder structure are similar

### Example

    C:\Repositories
        |
        |- JJ.TheProject
            |
            |- JJ.TheProject.sln
            |
            |- Data
                |
                |- JJ.TheProject.Data.csproj
                |
                |- Entities
                |- Repositories
                |- Helpers

Project file:

    JJ.TheProject.Data.csproj

Root namespace:

    JJ.TheProject.Data

Sub-namespace:

    JJ.TheProject.Data.Entities
    JJ.TheProject.Data.Repositories
    JJ.TheProject.Data.Helpers

### Structure

Assembly name:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]

Each [design pattern](patterns.md) a sub-folder:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]
        [.DesignPattern]

For smaller projects, a single sub-folder `Helpers` instead:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]
        [.Helpers]

For bigger projects [design pattern](patterns.md) splits up into partial domains or main [entities](patterns.md#entity):

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]
        [.DesignPattern] [.PartialDomain]

[back](.)