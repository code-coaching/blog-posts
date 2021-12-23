---
title: HTML
slug: html
tags:
  - HTML
categories:
  - Frontend
---

## Basisstructuur

HTML staat voor Hyper Text Markup Language. Het beschrijft de structuur van een
webpagina aan de hand van elementen.

Elementen bestaan uit een begin- en eindtag.

Een voorbeeld van een element is `<title>Voorbeeld 1</title>`, met `<title>` als
begintag en `</title>` als eindtag. `Voorbeeld 1` is de inhoud van het element.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Voorbeeld 1</title>
  </head>
  <body>
    <h1>Basisstructuur</h1>
    <p>
      Dit voorbeeld bevat de essentiële elementen om een webpagina te tonen met
      een titel en inhoud.
    </p>
  </body>
</html>
```

Om dit voorbeeld als webapagina te zien:

- maak een bestand aan genaamd `index.html`
- kopieer bovenstaande HTML en plak het als inhoud van het aangemaakte HTML-bestand
- sla het bestand op en open het in een webbrowser

![basisstructuur](/img/blog/basic-structure.nl.png)

## DOCTYPE

```html
<!DOCTYPE html>
<!-- ... -->
```

Alles tussen `<!-- -->` wordt gezien als commentaar in HTML.

Het DOCTYPE geeft aan dat het documenttype `html` is, hierdoor weet de webbrowser
hoe deze pagina weergegeven moet worden. Dit komt één keer voor,
bovenaan het document. `<!DOCTYPE html>` is de declaratie voor `HTML5`.

Oudere declaraties zien er iets ingewikkelder uit:

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

Dit komt omdat de oudere declaraties moeten verwijzen naar een DTD
(Document Type Definition).

Aangezien HTML5 de standaard is tegenwoordig, is het voldoende om enkel
`<!DOCTYPE html>` te onthouden.

## Elementen

Een element bestaat uit een begin- en eindtag.

```html
<head>
  <title>Voorbeeld 1</title>
</head>
```

`<head></head>` het `head`-element.
`<head>` de begintag van het `head`-element.
`</head>` de eindtag van het `head`-element.

Er zijn ook elementen zonder eindtag. Dit noemt men een leeg element.

`<br>`

De `<br>`-tag zal een lege lijn toevoegen.

Een leeg element kan ook als een "self closing element"
(Nederlands: element dat zichzelf afsluit) geschreven worden.

`<br />`

Dit is beide geldige HTML5.

### `<head>`-tag

```html
<!-- ... -->
<head>
  <title>Voorbeeld 1</title>
</head>
<!-- ... -->
```

Het element `head` bevat informatie over de webpagina. In het voorbeeld is hier
het element `title` terug te vinden, met daarin de inhoud `Voorbeeld 1`.

Hierin kunnen ook nog andere elementen geplaatst worden:

- `<style>`-tags om te verwijzen naar CSS-bestanden, om de stijling van de
  website anders te maken (bijvoorbeeld een roze achtergrond i.p.v. een witte).
  Dit komt terug in het onderdeel CSS binnen deze module.
- `<script>`-tags om te verwijzen naar JavaScript-bestanden. Dit komt in de
  module JavaScript terug.
- `<meta>`-tags om extra informatie over de webpagina te geven.

Enkele voorbeelden van `<meta>`-tags:

```html
<!-- ... -->
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>
<!-- ... -->
```

De `charset` duidt aan welke `character encoding` gebruikt wordt. `UTF-8` is
een van de meestgebruikte karaktersets.

De `viewport` bepaalt hoe de inhoud getoond moet worden, het gebruikte voorbeeld
is om de inhoud duidelijk te tonen op mobiele toestellen.

### Hoofdingen

Om verschillende groottes van titels te tonen, kan er gebruik gemaakt worden van
`<h1>` t.e.m. `<h6>`.

```html
<h1>Hoofding 1</h1>
<h2>Hoofding 2</h2>
<h3>Hoofding 3</h3>
<h4>Hoofding 4</h4>
<h5>Hoofding 5</h5>
<h6>Hoofding 6</h6>
```

![hoofdingen](/img/blog/headings.nl.jpeg)

### Paragrafen

Een paragraaf toont aan dat alle tekst binnen de begin- en eindtag van de
paragraaf, bij elkaar hoort.

```html
Deze twee regels lijken hetzelfde in de browser, toch is er een verschil!
<p>Deze twee regels lijken hetzelfde in de browser, toch is er een verschil!</p>
```

De tweede regel omschrijft voor de browser dat hetgeen getoond wordt, in de
context van een paragraaf is. De eerste regel toont gewoon een regel tekst.
De browser heeft geen informatie over wat de tekst voorstelt.

![2020-10-25-1603647352_screenshot_653x69](/img/blog/paragraphs.nl.jpeg)

### Links

Via links, hyperlinks, kan verwezen worden naar andere elementen of
volledige andere pagina's.

```html
<a href="https://9gag.com/gag/a9W9WyL">Dit is de getoonde tekst.</a>
```

Merk op dat een link die nog niet bezocht is, blauw is. En zodra de link
bezocht is, wordt deze paars. Dit wordt automatisch afgehandeld.

![link](/img/blog/link.nl.jpeg)
![bezochte link](/img/blog/link-visited.nl.jpeg)

### Afbeeldingen

```html
<img
  src="https://img-9gag-fun.9cache.com/photo/a9W9WyL_700bwp.webp"
  alt="Een meme van iemand"
/>
```

De `<img>`-tag is een leeg element. In het voorbeeld wordt geopteerd voor de
`self closing` variant.

`src` geeft de `source` (Nederlands: bron) aan waar de afbeelding gevonden kan
worden.

`alt` geeft de **alt**ernatieve tekst aan die getoond moet worden indien de
afbeelding te traag wordt ingeladen.

## Attributen

Elementen kunnen attributen bevatten. In de bovenstaande voorbeelden is dit
terug te vinden bij:

```html
<meta charset="UTF-8" />
<!-- charset is een attribuut met als waarde "UTF-8" -->

<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<!-- name is een attribuut met als waarde "viewport" -->
<!-- content is een attribuut met als waarde "width=device-width, initial-scale=1.0" -->
```

```html
<a href="https://9gag.com/gag/a9W9WyL">Dit is de getoonde tekst.</a>
<!-- href is een attribuut met als waarde de locatie van de website -->
```

```html
<img
  src="https://img-9gag-fun.9cache.com/photo/a9W9WyL_700bwp.webp"
  alt="Een meme van iemand"
/>
<!-- src is een attribuut met als waarde de afbeelding die getoond moet worden -->
<!-- alt is een attribuut met als waarde de tekst die getoond moet worden -->
```

## Lijsten

Er zijn twee soorten lijsten:

- Lijsten met ordering (Engels: ordered list)
- Lijsten zonder ordering (Engels: unordered list)

**O**rdered **L**ist, `<ol>`-element wordt gebruikt om een lijst met nummers
te tonen.

**U**nordered **L**ist, `<ul>`-element wordt gebruikt om een lijst zonder
nummers te tonen.

In beide gevallen kan een item toegevoegd worden, een **L**ist **I**tem: `<li>`.

```html
<h1>Cursisten</h1>

<ul>
  <li>Kwik</li>
  <li>Kwek</li>
  <li>Kwak</li>
</ul>

<h1>Cursisten</h1>

<ol>
  <li>Kwik</li>
  <li>Kwek</li>
  <li>Kwak</li>
</ol>
```

## Tabellen

Een tabel is een samenstelling uit verschillende elementen.

`<table>`: tussen de begin- en eindtag van het `table`-element, staan de andere
element.
`<tr>`: **t**able **r**ow, dit geeft aan dat het om één rij van de tabel gaat.
`<th>`: **t**able **h**eader, dit geeft aan dat het om een hoofdingelement gaat.
`<td>`: **t**able **d**ata, dit geeft aan dat het om een gewone cel in de tabel
gaat.

De elementen `<th>` en `<td>` zijn beide één cel in de tabel. Maar het
`<th>`-element zal de inhoud vetgedrukt maken en centreren. Het `<td>`-element
zal de inhoud links centreren.

De randen van de tabel kunnen zichtbaar gemaakt worden met CSS, dit wordt later
bekeken.

```html
<table>
  <tr>
    <th>Voornaam</th>
    <th>Achternaam</th>
    <th>Leeftijd</th>
  </tr>
  <tr>
    <td>Bart</td>
    <td>Duisters</td>
    <td>29</td>
  </tr>
  <tr>
    <td>Mark</td>
    <td>Duisters</td>
    <td>29</td>
  </tr>
</table>
```

![tabel](/img/blog/table.nl.jpeg)

```html
<table>
  <tr>
    <th colspan="2">Naam</th>
    <th>Leeftijd</th>
  </tr>
  <tr>
    <td>Bart</td>
    <td rowspan="2">Duisters</td>
    <td>29</td>
  </tr>
  <tr>
    <td>Mark</td>
    <td>29</td>
  </tr>
</table>
```

![tabel met colspan en rowspan](/img/blog/table-span.nl.jpeg)

De attributen 'colspan' en 'rowspan', geven aan hoeveel kolommen en rijen
gebruikt zullen worden door de data.

## Tekstopmaak

Het is mogelijk om de tekstopmaak te wijzigen met HTML-elementen.
Het is ook mogelijk om de tekstopmaak te wijzigen via CSS (module CSS).

```html
<b>b - bold / vetgedrukt</b>
```

```html
<strong>strong - belangrijke tekst</strong>
```

```html
<i>i - italic / cursief / schuingedrukt</i>
```

```html
<em>em - emphasized / benadrukt</em>
```

```html
sub <sub> subscript</sub> & sup <sup>superscript</sup>
```

```html
<mark>mark - markeren</mark>
```

```html
<del>del - delete / doorstreept</del>
```

```html
<ins>ins - insert / onderstreept</ins>
```

![tekstopmaak](/img/blog/text-formatting.nl.jpeg)

## Elementen: block & inline

Het is belangrijk om te begrijpen dat het plaatsen van HTML-elementen in een .html-bestand, niet aangeeft hoe de elementen getoond worden in de pagina die getoond wordt in de browser.

HTML-elementen hebben standaard een `display`-waarde. Er zijn twee mogelijke waarden: `block` en `inline`.

Het plaatsen van twee block-elementen onder elkaar of naast elkaar in het .html-bestand heeft niks te maken met hoe het getoond wordt in de browser. Ze worden onder elkaar getoond in de browser.

Het plaatsen van twee inline-elementen onder elkaar of naast elkaar in het .html-bestand heeft niks te maken met hoe het getoond wordt in de browser. Ze worden naast elkaar getoond in de browser.

### Block

Een element met als `display`-waarde `block` start altijd op een nieuwe regel en neemt de volledige beschikbare ruimte (links en rechts) in beslag.

Een voorbeeld is het `<div>`-element.

```html
<div>Eerste element</div>
<div>Tweede element</div>
```

![div](/img/blog/div.nl.png)

Alle elementen met als `display`-waarde `block`:

`<address>`
`<article>`
`<aside>`
`<blockquote>`
`<canvas>`
`<dd>`
`<div>`
`<dl>`
`<dt>`
`<fieldset>`
`<figcaption>`
`<figure>`
`<footer>`
`<form>`
`<h1>-<h6>`
`<header>`
`<hr>`
`<li>`
`<main>`
`<nav>`
`<noscript>`
`<ol>`
`<p>`
`<pre>`
`<section>`
`<table>`
`<tfoot>`
`<ul>`
`<video>`

### Inline

Een element met als `display`-waarde `inline` plaatst de inhoud op dezelfde regel en neemt zo veel ruimte in als nodig is voor de inhoud.

Een voorbeeld is het `span`-element.

```html
<span>Eerste element</span> <span>Tweede element</span>
```

![span](/img/blog/span.nl.png)

Alle elementen met als `display`-waarde `inline`:

`<a>`
`<abbr>`
`<acronym>`
`<b>`
`<bdo>`
`<big>`
`<br>`
`<button>`
`<cite>`
`<code>`
`<dfn>`
`<em>`
`<i>`
`<img>`
`<input>`
`<kbd>`
`<label>`
`<map>`
`<object>`
`<output>`
`<q>`
`<samp>`
`<script>`
`<select>`
`<small>`
`<span>`
`<strong>`
`<sub>`
`<sup>`
`<textarea>`
`<time>`
`<tt>`
`<var>`

## Semantische elementen

Vergelijk onderstaande HTML, bekijk specifiek de elementen in het `<body>`-element.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <div class="header">Een header</div>
    <div class="nav">
      <a href="#section1">Ga naar sectie 1</a>
      <a href="#section2">Ga naar sectie 2</a>
    </div>
    <div id="section1">
      <h1>Sectie 1</h1>
      <div>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </div>
    </div>
    <div id="section2">
      <h1>Sectie 2</h1>
      <div>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </div>
    </div>
    <div class="footer">Een footer</div>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <header>Een header</header>
    <nav>
      <a href="#section1">Ga naar sectie 1</a>
      <a href="#section2">Ga naar sectie 2</a>
    </nav>
    <section id="section1">
      <h1>Sectie 1</h1>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </p>
    </section>
    <section id="section2">
      <h1>Sectie 2</h1>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </p>
    </section>
    <footer>Een footer</footer>
  </body>
</html>
```

Beide voorbeelden zijn geldige HTML. Het verschil is dat bij het eerste voorbeeld allemaal `<div>`-elementen gebruikt worden.
Bij het tweede voorbeeld worden allemaal `semantische` elementen gebruikt. `Semantisch element` betekent dat de naam van
een element, omschrijft wat het doel van het element is.

Semantische elementen zijn makkelijker te indexeren door zoekmachines. Potentieel zorgt dit voor betere SEO (Search Engine Optimization, Nederlands: zoekmachineoptimalisatie).
