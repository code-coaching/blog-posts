---
postUuid: 654d92e4-7aff-482d-b2c1-448da1479137
title: Angular Tour of Heroes - Componenten
slug: angular-toh-componenten
tags:
  - Angular
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De beginsituatie is een bestaand Angular-project met een layoutcomponent en placeholderpagina's. De eindsituatie is een Angular-project waarin de pagina's voorzien zijn van componenten.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-styling.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-layouts-and-pages)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-components.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-components)

## Componenten

Componenten zijn essentiële bouwstenen van een Angular-applicatie. In de applicatie is al een component te zien voor de layout, genaamd `main-layout.component.ts`. Er zijn ook al componenten voor de verschillende pagina's.

De verschillende pagina's zijn:

- dashboard.component.ts - dit is een pagina waarop de `top heroes` getoond zullen worden.
- hero-list.component.ts - dit is een pagina waarop de lijst met `heroes` getoond zullen worden.
- hero-detail.component.ts - dit is een pagina waarop de details van een `hero` getoond zullen worden.

Een layout is een component en bevat HTML-elementen en andere componenten die op alle pagina's aanwezig zijn.

Een pagina is een component en bevat HTML-elementen en andere componenten die op een specifieke pagina aanwezig zijn.

Tijdens dit artikel zullen er nieuwe componenten worden toegevoegd aan de applicatie.

## Pagina stylen: dashboard

![dashboard](/img/blog/quasar-toh-dashboard.png)

Wijzig de inhoud van `src/app/pages/dashboard/dashboard.component.html`:

```html
<div>Top Heroes</div>

<div>Narco</div>
<div>Bombasto</div>
<div>Celeritas</div>
<div>Magneta</div>
```

![Dashboard elementen](/img/blog/quasar-toh-dashboard-elements.png)

De elementen zijn aanwezig, maar de styling komt niet overeen.

Voeg classes toe aan de elmenten om deze te kunnen stylen.

`src/app/pages/dashboard/dashboard.component.html`

```html
<div class="title">Top Heroes</div>

<div class="top-heroes">
  <div class="top-hero">Narco</div>
  <div class="top-hero">Bombasto</div>
  <div class="top-hero">Celeritas</div>
  <div class="top-hero">Magneta</div>
</div>
```

`src/app/pages/dashboard/dashboard.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}

.top-heroes {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 2rem;
}

.top-hero {
  display: flex;
  justify-content: center;
  align-items: center;

  background-color: #5f7d8c;
  color: white;

  height: 8rem;
  width: 10rem;
  font-weight: 600;
  font-size: 1.1rem;
}

.top-hero:hover {
  background-color: #eeeeee;
  color: #5f7d8c;
  cursor: pointer;
}
```

Merk op dat er een `container element` is toegevoegd rondom de `hero`-elementen.

![Tour of Heroes Dashboard Styling](/img/blog/quasar-toh-dashboard-styling.png)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/bca6449ed4c410206717b3b3fc46a19a3cabad1f..abac7025047da6531ec4b636d04b3c5f616a164e)

## Pagina stylen: hero-list

![HeroList](/img/blog/quasar-toh-hero-list.png)

Wijzig de inhoud van `src/app/pages/hero-list/hero-list.component.html`:

```html
<div>My Heroes</div>

<div>11 Mr. Nice</div>
<div>12 Narco</div>
<div>13 Bombasto</div>
<div>14 Celeritas</div>
<div>15 Magneta</div>
<div>16 RubberMan</div>
<div>17 Dynama</div>
<div>18 Dr IQ</div>
<div>19 Magma</div>
<div>20 Tornado</div>
```

![hero list elementen](/img/blog/quasar-toh-hero-list-elements.png)

Style de elementen, voeg waar nodig extra elementen toe om het gewenste resultaat te bekomen.

`src/app/pages/hero-list/hero-list.component.html`

```html
<div class="title">My Heroes</div>

<div class="hero-list">
  <div class="hero">
    <span class="hero-number">11</span>
    <span class="hero-name">Mr. Nice</span>
  </div>
  <div class="hero">
    <span class="hero-number">12</span> <span class="hero-name">Narco</span>
  </div>
  <div class="hero">
    <span class="hero-number">13</span>
    <span class="hero-name">Bombasto</span>
  </div>
  <div class="hero">
    <span class="hero-number">14</span>
    <span class="hero-name">Celeritas</span>
  </div>
  <div class="hero">
    <span class="hero-number">15</span> <span class="hero-name">Magneta</span>
  </div>
  <div class="hero">
    <span class="hero-number">16</span>
    <span class="hero-name">RubberMan</span>
  </div>
  <div class="hero">
    <span class="hero-number">17</span> <span class="hero-name">Dynama</span>
  </div>
  <div class="hero">
    <span class="hero-number">18</span> <span class="hero-name">Dr IQ</span>
  </div>
  <div class="hero">
    <span class="hero-number">19</span> <span class="hero-name">Magma</span>
  </div>
  <div class="hero">
    <span class="hero-number">20</span> <span class="hero-name">Tornado</span>
  </div>
</div>
```

`src/app/pages/hero-list/hero-list.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}

.hero-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.hero {
  display: flex;
  max-width: 10rem;
  background-color: #e6e6e6;
  cursor: pointer;
  color: #8d8d8d;
  border-radius: 0.5rem;
}
.hero:hover {
  background-color: #cfd8dc;
  color: white;
  margin-left: 0.25rem;
}

.hero-number {
  display: flex;
  justify-content: center;
  align-items: center;

  background-color: #5f7d8c;
  color: white;
  border-top-left-radius: 0.5rem;
  border-bottom-left-radius: 0.5rem;
  padding: 0.5rem;
  font-size: 0.75rem;
  font-weight: 600;
}
.hero-name {
  padding: 0.5rem;
  padding-left: 0.75rem;
  font-weight: 600;
}
```

![Tour of Heroes styled hero list](/img/blog/quasar-toh-hero-list-styled.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/abac7025047da6531ec4b636d04b3c5f616a164e..1eff5b79688c71a3da5c382301b69d40d3ee5491)

## Pagina uitbreiden: HeroList

Om te zorgen dat een hero geselecteerd kan worden, moet er een `click handler` toegevoegd worden aan elke hero. Aangezien met de huidige code, er tien aparte elementen zijn die een hero voorstellen, zouden er tien `click handlers` nodig zijn.

Om dit te vermijden, kan er gebruik gemaakt worden van een `@for`-loop in de `template`, gecombineerd met een lijst van heroes.

`src/app/pages/hero-list/hero-list.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  // Voeg hier een lijst van heroes toe
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];
}
```

`src/app/pages/hero-list/hero-list.component.html`

```html
<div class="title">My Heroes</div>

<div class="hero-list">
  @for (hero of heroes; track hero.number) {
  <div class="hero">
    <span class="hero-number">{{ hero.number }}</span>
    <span class="hero-name">{{ hero.name }}</span>
  </div>
  }
</div>
```

Hier wordt gebruikgemaakt van een `@for`-loop. Dit is onderdeel van de Angular `Control Flow`. Om te zorgen dat `change detection` optimaal werkt, moet er een `track` gebruikt worden. Dit zorgt ervoor dat elk element uniek geïdentificeerd wordt. Hier wordt gekozen voor het unieke nummer van de horo.

In de `class` wordt de `heroes`-lijst gedefinieerd, deze array bevat objecten met de properties `number` en `name`. Elk object stelt een hero voor.

Properties van de class zijn beschikbaar in de template/html.

### Een hero selecteren

![Klik hero in lijst](/img/blog/quasar-toh-click-hero-in-list.png)

Om te zorgen dat een hero geselecteerd kan worden, zijn er enkele dingen nodig:

- een `click handler` op elk hero element, dit is een functie die wordt aangeroepen als er op een hero wordt geklikt
- een `selectedHero`-variabele die de geselecteerde hero zal bevatten

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];

  // De property die de geselecteerde hero zal bevatten
  selectedHero: null | { number: number; name: string } = null;

  // De functie die gekoppeld wordt aan het click event
  onClickHero(hero: { number: number; name: string }) {
    this.selectedHero = hero;
  }
}
```

```html
<div class="title">My Heroes</div>

<div class="hero-list">
  @for (hero of heroes; track hero.number) {
  <div class="hero" (click)="onClickHero(hero)">
    <span class="hero-number">{{ hero.number }}</span>
    <span class="hero-name">{{ hero.name }}</span>
  </div>
  }
</div>

<div>{{ selectedHero | json }}</div>
```

De `click handler` wordt toegvoegd door middel van de `(click)`-directive. Deze directive wordt gebruikt om een functie aan te roepen als er op een element wordt geklikt. De functie die wordt aangeroepen wordt gedefinieerd in de `class` van deze component.

De functie die wordt aangeroepen wordt gedefinieerd als `onClickHero`, met als argument een hero. Deze functie wordt aangeroepen als er op een hero wordt geklikt.

In de functie wordt aan de `selectedHero`-variabele een nieuwe waarde toegekend. Deze variabele wordt gebruikt in de template om de geselecteerde hero te tonen `{{ selectedHero | json }}`, hier wordt gebruik gemaakt van een `Angular Pipe`, specifiek de `json` pipe (`| json`). Een pipe wordt gebruikt om de data om te vormen naar een ander formaat. In dit geval wordt de data omgevormd naar een JSON-string.

Merk op dat de `selectedHero` automatisch geüpdatet wordt in de browser bij het selecteren van een hero. De properties van een Angular `class` zijn `reactive`, ze zullen automatisch geüpdatet worden in de browser als de waarde wijzigt.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/1eff5b79688c71a3da5c382301b69d40d3ee5491..bfa7223c061a3284ff2a2e28ef23e564fa7f6bbb)

![selectedHero-variabele in HeroList](/img/blog/quasar-toh-selected-hero-ref.png)

Een kleine refactoring van de code om een `interface` toe te voegen.

`src/app/pages/hero-list/hero-list.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";

// Een interface, dit is een TypeScript-functionaliteit om een complex type te definiëren
interface Hero {
  number: number;
  name: string;
}

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];

  selectedHero: null | Hero = null; // Gebruik de interface

  // Gebruik de interface
  onClickHero(hero: Hero) {
    this.selectedHero = hero;
  }
}
```

De interface is toegevoegd om het complexe type `Hero` voor te stellen. Deze interface is niet nodig, maar het is een goede manier om de code leesbaarder te maken.

Voor het toevoegen van de Hero-interface:

```ts
// imports

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  // hier nog meer code

  selectedHero: null | { number: number; name: string } = null;

  onClickHero(hero: { number: number; name: string }) {
    this.selectedHero = hero;
  }
}
```

Na het toevoegen van de Hero-interface:

```ts
// imports

interface Hero {
  number: number;
  name: string;
}

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  // hier nog meer code

  selectedHero: null | Hero = null;

  onClickHero(hero: Hero) {
    this.selectedHero = hero;
  }
}
```

Het voordeel van deze TypeScript interface, is dat overal waar een Hero-object wordt gebruikt, kan de interface gebruikt worden om het type te definiëren.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/bfa7223c061a3284ff2a2e28ef23e564fa7f6bbb..7b4f79e7430e833aebae1e39838afcede2163d95)

### Stylen van selectedHero

```html
<div class="title">My Heroes</div>

<div class="hero-list">
  @for (hero of heroes; track hero.number) {
  <div class="hero" (click)="onClickHero(hero)">
    <span class="hero-number">{{ hero.number }}</span>
    <span class="hero-name">{{ hero.name }}</span>
  </div>
  }
</div>

@if (selectedHero) {
<div class="title">{{ selectedHero.name }} is my hero</div>
<button>Details</button>
}
```

Merk op dat de `@if`-statement gebruikt wordt. Dit zorgt ervoor dat de elementen enkel getoond worden als de variabele `selectedHero` `truthy` is. Of in andere woorden, als de variabele een waarde heeft, wordt het element getoond. Zonder de `@if`-statement zou de applicatie een error tonen, aangezien de `.name`-property van de variabele `selectedHero` niet bestaat wanneer `selectedHero` nog geen waarde heeft.

De elementen zijn aanwezig, de styling is nog niet in orde.

![elementen geselecteerde hero](/img/blog/quasar-toh-selected-hero-elements.png)

Nu de geselecteerde hero gekend is, moet de styling van de gekozen hero gewijzigd worden. De naam van de geselecteerde hero moet getoond worden in allemaal hoofdletters. De knop moet dezelfde styling krijgen als de al bestaande knoppen.

#### Styling van active hero

Toevoegen van een dynamische class:

```html
<div
  :class="{
    'hero--active': hero.number === selectedHero?.number,
  }"
></div>
```

Merk op dat er een `v-bind`-directive gebruikt wordt om variabelen te kunnen gebruiken in het toekennen van de class `v-bind:class` of de korte syntax `:class`. Dit kan gelezen worden als "de class `hero--active` wordt toegevoegd aan dit element als de statement `truthy` is.

De statement `hero.number === selectedHero?.number` evalueert naar `true` als de het nummer van de hero waarover geïtereerd wordt gelijk is aan het nummer van de geselecteerde hero.

Merk op de `?.`-operator. Deze operator zorgt ervoor dat `.number` enkel gelezen wordt als `selectedHero` een waarde heeft. Indien `selectedHero` `undefined` is, wordt `.number` niet gelezen en evalueert de statement `hero.number === selectedHero?.number` naar `false`.

De volledige template:

```html
<div class="title">My Heroes</div>

<div class="hero-list">
  @for (hero of heroes; track hero.number) {
  <div
    class="hero"
    [ngClass]="{ 'hero--active': hero.number === selectedHero?.number }"
    (click)="onClickHero(hero)"
  >
    <span class="hero-number">{{ hero.number }}</span>
    <span class="hero-name">{{ hero.name }}</span>
  </div>
  }
</div>

@if (selectedHero) {
<div class="title">{{ selectedHero.name }} is my hero</div>
<button>Details</button>
}
```

Voeg de CSS class `.hero--active` toe aan de al bestaande `src/app/pages/hero-list/hero-list.component.css`:

```css
.hero--active {
  background-color: #cfd8dc;
  color: white;
  margin-left: 0.25rem;
}
```

Wanneer er nu een hero geselecteerd is, wordt de class `hero--active` toegevoegd aan het element.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/7b4f79e7430e833aebae1e39838afcede2163d95..7f2ecfaa5fd80b51791531982c58df7cbff0e4ec)

![Styling van geselecteerde hero](/img/blog/quasar-toh-selected-hero-styling.png)

#### Naam hero in hoofdletters

Een manier om de naam in hoofdletters te tonen is om de naam in een element te plaatsen en vervolgens via css de naam in hoofdletters te plaatsen. In deze tutorial wordt er echter voor gekozen om de naam te wijzigen met behulp van een Angular Pipe.

`src/app/pages/hero-list/hero-list.component.html`

```html
<div class="title">{{ selectedHero.name | uppercase }} is my hero</div>
```

De pipe `uppercase` bestaat in het Angular framework. Dit zal de waarde van `selectedHero.name` omvormen naar hoofdletters.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/7f2ecfaa5fd80b51791531982c58df7cbff0e4ec..47ecd1c58e69e761aaf79a4102f5a599a9b21817)

#### Toevoegen details-knop

Aangezien de knop hetzelfde gestyled moet worden als de reeds bestaande knoppen in `main-layout.component.css`, kan de styling daar gekopieerd worden en vervolgens geplakt worden in de `src/app/pages/hero-list/hero-list.component.css`.

```css
button {
  background-color: #eeeeee;
  border-radius: 0.25rem;
  font-weight: 500;
  border: none;
  padding: 0.25rem 0.5rem;
  color: #567868;
}

button:hover {
  background-color: #e6e6e6;
  color: #0096e8;
  cursor: pointer;
}
```

Dit is een snelle oplossing, maar het zorgt echter voor een probleem. Wat als de `background-color` niet langer `#eeeeee` moet zijn, maar `deeppink`? Dan moet dit zowel in de `hero-list.component.css` als in de `main-layout.component.css` gewijzigd worden.

Om dit te voorkomen, kan er een aparte component gemaakt worden waarin de knop én de styling wordt geplaatst.

```sh
ng g c components/styled-button --skip-tests
```

`src/app/components/styled-button/styled-button.html`

```html
<button>Details</button>
```

`src/app/components/styled-button/styled-button.css`

```css
button {
  background-color: #eeeeee;
  border-radius: 0.25rem;
  font-weight: 500;
  border: none;
  padding: 0.25rem 0.5rem;
  color: #567868;
}

button:hover {
  background-color: #e6e6e6;
  color: #0096e8;
  cursor: pointer;
}
```

De custom component kan geïmporteerd worden in de `hero-list.component.ts`:

```ts
// imports
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component"; // importeer de component

interface Hero {
  number: number;
  name: string;
}

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent], // registreer de component
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  // hier nog meer code
}
```

`hero-list.component.html`

```html
@if (selectedHero) {
<div class="title">{{ selectedHero.name | uppercase }} is my hero</div>
<!-- gebruik de component -->
<app-styled-button></app-styled-button>
}
```

De reden dat we `<app-styled-button></app-styled-button>` moeten gebruiken in de html, komt door de `selector` van de component.

`src/app/components/styled-button/styled-button.component.ts`

```ts
import { Component } from "@angular/core";
import { CommonModule } from "@angular/common";

@Component({
  selector: "app-styled-button", // door deze regel
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./styled-button.component.html",
  styleUrl: "./styled-button.component.css",
})
export class StyledButtonComponent {}
```

Het probleem met de huidige `app-styled-button`, is dat het hardcoded de tekst `Details` bevat. Maar in de `main-layout.component.html` moet er `Dashboard` en `Heroes` worden weergegeven.

Een normaal `button`-element in html toont de tekst tussen een begin- en end-tag. Om dit te bekomen in de custom component, kan er gebruik gemaakt worden van `content projection`. Dit doen we door een `ng-content`-element toe te voegen aan de template van de custom component.

`components/styled-button/styled-button.component.html`

```html
<button>
  <ng-content></ng-content>
</button>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/47ecd1c58e69e761aaf79a4102f5a599a9b21817..5ada2b7babbfab93bfb4009f32e421e7de073c34)

Bij de wijziging is te zien `background-color: darken(#eeeeee, 10%);`, dit moet eigenlijk `background-color: #e6e6e6` zijn. In de tekst van deze blogpost is het aangepast.

Het is nu mogelijk om de knop ook te gebruiken in `main-layout.component.html`. De styling van de `button` kan uit `main-layout.component.css` verwijdert worden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/5ada2b7babbfab93bfb4009f32e421e7de073c34..8604b5a2d0659cc948241b69fef2898f796d671a)

Wanneer op de `Details`-knop geklikt wordt, moet er genavigeerd worden naar de route `/heroes/:id`. Waar :id de `number`-property van de geselecteerde hero is.

Om te kunnen navigeren, moeten we eerst zorgen dat er een `click` event wordt uitgestuurd als er op de knop wordt geklikt.

`styled-button.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component, EventEmitter, Output } from "@angular/core";

@Component({
  selector: "app-styled-button",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./styled-button.component.html",
  styleUrl: "./styled-button.component.css",
})
export class StyledButtonComponent {
  @Output() click = new EventEmitter<void>(); // voeg een Output toe
}
```

`styled-button.component.html`

```html
<button (click)="click.emit()">
  <ng-content></ng-content>
</button>
```

De `click` property is een `EventEmitter`. Dit is een Angular functionaliteit om events uit te sturen. De `click` property is een `Output`, dit wil zeggen dat de property gebruikt kan worden in de template.

`hero-list.component.html`:

```html
<app-styled-button (click)="router.navigate(['heroes', selectedHero.number])">
  Details
</app-styled-button>
```

Wanneer het `click` event wordt uitgestuurd, dan wordt er genavigeerd naar de route `/heroes/:id`, waarbij `:id` gelijk is aan de `number`-property van de geselecteerde hero.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/8604b5a2d0659cc948241b69fef2898f796d671a..95a7de527c13842d2faf9a5094988cfd9e52d2b8)

## Pagina stylen: hero-details

Voorzie een property om de hero in op te slaan. Importeer de `StyledButton`-component zodat deze gebruikt kan worden in de template.

`src/app/pages/hero-details/hero-details.component.ts`

```ts
import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';
import { StyledButtonComponent } from '../../components/styled-button/styled-button.component';

interface Hero {
  number: number;
  name: string;
}

@Component({
  selector: 'app-hero-details',
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: './hero-details.component.html',
  styleUrl: './hero-details.component.css',
})
export class HeroDetailsComponent {
  hero: null | Hero = { number: 15, name: 'Magneta' };
}
```

Aangezien het opnieuw een complex object is, voorzien we een interface.

Voorzie elementen in de template om de details van een hero te tonen.

`src/app/pages/hero-details/hero-details.component.html`

```html
@if(hero) {
<div class="title">{{ hero.name }} details!</div>

<div>id: {{ hero.number }}</div>
<div>name: <input [value]="hero.name" /></div>

<app-styled-button>Back</app-styled-button>
}
```

Via `Property Binding` wordt de waarde van `hero.name` toegekend aan de `value` van de `input`-tag.

Als er nu een hero geselecteerd wordt, of wanneer er manueel naar `/heroes/:id` genavigeerd wordt (bv. `/heroes/15`), dan wordt de `hero-details`-pagina getoond. Maar er is niks te zien in de browser. Om de elementen te kunnen stylen, kan er tijdelijk een hero hard-coded toegevoegd worden.

```ts
// hier nog meer code
export class HeroDetailsComponent {
  hero: null | Hero = { number: 15, name: "Magneta" };
}
```

Nu de hero zichtbaar is, kan de styling toegevoegd worden.

`src/app/pages/hero-details/hero-details.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/b9495d03e334e34c8064e5c06f3f2b817640b2b5..db39047cb01a32ed2ffaa2c92a82fa4aef3be9ce)

![Hero Details - setup](/img/blog/quasar-toh-hero-details-initial.png)

Voor er verdergegaan wordt met het stylen van de pagina, moet er gekeken worden naar hetgeen er gedupliceerd is op deze pagina ten opzichte van de pagina `hero-list`. Gedupliceerde dingen zijn kandidaat om apart gezet te worden zodat het herbruikbaar is.

### Refactor Hero Interface

De `Hero`-interface is gekopieerd en geplakt vanuit de `hero-list`-pagina.

```ts
interface Hero {
  number: number;
  name: string;
}
```

Stel dat er een extra property bijkomt in de `Hero`-interface, dan zou dit gewijzigd moeten worden in zowel de `hero-list`-pagina als de `hero-details`-pagina. Dit lijkt misschien niet erg, maar denk eens na wat er gedaan moet worden als deze interface in tien verschillende componenten gebruikt wordt.

Dit wordt opgelost door de interface in `src/app/components/models.ts` te plaatsen. Vanuit dit bestand wordt de interface geëxporteerd en vervolgens wordt de interface geïmporteerd in alle bestanden waar deze interface gebruikt wordt.

`src/app/components/models.ts`:

```ts
export interface Hero {
  number: number;
  name: string;
}
```

Wijzig de bestaande `Hero`-interface in de twee plaatsen waar het gebruikt wordt in de componenten, naar een import die verwijst naar de interface in `src/app/components/models.ts`.

```ts
import { Hero } from "../../components/models";
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/db39047cb01a32ed2ffaa2c92a82fa4aef3be9ce..696ffbd5f74f779a44a683cd3e2fd9d835e9d1ad)

### Refactor .title class

Ook de class `.title` wordt ondertussen meerdere keren gebruikt.

`src/app/layouts/main-layout/main-layout.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
}
```

`src/app/pages/dashboard/dashboard.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}
```

`src/app/pages/hero-details/hero-details.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

`src/app/pages/hero-list/hero-list.component.css`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

De gemeenschappelijke properties van deze class kunnen verplaatst worden naar de globale stylesheet `src/app/app.component.css`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/696ffbd5f74f779a44a683cd3e2fd9d835e9d1ad..5aaf76a62010e097569f52baf9da6eed51df40cc)

Merk op: Dit werkt **niet**. Angular werkt met `View Encapsulation`, dit wil zeggen dat de stylesheets van de componenten enkel van toepassing zijn op de component zelf, niet op de componenten die in de template van deze component gebruikt worden. Als er eerdere ervaring is met andere frameworks, zoals bijvoorbeeld Vue, dan ben je waarschijnlijk gewoon dat de css van een component van toepassing is op de component zelf én op de componenten die in de template van deze component gebruikt worden.

Om dit op te lossen, moet er gebruik gemaakt worden van `src/styles.css`, deze css is globaal en wordt toegepast op alle componenten.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/bef8f9370dd4df8aad8fe9a6e005ccbc1666bf49)

### Heroes - onBeforeMount - url parameter useRoute

Om te zorgen dat de correcte hero getoond kan worden, gaat er gekeken worden naar de url parameter genaamd `id`. Vervolgens zal de hero gefilterd worden uit de lijst van heroes, die in dit artikel gekopieerd zal worden vanuit de `hero-list`-pagina, in een later artikel zal de lijst van heroes verplaatst worden naar een `service` zodat er maar één unieke lijst van heroes bestaat.

Allereerst wordt de `heroes`-lijst gekopieerd en geplakt vanuit de `hero-list`-pagina.

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { Hero } from "../../components/models";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent {
  hero: null | Hero = { number: 15, name: "Magneta" };
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];
}
```

Om aan de routeparameter te kunnen, moet de `ActivedRoute`-service geïnjecteerd worden. Dit is een service die Angular voorziet om gegevens van de huidige route op te halen.

`src/app/pages/hero-details/hero-details.component.ts`

```ts
// nog imports
import { ActivatedRoute } from "@angular/router";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent {
  hero: null | Hero = { number: 15, name: "Magneta" };
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];

  // injecteer de service
  constructor(private route: ActivatedRoute) {}
}
```

Vervolgens wordt er gebruik gemaakt van `ngOnInit`, dit is een `lifecycle hook`.

```ts
import { CommonModule } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { ActivatedRoute } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  // voeg de interface OnInit toe
  hero: null | Hero = null;
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];

  constructor(private route: ActivatedRoute) {}
}
```

Door `implements OnInit` zal er nu een error zijn `Class 'HeroDetailsComponent' incorrectly implements interface 'OnInit'.⏎ Property 'ngOnInit' is missing in type 'HeroDetailsComponent' but required in type 'OnInit'.`. Dit komt omdat de interface `OnInit` een functie `ngOnInit` verwacht. Deze functie moet toegevoegd worden.

Deze hook wordt aangeroepen nadat de constructor van een component is uitgevoerd.

```ts
import { CommonModule } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { ActivatedRoute } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: null | Hero = null;
  heroes = [
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
    { number: 13, name: "Bombasto" },
    { number: 14, name: "Celeritas" },
    { number: 15, name: "Magneta" },
    { number: 16, name: "RubberMan" },
    { number: 17, name: "Dynama" },
    { number: 18, name: "Dr IQ" },
    { number: 19, name: "Magma" },
    { number: 20, name: "Tornado" },
  ];

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        const matchingHero = this.heroes.find((h) => h.number === id);
        if (matchingHero) {
          this.hero = matchingHero;
        }
      }
    });
  }
}
```

`this.route.params` is een `Observable`. Een observable kan op eender welk moment een waarde uitsturen. In dit geval zal de observable een waarde uitsturen wanneer één van de `params` wijzigt (in dit specifieke geval, hebben we enkel een param genaamd `id`). De `subscribe`-functie wordt uitgevoerd wanneer de observable een waarde uitstuurt. De waarde die uitgestuurd wordt, is een object met data van de parameters van de route, deze kennen we toe aan een variabele genaamd `params`. De `params`-variabele bevat een `id`-property met daarin de waarde die terug te vinden is in de route. Bijvoorbeeld `/heroes/15`, de `id`-property zal dan `15` bevatten. Dit is standaard aan string, vandaar dat deze omgezet wordt naar een `number` met behulp van de `+`-operator.

Merk op dat `hero: null | Hero = null;` niet langer de hard-coded hero bevat. Deze wordt nu toegekend op basis van de `id`-parameter. Test de code op twee manieren. De eerste manier is om via het dashboard naar de lijst van heroes te gaan, daar op een hero te klikken en vervolgens op de knop `Details` te klikken. De tweede manier is om manueel de route te bezoeken, bijvoorbeeld `/heroes/15`. Probeer ook om manueel een id van een hero in te geven die niet voorkomt in de lijst, bijvoorbeeld `/heroes/69`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/5aaf76a62010e097569f52baf9da6eed51df40cc..fd4cdc7a6dae955a2301d5f01e17e8ea3b91a9d4)

### Hero niet gevonden

Om de gebruiker van betere feedback te voorzien bij het invoeren van een hero die niet bestaat, zal een `Hero not found!` (hero niet gevonden!)-melding getoond worden als de `hero` null is. Dit wordt gedaan door gebruik te maken van de `@else`-control flow. Een alternatief is om de `@if`-control flow te gebruiken met de `!`-operator.

```html
@if(hero) {
<div>hero exists</div>
} @else {
<div>hero does not exist</div>
}
```

Alternatief met @if:

```html
<!-- hero bestaat -->
@if(hero) {
<div>hero exists</div>
}

<!-- hero bestaat niet -->
@if(!hero) {
<div>hero does not exist</div>
}
```

Toegepast in de code:

`hero-details.component.html`

```html
@if(hero) {
<div class="title">{{ hero.name }} details!</div>

<div>id: {{ hero.number }}</div>
<div>name: <input [value]="hero.name" /></div>

<app-styled-button>Back</app-styled-button>
} @else {
<div class="title">Hero not found!</div>
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/fd4cdc7a6dae955a2301d5f01e17e8ea3b91a9d4..6554c0650a8c8dd642c2ed032bd5c3ede5753f7b)

### Stylen en koppelen van back-knop

Om de knop correct te plaatsen wordt er een class toegevoegd aan de knop. Om te zorgen dat de knop terug navigeert naar de `hero-list`-pagina wanneer er op geklikt wordt, moet er een `click handler` gekoppeld worden.

`hero-details.component.ts`

```ts
// hier nog code
export class HeroDetailsComponent implements OnInit {
  // hier nog code
  constructor(private route: ActivatedRoute, public router: Router) {}
  // hier nog code
}
```

Let op: `ActivatedRoute` wordt met `private` geïnjecteerd, deze is alleen nodig in de `TypeScript`-code. `Router` wordt met `public` geïnjecteerd, deze is nodig in de `template`-tag.

`hero-details.component.html`

```html
<!-- hier nog code -->
<app-styled-button class="back-button" (click)="router.navigate(['/heroes'])">
  Back
</app-styled-button>
<!-- hier nog code -->
```

De `class` en `(click)` event handler zijn toegevoegd aan de `app-styled-button`-component.

`hero-details.component.css`

```css
// hier nog code
.back-button {
  margin-top: 1rem;
}
```

Merk op: er is géén margin-top te zien in de browser. Dit is omdat Angular-componenten standaard `display: inline-block` hebben.

```css
// hier nog code
.back-button {
  margin-top: 1rem;
  display: block; // zorg dat het custom element een block element is
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/6554c0650a8c8dd642c2ed032bd5c3ede5753f7b..bc50827c935c48e30d661e106da9952345540659)

### Two way binding - ngModel

Momenteel wordt er gebruik gemaakt van `[value]` om een waarde toe te kennen aan de input. Dit is `one way binding`, de input krijgt een waarde toegekend, maar indien de waarde gewijzigd wordt, wordt dit niet doorgevoerd naar de `hero`-property. Om `two way binding` te bekomen, moet gebruik gemaakt worden van `ngModel`.

`v-model` kent de waarde van de `hero`-ref toe aan de input, maar zal ook de waarde van `hero`-ref updaten wanneer de inputwaarde gewijzigd wordt.

One way binding `:value`

```html
<div>name: <input [value]="hero.name" /></div>
```

![One way binding](/img/blog/quasar-toh-one-way-binding.gif)

Two way binding `v-model`

`src/app/pages/hero-details/hero-details.component.ts`
```ts
// hier nog imports
import { FormsModule } from "@angular/forms";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule], // importeer FormsModule
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  // hier nog code
}
```

`src/app/pages/hero-details/hero-details.component.html`
```html
<div>name: <input [(ngModel)]="hero.name" /></div>
```

![Two way binding](/img/blog/quasar-toh-two-way-binding.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/bc50827c935c48e30d661e106da9952345540659..990e1db6418aa2d8c1d0778c8210c5824075a1e9)

## Conclusie

Een pagina kan opgesplitst worden in herbruikbare componenten.

Het is oké als componenten niet op voorhand gemaakt worden, maar toegevoegd worden wanneer er opgemerkt wordt dat er identieke blokken code zijn doorheen de codebase.

Het is belangrijk om potentiële componenten te identificeren. Hoe meer ervaring, hoe beter het identificeren van componenten zal gaan.
