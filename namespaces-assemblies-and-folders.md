---
title: "🍱 Namespaces, Assemblies & Folders"
image: "/images/namespaces-assemblies-and-folders-preview.png"
---

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

This article describes how namespaces and folders might be structured.


Company Name / Root Namespace
-----------------------------

In this [architecture](index.md) the root namespace is the company name, for instance:

    JJ


Layers
------

The 2<sup>nd</sup> level in the namespacing splits up into the following parts:

|             |             |
|-------------|-------------|
| [JJ.__Data__](layers.md#data-layer) | The [`Data`](layers.md#data-layer) layer including the [entity](patterns/data-access.md#entities) models and storage of data.
| [JJ.__Business__](layers.md#business-layer) | The [`Business`](layers.md#business-layer) logic: guarding the rules of the system.
| [JJ.__Presentation__](layers.md#presentation-layer) | The [`Presentation`](layers.md#presentation-layer) layer: the visual part of a program.
| [JJ.__Framework__](api.md#jjframework) | Reusable code, independent from any functional domain. Any layer in the [software architecture](index.md) can have reusable code to support it.

And second in line:

|              |              |
|--------------|--------------|
| [JJ.__Utilities__](aspects.md#utilities) | Small programs, e.g. health checks, loading stuff in a database.
| JJ.__Demos__ | Code for demo purposes or trying things out.


Functional Domains
------------------

The 3<sup>rd</sup> level in the namespacing would be the *functional domain:*

- JJ.Data.__Calendar__  
- JJ.Data.__Synthesizer__  
- JJ.Business.__Calendar__  
- JJ.Business.__Synthesizer__  
- JJ.Presentation.__Calendar__  
- JJ.Presentation.__Synthesizer__  

The 'functional domain' of the [framework layer](api.md#jjframework) might be a technical [aspect](aspects.md#-aspects):

- [JJ.Framework.__Validation__](patterns/business-logic.md#validators)
- [JJ.Framework.__Security__](aspects.md#security)
- [JJ.Framework.__Logging__](aspects.md#logging)


Technologies
------------

The 4<sup>th</sup> level in the namespacing denotes which [technology](api.md#-apis) is used. It is sort of analogous to a file extension. You might find two assemblies: one *platform-independent* and one *platform-specific:*

- JJ.Data.Calendar  
- JJ.Data.Calendar.[__NHibernate__](api.md#nhibernate)
- JJ.Presentation.Calendar  
- JJ.Presentation.Calendar.[__Mvc__](api.md#mvc)
- JJ.Framework.Logging  
- JJ.Framework.Logging.__DebugOutput__  

This means that the *platform-independent* part of the code is separate from the *platform-specific* part. This also means, that quite a portion of the code can be shared between platforms. That way we can choose which [technologies](api.md#-apis) we want to depend on.


Patterns
--------

The next level in the namespacing can be a [design pattern](patterns.md). It would become a *sub-folder* inside an assembly:

- JJ.Data.Calendar.NHibernate.__Mappings__
- JJ.Business.Calendar.__Validators__
- JJ.Presentation.Ordering.Mvc.__Controllers__


Partial Domains
---------------

For bigger projects, a [design pattern](patterns.md) folder may split up into sub-folders for main [entities](patterns/data-access.md#entities) or *partial domains*:

- JJ.Business.Synthesizer.Validation.__Documents__
- JJ.Business.Synthesizer.Validation.__Operators__
- JJ.Business.Synthesizer.Validation.__Scales__


Tests
-----

Assemblies with [automated tests](aspects.md#automated-testing) can add the suffix `Tests`:

- JJ.Business.Calendar.__Tests__  
- JJ.Presentation.Calendar.Mvc.__Tests__  


Order of the Elements
---------------------

### 1st Layer then Domain

Putting the [main layers](layers.md#-layers) ([data](layers.md#data-layer), [business](layers.md#business-layer), [presentation](layers.md#presentation-layer)) before the *functional domain* was a choice, that made sense at the time:

- JJ.Data.__MainEntities__
- JJ.Business.__Magic__
- JJ.Business.__GreatCalculator__
- JJ.Presentation.__LandingPage__
- JJ.Presentation.__ProprietarySite__
- JJ.Presentation.__InternalManager__
- JJ.Presentation.__CoolHub__

Not every software product had a [data](layers.md#data-layer), [business](layers.md#business-layer) or [presentation](layers.md#presentation-layer) layer. Most products just had *one* of those layers.

There was a certain *n-to-n* relationship between products. A functional domain could be *missing* a layer, an app could use *multiple* functional domains, a single functional domain could have multiple front-ends. 

It made more sense there, to make the [main layer](layers.md#-layers) the first subdivision, and drop in the functional domains from there.

### 1st Domain then Layer

In other projects it might make more sense, put the functional domain 1<sup>st</sup> and the [main layer](layers.md#-layers) 2<sup>nd</sup>.

Functional domain:

- JJ.__Calendar__.Data
- JJ.__Calendar__.Business
- JJ.__Calendar__.Presentation

Main layer:

- JJ.Calendar.__Data__
- JJ.Calendar.__Business__
- JJ.Calendar.__Presentation__

### Scrambling Functional and Technical

In this namespacing, the functional and technical concerns seem scrambled.

These are the functional concerns:

- JJ.Data.__Ordering__.NHibernate.Mappings.__Products__

And these are technical concerns:

- JJ.__Data__.Ordering.__NHibernate.Mappings__.Products

### 1st Big then Small

What happened here, is an attempt to organize things into *bigger and smaller* chunks.

The split up per *company* may be the largest concern, while a 2<sup>nd</sup> concern is the split up into [main layer](layers.md#-layers) ([data](layers.md#data-layer), [business](layers.md#business-layer), [presentation](layers.md#presentation-layer)).

A functional domain (`Calendar`, `Ordering`) was considered a larger concern than the specific [technology](api.md#-apis) used (e.g. [`NHibernate`](api.md#nhibernate), [`MVC`](api.md#mvc)).

And a [design pattern](patterns.md) may be a level of detail below that.

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

But this might get in the way of our plans to put the [assembly 1<sup>st</sup> and the folder 2<sup>nd</sup>](#1st-assembly-then-folders).


Summary
-------

### Guidelines

- Assembly names, namespaces and folder structure similar
- Assembly name = root namespace
- Solution files in repository root

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
                |- Mappings
                |- Helpers

Project file:

    JJ.TheProject.Data.csproj

Root namespace:

    JJ.TheProject.Data

Sub-namespaces:

    JJ.TheProject.Data.Entities
    JJ.TheProject.Data.Mappings
    JJ.TheProject.Data.Helpers

### Structure

Assembly name:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]

Sub-folders for [design patterns](patterns.md):

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]
        [.DesignPattern]

Smaller projects, single sub-folder `Helpers` instead:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]
        [.Helpers]

Bigger projects' [design patterns](patterns.md) split up into main [entities](patterns/data-access.md#entities) or partial domains:

    Company.SoftwareLayer.FunctionalDomain [.Technology] [.Tests]
        [.DesignPattern] [.PartialDomain]

[back](.)