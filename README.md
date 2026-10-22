# Cascade lab

Een ontwerplab voor startanimaties van webpagina's. Je stelt in hoe onderdelen van een pagina in beeld komen, ziet het resultaat direct op een proefpagina, en exporteert de instellingen als een blauwdruk die je in elk project kunt gebruiken.

## Het idee

Bij het openen van een pagina komen onderdelen niet allemaal tegelijk in beeld, maar na elkaar. Het verschil met de meeste moderne animaties zit in de timing: elk volgend onderdeel start al voordat het vorige klaar is. Daardoor voelt het als één rustige golf in plaats van een rij losse effecten.

Het principe komt van een jQuery-site uit 2010. Daar startte elk onderdeel 20 ms na het vorige, terwijl het infaden 200 ms duurde.

Het doel is drieledig:

- **Snelheid**: wat direct klaar is, komt eerst. Wat tijd kost om te laden, komt later. Het wachten valt zo niet op.
- **Rust**: geen flikkering en geen onnodige beweging.
- **Blik sturen**: de volgorde volgt de leesrichting, van logo en menu naar de rest.

## Begrippen

| Begrip | Betekenis |
|---|---|
| Bedrijf | Een groep onderdelen die samen in beeld komt, zoals "Golf" of "Uitgelicht". Een pagina kan meerdere bedrijven hebben. |
| Start | Wanneer het bedrijf begint, in ms na het laden. |
| Tussenruimte | Hoeveel ms elk volgend onderdeel later start dan het vorige. |
| Duur | Hoe lang één onderdeel erover doet om helemaal in beeld te komen. |
| Overlap | Duur gedeeld door tussenruimte. Bij 1 : 8 zijn er steeds zo'n acht onderdelen tegelijk in beweging. Hoe hoger, hoe meer het één golf wordt. |
| Easing | Het verloop van de beweging. `swing` start en eindigt langzaam (zoals jQuery). `ease-out` start snel en eindigt zacht. `lineair` is gelijkmatig. |
| Schuiven | Hoeveel px een onderdeel omhoog schuift tijdens het infaden. 0 is alleen infaden. |

## Het lab gebruiken

### De proefpagina

Links staat een voorbeeldpagina met vaste onderdelen: logo, menu, titel, intro, een strook met drie uitgelichte items, negen tegels en een zijkolom. De strook is bijzonder: die opent in hoogte en duwt de rest omlaag. Alle andere onderdelen faden in.

Onderdelen die in geen enkel bedrijf zitten, staan direct in beeld.

### Bediening

| Actie | Hoe |
|---|---|
| Opnieuw afspelen | Knop **Afspelen**, spatie, of klik op de pagina |
| Paneel verbergen | Knop **Verberg** of toets H |
| Thema wisselen | Knop **Thema** (systeem, licht, donker) of toets T |

### Bedrijven samenstellen

- **+ Bedrijf** voegt een nieuw bedrijf toe.
- Per bedrijf: hernoemen, omhoog of omlaag zetten (↑ ↓), verwijderen (×).
- **⇥** laat het bedrijf starten op het moment dat het vorige klaar is.
- Elke waarde heeft een slider en een invulvak. Ze lopen gelijk.
- Onder **Onderdelen en volgorde** kies je welke onderdelen in het bedrijf zitten en in welke volgorde ze komen. Een onderdeel kan maar in één bedrijf zitten. Voeg je het toe aan een ander bedrijf, dan verhuist het.

Bovenin het paneel zie je de overlap van het eerste bedrijf, wanneer de golf klaar is, en wanneer alles klaar is. De tijdlijn eronder toont elk onderdeel als een gekleurde balk, in de kleur van zijn bedrijf.

### Templates

Er zijn vier standaardtemplates:

| Template | Kenmerk |
|---|---|
| 2010 origineel | De oorspronkelijke jQuery-timing: 20 ms tussenruimte, 200 ms duur, overlap 1 : 10 |
| Jouw keuze | Iets rustiger: 25 ms tussenruimte, 200 ms duur, overlap 1 : 8 |
| Gulden snede | Fibonacci-getallen (21, 89, 144, 233, 377 ms), overlap ≈ 1 : 6,9 |
| Modern | Typische hedendaagse timing: 80 ms tussenruimte, 400 ms duur, met schuiven |

Eigen templates maak je door een naam in te vullen en op **Opslaan** te klikken. Standaardtemplates kun je niet verwijderen of overschrijven. Een eigen template verwijder je met **Verwijder** (twee keer klikken).

Waar templates bewaard worden, hangt af van hoe je het lab opent:

- **In Claude**: bij de pagina zelf, dus ook op een andere computer.
- **Als los HTML-bestand**: in die browser.

Die twee zien elkaar niet. Een template overzetten doe je door hem te exporteren en `cascade.json` aan de andere kant te importeren.

## Exporteren

**Downloaden (.zip)** maakt een zip met vier bestanden. In Claude vraagt het eerst om bevestiging. Als los bestand downloadt het direct. Lukt downloaden niet, bijvoorbeeld zonder internet, dan staan de bestanden onder **Bestanden bekijken** om te kopiëren.

Alleen bedrijven met onderdelen gaan mee in de export.

| Bestand | Inhoud |
|---|---|
| `cascade.css` | Verbergt de onderdelen uit de blauwdruk tot de animatie start, en zorgt voor een rustige scrollbar |
| `cascade.js` | De instellingen (`CONFIG`) en dezelfde animatiecode als het lab. Wat je in het lab ziet, is wat je krijgt. |
| `README.md` | Uitleg bij deze specifieke blauwdruk: de bedrijven in een tabel, de namen die je moet gebruiken, en hoe je het toepast |
| `cascade.json` | Alleen de instellingen. Importeer dit in het lab om later verder te werken. |

## Een blauwdruk toepassen in een project

### 1. Bestanden plaatsen

Zet `cascade.css` en `cascade.js` in je project.

### 2. Laden in `<head>`

```html
<link rel="stylesheet" href="cascade.css">
<script src="cascade.js"></script>
```

Laad het script zonder `defer` of `async`. Het moet vóór de rest van de pagina draaien, anders flitst de inhoud even in beeld voordat de animatie begint.

### 3. Elementen markeren

Geef elk element dat mee moet doen het attribuut `data-cascade`, met de naam uit het lab:

```html
<div data-cascade="logo">…</div>
<nav data-cascade="nav">…</nav>
<div data-cascade="tegel1">…</div>
```

De namen in het lab zijn: `logo`, `nav`, `titel`, `intro`, `strook`, `uit1` t/m `uit3`, `tegel1` t/m `tegel9`, `zijkop1`, `zijblok1`, `zijkop2`, `zijblok2`. De `README.md` in de export noemt alleen de namen die die blauwdruk gebruikt.

De namen hoeven niet letterlijk te passen bij wat het element in je project is. `tegel3` kan in je project een foto, een kaart of een alinea zijn. Het gaat om de plek in de volgorde.

Let op `strook`: dat element opent in hoogte. Gebruik het voor een blok dat andere inhoud omlaag mag duwen.

### Wat er gebeurt

- Elementen zonder `data-cascade` staan direct in beeld.
- Ontbreekt een element uit de blauwdruk, dan slaat het script het over.
- Opnieuw afspelen vanuit de console: `Cascade.replay()`.

## Aanpassen

- **Snel een getal wijzigen**: pas `CONFIG` aan in `cascade.js`.
- **Opnieuw afstellen**: importeer `cascade.json` in het lab, stel bij, en exporteer opnieuw.
- **Scrollbarkleur**: zet `--cascade-scrollbar` op `:root`, bijvoorbeeld `:root { --cascade-scrollbar: #c4ced2; }`.

## Veiligheid en toegankelijkheid

- Bij de systeeminstelling voor minder beweging staat alles direct in beeld.
- Als het script niet laadt, wordt er niets verborgen.
- Na 8 seconden wordt alles hoe dan ook zichtbaar.
- De scrollbar houdt vast ruimte vrij, zodat de pagina niet verspringt als hij tijdens de cascade langer wordt.

## Beperkingen

- De proefpagina ligt vast. Je kunt nog geen eigen pagina in het lab laden.
- Maximaal 12 bedrijven.
- De zip-functie wordt van internet geladen (cdnjs). Offline werkt alleen kopiëren.
