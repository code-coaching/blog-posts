---
  title: CSS
  keywords: ["CSS"]
  modules: ["CSS"]
---

HTML bevat de structuur van een pagina. CSS bevat de styling van een pagina.

CSS is een afkorting voor **C**ascading **S**tyle **S**heets. `Cascading` is wat
een waterval doet, het vloeit van een hoger gedeelte naar een lager gedeelte.

HTML verzorgt de elementen op een webpagina. CSS verzorgt hoe elementen getoond
worden.

Dit is beter te begrijpen met een voorbeeld, bekijk deze
[demo](https://www.w3schools.com/CSS/CSS_intro.asp). In de demo zijn vijf
variaties te zien van dezelfde HTML, met verschillende CSS.

## HTML-elementen

Sommige HTML-elementen voegen zelf styling toe. Om aan te tonen dat een
HTML-element nagebouwd kan worden, wordt er vertrokken vanuit een element
zonder styling en daarop wordt CSS toegepast om tot een gelijkaardig resultaat
te komen.

```html
<h1>Hoofding 1 met h1 element</h1>
<div>Hoofding 1 met CSS</div>
<div style="font-size: 28px; font-weight: 700;">Hoofding 1 met CSS</div>
```

![h1-element namaken met CSS](/img/html-CSS-duplicate.nl.jpeg)

## CSS koppelen

Er zijn drie manieren om CSS te koppelen aan een .html-bestand: `inline` (Nederlands: in dezelfde regel), `internal` (Nederlands: intern) en `external` (Nederlands: extern).

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <!-- Externe CSS -->
    <!-- Inoud van external.CSS
    .blauw {
        font-size: 28px;
        color: blue;
    }
    -->
    <link rel="stylesheet" type="text/CSS" href="external.CSS" />

    <!-- Interne CSS -->
    <style>
      div {
        font-size: 28px;
        color: red;
      }
    </style>
  </head>

  <body>
    <!-- 
        - Externe CSS wordt bekeken (omdat deze eerst wordt ingeladen in <head>)
            - Externe CSS heeft geen overeenkomende selector, er wordt niks toegepast
        - Interne CSS wordt bekeken (omdat deze als tweede wordt ingeladen in <head>)
            - Interne CSS heeft een overeenkomende selector: div
            - De styling `font-size: 28px;` en `color: red;` wordt toegepast
        - Inline CSS wordt toegepast (altijd als 'laatste')
            - Element bevat geen inline CSS, er wordt niks toegepast
    -->
    <div>Grote rode tekst</div>

    <!-- 
        - Externe CSS wordt bekeken (omdat deze eerst wordt ingeladen in <head>)
            - Externe CSS heeft geen overeenkomende selector, er wordt niks toegepast
        - Interne CSS wordt bekeken (omdat deze als tweede wordt ingeladen in <head>)
            - Interne CSS heeft een overeenkomende selector: div
            - De styling `font-size: 28px;` en `color: red;` wordt toegepast
        - Inline CSS wordt toegepast (altijd als 'laatste')
            - Element bevat inline CSS
            - De styling `font-size: 28px;` en `color: deeppink;` wordt toegepast
            - Dit overschrijft de eerder toegepaste `font-size: 28px;` en `color: red;`
    -->
    <div style="font-size: 28px; color: deeppink;">Grote roze tekst</div>

    <!--
        - Externe CSS wordt bekeken (omdat deze eerst wordt ingeladen in <head>)
            - Externe CSS heeft een overeenkomende selector: .blauw 
              (komt overeen met class='blauw')
            - De styling `font-size: 28px;` en `color: blue;` wordt toegepast
        - Interne CSS wordt bekeken (omdat deze als tweede wordt ingeladen in <head>)
            - De styling wordt NIET overschreven, een class-selector is specifieker 
              dan een element-selector
        - Inline CSS wordt toegepast (altijd als 'laatste')
            - Element bevat geen inline CSS, er wordt niks toegepast
    -->
    <div class="blauw">Grote blauwe tekst</div>
  </body>
</html>
```

Resultaat:

![CSS Cascading](/img/css-cascading.nl.png)

## Selectors

Bij inline CSS wordt de CSS direct toegepast op het element waar het style-attribuut op geplaatst wordt.

Bij internal en external CSS moet gebruikgemaakt worden van 'selectors' om te zorgen dat de CSS-regels
worden toegepast op bepaalde elementen.

### Element

Het is mogelijk elementen te selecteren op basis van de naam van een element. Het `body`-element kan geselecteerd worden met het woord `body`. Een `div`-element kan geselecteerd worden met het woord `div`.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* 
      * Dit selecteert ALLE elementen met de tag <body></body> 
      */
      body {
        /* Wijzig de achtergrondkleur naar zwart */
        background-color: black;
      }

      /* 
      * Dit selecteert ALLE elementen met de tag <div></div> 
      */
      div {
        /* Wijzig de achtergrondkleur naar roze */
        background-color: deeppink;
        /* Wijzig de margin (de witruimte rondom het element) naar 10px (standaard 0px) */
        margin: 10px 10px 10px 10px;
      }
    </style>
  </head>

  <body>
    <div>Eerste element</div>
    <div>Tweede element</div>
    <div>Derde element</div>
  </body>
</html>
```

![CSS selector element](/img/css-selector-element.nl.png)

### class

Het is mogelijk om elementen te selecteren op basis van de naam van een class.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* 
      * Dit selecteert ALLE elementen met de class "element"
      */
      .element {
        background-color: deeppink;
        margin: 10px 10px 10px 10px;
      }
    </style>
  </head>

  <body>
    <div class="element">Eerste element</div>
    <div class="element">Tweede element</div>
    <div class="element">Derde element</div>
  </body>
</html>
```

![CSS selector class](/img/css-selector-class.nl.png)

### id

Het is mogelijk om elementen te selecteren op basis van de naam van een id.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Document</title>
    <style>
      /* 
      * Dit selecteert ALLE elementen met de class "element"
      */
      .element {
        background-color: deeppink;
        margin: 10px;
      }

      /*
      * Dit selecteert ALLE elementen met id "element-2"
      */
      #element-2 {
        background-color: blue;
        color: white;
      }
    </style>
  </head>

  <body>
    <div class="element">Eerste element</div>
    <!-- 
    Merk op: de tweede div heeft zowel de class "element"
    als de id "element-2". Doordat CSS alle styling van
    boven naar onder toepast, zal eerst de styling van
    ".element" toegepast worden. Daarna wordt de styling
    van "#element-2" toegepast. Doordat background-color
    zowel bij ".element" en "#element-2" gedefinieerd is,
    zal de onderste waarden de bovenste waarden overschrijven.
    -->
    <div class="element" id='element-2'>Tweede element</div>
    <div class="element">Derde element</div>
  </body>
</html>
```

![CSS selector id](/img/css-selector-id.nl.png)