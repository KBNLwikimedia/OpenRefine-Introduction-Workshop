# Workshop outline and notes
*Latest update: 3 August 2023*

## Outline workshop
* Korte introductie over deze workshop
* Korte achtergrond over de cursusleider - Weet zelf het nodige, maar ben zeker geen volleerd OpenRefine-expert. 

Twee documenten bij deze workshop:
* [Overzicht online les- en hulpmaterialen OpenRefine_OJ22062023.pdf](https://raw.githubusercontent.com/KBNLwikimedia/OpenRefine-Introduction-Workshop/master/Overzicht%20online%20les-%20en%20hulpmaterialen%20OpenRefine_OJ22062023.pdf). Tutorials, cursussen, documentatie, howto’s, video’s etc. over Openrefine
* [Werkmaterialen introductieworkshop OpenRefine, KB, 4 juni 2023](https://raw.githubusercontent.com/KBNLwikimedia/OpenRefine-Introduction-Workshop/master/Werkmaterialen%20introductieworkshop%20OpenRefine%204%20juli%202023_OJ27062023.pdf). Hier staan links + data in die we in de workshop gaan gebruiken.

## Context workshop
In deze workshop gaan we aan de slag met de winnaars van de Halewijnprijs
* Website: https://www.halewijnprijs.nl/  (tabblad Winnaars)
* Wikipedia: https://nl.wikipedia.org/wiki/Halewijnprijs 
* Wikidata: https://www.wikidata.org/wiki/Q1570893 

De Halewijnprijs is een Nederlandse literatuurprijs
- Voorbeeld van een Wikidata-item over een literatuurprijs incl. de winnaars: P.C. Hooft-prijs - https://www.wikidata.org/wiki/Q1379623 
- Voorbeeld Wikidata-item over een winnaar van een literatuurprijs: Theun de Vries - https://www.wikidata.org/wiki/Q2143934 

##  Uitleg over de 4 OR-functies ------------ 
- Manipulatie/transformatie van data
- Reconciliatie tegen Wikidata, VIAF/NTA
- Extra data ophalen uit Wikidata, VIAF/NTA
- Uploaden naar Wikidata

## Werkvormen ------------ 
- Demos door Olaf
- Simultaan meedoen door cursisten
- Zelfsandige opdrachten door cursisten

Tip: Zet 5G Wifi access point op in je smartphone om slechte KB-wifi te omzeilen

##  Ready to go? ------
Wifi werkt?
Laptop opgeladen/voeding? 
PDF Werkmaterialen introductieworkshop OpenRefine bij de hand?
Start OpenRefine op
Zet leeg Excel-blad klaar


------ Opstarten OR ------------ 
- Leg interface uit : Create project - Open (bestaand) project - Import project - Language settings
- Onderaan (bij Open Project): Browse workspace directory + settings in openrefine.l4j.ini 
- Zet deze workspace directory NIET op OneDrive of in de MS-cloud i.v.m. syncing issues
- Linksonder: Version 3.7.2 [f7ad526]

## Aanmaken OR project
We gaan een project maken met data van de winnaars van de Halewijnprijs, zoals die vernoemd zijn op https://www.halewijnprijs.nl/ (tabblad Winnaars). 

Copy-paste deze data naar klembord. 
Twee manieren om deze data in OR te krijgen: 

1) rechtstreeks (clipboard)
Plak data in invoerveld --> Next --> Automatische herkenning vd kolommen Jaar-Auteur
Parse data as "Fixed-width field text files" (want: jaartal is steeds 4 cijfers) --> laat ook andere opties zien. Leg uit waarom "Line-based text files" en "CSV / TSV / separator-based files" in dit geval minder geschikt zijn
Kies Column widths = 4
Character encoding = UTF-8
Vul project name + tags in (rechtsboven)
Create project 

2) via Excel (This computer)
Plak data vanaf https://www.halewijnprijs.nl in lege Excel
Gegevens --> Tekst naar kolommen --> Vaste breedte --> Datatype van beide kolommen = tekst
Voeg kolomheaders toe
Sla Excel op.
Importeer Excel in OR
Vul project name + tags in (rechtsboven)
Create project 

Na "Create project"
32 rijen, breng aantal weergegeven rijen van 10 naar 50
Pas kolomnamen aan - Jaar + Winnaar

=== Laat cursisten bovenstaande beide manieren om een project te maken nu zelf nadoen ===

## Opschonen data------------ 

Opschonen data: kolommen Jaar en Winnaar: Edit cells- > Common transform --> Trim trailing and leading whitespaces + Collapse consecutive whitespaces
Je kunt dit via de kolom "All --> Edit all columns" (links) ook voor alle kolommen tegelijk doen

We willen de sortering naar jaar omdraaien: 1987 bovenaan, 2018 onderaan
Kolom Jaar --> Sort + Reverse + Remove sort
== Laat cursisten dit proberen/uitzoeken ==

Kolom Winnaar --> Edit column --> Split into serveral columns --> split by whitespace
Oh nee, dit wilden we niet --> toon Undo/Redo-tabblad (links)

In de hierna nog volgende uitleg zal ik aldoende meer datatransformaties de revu laten passeren

## Reconciliatie tegen Wikidata------------ 
We gaan nu reconnen tegen Wikidata 
- namen opzoeken in Wikidata, van strings naar things
- Daarmee kun je ook extra/externe data ophalen uit Wikidata

Kolom Winnaar --> Reconcile --> Start reconciling
Als het goed is staat Wikidata (EN) standaard gelecteerd als recon-service. Zo niet --> Discover services + Add standard service. 
Toon https://reconciliation-api.github.io/testbench/
Voeg Wikidata (NL) als recon service toe --> https://wikidata.reconci.link/nl/api
Kies Wikidata (NL) als recon service (want Halewijnprijs gaat over NL schrijvers)
Kies Q5 (mens) als type, want schrijvers zijn mensen
Start reconciling

Je krijgt nu allerlei matchsuggesties, licht toe
Controleer steeksproefsgewijs de 100% matches, want ook deze kunnen fouten bevatten
Leg uit hoe je bepaalt welke suggestie de juiste is adhv mouse-overs en inspectie Wikidata-item
Let op Otto de Kat! Pseudoniem van JG Gaarlandt (dus niet de kunstschilder!) 
Paul Hermans = de 3e suggestie
Bij somminge mouseovers staat al Halewijprijs genoemd!
Hub Beurkens = Huub Beurkens, na "Search for match" en controle van https://nl.wikipedia.org/wiki/Huub_Beurskens#Prijzen

OK, we hebben nu 32 rijen met gereconcilde winnaars!

Haal manifeste Qids op: Reconcile --> Add entity identifiers column. Kolomnaam = WDqid_winnaar
Deze kolom met Q-ids kun je ook reconnen tegen Wikidata! --> Laat zien  --> Kolom behouden, want straks gaan we de andere kolom 'Winnaar' leeggooien om de reconnen tegen de NTA
Haal recon matches weer weg: Reconcile --> Actions --> Clear reconcilation data

## Extra/externe data ophalen uit Wikidata ------------ 
We willen nu extra data van deze schrijvers ophalen uit Wikidata
- Afbeelding: https://www.wikidata.org/wiki/Property:P18
- Geboorteplaats https://www.wikidata.org/wiki/Property:P19  
- Geboortedatum: https://www.wikidata.org/wiki/Property:P569
- Beroep: https://www.wikidata.org/wiki/Property:P106 

Edit column --> Add column from reconciled values

Licht resultataten toe, 
* Van 32 naar 81 rijen, want P106-beroep heeft meerder waardes
* Nog steeeds 32 records 
* Afbeelding = "Eva Meijer 2018.jpg" en bijbehorende URL op Commons - https://commons.wikimedia.org/wiki/File:Eva_Meijer_2018.jpg
* Werk verder in record-view
Kort geboortedatum in van 1962-01-01T00:00:00Z naar 1962-01-01. Dit doe je als volgt: Edit column --> split into several columns, separator = T, remove 2e kolom, renmame kolom 'geboortedatum 1' --> 'geboortedatum'

##  Meervoudige cellen samen tot lijst ------------ 
Voeg meervoudige beroepen samen tot lijst, met " - " als separator
Edit cells --> Join multi-valued cells, separtor = " - "
Nog steeds 32 records, maar 36 rows
Herhaal dit voor geboorteplaats --> 33 rows
Herhaal dit voor geboortedatum (Rudi Hermans heeft er twee)--> 32 rows

== Opdracht: maak van de geboortedatum 3 kolommen: YY-MM-DD ==
== Opdracht: splits alle beroepen in aparte kolommen ==

## Facetten aanbrengen ------------ 
Selecteer de de auteurs zonder geboorteplaats -->  Facet by star --> tabblad "Facet/Filter" links --> Starred Rows --> true (8)
We hebben nu een lijstje van 8 auteurs zonder geboorteplaats

## OPTIONEEL: Geboorteplaatsen toevoegen ------------ 
Geboorteplaatsen opzoeken in Wikipedia en andere bronnen
* Peter Drehmanns = Roermond (bron: https://nl.wikipedia.org/wiki/Peter_Drehmanns)
* Fred Papenhove = Den Haag (bron: https://www.singeluitgeverijen.nl/auteur/fred-papenhove/ )
* Stijn van der Loo = Eindhoven (bron: https://www.brabantcultureel.nl/2019/10/13/stijn-van-der-loo-stopt-dementie-scheiden-en-een-midlifecrisis-in-een-verhaal/ )
* Adriaan Jaeggi = Wassenaar (bron: https://nl.wikipedia.org/wiki/Adriaan_Jaeggi)
* Laurens Spoor = Den Haag (bron: https://theaterencyclopedie.nl/wiki/Laurens_Spoor )
* Gijs IJlander = Alkmaar (bron: https://nl.wikipedia.org/wiki/Gijs_IJlander)
* André Janssens = Sint-Amandsberg (bron: https://www.dbnl.org/tekst/_ons003198701_01/_ons003198701_01_0063.php)
* Jan Huyskens = Horn (bron: http://streektaalzang.nl/strk/limb/limbjhun.htm)

- Voeg handmatig deze geboortplaatsen toe (of alleen de eerste 4 of zo)
- Recon deze plaatsen (Nederlandse gemeente, of Stad)
- Pas filtering/facet aan --> Facets --> Knop "Remove all" (links bovenaan)
- Gereconde plaatsen staan nu in de totaaltabel
- Haal facet-sterren handmatig weg

## Afbeeldingen: van naam naar URL ------------ 
Maak van afbeelding een volledige URL op Commons mbv GREL-expressie
Eva Meijer 2018.jpg --> https://commons.wikimedia.org/wiki/File:Eva_Meijer_2018.jpg
Kolom: afbeelding --> Edit column --> Add column based on this column
GREL: "https://commons.wikimedia.org/wiki/File:" + value (een soort CONCAT-functie)
Kolomnaam = CommonsURL
Je krijgt nu halvegare URLs door de spaties in de bestandsnamen
Fix: Edit cells --> Replace --> vervang spatie ( ) door underscore (_)
Klik op een paar URLs om resultaat te checken

## Recon tegen de NTA ---------------------------
Haal alle recon-data uit de kolom Winnaar weg (reset)
Reconcile --> Actions --> Clear recon data
Kolom bevat nu weer de kale strings waarmee we begonnen zijn
Reconcile --> Start reconciling --> NTA staat niet standaard in de lijst van recon services
Discover services --> https://reconciliation-api.github.io/testbench/ --> Kies NTA (obv Termennetwerk NDE): 
https://termennetwerk-api.netwerkdigitaalerfgoed.nl/reconcile/http://data.bibliotheken.nl/thesp/sparql 
Kies "Reconcile against no particular type"
 
Licht resultaten na reconciliatie toe. Dit zijn dus URLs in het Termennetwerk, niet rechtstreeks van de NTA op data.b.nl
Check NTA-suggesties tegen de geboortedatum die we eerder uit Wikidatas hebben opgehaald
Kies beste matches voor elke auteur
Haal NTA-URIs op (data.bibliotheken.nl)--> Reconcile --> Add entity identifiers column. Kolomnaam = NTAuri

## OPTIONEEL, GEAVANCEERD, ALS ER TIJD OVER IS ------------ 
Voorbeeld NTA URI: http://data.bibliotheken.nl/doc/thes/p075253429
Je kunt er nu een JSON-uri van maken door er .json achter te plakken: http://data.bibliotheken.nl/doc/thes/p075253429.json
JSON url : Edit columns --> Add column based on this columns 
GREL = value + ".json". Kolomnaam = NTAjsonURL
Daarna JSON-response ophalen : Add column by fetching URLs. Kolomnaam = NTAjsonResponse
Op basis van deze JSON-respons: Extractie van externe IDs (ISNI, VIAF, Wikidata, DBNL) 
GREL: value.parseJson()["@graph"][1]["sameAs"].join(" - ")
Kolomnaan = NTAJsonExternalIDs
Daarna op deze kolom: Edit columns --> Split into several columns 

## Winaars toevoegen aan Q-item Halewijn-prijs ---------------------------
We willen (de Qitems van) alle winnaars toevegen aan Q-item over Halewijn-prijs
Laat eerst voorbeld item met prijswinnaars zien: PChooftprijs - https://www.wikidata.org/wiki/Q1379623
Bespreek opbouw van dit item  
Winnaar: https://www.wikidata.org/wiki/Property:P1346
Qualifier: tijdstip (jaartal) = https://www.wikidata.org/wiki/Property:P585
Dit datastructuurtje willen we voor de Halewijnprijs nabouwen

## Opbouwen schema in OR ------------
We gaan een Wikidata OR schema bouwen
Knop rechtsboven: Extensions Wikibase --> Edit wikibase schema
Bespreek interface, groene strepen = gereconde kolommen
Add item - https://www.wikidata.org/wiki/Q1570893 (Halewijnprijs)
Add statement --> Sleep kolommen naar vakjes. Voeg P1346 (winnaar) met qualifier P585 (jaartal) toe
Let op dat je de Winnaars-kolom met de Wikidata-info pakt, niet die met de VIAF/NTA info!! 
Laat tabbladen Issues en Preview zien
Add reference: reference URL (P854) = https://www.halewijnprijs.nl/ 
Check in de Preview of alles OK is

## Uploaden naar Wikidata ------------
Extensions Wikibase --> Upload edits to Wikibase
Login-scherm 
Cursisten hebben account aangemaakt (maar zijn die al wel autoconfirmed!!??)
Laat 1 vd cusisten dit proberen, als het niet werkt kan ik de upload doen!



