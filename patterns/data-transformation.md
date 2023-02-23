---
title: "🐛 Patterns : Data Transformation"
redirect_from:
  - /patterns-data-transformation.md
---

`[ Draft ]`

🐛 Patterns : Data Transformation
==================================

[back](patterns.md)

<h3>Contents</h3>

- [Converter](#converter)
- [TryGet-Insert-Update](#tryget-insert-update)
- [TryGet-Insert-Update-Delete / Full-CRUD Conversion / Collection Conversion](#tryget-insert-update-delete--full-crud-conversion--collection-conversion)
- [State Flagging](#state-flagging)
- [DocumentModel](#documentmodel)
- [Selector-Model-Generator-Result](#selector-model-generator-result)


Converter
---------

A `class` that converts one data structure to another. Typically more is involved than just converting a single `object`. A whole `object` graph might be converted to another, or a flat list or raw data to be parsed might be converted to an `object` structure or the other way around.

By implementing it as a `Converter`, it simplifies the code. You can then say that the only responsibility of the `class` is to simply transform one data structure to another: nothing more, nothing less and leave other responsibilities to other `classes`.


TryGet-Insert-Update
--------------------

When converting one type to another one might use the `TryGet-Insert-Update` pattern. Especially when converting an [`Entity`](patterns-data-access.md#entities) with related [`Entities`](patterns-data-access.md##entities) from one structure to another this pattern will make the code easier to read.

[`TryGet`](patterns-other.md#tryget) first gets a possible existing destination [`Entity`](patterns-data-access.md#entities).

`Insert` will create the [`Entity`](patterns-data-access.md#entities) if it did not exist yet, possibly setting some defaults.

`Update` will update the rest of the properties of either the existing or newly created `object`.

When you do these actions one by one for one destination [`Entity`](patterns-data-access.md#entities) after another, you will get readable code for complex conversions between data structures.

Note that deletion of destination `objects` is not managed by the `TryGet-Insert-Update` pattern.


TryGet-Insert-Update-Delete / Full-CRUD Conversion / Collection Conversion
--------------------------------------------------------------------------

Used for managing complex conversions between data structures, that require insert, update and delete operations. There is no one way of implementing it, but generally it will involve the following steps:

- Loop through the source collection.
- [`TryGet`](patterns-other.md#tryget): look up an item in the destination collection.
- `Insert`: create a new item in the destination collection if none exists.
- `Update`: update the newly created or existing destination item.
- Do `Delete` operations after that:
- Generally you can use an `Except` operation on the collections of existing items and items to keep, to get the collection of items to `Delete`.
- Then you loop through that collection and `Delete` each item.

<h3>Considerations</h3>

Converting one collection to another may involve more than creating a destination `object` for each source `object`. What complicates things, is that there may already be a destination collection. That means that insert, update and delete operations are required. There are different ways to handle this depending on the situation. But a general pattern that avoids a lot of complexity, is to do the inserts and updates in one loop, and do the deletes in a second loop. The inserts and updates are done first by looping through the source collection and applying the [`TryGet-Insert-Update` pattern](patterns-other.md#tryget-insert-update) on each item, while the `Delete` operations are done separately after that by comparing collections of [`Entities`](patterns-data-access.md#entities) to figure out which items are obsolete.

In a little more detail:

- Loop through the source collection.
- [`TryGet`](patterns-other.md#tryget): look up an item in the destination collection.
- `Insert`: create a new item in the destination collection if none exists.
- `Update`: update the newly created or existing destination item.
- Do `Delete` operations after that:
- Generally you can use an `Except` operation on the collections of existing items and items to keep, to get the collection of items to `Delete`.
- Then you loop through that collection and `Delete` each item.

Here follows some pseudo code for how to do it:

```cs
void ConvertCollection(IList sourceCollection, IList destCollection)
{
    foreach (var sourceItem in sourceCollection)
    {
        var destItem = TryGet(...);
        if (destItem == null)
        {
            destItem = Insert();
        }

        destItem.Name = sourceItem.Name; // Update
    }
    
    var itemsToDelete = destCollection.Except(sourceCollection);
    foreach (var itemToDelete in itemsToDelete)
    {
        Delete(itemToDelete);
    }
}
```

The specific way to implement it, is different in every situation. Reasons that there are many ways to do it are:

- You cannot always count on instance integrity.
- You cannot always count on identity integrity.
- The key to a destination item might be complex, instead of just an ID.
- You do not always have a [`Repository`](patterns-data-access.md#repository).
- It does not always need to be full-[`CRUD`](layers.md#crud).
- You might need to report exactly what operation is executed on each [`Entity`](patterns-data-access.md#entities).
- You might need a separate normalized [*singular* form](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities) of the conversion, that may conflict with the way of working in the [*plural* form](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities).
- An alternative `isNew` detection might be needed.
- Some persistence technologies will behave unexpectedly when first retrieving and then writing and then retrieving again. Intermediate redundant retrievals should be avoided. Or not, depending on the situation.

Each variation has either overhead or elegance depending on the situation. If you always pick the same way of doing it, you may end up with unneccesary and unsensical overhead, or with an overly complicated expression of what you are trying to do.

The general forms above is a good starting point. Then it needs to work correctly. The next quality demand is a tie between readability and performance.


State Flagging
--------------

An alternative to [`TryGet-Insert-Update-Delete` pattern](#tryget-insert-update-delete--full-crud-conversion--collection-conversion), which kind of does a full diff of a source and destination structure, is maintaining a kind of flagging in the source structure: `Added`, `Modified`, `Deleted` and `Unmodified`.

A downside is that when two people try to save a piece of data at the same time, you may end up with a corrupted structure. It depends on the situation whether this would happen at all, since not all data may be edited by every user.

Another downside to flagging is that the source structure must be adapted to it, which is not always an option / a good option.

The [`TryGet-Insert-Update-Delete` pattern](#tryget-insert-update-delete--full-crud-conversion--collection-conversion), though, creates a last-user-wins situation, because not flagging determines whether it is an `Update` or `Insert`, but actual existence of dest `object` determines it.


DocumentModel
-------------

An analog of a [`ViewModel`](patterns-presentation.md#viewmodels), but then for document generation, rather than [`View`](patterns-presentation.md#views) rendering. It is a `class` that contains all data that should be displayed in the document. It can end with the suffix `Model` instead of `DocumentModel` for brevity, but then it must be clear from the context that we are talking about a document model.

Just as with [`ViewModels`](patterns-presentation.md#viewmodels), inheritance structures are not allowed. To prevent inheritance structures it may be wise to make the `DocumentClasses classes sealed`.


Selector-Model-Generator-Result
-------------------------------

<h3 id="">Contents</h3>

- [Introduction](#selector-model-generator-result-introduction)
- [Generating a Document](#generating-a-document)
- [Data Source Independence](#selector-model-generator-result-data-source-independence)
- [Multiple Import Formats](#selector-model-generator-result-multiple-import-formats)
- [Limiting Complexity](#selector-model-generator-result-limiting-complexity)
- [MVC](#selector-model-generator-result-mvc)


<h3 id="selector-model-generator-result-introduction">
Introduction
</h3>

For data transformations you may want to split up the transformation in two parts:

- A `Selector` which returns the data as an `object` graph, or `Model`.
- A `Generator` (or `Converter`) that converts the `object` graph (or `Model`) into a specific format.

This is especially useful if there are either multiple input formats or multiple output formats or both, or if in the future either the input format or output format could change.

This basic pattern is present in many architectures and can be applied to many different parts of architecture.

Her follow some examples.


<h3 id="generating-a-document">
Generating a Document
</h3>

An example of where it is useful, is generating a document in multiple format e.g. `XLSX`, `CSV` and `PDF`. In that case the data selection and basic tranformations are programmed once (a `Selector` that produces a `Model`) and exporting three different file formats would require programming three different generators. Reusable generators for specific file formats such as `CSV` may be programmed. Those will make programming a specialized generators very easy. So then basically exporting a document is mostly reading out a data source and producing an `object` graph.


<h3 id="selector-model-generator-result-data-source-independence">
Data Source Independence
</h3>

The [`Selector-Model-Generator-Result`](#selector-model-generator-result) pattern is also useful when the same document can have different data sources. Let's say you want to print an invoice out of the system, but print another invoice out of an ordering system in the same formatting e.g. a `PDF`. This requires 2 `Selectors`, 1 `Model` and 1 `Generator`, instead of 2 `Generators` with complex code and potentially different-looking `PDF's`.


<h3 id="selector-model-generator-result-multiple-import-formats">
Multiple Import Formats
</h3>

You might want to import similar data out of multiple different data sources or multiple file formats. By splitting the work up into a `Selector` and a `Generator` you can share must of the code between the two imports, and reduce the complexity of the code.


<h3 id="selector-model-generator-result-limiting-complexity">
Limiting Complexity
</h3>

Even if you do not expect multiple input formats or multiple output formats or a change in input or output format, the split up in a `Selector` and a `Generator` can be used to make the code less complicated to write, and subsequently also prevent errors and save time programming and maintaining the code.


<h3 id="selector-model-generator-result-mvc">
MVC
</h3>

[`MVC`](api.md#mvc) itself contains a specialized version of this very pattern. The following layering stacks are completely analogous to eachother:

- [`Selector` - `Model` - `Generator` – `Result`](#selector-model-generator-result)
- [`Controller`](patterns-presentation-mvc.md#controller) - [`ViewModel`](patterns-presentation.md#viewmodels) - view engine – [`View`](patterns-presentation.md#views)

[back](patterns.md)
