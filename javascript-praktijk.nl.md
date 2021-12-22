---
title: JavaScript - Praktijk
slug: javascript-praktijk
keywords:
  - JavaScript - Praktijk
modules:
  - JavaScript - Praktijk
  - Jaar 1
---

## JavaScript koppelen

JavaScript kan gekoppeld worden in een HTML-pagina. JavaScript **kan** ingeladen worden in de head-tag. Maar om in de JavaScript-bestanden aan de HTML-elementen te kunnen, moet de JavaScript ingeladen worden **na** alle HTML-elementen, onderaan in de body-tag.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <!-- CSS koppelen -->
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="titel">Voorbeeld</h1>
    <h1 class="titel">Voorbeeld</h1>
    <h1 id="speciaal">Voorbeeld</h1>

    <!-- Interne JavaScript koppelen met script-tag onderaan in de body-tag -->
    <script>
      console.log(
        "Dit wordt in de console geprint wanneer de pagina wordt ingeladen"
      );
    </script>

    <!-- Externe JavaScript koppelen met een script-tag onderaan in de body-tag  -->
    <script src="index.js"></script>
  </body>
</html>
```

```js
// Deze code staat in het bestand genaamd: index.js
console.log(
  "Dit wordt ook in de console geprint wanneer de pagina wordt ingeladen"
);
```

In de browser kan aangetoond worden dat de JavaScript correct wordt ingeladen:

![js](/img/blog/js.nl.jpeg)

### defer

Er is een manier om de JavaScript toch in de head-tag in te laden. Met het attribuut `defer`.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <!-- CSS koppelen -->
    <link rel="stylesheet" href="style.css" />
    <!-- Externe JavaScript koppelen met een script-tag gebruikmakende van het attribuut "defer" -->
    <script defer src="index.js"></script>
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="titel">Voorbeeld</h1>
    <h1 class="titel">Voorbeeld</h1>
    <h1 id="speciaal">Voorbeeld</h1>
  </body>
</html>
```

Het is beter om een JavaScript met `defer` in te laden in de head-tag, dan JavaScript inladen onderaan de body-tag.

Het verschil is dat bij het inladen van JavaScript in de body-tag, de JavaScript wordt ingeladen en vervolgens wordt uitgevoerd tijdens het verwerken van de HTML. Bij het inladen van JavaScript in de head-tag met het attribuut `defer`, wordt de JavaScript ingeladen tijdens het verwerken van de HTML en vervolgens wordt **na** het verwerken van de HTML de JavaScript uitgevoerd.

## DOM-manipulatie

Document Object Model, alle elementen van de webpagina.

Er kunnen elementen aan de DOM toegevoegd worden door de elementen in de HTML-pagina's toe te voegen. Dit kan ook gedaan worden via JavaScript

Er is een globaal object aanwezig in JavaScript waarin alle informatie van de HTML-pagina beschikbaar is, inclusief alle elementen. Dit is het object genaamd 'document'.

In Firefox, wanneer in de console `document` getypt wordt, kunnen alle properties en methodes die bestaan op het object bekeken worden.

![document](/img/blog/document.jpeg)

Op dit object zijn er methodes aanwezig om de elementen van de DOM op te vragen. Twee van de methodes zijn `document.querySelector()` en `document.querySelectorAll()`.

## querySelector & querySelectorAll

Dit geeft het eerste element terug dat overeenkomt met de selector die meegegeven wordt als parameter. De selectors die meegegeven worden, zijn dezelfde selectors zoals gebruikt worden in CSS.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="titel">Titel een</h1>
    <h1 class="titel">Titel twee</h1>
    <h1 id="speciaal">Titel drie</h1>

    <!-- In het voorbeeld wordt gebruik gemaakt van interne JavaScript, deze code kan ook in externe JavaScript staan. -->
    <script>
      const a = document.querySelector("h1");
      // Variabele a bevat één element, de h1-tag met als tekst 'Titel een'

      const b = document.querySelector(".titel");
      // Variabele b bevat één element, de h1-tag met als tekst 'Titel een'

      const c = document.querySelector("#speciaal");
      // Variabele c bevat één element, de h1-tag met als tekst 'Titel drie'

      const d = document.querySelectorAll("h1");
      // Variabele d bevat een NodeList (soort array) met drie elementen, alle h1-tags

      const e = document.querySelectorAll(".titel");
      // Variabele e bevat een NodeList (soort array) met twee elementen, alle h1-tags met als class 'titel' (de eerste twee h1-tags)

      const f = document.querySelectorAll("#speciaal");
      // Variabele f bevat een NodeList (soort array) met één element, alle h1-tags met als identifier 'speciaal' (de derde h1-tag)
    </script>
  </body>
</html>
```

De waarde van variabele `a` en `d` geprint in de console.

![querySelector-querySelectorAll](/img/blog/querySelector-querySelectorAll.nl.png)

In Firefox is het mogelijk om te zien welke properties/methodes er bestaan op een opgevraagd element.

![querySelector-h1](/img/blog/querySelector-h1.nl.jpeg)

## classList

Een van de bestaande properties op een element, is `classList`. Deze property is opnieuw een object waarop methodes bestaand om classes toe te voegen en te verwijderen.

![classList](/img/blog/classList.nl.jpeg)

Via de methode `add` kan een class worden toegevoegd.
Via de methode `remove` kan een class worden verwijderd.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <style>
      /* De class wordt hier aangemaakt */
      .toegevoegd {
        color: green;
        font-size: 20px;
      }
    </style>
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="titel">Voorbeeld</h1>
    <h1 class="titel">Voorbeeld</h1>
    <h1 id="speciaal">Voorbeeld</h1>

    <script>
      const specialeH1 = document.querySelector("#speciaal");
      specialeH1.classList.add("toegevoegd"); // Hier wordt de class '.toegevoegd', toegevoegd aan het class-attribuut van het element in de variabele 'specialeH1'
    </script>
  </body>
</html>
```

![class-toegevoegd](/img/blog/class-added.nl.jpeg)

De `class="toegevoegd"` is aan de DOM toegevoegd op het moment dat de JavaScript-code is uitgevoerd.

## setAttribute & removeAttribute

Via `setAttribute` kunnen er attributen toegevoegd worden aan een element.
Via `removeAttribute` kunnen er attributen verwijderd worden van een element.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="titel">Voorbeeld</h1>
    <h1 class="titel">Voorbeeld</h1>
    <h1 id="speciaal">Voorbeeld</h1>

    <script>
      const specialeH1 = document.querySelector("#speciaal");
      specialeH1.setAttribute("hidden", "");
    </script>
  </body>
</html>
```

![hidden](/img/blog/hidden.jpeg)
Doordat via JavaScript het attribuut 'hidden' wordt toegevoegd, wordt het element niet getoond in de browser.

### event handler

Via event handlers (Nederlands: afhandelen van events) kan er JavaScript uitgevoerd worden afhankelijk van een event/actie die gebeurd in HTML.

Een voorbeeld is het aanroepen van een JavaScript-functie wanneer er geklikt wordt op een element.

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <script defer src="index.js"></script>
  </head>
  <body>
    <h1 onclick="afhandelenKlik()">Voorbeeld</h1>
    <div></div>
  </body>
</html>
```

index.js

```js
function afhandelenKlik() {
  const divEl = document.querySelector("div");
  divEl.innerText = "Er is op de header geklikt!";
}
```
