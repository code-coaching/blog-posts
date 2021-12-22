---
title: Basis programmeren - Syntax
keywords:
  - Leren Programmeren
modules:
  - Leren Programmeren
  - Jaar 1
---

Om in een programmeertaal een werkend programma te coderen, moet de `syntax` van de programmeertaal gevolgd worden. Dit zijn alle regeltjes die gevolgd moeten worden om een werkend programma te schrijven.

## Keywords

In JavaScript zijn er gereserveerde sleutelwoorden (Engels: keywords). Enkele voorbeelden zijn `var`, `let`, `const`, `function`, `return` ... Deze woorden kunnen niet gebruikt worden als naam van een variabele of functie.

```js
const let = 3;
// een variabelenaam kan geen gereserveerd keyword zijn, dit resulteert in een error
```

## Statements

Een JavaScript-programma bestaat uit een lijst van statements.

```js
var x, y, uitkomst; // Statement 1
x = 6; // Statement 2
y = 9; // Statement 3
uitkomst = x + y; // Statement 4
```

## Semicolon

Een semicolon (Nederlands: puntkomma) wordt in veel programmeertalen gebruikt om een statement af te sluiten. Dit is optioneel in JavaScript.

```js
var x, y, uitkomst;
x = 6;
y = 9;
uitkomst = x + y;
```

In JavaScript is het gebruik van semicolons optioneel (tenzij er meerdere statements op één lijn staan).

## Witruimte

JavaScript houdt geen rekening met spaties, nieuwe regels ...

```
var x,y,uitkomst;x=6;y=9;uitkomst=x+y;
```

Is gelijk aan:

```js
var x, y, uitkomst;
x = 6;
y = 9;
uitkomst = x + y;
```

Voor betere leesbaarheid, is het aangeraden om gebruik te maken van witruimte.

## Declareren

```js
var a, b, uitkomst;
```

Het declareren van een variabele, maakt een variabele aan. Gedeclareerde variabelen hebben nog geen waarde. Er is simpelweg een variabele aangemaakt, waar later een waarde aan toegekend kan worden.

## Toekennen

```js
var a, b, uitkomst; // Declaratie van variabelen a, b en uitkomst
a = 6; // Toekenning van waarde 6 aan variabele a
b = 9; // Toekenning van waarde 9 aan variabele b
```

Het is ook mogelijk om het declareren en toekennen te combineren. Dit wordt initialiseren genoemd.

```js
var a = 6,
  b = 9,
  uitkomst;
```

## Operatoren

JavaScript gebruikt rekenkundige operatoren `+`, `-`, `*`, `/` om waarden te berekenen.

JavaScript gebruikt een toekenningsoperator `=` om waarden toe te kennen aan variabelen.

```js
var uitkomst = 6 + 9;
// Rekenkundige operator + wordt gebruikt om 6 en 9 op te tellen
// Toekenningsoperator = wordt gebruikt om de uitgerekende waarde 15 toe te kennen aan de variabele uitkomst
```

Rekenkundige operatoren volgen de prioriteitsregels uit de wiskunde.

| operator | uitleg                                               |
| :------- | :--------------------------------------------------- |
| ()       | Wat tussen haakjes staat heeft de hoogste prioriteit |
| \*\*     | Machtsverheffing                                     |
| \* en /  | Vermenigvuldigen en delen (dezelfde prioriteit)      |
| + en -   | Optellen en aftrekken (dezelfde prioriteit)          |

```
3 * 2 + 5 = 11
3 * (2 + 5) = 3 * 7 = 21
```

## Uitdrukkingen

Uitdrukkingen of expressies zijn combinaties van waarden, variabelen en operatoren. Het berekenen van een uitdrukking resulteert in een waarde.

Het berekenen van de waarde wordt evalueren genoemd.

De uitdrukking `6 + 9` evalueert naar `15`.

```js
var x = 6;
x + 9; // De uitdrukking x + 9 evalueert naar 15
```

```js
var x = 6;
x = x + 9;
// De uitdrukking x + 9 evalueert naar 15, het resultaat wordt toegekend aan x
```

## Commentaar

Commentaar in JavaScript wordt niet uitgevoerd. Commentaar op één lijn wordt gedaan met `//`. Met `/*` en `*/` kunnen meerdere regels in commentaar gezet worden.

```js
var x = 6; // Dit is commentaar en hier wordt niks mee gedaan
// var y = 9; Deze hele regel staat in commentaar, er wordt dus géén variabele y aangemaakt.

/*
Dit is allemaal commentaar.
Ook deze regel is commentaar.
*/
```

## Hooflettergevoelig

Namen van variabelen zijn hoofdlettergevoelig. Dit betekent dat `voornaam` en `voorNaam` gezien wordt als twee aparte variabelen. Een variabelenaam moet beginnen met een letter, `$` of `_`.

```js
var voornaam, voorNaam;
voornaam = "Bart";
voorNaam = "Barry";
```

## Variabelen

Er zijn in JavaScript drie manieren om een variabele te maken.
`var`, `let` en `const`. Vroeger was er enkel `var`. Het verschil tussen een variabele aangemaakt met één van de drie mogelijkheden wordt duidelijk met voorbeelden.

Een goede regel om te volgen is om géén `var` te gebruiken. Begin met `const`. Stel dat de variabele opnieuw een waarde toegekend moet krijgen, dan wordt `const` gewijzigd naar `let`. Om problemen te voorkomen, declareer de variabelen altijd voordat de variabelen gebruikt worden.

### var

```js
var getal = 13; // Declareren van variabele getal, toekennen van waarde 13
var getal; // Opnieuw declareren van variabele getal
console.log(getal); // 13
```

console.log(); print de uitkomst tussen `(` en `)` in de console.

### let

```js
let getal = 13; // Declareren van variabele getal, toekennen van waarde 13
let getal; // Dit geeft een error, getal kan niet opnieuw gedeclareerd worden
```

```js
let getal = 13; // Declareren van variabele getal, toekennen van waarde 13
getal = 14; // Het is wel mogelijk om opnieuw een waarde toe te kennen
console.log(getal); // 14
```

### const

```js
const getal = 13; // Declareren van variabele getal, toekennen van waarde 13
const getal; // Dit geeft een error, getal kan niet opnieuw gedeclareerd worden
```

```js
const getal = 13; // Declareren van variabele getal, toekennen van waarde 13
getal = 14; // Dit geeft een error, er kan niet opnieuw een waarde toegekend worden
```

## Operatoren

### Toekenning

| operator | omschrijving             | voorbeeld                     |
| :------- | :----------------------- | :---------------------------- |
| =        | Toekennen van een waarde | const a = 3; const b = a + 4; |

### Rekenkundig

| operator | omschrijving     | voorbeeld     |
| :------- | :--------------- | :------------ |
| +        | optellen         | 6 + 9 = 15    |
| -        | aftrekken        | 15 - 9 = 6    |
| \*       | vermenigvuldigen | 5 \* 2 = 10   |
| \*\*     | macht            | 5 \*\* 2 = 25 |
| /        | delen            | 10 / 2 = 5    |
| %        | modulus          | 11 % 2 = 1    |

<br/>

### Vergelijking

| Operator | Omschrijving                           |
| :------- | :------------------------------------- |
| ==       | gelijk aan                             |
| ===      | gelijke waarde en hetzelfde type       |
| !=       | niet gelijk aan                        |
| !==      | niet gelijk aan of niet hetzelfde type |
| >        | groter dan                             |
| <        | kleiner dan                            |
| >=       | groter of gelijk aan                   |
| <=       | kleiner dan of gelijk aan              |
| ?        | ternaire operator                      |

<br/>

## Datatypes

Datatypes zorgen ervoor dat programmeertalen weten wat er moet gebeuren de uitdrukkingen.

### Primitieve datatypes

- `string`
  - Een verzameling van karakters beginnend en eindigend met een enkele of dubbele quote.
  - bv. `"Dit is een string."`, `'Dit is ook een string.'`
- `number`
  - Een `number` is een nummer, dit kan met of zonder komma.
  - bv. `69`, `3.141592653`
- `boolean`
  - bv. `true`, `false`
- `undefined`
  - Wanneer een variabele nog geen waarde toegekend heeft gekregen, is het type `undefined`.
  - bv. `let x;`, het type van de variabele `x` is `undefined`

### Complexe datatypes

- `object`
- `function`

### Functie

Een functie heeft het type `function`.

### Object

Zowel een object `{name: 'Sylvia'}`, als een array `['Sylvia', 'Bart', 'John']` en `null` hebben het type `object`.

### typeof

Met `typeof` kan gekeken worden naar het type.

```js
typeof "Sylvia"; // string
typeof "Barry"; // string
typeof 69; // number
typeof 3.141592653; // number
typeof true; // boolean
typeof false; // boolean
typeof x; // undefined
typeof { name: "Sylvia" }; // object
typeof [1, 2, 3]; // object
typeof null; // object
typeof function som() {}; // function
```

In JavaScript zijn de datatypes dynamisch, een variabele kan meerdere types van data toegekend krijgen.

```js
let x; // x heeft het type undefined
x = 69; // x heeft het type Number
x = "Sylvia"; // x heeft het type String
```

Wanneer `+` gebruikt wordt bij twee `numbers`, dan worden de nummers bij elkaar opgeteld.
Wanneer `+` gebruikt wordt op twee `strings`, dan worden de twee `strings` samengevoegd.
Wanneer `+` gebruikt wordt op een `number` en een `string`, dan wordt `number` omgezet naar een `string` en zal het resultaat dus samengevoegd worden.

Uitdrukkingen worden van links naar rechts geëvalueerd.

```js
1 + 2; // 3
1 + "2"; // "12"
"Bart" + " " + "Duisters"; // "Bart Duisters"
1 + "Bart"; // "1Bart"
1 + 2 + "Bart"; // "3Bart"
```

## Functies

Een functie is een blok van code dat op een later moment aangeroepen kan worden. Wanneer een functie wordt aangeroepen, zal de code uitgevoerd worden.

```js
function functienaam(parameter1, paramater2) {
  /*
   * parameter1 en parameter 2 zijn variabelen,
   * enkel beschikbaar in het codeblok van de functie { en }
   */
  const resultaat = parameter1 - parameter2; // Een voorbeeld van een operator (aftrekken)
  return resultaat; // Teruggeven van een resultaat
}
```

Een functie wordt gedefinieerd door het woord keyword `function`, gevolgd door een functienaam (bestaande uit `letters`, `getallen`, `_` en `$`). Tussen de `(` en `)` worden de `parameters`/`argumenten` geplaatst. Dit zijn de waarden die doorgegeven worden aan de functie op het moment dat de functie aangeroepen wordt. De `parameters` worden behandeld als lokale variabelen tussen `{` en `}`. Alles tussen `{` en `}` wordt de `body` van de functie genoemd.

### Aanroepen

Stel dat er een functie is om "Hallo wereld!" te printen in de console.

```js
function helloWorld() {
  console.log("Hallo wereld!");
}
```

Deze kan aangeroepen worden met:

```js
helloWorld(); // Dit zal "Hallo wereld!" printen in de console
```

_Merk op_: De functienaam is in het Engels. Wanneer een programmeur start in een bedrijf, dan zal de code waarschijnlijk in het Engels geschreven worden.

Stel dat er een functie is om twee getallen op te tellen.

```js
function sum(a, b) {
  return a + b;
}
```

Wanneer JavaScript uitgevoerd wordt en een `return` wordt geëvalueerd, dan zal JavaScript stoppen met het uitvoeren van de functie.

Deze kan aangeroepen worden met:

```js
sum(1, 2); // 3
sum(6, 9); // 15
sum(9, "Bart"); // "9Bart"
```

## Scope

Scope is Engels voor reikwijdte/omvang. Wanneer er gesproken wordt over de `scope` van een variabele, dan gaat het over de reikwijdte van de variabele.

Enkele voorbeelden zullen dit verduidelijken.

```js
let x = 4; // Overal bruikbaar, de scope is globaal

function scopeExample(parameter) {
  // De paramater is enkel bruikbaar binnen de functie scopeExample
  let y = 3;
  // De variabele y is enkel bruikbaar binnen de functie scopeExample
  console.log("x: ", x); // "x: 4"
  console.log("y: ", y); // "y: 3"
  console.log("x + y: ", x + y); // "x + y: 7"
  console.log(parameter); // [waarde van parameter]
}

scopeExample("Bart");
// "x: 4"
// "y: 3"
// "x + y: 7"
// "Bart"

console.log(x); // 4
console.log(y); // undefined
console.log(parameter); // undefined
```

## Keywords

In JavaScript zijn er gereserveerde sleutelwoorden (Engels: keywords). Enkele voorbeelden zijn `var`, `let`, `const`, `function`, `return` ... Deze woorden kunnen niet gebruikt worden als naam van een variabele of functie.

```js
const let = 3;
// een variabelenaam kan geen gereserveerd keyword zijn, dit resulteert in een error
```

## Conditionele statements

Met conditionele statements is het mogelijk om verschillende code uit te voeren, afhankelijk van de `condities` die voldaan zijn. Een synoniem voor `conditie`, is `voorwaarde`. Er wordt gesproken over conditionele statements en voorwaardelijke statements, dit betekent hetzelfde.

JavaScript kent vier verschillende conditionele statements.

`if`, `else`, `else if`, `switch`

Het wordt duidelijk met voorbeelden.

### if

```js
const x = 4;
const y = 3;

/* 
  Het if-statement kan gelezen worden als: 
  "Als de voorwaarde 'x > y' evalueert naar 'true', voer dan de code tussen '{' en '}' uit."
*/
if (x > y) {
  // Deze code wordt uitgevoerd omdat x > y -> 4 > 3 evalueert naar 'true'
  console.log("x is groter dan y");
}
```

Een `if`-statement bestaat uit het keyword `if`, een conditie die geëvalueerd wordt `(conditie)` (in het voorbeeld wordt de conditie `x > y` geëvalueerd) en een codeblok met code die uitgevoerd moet worden indien de conditie evalueert naar `true`.

### else

Een `else`-statement gaat altijd gekoppeld met een `if`-statement. Wanneer de voorwaarde van de `if`-statement evalueert naar `false` en er is een `else`-statement gedefineerd, dan wordt de code in het codeblok van de `else`-statement uitgevoerd.

```js
const x = 3;
const y = 4;

// x > y -> 3 > 4 -> false
if (x > y) {
  // De code in het codeblok van de if-statement wordt niet uitgevoerd
  console.log("x is groter dan y");
} else {
  // De code in het codeblok van de else-statement wordt uitgevoerd
  console.log("x is kleiner dan y");
}
```

### else if

Een `else if`-statement gaat altijd gekoppeld met een `if`-statement. Dit maakt het mogelijk om een nieuwe voorwaarde te evalueren indien de eerste voorwaarde naar `false` evalueert.

```js
const x = 100;

if (x <= 10) {
  // De code in het codeblok van de if-statement wordt niet uitgevoerd
  console.log("x is kleiner of gelijk aan 10");
} else if (x <= 100) {
  // De code in dit codeblok wordt wel uitgevoerd aangezien x <= 100 naar true evalueert
  console.log("x is groter dan 10, maar kleiner of gelijk aan 100");
} else {
  // De code in dit codeblok wordt niet uitgevoerd
  console.log("x is groter dan 100");
}
```

### switch

Een `switch`-statement wordt gebruikt om een codeblok uit te voeren indien een van de mogelijke opties overeenkomt met de geëvalueerde expressie.

Een codeblok van een switch case bevat het keyword `case`. Het codeblok van een `case` is gedefinieerd na een dubbelpunt `:`, een codeblok eindigt wanneer het keyword `break` wordt bereikt.

```js
const naam = "Bart";

switch (naam) {
  case "Sylvia":
    console.log("De naam is Sylvia");
    break;
  case "Bart":
    // Dit codeblok wordt uitgevoerd
    console.log("De naam is Bart");
    // Wanneer 'break' bereikt wordt, gaat de code van HIER*
    break;
  case "Barry":
    console.log("De naam is Barry");
    break;
}
// naar HIER*
```

<br/>
Het is mogelijk om een `default` te voorzien, dit wordt uitgevoerd wanneer geen enkele `case` overeenkomt met de geëvalueerde expressies.

```js
const naam = "Mark";

switch (naam) {
  case "Sylvia":
    console.log("De naam is Sylvia");
    break;
  case "Barry":
    console.log("De naam is Barry");
    break;
  default:
    // Dit codeblok wordt uitgevoerd
    console.log("De naam komt niet voor");
    break;
}
```

<br/>
Het is mogelijk om meerdere `case`s te plaatsen bij één codeblok.

```js
const vandaag = "dinsdag";

switch (vandaag) {
  case "maandag":
  case "woensdag":
    console.log("Werken + Lesgeven tweedejaars");
    break;
  case "dinsdag":
  case "donderdag":
    // Dit codeblok wordt uitgevoerd
    console.log("Werken + Lesgeven eerstejaars");
    break;
  case "vrijdag":
    console.log("Werken");
    break;
  case "zaterdag":
  case "zondag":
    console.log("Lesvoorbereidingen");
    break;
}
```

## Iteratieve statements

Iteratie is een synoniem van herhaling. Met een iteratieve statement kan code
herhaald worden uitgevoerd.

JavaScript heeft vijf iteratieve statements:
`for`, `for in`, `for of`, `while` en `do while`

Dit worden ook wel lussen genoemd. In het Engels `loops`.

We bekijken de `for loop`, `while loop` en `do while loop`.

### while

In het Nederlands `zolang als` of `terwijl`. De code in het codeblok zal
uitgevoerd worden zolang als de expressie naar `true` evalueert.

```js
let i = 0;

// Voordat de eerste iteratie begint wordt de expressie (i < 3) geëvalueerd.
// Aangezien i de waarde 0 heeft, evalueert (0 < 3) naar true.
// Het codeblok van de while wordt uitgevoerd.
while (i < 3) {
  console.log("Loop: ", i); // *
  // * Eerste iteratie wordt geprint "Loop: 0"
  // * Tweede iteratie wordt geprint "Loop: 1"
  // * Derde iteratie wordt geprint "Loop: 2"
  i++; // **
  // ** Eerste iteratie wordt 0 opgehoogd naar 1, vervolgens wordt 1 < 3 geëvalueerd
  // ** Tweede iteratie wordt 1 opgehoogd naar 2, vervolgens wordt 2 < 3 geëvalueerd
  // ** Derde iteratie wordt 2 opgehoogd naar 3, vervolgens wordt 3 < 3 geëvalueerd
}
// Na de derde iteratie wordt 3 < 3 geëvalueerd, dit is false, JavaScript stopt
// met het uitvoeren van de while loop.
```

### do while

In het Nederlands `doe ... zolang als` of `doe ... terwijl`. De code in het
codeblok zal uitgevoerd worden, na de eerste iteratie wordt de eerste evaluatie
van de expressie in de while gedaan.

```js
let i = 0;

// Het codeblok van de do while wordt uitgevoerd.
do {
  console.log("Loop: ", i); // *
  // * Eerste iteratie wordt geprint "Loop: 0"
  // * Tweede iteratie wordt geprint "Loop: 1"
  // * Derde iteratie wordt geprint "Loop: 2"
  i++; // **
  // ** Eerste iteratie wordt 0 opgehoogd naar 1, vervolgens wordt 1 < 3 geëvalueerd
  // ** Tweede iteratie wordt 1 opgehoogd naar 2, vervolgens wordt 2 < 3 geëvalueerd
  // ** Derde iteratie wordt 2 opgehoogd naar 3, vervolgens wordt 3 < 3 geëvalueerd
} while (i < 3);
// Na de derde iteratie wordt 3 < 3 geëvalueerd, dit is false, JavaScript stopt
// met het uitvoeren van de do while loop
```

Bovenstaande code geeft hetzelfde resultaat als het voorbeeld van de while loop.
Toch is er een belangrijk verschil. Dit wordt duidelijk met onderstaande voorbeelden.

```js
let i = 2;

while (i < 1) {
  console.log("While loop");
  i++;
}

console.log("i is nu: ", i);

do {
  console.log("Do while loop");
  i++;
} while (i < 1);

console.log("i is nu: ", i);
/*
* Het volgende wordt uitgeprint:
i is nu: 2
Do while loop
i is nu: 3
* Merk op dat de while loop niet uitgevoerd wordt 
* en dat de do while wél uitgevoerd wordt
*/
```

### for

In het Nederlands `voor`. Voor zolang de expresie geldig is, voer het codeblok uit.

```js
// in de expressie van de for loop staan drie statements
// statement 1: het initialiseren van variabele i met keyword let met waarde 0
// statement 2: vergelijking van i met waarde 3, dit gebeurt voor elke iteratie
// statement 3: ophoging van i met 1, dit gebeurt na elke iteratie
for (let i = 0; i < 3; i++) {
  console.log("Loop: ", i); // *
  // * Eerste iteratie wordt geprint "Loop: 0"
  // * Tweede iteratie wordt geprint "Loop: 1"
  // * Derde iteratie wordt geprint "Loop: 2"
}
```

## Array

Een array is een manier om een lijst van gegevens bij te houden.

```js
const fibonacciReeks = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34];
```

De Fibonacci-nummers is een reeks van nummers. Om het volgende nummer in de reeks
te bepalen wordt de som van de vorige twee getallen genomen.

Het eerste getal is 0, het tweede getal is 1, het derde getal is de som van
het eerste getal met het tweede getal en bijgevolg dus 1.
Het vierde getal is de som van het tweede getal (1) en het derde getal (1) en
bijgevolg dus 2.

De variabele `fibonacciReeks` bevat de eerste tien getallen van de
Fibonacci-nummers.

De waarden van een array kunnen opgevraagd worden via de `index`. Een index
begint bij 0. Het eerste getal heeft index 0, het vijfde getal heeft index 4 en
het tiende getal heeft index 9.

Om te weten wat de waarde van het zesde getal is:

```js
console.log(fibonacciReeks[5]); // Dit zal het getal 5 loggen
```

Een waarde toekennen aan een array kan door aan te geven in welke index de
waarde moet komen. Het elfde getal toevoegen:

```js
const fibonacciReeks = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34];
fibonacciReeks[10] = 55;
```

Merk op dat de array wordt gewijzigd, ook al is het een variabele gedeclareerd
met het keyword `const`. Dit komt in een latere module in detail terug.

Voor nu is het voldoende om te onthouden dat:

- Een variabele gedeclareerd met het keyword `const` kan niet opnieuw een
  waarde toegekend krijgen.
- De array op zich is de waarde. Er zal geen nieuwe array toegekend kunnen worden.
- De waarden in de array, veranderen de array, maar het is nog altijd dezelfde
  array die toegekend is aan de variabele.

## Object

Een object is een manier om complexe data bij te houden. Bijvoorbeeld een wagen,
een wagen bestaat uit verchillende onderdelen en heeft verschillende
eigenschappen.

Een object dat een wagen voorstelt.

```js
{
  merk: "Opel",
  type: "Manta 400",
  aantalBanden: 4,
  aantalDeuren: 2,
  pk: 114
}
```

Een object dat een persoon voorstelt.

```js
{
  voornaam: "Bart",
  achternaam: "Duisters",
  geboorteDatum: "13-08-1991",
}
```

Een object kan, net als een array, gezien worden als een waarde die toegekend
kan worden aan een variabele. Net als een array, het object op zich is de waarde
die toegekend wordt aan een variabele. Het wijzigen van de properties en
waarden van de properties, wijzigt het object, maar het is nog altijd hetzelfde
object dat toegekend is aan de variabele.

```js
const persoon = {
  voornaam: "Bart",
  achternaam: "Duisters",
  geboorteDatum: "13-08-1991",
};

// Wijzigen van een waarde van een bestaande property
persoon.voornaam = "Mark";

// Extra property toekennen aan het object
persoon.leeftijd = 29;
```

Het object, met de wijzigingen, ziet er nu als volgt uit:

```js
{
  voornaam: "Mark",
  achternaam: "Duisters",
  geboorteDatum: "13-08-1991",
  leeftijd: 29,
}
```

Aangezien JavaScript een dynamische taal is, kunnen we alle types toekennen
als variabele.

```js
const persoon = {
  voornaam: "Bart",
  achternaam: "Duisters",
  geboorteDatum: "13-08-1991",
  fibonacciReeks: [0, 1, 1, 2, 3, 5, 8, 13],
};

console.log(persoon.fibonacciReeks[4]);
// Dit print de waarde 3
```

Een `function` is ook een type. Het is dus ook mogelijk om een
functie toe te kennen als waarde van een property in een object.

```js
const persoon = {
  voornaam: "Bart",
  achternaam: "Duisters",
  geboorteDatum: "13-08-1991",
  fibonacciReeks: [0, 1, 1, 2, 3, 5, 8, 13],
  zegHallo: function () {
    console.log("Hallo");
  },
};

persoon.zegHallo(); // Dit print "Hallo" in de console
```

```js
const persoon = {
  zegHallo: function () {
    console.log("Hallo");
  },
  zegDoei: () => {
    console.log("Doei");
  },
};
```

Nieuwe termen:

- `Anonieme functie`: een functie zonder functienaam.
- `Arrow notation`: een `=>` achter de functiehaakjes i.p.v. het keyword
  `function` voor de functiehaakjes.
