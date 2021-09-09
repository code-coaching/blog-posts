---
  title: JavaScript - Basis
  keywords: ["JavaScript - Basis"]
  modules: ["JavaScript - Basis", "Jaar 1"]
---

Het is essentieel om alle kennis uit `Leren Programmeren` te beheersen.

Bepaalde onderdelen waarop verder gebouwd zal worden, worden hieronder kort overlopen.

## Variabelen

- `var`: Sleutelwoord om een variabele aan te maken, verouderd.
- `let`: Sleutelwoord om een variabele aan te maken, deze kan op een later moment een nieuwe waarde toegekend krijgen.
- `const`: Sleutelwoord om een variabele aan te maken, de variabele moet meteen een waarde toegekend krijgen en kan daarna niet opnieuw een waarde toegekend krijgen.

## Datatypes

### Primitieve datatypes

- `string`
  - bv. `"Dit is een string."`, `'Dit is ook een string.'`
- `number`
  - bv. `69`, `3.141592653`
- `boolean`
  - bv. `true`, `false`
- `undefined`
  - Wanneer een variabele nog geen waarde toegekend heeft gekregen, is het type `undefined`.

### Complexe datatypes

- `object`
- `function`

## Function

```js
/* Functie aanmaken */
function zegHallo(naam) {
  // Codeblok van de functie tussen { en }
  console.log("Hallo " + naam + "!");
  console.log(`Hallo ${naam}!`);
}

/*
 * Anonieme functie toekennen aan een variabele.
 * Gebruik maken van 'arrow notation'.
 */
const zegDoei = (naam) => {
  console.log(`Doei ${naam}!`);
};

/* Functie aanroepen met een parameter */
zegHallo("Bart"); // Print 'Hallo Bart!'

/* Functie aanroepen met een parameter */
zegDoei("Bart"); // Print 'Doei Bart!'
```

## Conditionele statements

Afhankelijk of een bepaalde 'conditie' ofwel 'voorwaarde' voldaan is, wordt code uitgevoerd.

### if

```js
const voorwaarde = true;

if (voorwaarde) {
  // Dit codeblok wordt uitgevoerd
  console.log("De voorwaarde is true");
}
```

### if - else

```js
const voorwaarde = false;

if (voorwaarde) {
  // Dit codeblok wordt NIET uitgevoerd
  console.log("De voorwaarde is true");
} else {
  // Dit codeblok wordt uitgevoerd
  console.log("De voorwaarde is false");
}
```

### if - else if - else

```js
const voorwaarde = 300;

if (voorwaarde < 100) {
  // Dit codeblok wordt NIET uitgevoerd
  console.log("De voorwaarde is kleiner dan 100");
} else if (voorwaarde < 200) {
  // Dit codeblok wordt NIET uitgevoerd
  console.log("De voorwaarde is kleiner dan 200");
} else {
  // Dit codeblok wordt uitgevoerd
  console.log("De voorwaarde is gelijk of groter dan 200");
}
```

### switch

```js
const naam = "John Duck";

switch (naam) {
  case "Mark":
    // Dit codeblok wordt NIET uitgevoerd
    console.log("Hallo Mark!");
    break; // Stop met het uitvoeren van de switch
  case "Bart":
  case "Barry":
    // Dit codeblok wordt NIET uitgevoerd
    // Twee cases, beide cases voeren dit codeblok uit
    console.log("Hallo Bart of Barry!");
    break;
  case "John Duck":
    // Dit codeblok wordt uitgevoerd
    console.log("Hallo John Duck!");
    break;
  default:
    // Dit codeblok wordt NIET uitgevoerd
    console.log("Hallo vreemdeling!");
  // Geen break nodig, de switch stopt sowieso met uitvoeren op dit punt
}
```

## Iteratieve statements

### for

```js
for (let i = 0; i < 10; i++) {
  // Dit codeblok wordt 10x uitgevoerd
  // de eerste lus heeft i waarde 0
  // de tiende lus heeft i waarde 9
  console.log(`Dit is lus ${i}`);
}
```

### while

```js
let i = 0;
while (i < 10) {
  // Dit codeblok wordt 10x uitgevoerd
  // de eerste lus heeft i waarde 0
  // de tiende lus heeft i waarde 9
  console.log(`Dit is lus ${i}`);
  i++;
}
```

### do while

Het verschil tussen de `while` en `do while` is dat een `do while` **altijd één keer uitgevoerd** wordt en vervolgens wordt het statement bekeken om te kijken of het nogmaals uitgevoerd moetw orden.

```js
let i = 0;
do {
  // Dit codeblok wordt 10x uitgevoerd
  // de eerste lus heeft i waarde 0
  // de tiende lus heeft i waarde 9
  console.log(`Dit is lus ${i}`);
  i++;
} while (i < 10);
```

## Objecten

Bijna alles is een object in JavaScript. Het is dus belangrijk om objecten door en door te begrijpen.

```js
const persoon = {
  voornaam: "John",
  achternaam: "Duck",
  volledigeNaam: function () {
    return this.voornaam + " " + this.achternaam;
  },
};

const persoon2 = {
  voornaam: "Bart",
  achternaam: "Duisters",
  volledigeNaam: () => {
    return `${this.voornaam} ${this.achternaam}`;
  },
};

console.log(persoon.volledigeNaam()); // Dit print "John Duck"
console.log(persoon2.volledigeNaam()); // Dit print "undefined undefined" - BELANGRIJK!
```

Bovenstaande variabelen `persoon` en `persoon2` hebben beide een object toegekend gekregen. Beide objecten hebben drie properties (Nederlands: eigenschappen), namelijk `voornaam`, `achternaam` en `volledigeNaam`.

Er is geen verschil in de `return` statement van beide functies, ze geven beide een geconcateneerde (samengevoegde) string terug.

Er is **wel** een verschil in het toekennen van een anonieme functie `function () {}` en het toekennen van een anonieme arrow functie `() => {}`.
De arrow function **weet niet wat `this` is**. Er moet dus gebruikgemaakt worden van een normale anonieme functie om te zorgen dat `this` gekend is.

## this

```js
const persoon = {
  voornaam: "Bart",
  achternaam: "Duisters",
  volledigeNaam: function () {
    return `${this.voornaam} ${this.achternaam}`;
  },
};
```

In bovenstaand voorbeeld wordt er gebruikgemaakt van `this`. `this` verwijst naar `this object` (Nederlands: DIT object). `this` verwijst in bovenstaand voorbeeld naar de variabele `persoon`, er zou dus ook geschreven kunnen worden:

```js
const persoon = {
  voornaam: "Bart",
  achternaam: "Duisters",
  volledigeNaam: function () {
    return `${persoon.voornaam} ${persoon.achternaam}`;
  },
};
```

## class

Een class kan gezien worden als een template/sjabloon om een object aan te maken.

Neem onderstaand voorbeeld, waar er drie objecten worden aangemaakt.
Elk object stelt een persoon voor. Elk object heeft twee properties,
namelijk `voornaam` en `achternaam`. Elk object heeft één methode,
namelijk `volledigeNaam`.

Voor elke nieuwe persoon, moet elke property opnieuw getypt worden
en elke methode moet opnieuw volledig uitgetypt worden.

Stel dat elke persoon een nieuwe property krijgt, genaamd `bijnaam`.
Dan zal in elk object apart de nieuwe property toegevoegd moeten worden.

Indien er tien objecten gemaakt zijn die een persoon voorstellen, betekent
dit dat er tien keer een property toegevoegd moet worden.

```js
const persoon = {
  voornaam: "John",
  achternaam: "Duck",
  volledigeNaam: function () {
    return `${this.voornaam} ${this.achternaam}`;
  },
};

const persoon2 = {
  voornaam: "Bart",
  achternaam: "Duisters",
  volledigeNaam: function () {
    return `${this.voornaam} ${this.achternaam}`;
  },
};

const persoon3 = {
  voornaam: "Mark",
  achternaam: "Duisters",
  volledigeNaam: function () {
    return `${this.voornaam} ${this.achternaam}`;
  },
};
```

Om te zorgen dat er niet voor elke persoon opnieuw een object met alle properties aangemaakt moet worden. Kan er een `class` aangemaakt worden.

```js
class Persoon {
  voornaam = "Anoniempje";
  achternaam;

  volledigeNaam() {
    return `${this.voornaam} ${this.achternaam}`;
  }
}

const persoon = new Persoon();
persoon.voornaam = "John";
persoon.achternaam = "Duck";

const persoon2 = new Persoon();
persoon2.voornaam = "Bart";
persoon2.achternaam = "Duisters";

console.log(persoon.volledigeNaam());
console.log(persoon2.volledigeNaam());
```

Properties gedefinieerd binnen het codeblok van een `class`, worden gedefinieerd zonder `const` of `let`.
Functies (ook wel `methodes` genoemd) gedefinieerd binnen het codeblok van een `class`, worden gedefinieerd zonder het sleutelwoord `function`.

Wat afgeleid kan worden uit bovenstaande code, is dat `new Persoon()`, een object teruggeeft, met de properties `voornaam`, `achternaam` en de methode `volledigeNaam`.
Met het sleutelwoord `new` wordt er een nieuwe instantie gemaakt van de `class`, genaamd `Persoon`. Elke instantie van de `class` `Persoon` krijgt standaard een property `voornaam` met als standaardwaarde (Engels: default value) `'Anoniempje'`, een property `achternaam` zonder standaardwaarde (de waarde is dus `undefined`) en een methode `volledigeNaam()`.

Na het toekennen van een instantie van een `class` aan een variabele, kunnen de properties en methodes aangesproken worden alsof het een object is (omdat het achterliggend ook effectief een object is), met de puntnotatie. Dit wordt bijvoorbeeld gedaan om waarden toe te kennen aan `persoon.voornaam` en `persoon.achternaam`.

### constructor

Om te zorgen dat er niet altijd eerst een instantie aangemaakt moet worden om vervolgens de properties één voor één een waarde toe te kennen. Kan er een speciale methode gebruikt worden, de `constructor`-methode. Dit is een methode die aangeroepen wordt, op het moment dat een instantie van een object aangemaakt wordt.

```js
class Persoon {
  voornaam;
  achternaam;

  constructor() {
    /*
     * Code in dit codeblok wordt uitgevoerd op het moment
     * dat 'new Persoon()' wordt aangeroepen.
     */
    console.log("Ik word uitgevoerd");
  }

  volledigeNaam() {
    return `${this.voornaam} ${this.achternaam}`;
  }
}

const persoon = new Persoon(); // De constructor-methode wordt uitgevoerd
// 'Ik word uitgevoerd' wordt geprint

const persoon2 = new Persoon(); // De constructor-methode wordt uitgevoerd
// 'Ik word uitgevoerd wordt geprint
```

Aan deze speciale methode kunnen ook parameters meegegeven worden.

```js
class Persoon {
  voornaam;
  achternaam;

  constructor(parameter1, parameter2) {
    this.voornaam = parameter1;
    this.achternaam = parameter2;
  }

  volledigeNaam() {
    return `${this.voornaam} ${this.achternaam}`;
  }
}

const persoon = new Persoon("Bart", "Duisters");
console.log(persoon.volledigeNaam()); // Bart Duisters
```

### static

Soms is het interessant om geen instantie te moeten maken van een class en
toch de methodes aan te kunnen roepen. Hiervoor wordt gebruikgemaakt van een
voorbeeld met een class genaamd 'Utils' (van het Engels 'utilities', Nederlands 'gereedschap').

```js
let a = 3;
let b = 4;
let grootsteGetal;

if (a > b) {
  grootsteGetal = a;
} else {
  grootsteGetal = b;
}
console.log(`Het grootste getal is: ${a}`);

a = 4;
b = 5;

if (a > b) {
  grootsteGetal = a;
} else {
  grootsteGetal = b;
}
console.log(`Het grootste getal is: ${b}`);
```

Er wordt twee keer hetzelfde gedaan, dit kan dus in een functie gestoken worden.

```js
function grootsteGetal(a, b) {
  if (a > b) {
    return a;
  }
  return b;
}

let a = 3;
let b = 4;
let grootsteGetal;

console.log(`Het grootste getal is: ${grootsteGetal(a, b)}`);

a = 4;
b = 5;

console.log(`Het grootste getal is: ${grootsteGetal(a, b)}`);
```

Op dit moment is er maar één JavaScript-bestand. Maar stel dat er in een nieuw
bestand opnieuw logica moet zijn om te bepalen wat het grootste getal is,
dan zou er opnieuw dezelfde functie geschreven moeten worden.

In plaats van altijd code/logica te schrijven op het moment dat er gekend
moet zijn welk nummer groter is, kan dit in een class gestoken worden als method.
Omdat dit vaak gebruikt moet worden, is het interessant om dit als static method
toe te voegen. Dit zorgt ervoor dat er geen instantie van de class gemaakt
moet worden om gebruik te maken van de methode.

```js
// Gebruik even de verbeelding, Utils staat in een apart JavaScript-bestand.
class Utils {
  // Nieuw keyword: static
  // Dit maakt het mogelijk om de functie op te roepen via: Utils.grootsteGetal(a, b);
  static grootsteGetal(a, b) {
    if (a > b) {
      return a;
    }
    return b;
  }
}

// Opnieuw in de verbeelding, dit is het tweede JavaScript-bestand.
const a = 3,
  b = 5;
console.log(`Het grootste getal is: ${Utils.grootsteGetal(a, b)}`);

// Opnieuw in de verbeelding, dit is het derde JavaScript-bestand.
const x = 4,
  y = 6;
console.log(`Het grootste getal is: ${Utils.grootsteGetal(x, y)}`);
```

### extends & super

Stel dat er code is waarin verschillende classes voorzien zijn om verschillende
dieren aan te maken.

```js
class Leeuw {
  naam;
  leeftijd;

  constructor(naam, leeftijd) {
    this.naam = naam;
    this.leeftijd = leeftijd;
  }

  eet() {
    return `${this.naam} is aan het eten.`;
  }

  slaap() {
    return `${this.naam} is aan het slapen.`;
  }

  jagen() {
    return `${this.naam} is aan het jagen.`;
  }
}

class Hond {
  naam;
  leeftijd;
  roepnaam;

  constructor(naam, leeftijd, roepnaam) {
    this.naam = naam;
    this.leeftijd = leeftijd;
    this.roepnaam = roepnaam;
  }

  eet() {
    return `${this.naam} is aan het eten.`;
  }

  slaap() {
    return `${this.naam} is aan het slapen.`;
  }
}

class Olifant {
  naam;
  leeftijd;

  constructor(naam, leeftijd) {
    this.naam = naam;
    this.leeftijd = leeftijd;
  }

  eet() {
    return `${this.naam} is aan het eten.`;
  }

  slaap() {
    return `${this.naam} is aan het slapen.`;
  }

  eetVeel() {
    return `${this.eet()} En blijft eten.`;
  }
}

const leeuw = new Leeuw("Leeuwtje", 13);
const hond = new Hond("Samson", 8);
const olifant = new Olifant("Dumbo", 9);

console.log(leeuw.eet()); // Leeuwtje is aan het eten.
console.log(leeuw.jagen()); // Leeuwtje is aan het jagen.
console.log(leeuw.slaap()); // Leeuwtje is aan het slapen.

console.log(hond.eet()); // Samson is aan het eten.
console.log(hond.slaap()); // Samson is aan het slapen.

console.log(olifant.eet()); // Dumbo is aan het eten.
console.log(olifant.eetVeel()); // Dumbo is aan het eten. En blijft eten.
console.log(olifant.slaap()); // Dumbo is aan het slapen.
```

Elke class heeft twee properties die gelijkend zijn: `naam` en `leeftijd`.

Elke class heeft twee functies die gelijkend zijn: `eet()` en `slaap()`.

Als developer is het niet leuk om voor elke class opnieuw dezelfde property te moeten typen
en om opnieuw dezelfde functie te moeten typen.

Er is iets voorzien waardoor het maar één keer getypt moet worden, het keyword `extends`
en het gebruik van `super`.

Nieuwe termen: `super class` en `sub class`.

Een class die van een andere class `extends`, is een sub class (ook wel `child` genoemd).
De class waarvan `extends` wordt, is een super class (ook wel `parent` genoemd).

In het Nederlands kan `extends` verwoord worden door `overerving`.

Onderstaande code doet exact hetzelfde:

```js
class Dier {
  naam;
  leeftijd;

  constructor(naam, leeftijd) {
    this.naam = naam;
    this.leeftijd = leeftijd;
  }

  eet() {
    return `${this.naam} is aan het eten.`;
  }

  slaap() {
    return `${this.naam} is aan het slapen.`;
  }
}

class Leeuw extends Dier {
  constructor(naam, leeftijd) {
    super(naam, leeftijd);
  }

  jagen() {
    return `${this.naam} is aan het jagen.`;
  }
}

class Hond extends Dier {
  roepnaam;

  constructor(naam, leeftijd, roepnaam) {
    super(naam, leeftijd);
    this.roepnaam = roepnaam;
  }
}

class Olifant extends Dier {
  constructor(naam, leeftijd) {
    super(naam, leeftijd);
  }

  eetVeel() {
    return `${super.eet()} En blijft eten.`;
  }
}

const leeuw = new Leeuw("Leeuwtje", 13);
const hond = new Hond("Samson", 8);
const olifant = new Olifant("Dumbo", 9);

console.log(leeuw.eet()); // Leeuwtje is aan het eten.
console.log(leeuw.jagen()); // Leeuwtje is aan het jagen.
console.log(leeuw.slaap()); // Leeuwtje is aan het slapen.

console.log(hond.eet()); // Samson is aan het eten.
console.log(hond.slaap()); // Samson is aan het slapen.

console.log(olifant.eet()); // Dumbo is aan het eten.
console.log(olifant.eetVeel()); // Dumbo is aan het eten. En blijft eten.
console.log(olifant.slaap()); // Dumbo is aan het slapen.
```

### Access modifiers

In andere programmeertalen is het mogelijk om gebruik te maken van `access modifiers` (Nederlands: toegangsmodifier). Met andere woorden, het is mogelijk om de toegang tot bepaalde properties en methoden te beperken.

Meestal is er een keyword `public` en een keyword `private` dat gebruikt wordt om te bepalen of een property/methode
overal (public) of enkel binnen een instantie van de class (private) beschikbaar is.

Stel dat deze keywords zouden bestaan in JavaScript, dan zou het er zo uitzien:

```js
// GEEN GELDIGE JAVASCRIPT CODE
class Persoon {
  private pinCode;
  private wachtwoord;
  public naam;
  public leeftijd;

  constructor(pin, wachtwoord, naam, leeftijd) {
    this.pinCode = pin;
    this.wachtwoord = wachtwoord;
    this.naam = naam;
    this.leeftijd = leeftijd;
  }

  private getPin() {
    return this.pinCode;
  }

  public getPinCode() {
    return this.pinCode;
  }

  public getWachtwoord()
}

const persoon = new Persoon(1111, 1234, 'John Duck', 29);
console.log(persoon.naam); // "John Duck"
console.log(persoon.leeftijd); // 29
console.log(persoon.pinCode); // Dit zou niks printen, pinCode is private
console.log(persoon.wachtwoord); // Dit zou niks printen, wachtwoord is private

console.log(persoon.getPin()); // Dit zou niks printen, getPin() is private
console.log(persoon.getPinCode()); // 1111
/* 
* De property pinCode is private, en dus enkel beschikbaar binnen het codeblok van de class Persoon.
* De methode getPinCode() is public, deze kan dus aangeroepen worden op de instantie.
* Omdat getPinCode() binnen het codeblok van de class staat, kan deze aan de private properties/methodes.
*/
```

In JavaScript bestaan de keywords `public` en `private` niet. De code zou er dus als volgt uitzien:

```js
class Persoon {
  pinCode;
  wachtwoord;
  naam;
  leeftijd;

  constructor(pin, wachtwoord, naam, leeftijd) {
    this.pinCode = pin;
    this.wachtwoord = wachtwoord;
    this.naam = naam;
    this.leeftijd = leeftijd;
  }

  getPin() {
    return this.pinCode;
  }

  getPinCode() {
    return this.pinCode;
  }
}

const persoon = new Persoon(1111, 1234, 'John Duck', 29);
console.log(persoon.naam); // "John Duck"
console.log(persoon.leeftijd); // 29
console.log(persoon.pinCode); // 1111
console.log(persoon.wachtwoord); // 1234

console.log(persoon.getPin()); // 1111
console.log(persoon.getPinCode()); // 1111
```

Alles is standaard `public`, alle properties en methodes kunnen aangeroepen worden op de instantie van een class.

Er is echter een conventie die gebruikt wordt bij developers om toch aan te geven dat iets eigenlijk `private` is.
Door middel van een `_` (underscore) toe te voegen vooraan de naam van de property/methode, wordt aangegeven dat
een property/methode eigenlijk `private` is.

```js
class Persoon {
  _pinCode;
  _wachtwoord;
  naam;
  leeftijd;

  constructor(pin, wachtwoord, naam, leeftijd) {
    this._pinCode = pin;
    this._wachtwoord = wachtwoord;
    this.naam = naam;
    this.leeftijd = leeftijd;
  }

  _getPin() {
    return this._pinCode;
  }

  getPinCode() {
    return this._pinCode;
  }
}

const persoon = new Persoon(1111, 1234, 'John Duck', 29);
console.log(persoon.naam); // "John Duck"
console.log(persoon.leeftijd); // 29
console.log(persoon._pinCode); // 1111
console.log(persoon._wachtwoord); // 1234

console.log(persoon._getPin()); // 1111
console.log(persoon.getPinCode()); // 1111
```

De properties/methodes met een underscore voor zijn even goed public, dus deze kunnen aangeroepen worden.
Maar, als developer weet je dat dit eigenlijk niet de bedoeling is. Op deze manier is het toch mogelijk
om een soort van `access modifiers` te hebben binnen JavaScript.

### get & set

Het keyword `get` en het keyword `set` kunnen gebruikt worden om `getters` en `setters` te maken.
Dit zijn methodes die net iets anders werken.

Stel dat er een class is Cursist:
```js
class Cursist {
  _voornaam;
  _achternaam;

  constructor(voornaam, achternaam) {
    this._voornaam = voornaam;
    this._achternaam = achternaam;
  }

  getVoornaam() {
    return this._voornaam;
  }

  setVoornaam(voornaam) {
    this._voornaam = voornaam;
  }

  getAchternaam() {
    return this._achternaam;
  }

  setAchternaam(achternaam) {
    this._achternaam = achternaam;
  }

  getNaam() {
    if (this._voornaam && this._achternaam) {
      return `${this._voornaam} ${this._achternaam}`;
    } else if (this._voornaam) {
      return this._voornaam;
    } else {
      return this._achternaam;
    }
  }
}

const cursist = new Cursist("John", "Duck");

console.log(cursist.getVoornaam()); // "John"
console.log(cursist.getAchternaam()); // "Duck"
cursist.setVoornaam("Jane");
console.log(cursist.getNaam()); // "Jane Duck"
```

Dit zou werken en het is perfect geldige JavaScript. Maar het zou fijn zijn als de
voornaam, achternaam en volledig naam opgevraagd kunnen worden als property.

Hiervoor kan er gebruikgemaakt worden van `get` en `set`.

```js
class Cursist {
  _voornaam;
  _achternaam;

  constructor(voornaam, achternaam) {
    this._voornaam = voornaam;
    this._achternaam = achternaam;
  }

  get voornaam() {
    return this._voornaam;
  }

  set voornaam(voornaam) {
    this._voornaam = voornaam;
  }

  get achternaam() {
    return this._achternaam;
  }

  set achternaam(achternaam) {
    this.achternaam = achternaam;
  }

  get naam() {
    if (this._voornaam && this._achternaam) {
      return `${this._voornaam} ${this._achternaam}`;
    } else if (this._voornaam) {
      return this._voornaam;
    } else {
      return this._achternaam;
    }
  }
}

const cursist = new Cursist("John", "Duck");

console.log(cursist.voornaam); // "John"
console.log(cursist.achternaam); // "Duck"
cursist.voornaam = "Jane";
console.log(cursist.naam); // "Jane Duck"
```

Het voordeel van het gebruiken van `getters` en `setters`, is dat het gebruikt kan worden
alsof het properties zijn. Maar in feite zijn het methodes, waardoor er dus extra logica
uitgevoerd kan worden. Een voorbeeld hier van is `get naam()`. Op de instantie wordt
het opgeroepen alsof het een property is `cursist.naam`, maar achterliggend wordt
het codeblok van de `getter methode` uitgevoerd.

## Ingebouwde objecten

Er zijn veel verschillende ingebouwde objecten. Deze objecten maken achterliggend gebruik van een class.

Het is niet nodig om letterlijk elke functie en elke property van buiten te kennen.
Om te weten wat er mogelijk is met een object dat globaal aanwezig is binnen JavaScript, kan er documentatie
geraadpleegd worden.

Een voorbeeld is de Array class. De [documentatie](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) kan teruggevonden worden op een website onderhouden door Mozilla
(de makers van Firefox en Thunderbird).
