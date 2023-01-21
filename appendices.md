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

- [`Data`](layers.md#data-layer): [Database structure](database-conventions.md)
- [`Data`](layers.md#data-layer): [Data migration](database-conventions.md#upgrade-scripts)
- [`Data`](layers.md#data-layer): [Entity classes](patterns.md#entity)
- [`Data`](layers.md#data-layer): [`Repository interfaces`](patterns.md#repository-interfaces)
- [`Data`](layers.md#data-layer): Default [`Repositories`](patterns.md#repository)
- [`Data`](layers.md#data-layer): [`NHibernate`](api.md#nhibernate) [`Mappings`](patterns.md#mapping)
- [`Data`](layers.md#data-layer): [`SQL` queries](api.md#sql)
- [`Data`](layers.md#data-layer): [`NHibernate`](api.md#nhibernate) [`Repositories`](patterns.md#repository) (optional)
- [`Data`](layers.md#data-layer): Other [`Repositories`](patterns.md#repository) (optional)
- [`Data`](layers.md#data-layer): Other [`Mappings`](patterns.md#mapping) (optional)
- [`Data`](layers.md#data-layer): [`Helpers`](patterns.md#helper) 
- [`Business`](layers.md#business-layer): [`LinkTo`](patterns.md#linkto) ([Bidirectional Relationship Synchronization](aspects.md#bidirectional-relationships))
- [`Business`](layers.md#business-layer): [`Unlink`](patterns.md#linkto) ([Bidirectional Relationship Synchronization](aspects.md#bidirectional-relationships))
- [`Business`](layers.md#business-layer): [Cascading](aspects.md#cascading): [`DeleteRelatedEntitiesExtensions`](aspects.md#cascading)
- [`Business`](layers.md#business-layer): [Cascading](aspects.md#cascading): [`UnlinkRelatedEntitiesExtensions`](aspects.md#cascading)
- [`Business`](layers.md#business-layer): [`Enums`](aspects.md#enums)
- [`Business`](layers.md#business-layer): [`Resource `strings``](patterns.md#resource-strings)
- [`Business`](layers.md#business-layer): [`EnumExtensions`](aspects.md#enum-like-entities)
- [`Business`](layers.md#business-layer): [`Validators`](patterns.md#validators)
    - `Delete` [`Validators`](patterns.md#validators) too
    - `Warning` [`Validators`](patterns.md#validators) (optional)
- [`Business`](layers.md#business-layer): [`SideEffects`](patterns.md#sideeffects)
    - [`SetDefaults`](aspects.md#defaults) [`SideEffects`](patterns.md#sideeffects) too
- [`Business`](layers.md#business-layer): [`Facades`](patterns.md#facade)
- [`Business`](layers.md#business-layer): `Extensions`
- [`Business`](layers.md#business-layer): [`RepositoryWrappers`](patterns.md#repositorywrappers)
- [`Business`](layers.md#business-layer): [`Dtos`](patterns.md#dto) (optional)
- [`Business`](layers.md#business-layer): [`Calculations`](aspects.md#calculation)
- [`Business`](layers.md#business-layer): [`Visitors`](patterns.md#visitor)
- [`Business`](layers.md#business-layer): [`Factories`](patterns.md#factory) (optional)
- [`Business`](layers.md#business-layer): [`EntityWrappers`](patterns.md#wrapper) (optional)
- [`Business`](layers.md#business-layer): Other [`Helpers`](patterns.md#helper) (optional)
- [`Presentation`](layers.md#presentation-layer): [`ViewModels`](patterns.md#viewmodel)
    - `Item` [`ViewModels`](patterns.md#viewmodel)
    - `ListItem` [`ViewModels`](patterns.md#viewmodel) (some may only need `IDNameDto`, no `ListItem` [`ViewModel`](patterns.md#viewmodel))
    - `Lookup` [`ViewModel`](patterns.md#viewmodel)
    - `List` [`ViewModels`](patterns.md#viewmodel)
    - `Detail` [`ViewModels`](patterns.md#viewmodel)
    - `DocumentViewModel` (optional)
- [`Presentation`](layers.md#presentation-layer): [`ToViewModel`](patterns.md#toviewmodel)
    - [Singular forms](patterns.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - [`WithRelatedEntities` forms](patterns.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - [`ToListItemViewModel`](patterns.md#toviewmodel)
    - [`ToLookupViewModel`](patterns.md#toviewmodel)
    - [`ToScreenViewModel`](patterns.md#toviewmodel)
    - [`ToDocumentViewModel`](patterns.md#toviewmodel) (optional)
    - [`CreateEmptyViewModel`](patterns.md#toviewmodel) (not every [`ViewModel`](patterns.md#viewmodel) needs one)
- [`Presentation`](layers.md#presentation-layer): [`ToEntity`](patterns.md#toentity)
    - [Singular forms](patterns.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - [`WithRelatedEntities` forms](patterns.md#singular-plural-non-recursive-recursive-and-withrelatedentities)
    - From screen [`ViewModel`](patterns.md#viewmodel)
    - `DocumentViewModel` (optional)
- [`Presentation`](layers.md#presentation-layer): [`Presenters`](patterns.md#presenter)
    - `List` [`Presenters`](patterns.md#presenter)
    - `Detail` [`Presenters`](patterns.md#presenter)
    - (`Edit` [`Presenters`](patterns.md#presenter))
    - `Save` methods in `Detail` (or `Edit`) [`Presenters`](patterns.md#presenter).
    - `MainPresenter` (optional)
- [`Presentation`](layers.md#presentation-layer):
    - [`Views`](patterns.md#views) ([`MVC`](api.md#mvc) / `UserControls` / ... )
    - `List` [`Views`](patterns.md#views)
    - `Detail` [`Views`](patterns.md#views)
    - `Main` [`View`](patterns.md#views) (optional)


Appendix B: Knopteksten en berichtteksten in applicaties (resource strings) [ Dutch ]
-------------------------------------------------------------------------------------

Er is een bepaalde structuur waar binnen we werken voor knopteksten en meldingen in onze applicaties. De hele bedoeling is maximale herbruikbaarheid, minimaal vertaal werk en correcte teksten. Dat doen we door hele algemene teksten op plaats X te zetten, zo veel mogelijk domein termen op plaats Y, en alleen wat er dan over is, komt in specifieke projecten te staan. Dit kan het verschil betekenen tussen 100'en of 10000 teksten.

Resources worden op dit moment overal neergezet waar ze niet thuis horen, met verkeerd hoofdlettergebruik en verkeerde interpunctie. En op andere plekken worden resources gewoonweg niet gebruikt en staat alles hard op 1 taal.

Hier moet secuurder mee om worden gegaan. Van ontwikkelaars wordt verwacht zowel de Nederlandse taal als de Engelse taal in de resource files te zetten. Bij twijfel over Engels, vraag het een collega.

### Hoofdletters, interpunctie, spelling

Hoofdlettergebruik etc. is conform de taalregels van de betreffende taal.

1. Resources kunnen hele zinnen zijn. Die moeten correct geschreven zijn: In het Nederlands begint dat met een hoofdletter en eindigt het met een punt, tenzij het een vraag is, dan met een vraagteken. Blijkbaar is het nodig om dit aan te geven, want het gebeurt vaak niet goed.
2. Voor Engels gebruiken we de Amerikaanse spelling.
3. Namen van properties, classes en andere titels zoals knopteksten zijn in het Nederlandse zijn als volgt: "Links in artikel", dus alleen beginnen met een hoofdletter. En dus geen punt erachter.
4. Namen van properties, classes en andere titels zoals knopteksten in Engelse titels doen we als volgt: "Table of Contents", dus alle woorden beginnen met een hoofdletter, alleen onbelangrijke woorden zoals 'in', 'and', etc. in kleine letters.
5. Er is dus een verschil in hoofdlettergebruik tussen volzinnen en losse titels.

### Assemblies

Resources worden met de assemblies meegecompileerd*.

Termen worden zo veel mogelijk hergebruikt. Daarom zijn er plekken bedacht waar de termen thuis horen. Je moet in deze volgorde op zoek naar een resource die misschien al bestaat:

(Update: De hoeveelheid verschillende plekken waar resources staan is een zwakte van dit ordeningssysteem, omdat het verwarrend kan zijn. In toekomstige oplossingen is het wellicht een idee om resource teksten mmer op één centrale plek te zetten. Dubbelzinnigheid van termen in meerdere domeinmodellen is daarbij wellicht meer een uitzondering dan een regel, waar omheen gewerkt kan worden.)

1. 'Save', 'Close', 'Edit', etc. staan in `JJ.Framework.Resources`, toegankelijk via de `CommonResourceFormatter` class.
2. Validatiemeldingen uit [`JJ.Framework.Validation`](api.md#jj-framework-validation), toegankelijk via de `ValidationResourceFormatter` class.
3. CanonicalModel: een tussenmodel voor uitwisseling van gegevens tussen verschillende systemen, toegankelijk via de `CanonicalResourceFormatter` class.
4. Business layers bevatten alleen vertalingen voor de overige teksten die niet in het canonical model staan.
5. Ook teksten die niet direct domeintermen zijn, maar wel in applicaties worden gebruikt op plekken waar het gaat over een bepaald functioneel domein, mogen in de business layer, zijn resources gezet worden.
6. [Presentation layer](layers.md#presentation-layer) bevat over het algemeen geen teksten. Die zetten we in de business layer: we hebben al genoeg plekken waar we resources neerzetten.

### Tips

1. Gebruik van placeholders zoals {0} is toegestaan, maar dan moet je wel een class erbij maken, die de placeholders vervangt. Zie JJ.Framework.Resources voor een voorbeeld. Het is dan verstandig om de resources zelf internal te maken en alleen de class die de placeholders vervangt public te maken. Kijk echter uit dat je het daarbij geschikt houdt voor meerdere talen, want een creatief met placeholder opgebouwde [resource string](patterns.md#resource-strings) werkt al gauw niet voor een andere taal.
2. Negeer dat de beschreven werkwijze kan resulteren in berichtteksten met hoofdlettergebruik zoals: 'Het __Ordernummer__ is niet ingevuld bij de __Bestelling__.' Als we dit aanpakken, doen we dat met een algoritme, niet met nog meer resources.
3. Het is verstandig om teksten in de applicatie algemeen te houden. Dus bijv. 'Artikelen', i.p.v. 'Artikelen in dit boek'. Dit scheelt vertaalwerk. Ook dit is verstandig: 'Artikel 1: Naam is verplicht.' Daarbij zijn de teksten 'Artikel', 'Naam' en '{0} is verplicht.' waarschijnlijk allang vertaald. Door de 'dubbele punt' notatie aan te houden ('Artikel 1:'), voorkom je het verwerken van de term in een zin, wat voor iedere taal een compleet andere grammatica kan zijn, wat voorkomt dat er complete volzinnen vertaald moeten worden.

*\* Resources staan niet in een database, omdat het applicaties trager maakt en kun je de code niet draaien op omgevingen waarbij je geen toegang hebt tot de database. In sommige situaties kun je niet eens compileren zonder toegang te hebben tot een specifieke database. Bovendien geeft mee compileren van resources in specifieke projecten ons de mogelijkheid dubbelzinnige termen anders te vertalen per business domein.*

[back](.)
