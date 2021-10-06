---
  title: Van JavaScript naar TypeScript
  keywords: ["TypeScript", "JavaScript"]
  modules: ['Jaar 2']
---

Vereisten om dit blog optimaal te volgen:

- [basiskennis JavaScript](blog/nl/javascript-basis/), de kennis bouwt verder op JavaScript
- basiskennis [NodeJS](https://nodejs.org/en/), kunnen uitvoeren van `npm`-commando's)

Wanneer er verwezen wordt naar `terminal` in onderstaande tekst en er wordt met Windows gewerkt, dan moet het uitgevoerd worden in `Git Bash`. Indien CTRL+V niet werkt om gekopieerde inhoud te plakken in de terminal, probeer SHIFT+INSERT.

## Wat is TypeScript?

TypeScript is een `strongly typed` (Nederlands: sterk getypeerde) programmeertaal. In tegenstelling tot JavaScript, wat een `weakly typed` (Nederlands: zwak getypeerde) programmeertaal is.
Moet er dan opnieuw, vanaf nul een taal aangeleerd worden? Nee! Alle JavaScript, is ook geldige TypeScript. TypeScript voorziet extra syntax bovenop alle geldige JavaScript om `type safety` (Nederlands: typeveiligheid) te bekomen.

Een voorbeeld.

JavaScript - index.js

```js
let volledigeNaam;
volledigeNaam = "Bart";

function zegHallo(naam, leeftijd) {
  return `Hallo ${naam}, je bent ${leeftijd} jaar!`;
}

const resultaat = zegHallo(volledigeNaam, 30);
console.log(resultaat);
```

TypeScript - index.ts

```js
let volledigeNaam: string;
volledigeNaam = "Bart";

function zegHallo(naam: string, leeftijd: number): string {
  return `Hallo ${naam}`;
}

const resultaat = zegHallo(volledigeNaam, 30);
console.log(resultaat);
```

Er zijn twee verschillen tussen TypeScript en JavaScript.

1. Er zijn types toegekend:

- `: string` achter de variabele `volledigeNaam`, de parameter `naam` en de functie `zegHallo`.
- `: number` achter de parameter `leeftijd`.

2. De bestandsnaam eindigt in `.ts` in plaats van `.js`.

## Maar webbrowsers werken toch enkel met HTML, CSS en JavaScript?

Correct! TypeScript zal gecompileerd moeten worden naar JavaScript. In de meeste moderne frameworks zit dit automatisch verwerkt. Om een beter beeld te krijgen hoe dit verloopt, zal er een klein project opgezet worden zonder framework.

**Maak een nieuwe map aan om in te werken en ga in deze map.**

```sh
mkdir typescript-voorbeeld && cd $_
```

Uitleg commando:

- `mkdir typescript-voorbeeld` maakt een nieuwe map aan genaamd `typescript-voorbeeld`.
- `&&` zorgt ervoor dat we twee commando's achter elkaar kunnen uitvoeren.
- `cd` is **c**hange **d**irectory (Nederlands: wijzigen van map)
- `$_` verwijst naar de pas aangemaakte mapnaam

**Maak van de map een `npm`-project.**

```sh
npm init
```

Op alle vragen die er komen, klik `Enter`.

Er is een package.json aanwezig, dit betekent dat het een `npm`-project is en er kunnen vervolgens dependencies geïnstalleerd worden. Dependencies is software gemaakt door andere mensen, beschikbaar op [npm](https://www.npmjs.com).

Om TypeScript te kunnen gebruiken in het project, moet dit geïnstalleerd worden.

```sh
npm install --save-dev typescript
```

Opmerking: Indien een dependency alleen nodig is tijdens het ontwikkelen, kan deze met --save-dev geïnstalleerd worden. Het verschil is dat met `--save-dev` de depdency in package.json zal toegevoegd worden in de property `devDependencies`, installeren zonder `--save-dev` zal de dependency toevoegen in de property `dependencies`.

Dit zorgt ervoor dat het `tsc`-commando beschikbaar is in `npm`-scripts.

In `package.json` kan een extra script toegevoegd worden.

```sh
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "compile": "tsc index.ts",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

De regel `"compile": "tsc index.ts",` is nieuw.

Opmerking: Er is een `package-lock.json` toegevoegd. Hierin moet nooit iets gewijzigd worden, dit wordt automatisch gedaan bij een `npm install`.
Opmerking: Er is een map `node_modules` toegevoegd. Hierin zit de software die geïnstalleerd is tijdens het uitvoeren van `npm install --save-dev typescript`.
Opmerking: Indien er géén `node_modules`-map aanwezig is, maar wel een `package.json` met `dependencies` of `devDependencies`, dan kan `npm install` uitgevoerd worden in een terminal om de software te installeren. Dit zal geïnstalleerd worden in `node_modules`.

**Voeg een bestand `index.ts` toe**

```sh
touch index.ts
```

Aangezien alle JavaScript ook geldige TypeScript is, kan in het `index.ts`-bestand ook JavaScript geplaatst worden.

```js
let volledigeNaam;
volledigeNaam = "Bart";

function zegHallo(naam, leeftijd) {
  return `Hallo ${naam}, je bent ${leeftijd} jaar!`;
}

const resultaat = zegHallo(volledigeNaam);
console.log(resultaat);
```

Vervolgens kan het TypeScript-bestand door de TypeScript-compiler gevoerd worden om een JavaScript-bestand te bekomen.

```sh
npm run compile
```

De output in de terminal na het uitvoeren van bovenstaande commando:

```sh
> test@1.0.0 compile
> tsc index.ts

index.ts:8:19 - error TS2554: Expected 2 arguments, but got 1.

8 const resultaat = zegHallo(volledigeNaam);
                    ~~~~~~~~~~~~~~~~~~~~~~~

  index.ts:4:25
    4 function zegHallo(naam, leeftijd) {
                              ~~~~~~~~
    An argument for 'leeftijd' was not provided.


Found 1 error.
```

De TypeScript-compiler ziet dat er twee parameters nodig zijn bij de functie `zegHallo` en deze wordt aangeroepen met één parameter. Ook al wordt er een error getoond, toch wordt er een bestand `index.js` aangemaakt.

De inhoud van dit bestand:

```js
var volledigeNaam;
volledigeNaam = "Bart";
function zegHallo(naam, leeftijd) {
  return "Hallo " + naam + ", je bent " + leeftijd + " jaar!";
}
var resultaat = zegHallo(volledigeNaam);
console.log(resultaat);
```

JavaScript kan uitgevoerd worden met `NodeJS`:

```sh
node index.js
```

Dit geeft als resultaat in de terminal:

```sh
Hallo Bart, je bent undefined jaar!
```

De `undefined` is niet de output die developers willen zien!

Hier wordt de kracht van TypeScript, zelfs zonder typings toe te voegen, duidelijk. De TypeScript-compiler gaf al aan dat er twee parameters verwacht worden en dat de functie aangeroepen wordt met maar één parameter. Nog voor de JavaScript uitgevoerd wordt, kan er gezien worden dat er iets niet goed zit.

Inhoud van index.ts na het toevoegen van de tweede parameter:

```js
let volledigeNaam;
volledigeNaam = "Bart";

function zegHallo(naam, leeftijd) {
  return `Hallo ${naam}, je bent ${leeftijd} jaar!`;
}

const resultaat = zegHallo(volledigeNaam, 30);
console.log(resultaat);
```

Opnieuw door de TypeScript-compiler voeren:

```sh
npm run compile
```

Dit heeft als output:

```sh
> test@1.0.0 compile
> tsc index.ts
```

Géén error meer! En het uitvoeren van het JavaScript-bestand heeft ook geen `undefined` meer in het resultaat staan:

```sh
node index.js
```

Output:

```sh
Hallo Bart, je bent 30 jaar!
```

**Typings toevoegen aan het TypeScript-bestand**

Wijzigingen in het bestand:

- `: string` en `: number` typings toegevoegd aan de variabele `volledigeNaam`, de parameters `naam` en `leeftijd` en een type toegevoegd aan de functie `zegHallo`.
- Bij het aanroepen van de functie zijn de argumenten omgewisseld.
- De tweede parameter heeft een typfout, er staat `volledigNaam` i.p.v. `volledigeNaam`.

```js
let volledigeNaam: string;
volledigeNaam = "Bart";

function zegHallo(naam: string, leeftijd: number): string {
  return `Hallo ${naam}, je bent ${leeftijd} jaar!`;
}

const resultaat = zegHallo(30, volledigNaam);
console.log(resultaat);
```

Compile het `index.ts`-bestand, dit heeft als output:

```sh
> test@1.0.0 compile
> tsc index.ts

index.ts:8:28 - error TS2345: Argument of type 'number' is not assignable to parameter of type 'string'.

8 const resultaat = zegHallo(30, volledigNaam);
                             ~~

index.ts:8:32 - error TS2552: Cannot find name 'volledigNaam'. Did you mean 'volledigeNaam'?

8 const resultaat = zegHallo(30, volledigNaam);
                                 ~~~~~~~~~~~~

  index.ts:1:5
    1 let volledigeNaam: string;
          ~~~~~~~~~~~~~
    'volledigeNaam' is declared here.


Found 2 errors.
```

Doordat de parameters nu types toegekend hebben gekregen, ziet de compiler dat het niet oké is om een nummer door te geven als eerste argument. De compiler merkt ook op dat `volledigNaam` niet bestaat en geeft als suggestie dat het misschien `volledigeNaam` moet zijn.

Wijzigingen in het bestand:

- Bij het aanroepen van de functie zijn de argumenten omgewisseld.
- `volledigNaam` is gewijzigd naar `volledigeNaam`.

```js
let volledigeNaam: string;
volledigeNaam = "Bart";

function zegHallo(naam: string, leeftijd: number): string {
  return `Hallo ${naam}, je bent ${leeftijd} jaar!`;
}

const resultaat = zegHallo(volledigeNaam, 30);
console.log(resultaat);
```

Bij het compileren zijn er geen errors meer.

## Conclusie

Alle JavaScript is geldige TypeScript. TypeScript voegt extra functionaliteit toe om het developers makkelijker te maken, vergissingen zoals het doorgeven van verkeerde types worden tijdens het compileren al opgemerkt. Aangezien browsers alleen met JavaScript kunnen werken, zal TypeScript dus gecompileerd/omgezet worden naar JavaScript.
