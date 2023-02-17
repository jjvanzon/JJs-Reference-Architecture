`[ Draft ]`

➕ Appendices
==============

[back](.)

<h3>Contents</h3>

- [Appendix A: Layering Checklist](#appendix-a-layering-checklist)
- [Appendix B: Knopteksten en berichtteksten in applicaties (resource strings) [ Dutch ]](#appendix-b-knopteksten-en-berichtteksten-in-applicaties-resource-strings--dutch-)
    - [Hoofdletters, interpunctie, spelling](#hoofdletters-interpunctie-spelling)
    - [Assemblies](#assemblies)
    - [Tips](#tips)


Appendix A: Layering Checklist
------------------------------

This checklist might be used if you want to bulk-program the [architecture](index.md) for an application by going through all the layers one by one, or if you want to build a feature and make sure you have not forgotten any technical issues.

- [`Data`](layers.md#data-layer): [Database structure](database-conventions.md#-database-conventions)
- [`Data`](layers.md#data-layer): [Data migration](database-conventions.md#upgrade-scripts)
- [`Data`](layers.md#data-layer): [Entity classes](patterns-data-access.md#entities)
- [`Data`](layers.md#data-layer): [`Repository interfaces`](patterns-data-access.md#repository-interfaces)
- [`Data`](layers.md#data-layer): Default [`Repositories`](patterns-data-access.md#repository)
- [`Data`](layers.md#data-layer): [`NHibernate`](api.md#nhibernate) [`Mappings`](patterns-data-access.md#mapping)
- [`Data`](layers.md#data-layer): [`SQL` queries](api.md#sql)
- [`Data`](layers.md#data-layer): [`NHibernate`](api.md#nhibernate) [`Repositories`](patterns-data-access.md#repository) (optional)
- [`Data`](layers.md#data-layer): Other [`Repositories`](patterns-data-access.md#repository) (optional)
- [`Data`](layers.md#data-layer): Other [`Mappings`](patterns-data-access.md#mapping) (optional)
- [`Data`](layers.md#data-layer): [`Helpers`](patterns-other.md#helper) 
- [`Business`](layers.md#business-layer): [`LinkTo`](patterns-business-logic.md#linkto) ([bidirectional relationship syncing](aspects.md#bidirectional-relationship-synchronization))
- [`Business`](layers.md#business-layer): [`Unlink`](patterns-business-logic.md#unlink) ([bidirectional relationship syncing](aspects.md#bidirectional-relationship-synchronization))
- [`Business`](layers.md#business-layer): [`Cascading`](aspects.md#cascading): [`DeleteRelatedEntities`](patterns-business-logic.md#cascading)
- [`Business`](layers.md#business-layer): [`Cascading`](aspects.md#cascading): [`UnlinkRelatedEntities`](patterns-business-logic.md#cascading)
- [`Business`](layers.md#business-layer): [`Enums`](aspects.md#enums)
- [`Business`](layers.md#business-layer): [`Resource strings`](patterns-business-logic.md#resource-strings)
- [`Business`](layers.md#business-layer): [`EnumExtensions`](aspects.md#enum-like-entities)
- [`Business`](layers.md#business-layer): [`Validators`](patterns-business-logic.md#validators)
    - `Delete` [`Validators`](patterns-business-logic.md#validators) too
    - `Warning` [`Validators`](patterns-business-logic.md#validators) (optional)
- [`Business`](layers.md#business-layer): [`SideEffects`](patterns-business-logic.md#sideeffects)
    - [`SetDefaults`](aspects.md#defaults) [`SideEffects`](patterns-business-logic.md#sideeffects) too
- [`Business`](layers.md#business-layer): [`Facades`](patterns-business-logic.md#facade)
- [`Business`](layers.md#business-layer): `Extensions`
- [`Business`](layers.md#business-layer): [`RepositoryWrappers`](patterns-data-access.md#repositorywrappers)
- [`Business`](layers.md#business-layer): [`Dtos`](patterns-data-access.md#dto) (optional)
- [`Business`](layers.md#business-layer): [`Calculations`](aspects.md#calculation)
- [`Business`](layers.md#business-layer): [`Visitors`](patterns-business-logic.md#visitor)
- [`Business`](layers.md#business-layer): [`Factories`](patterns-other.md#factory) (optional)
- [`Business`](layers.md#business-layer): [`EntityWrappers`](patterns-other.md#wrapper) (optional)
- [`Business`](layers.md#business-layer): Other [`Helpers`](patterns-other.md#helper) (optional)
- [`Presentation`](layers.md#presentation-layer): [`ViewModels`](patterns-presentation.md#viewmodels)
    - `Item` [`ViewModels`](patterns-presentation.md#viewmodels)
    - `ListItem` [`ViewModels`](patterns-presentation.md#viewmodels) (some may only need an [`IDAndName`](api.md#jj-canonical) [`DTO`](patterns-data-access.md#dto))
    - `Lookup` [`ViewModel`](patterns-presentation.md#viewmodels)
    - `List` [`ViewModels`](patterns-presentation.md#viewmodels)
    - `Detail` [`ViewModels`](patterns-presentation.md#viewmodels)
    - `DocumentViewModel` (optional)
- [`Presentation`](layers.md#presentation-layer): [`ToViewModel`](patterns-presentation.md#toviewmodel)
    - [Singular forms](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - [`WithRelatedEntities` forms](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - [`ToListItemViewModel`](patterns-presentation.md#toviewmodel)
    - [`ToLookupViewModel`](patterns-presentation.md#toviewmodel)
    - [`ToScreenViewModel`](patterns-presentation.md#toviewmodel)
    - [`ToDocumentViewModel`](patterns-presentation.md#toviewmodel) (optional)
    - [`CreateEmptyViewModel`](patterns-presentation.md#toviewmodel) (not every [`ViewModel`](patterns-presentation.md#viewmodels) needs one)
- [`Presentation`](layers.md#presentation-layer): [`ToEntity`](patterns-presentation.md#toentity)
    - [Singular forms](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - [`WithRelatedEntities` forms](patterns-other.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - From screen [`ViewModel`](patterns-presentation.md#viewmodels)
    - `DocumentViewModel` (optional)
- [`Presentation`](layers.md#presentation-layer): [`Presenters`](patterns-presentation.md#presenters)
    - `List` [`Presenters`](patterns-presentation.md#presenters)
    - `Detail` [`Presenters`](patterns-presentation.md#presenters)
    - (`Edit` [`Presenters`](patterns-presentation.md#presenters))
    - `Save` methods in `Detail` (or `Edit`) [`Presenters`](patterns-presentation.md#presenters).
    - `MainPresenter` (optional)
- [`Presentation`](layers.md#presentation-layer):
    - [`Views`](patterns-presentation.md#views) ([`MVC`](api.md#mvc) / `UserControls` / ... )
    - `List` [`Views`](patterns-presentation.md#views)
    - `Detail` [`Views`](patterns-presentation.md#views)
    - `Main` [`View`](patterns-presentation.md#views) (optional)


Appendix B: Knopteksten en berichtteksten in applicaties (resource strings) [ Dutch ]
-------------------------------------------------------------------------------------

`< TODO: Translate to English. >`  

Er is een structuur waarmee we knopteksten en meldingen in onze applicaties kunnen organiseren. Het doel is om deze teksten maximaal herbruikbaar te maken, vertaalwerk te minimaliseren en correcte teksten te gebruiken. Hiervoor plaatsen we algemene teksten op een bepaalde locatie, domein termen op een andere en specifieke projecten krijgen de overige teksten. Dit kan het aantal teksten drastisch verminderen van 10.000 tot misschien een paar 100.

Momenteel worden resources op allerlei plekken geplaatst met willekeurig hoofdlettergebruik en gebruik van leestekens. En in andere gevallen werden ze helemaal niet gebruikt en stond alles hard op één taal.

Om dit beter aan te pakken, zouden we graag willen dat ontwikkelaars zowel Nederlandse als Engelse teksten toevoegen aan de resource files. Als er twijfel is over het Engels, kun je het misschien een collega vragen.

### Hoofdletters, interpunctie, spelling

Hoofdlettergebruik etc. is conform de taalregels van de betreffende taal.

1. Resources kunnen hele zinnen zijn. Die moeten correct geschreven zijn: In het Nederlands begint dat met een hoofdletter en eindigt het met een punt, tenzij het een vraag is, dan met een vraagteken.
2. Voor Engels gebruiken we de Amerikaanse spelling.
3. Namen van properties, classes en andere titels zoals knopteksten zijn in het Nederlandse zijn als volgt: "Links in artikel", dus alleen beginnen met een hoofdletter. En dus geen punt erachter.
4. Namen van properties, classes en andere titels zoals knopteksten in Engelse titels doen we als volgt: "Table of Contents", dus alle woorden beginnen met een hoofdletter, alleen onbelangrijke woorden zoals "in", "and", etc. in kleine letters.
5. Er is dus een verschil in hoofdlettergebruik tussen volzinnen en losse titels.

### Assemblies

Resources worden met de assemblies meegecompileerd*.

Termen worden zo veel mogelijk hergebruikt. Daarom zijn er plekken bedacht waar de termen thuis horen. Je kunt in deze volgorde op zoek naar een resource die misschien al bestaat:

1. "Save", "Close", "Edit", etc. staan in `JJ.Framework.Resources`, toegankelijk via de `CommonResourceFormatter` class.
2. Validatiemeldingen uit [`JJ.Framework.Validation`](api.md#jj-framework-validation), toegankelijk via de `ValidationResourceFormatter` class.
3. CanonicalModel: een tussenmodel voor uitwisseling van gegevens tussen verschillende systemen, toegankelijk via de `CanonicalResourceFormatter` class.
4. Business layers bevatten alleen vertalingen voor de overige teksten die niet in het canonical model staan.
5. Ook teksten die niet direct domeintermen zijn, maar wel in applicaties worden gebruikt op plekken waar het gaat over een bepaald functioneel domein, mogen in de business layer worden gezet.
6. [Presentation layer](layers.md#presentation-layer) bevat over het algemeen geen teksten. Die zetten we in de business layer, om niet te veel plekken te hebben met resources.

### Tips

1. Gebruik van placeholders zoals {0} is ok, maar dan liefst wel met een class erbij, die de placeholders vervangt (een `ResourceFormatter`). Zie `JJ.Framework.Resources` voor een voorbeeld. Je kunt dan het beste de resources zelf internal te maken en alleen de class die de placeholders vervangt `public` maken. Hou het geschikt voor meerdere talen, want een creatief met placeholder opgebouwde [resource string](patterns-business-logic.md#resource-strings) werkt al gauw niet voor een andere taal.
2. Maak je niet druk om dat het kan resulteren in teksten met hoofdlettergebruik zoals: "Het __Ordernummer__ is niet ingevuld bij de __Bestelling__." Zouden we dit aanpakken, dan doen we dit misschien liever dynamisch via een algoritme om het hoofdlettergebruik kloppend te maken.
3. Het is misschien verstandig om teksten in de applicatie algemeen te houden. Dus bijv. "Artikelen", i.p.v. "Artikelen in dit boek". Dit scheelt vertaalwerk. Ook deze 'dubbele punt' notatie is verstandig: "Artikel 1: Naam is verplicht." Daarbij zijn de teksten "Artikel", "Naam" en "{0} is verplicht." waarschijnlijk al vertaald en hoeft dat niet nog eens opnieuw. Deze 'dubbele punt' notatie ("Artikel 1:"), kan zorgen dat het ook blijft werken voor talen met een andere zinsbouw.

*\* Resources staan liever niet in een database, omdat het applicaties trager kan maken en kun je de code niet draaien op omgevingen waarbij je geen toegang hebt tot de database. In sommige situaties kun je dan niet compileren zonder toegang te hebben tot een specifieke database. Bovendien geeft mee compileren van resources in specifieke projecten ons de mogelijkheid dubbelzinnige termen anders te vertalen per business domein.*

[back](.)
