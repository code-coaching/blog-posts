---
postUuid: a582b6e0-8f3b-4d8b-aa2e-e0a5890812f8
title: Angular Tour of Heroes - Services
slug: angular-toh-services
tags:
  - Angular
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De beginsituatie is een bestaand Angular-project met een layoutcomponenent en werkende pagina's. De eindsituatie is een Angular-project waarin de data verplaatst is naar services.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-components.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-components)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-service-functions.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-services)

## Services

Een service is een manier om data te delen tussen verschillende componenten. Een service is een singleton, dit wil zeggen dat er maar één instantie van een service is. Deze instantie wordt gedeeld tussen alle componenten die de service gebruiken. Binnen Angular wordt er gebruik gemaakt van `dependency injection` om een service te gebruiken.

Momenteel is er een "probleem" met de `hero`-data. De lijst met `hero`-objecten wordt meerdere keren gedefinieerd in verschillende componenten.

Zowel in de pagina `hero-details` als in de pagina `hero-list` wordt de lijst met `hero`-objecten gedefinieerd:

```ts
// hier nog code
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
// hier nog code
```

Dit is een "probleem". Wanneer er een nieuwe hero wordt toegevoegd, verwijderd of gewijzigd dan moet dit op alle pagina's gebeuren.

## Hero Service

Maak een service aan om de `hero`-data te beheren. Gebruik de service vervolgens op de locatie waar de lijst met `hero`-objecten gebruikt wordt.

```sh
ng g s services/hero --skip-tests
```

Dit maakt een service aan in de map `services` met de naam `hero.service.ts`.

```ts
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor() {}
}
```

Dit is een class met een `@Injectable`-decorator. Dit is een Angular-decorator die ervoor zorgt dat de service gebruikt kan worden via `dependency injection`. De `providedIn: 'root'`-property zorgt ervoor dat de service een singleton is over de hele applicatie (vanaf "de root").

Voeg de lijst van heroes toe aan de service:

```ts
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor() {}

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

Vervolgens wordt de service geïnjecteerd op de plaatsen waar momenteel een hard-coded lijst met `hero`-objecten wordt gebruikt.

Vervang de hard-coded data in zowel `hero-list.component.ts` als `hero-details.component.ts` door de data uit de service.

Verwijder de hard-coded lijst.

`src/app/pages/hero-list/hero-list.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { Router } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service"; // importeer de service

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  // injecteer de service - public omdat deze gebruikt wordt in de template
  constructor(
    public router: Router,
    public heroService: HeroService
  ) {}

  selectedHero: null | Hero = null;

  onClickHero(hero: Hero) {
    this.selectedHero = hero;
  }
}
```

`src/app/pages/hero-list/hero-list.component.html`

```html
<!-- hier nog code -->
<div class="hero-list">
  <!-- wijzig heroes naar heroService.heroes -->
  @for (hero of heroService.heroes; track hero.number) {
  <!-- hier nog code -->
  }
</div>
<!-- hier nog code -->
```

`src/app/pages/hero-details/hero-details.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute, Router } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service"; // importeer de service

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: null | Hero = null;

  // injecteer de service - private omdat deze niet gebruikt wordt in de template
  constructor(
    private route: ActivatedRoute,
    public router: Router,
    private heroService: HeroService
  ) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        // wijzig this.heroes naar this.heroService.heroes
        const matchingHero = this.heroService.heroes.find(
          (h) => h.number === id
        );
        if (matchingHero) {
          this.hero = matchingHero;
        }
      }
    });
  }
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/9f45855d2bb667bd8b89e47be4123b463ec6f28b)

## Selected Hero

De `selectedHero` wordt momenteel alleen gebruikt in de pagina `hero-list`. Het is dus niet per se nodig om deze te verplaatsen naar de service. Maar het is wel handig om alle data rondom heroes te verplaatsen naar de service, op deze manier kan er altijd gewerkt worden via de service als er met `hero`-data gewerkt wordt.

Verplaats de `selectedHero` naar de service:

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor() {}

  selectedHero: null | Hero = null; // verplaats de selectedHero naar de service

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

In de `hero-list.component.ts` wordt de `selectedHero` aangesproken via de service.

`hero-list.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { Router } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  constructor(
    public router: Router,
    public heroService: HeroService
  ) {}

  // verwijder de selectedHero uit de component, deze staat nu in de service

  onClickHero(hero: Hero) {
    // wijzig this.selectedHero naar this.heroService.selectedHero
    this.heroService.selectedHero = hero;
  }
}
```

Gebruik in de template de `selectedHero` van de service.

`hero-list.component.html`

```html
<div class="title">My Heroes</div>

<div class="hero-list">
  @for (hero of heroService.heroes; track hero.number) {
  <div
    class="hero"
    [ngClass]="{
      'hero--active': hero.number === heroService.selectedHero?.number
    }"
    (click)="onClickHero(hero)"
  >
    <span class="hero-number">{{ hero.number }}</span>
    <span class="hero-name">{{ hero.name }}</span>
  </div>
  }
</div>

@if (heroService.selectedHero) {
<div class="title">
  {{ heroService.selectedHero.name | uppercase }} is my hero
</div>
<app-styled-button
  (click)="router.navigate(['heroes', heroService.selectedHero.number])"
>
  Details
</app-styled-button>
}
```

Bevestig dat het selecteren van een hero in de lijst nog altijd werkt op de `hero-list`-pagina.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/9f45855d2bb667bd8b89e47be4123b463ec6f28b)

## Top Heroes

Momenteel worden de top heroes getoond in de `dashboard`-pagina. Dit is momenteel enkel hard-coded data in de template van de `dashboard`-pagina. Maar, dit zijn de namen van `hero`-objecten die in de lijst staan in de service.

Voorzie een manier om de "top heroes" te tonen in de `dashboard`-pagina. We zullen de laatste 4 heroes tonen als "top heroes". Voeg een functie toe aan de service die de laatste 4 heroes teruggeeft.

`hero.service.ts`

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor() {}

  selectedHero: null | Hero = null;

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

  topHeroes() {
    return this.heroes.slice(-4);
  }
}
```

`dashboard.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { HeroService } from "../../services/hero.service"; // importeer de service

@Component({
  selector: "app-dashboard",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./dashboard.component.html",
  styleUrl: "./dashboard.component.css",
})
export class DashboardComponent {
  // injecteer de service - public omdat deze gebruikt wordt in de template
  constructor(public heroService: HeroService) {}
}
```

`dashboard.component.html`

```html
<div class="title">Top Heroes</div>

<div class="top-heroes">
  @for(hero of heroService.topHeroes(); track hero.number) {
  <div class="top-hero">{{ hero.name }}</div>
  }
</div>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/0cfe65d25f036749afeb169c009992c165abf09e)

## Doorklikken van dashboard naar detail

Voeg een functie toe die naar de detailpagina navigeert wanneer er op een hero wordt geklikt.

In de `dashboard`-pagina wordt de router geïnjecteerd:

`dashboard.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { HeroService } from "../../services/hero.service";
import { Router } from "@angular/router"; // importeer de router

@Component({
  selector: "app-dashboard",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./dashboard.component.html",
  styleUrl: "./dashboard.component.css",
})
export class DashboardComponent {
  // injecteer de router - public omdat deze gebruikt wordt in de template
  constructor(
    public heroService: HeroService,
    public router: Router
  ) {}
}
```

Navigeer naar de `hero-details`-pagina wanneer er op een hero wordt geklikt.

`dashboard.component.html`

```html
<div class="title">Top Heroes</div>

<div class="top-heroes">
  @for(hero of heroService.topHeroes(); track hero.number) {
  <div class="top-hero" (click)="router.navigate(['heroes', hero.number])">
    {{ hero.name }}
  </div>
  }
</div>
```

Wanneer er nu op een hero wordt geklikt, dan wordt er naar de `hero-details`-pagina genavigeerd. Wanneer er op de `hero-details`-pagina op de `back`-knop wordt geklikt, dan wordt er naar de `hero-list`-pagina genavigeerd. Dit is het gewenste gedrag wanneer er vanuit de `hero-list`-pagina naar de `hero-details`-pagina wordt gegaan. Dit is **niet** het gewenste gedrag wanneer er vanuit de `dashboard`-pagina naar de `hero-details`-pagina wordt gegaan.

Wijzig de functionaliteit van de `hero-details`-pagina:

`hero-details.component.ts`:

```ts
import { CommonModule, Location } from "@angular/common"; // importeer Location
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute } from "@angular/router"; // verwijder Router
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: null | Hero = null;

  constructor(
    private route: ActivatedRoute,
    public location: Location, // injecteer Location (verwijder Router)
    private heroService: HeroService
  ) {}

  // hier nog code
}
```

Probeer opnieuw zowel vanuit de `HeroList` als de `dashboard`-pagina naar de `HeroDetails`-pagina te navigeren en vervolgens op de `back`-knop te klikken.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/f9553ca8aaca2bd19e742aee8dbd42dac253dcc4)

## Hero wijzigen

In principe werkt deze functionaliteit al. Probeer maar!

Klik in het dashboard op een hero, wijzig de naam en klik op `back`. Je zal zien dat de naam van de hero gewijzigd is. Dit komt doordat objecten in JavaScript `by reference` zijn. Dit wilt zeggen dat wanneer er een object wordt aangepast, dat dit object overal aangepast is waar er naar dat object gerefereerd/verwezen wordt.

Dit is echter niet de gewenste functionaliteit. De gewenste functionaliteit is dat er een `save`-knop is op de detailpagina. Wanneer er géén gewijziging gedaan wordt en er wordt op `back` geklikt, dan moet er geen wijziging zijn doorgevoerd. Als er wel een wijziging is gedaan, dan moet er op `save` geklikt worden om de wijziging door te voeren.

Om te zorgen dat de hero niet meer verwijst (by reference) naar het object in de array van heroes in de service, moet er een kopie gemaakt worden van het object. Dit kan gedaan worden door gebruik te maken van `structuredClone`.

`hero-details.component.ts`

```ts
// imports

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  // hier nog code

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        const matchingHero = this.heroService.heroes.find(
          (h) => h.number === id
        );
        if (matchingHero) {
          this.hero = structuredClone(matchingHero); // gebruik structuredClone
        }
      }
    });
  }
}
```

Dit zorgt ervoor dat het object dat aan `this.hero` wordt toegekend een kopie is van het object dat in de array van heroes staat. Wanneer er nu een wijziging wordt gedaan aan `this.hero` (bijvoorbeeld het wijzigen van `hero.name` in de template) dan wordt dit enkel gewijzigd op de kopie, niet op het originele object.

Probeer opnieuw de stappen uit die hierboven beschreven staan. Je zal zien dat de wijziging niet meer wordt doorgevoerd wanneer er op `back` wordt geklikt.

Vervolgens gaan we de logica voor het zoeken van de `matchingHero` verplaatsen naar de service. Dit is een logische stap, aangezien de service verantwoordelijk is voor alle data rondom heroes.

`hero.service.ts`:

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  // hier nog code

  findHero(number: number): Hero | undefined {
    const matchingHero = this.heroes.find((h) => h.number === number); // find geeft ofwel de hero terug ofwel undefined
    return structuredClone(matchingHero); // gebruik structuredClone zodat er een kopie wordt teruggegeven
  }
}
```

Koppel de functie in de `hero-details`-pagina.

`hero-details.component.ts`

```ts
import { CommonModule, Location } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: undefined | Hero; // wijzig van null naar undefined

  constructor(
    private route: ActivatedRoute,
    public location: Location,
    public heroService: HeroService // wijzig van private naar public, we gaan de service gebruiken in de template
  ) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        this.hero = this.heroService.findHero(id); // gebruik de findHero functie van de service
      }
    });
  }
}
```

Voeg een functie toe om de hero te wijzigen in de service.

`hero.service.ts`

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  // hier nog code

  updateHero(hero: Hero) {
    const index = this.heroes.findIndex((h) => h.number === hero.number); // findIndex geeft ofwel de index terug, ofwel -1 als het niet gevonden is
    if (index !== -1) {
      this.heroes[index] = structuredClone(hero); // neem een kopie van de hero (structuredClone), zodat er geen referentie meer is naar het originele object
    }
  }
}
```

Voeg een knop toe om de hero te wijzigen in de `hero-details`-pagina. Voorzie ook een div rondom de knoppen om deze te kunnen stylen.

`hero-details.component.html`

```html
@if (hero) {
<div *ngIf="hero"></div>
<div class="title">{{ hero.name }} details!</div>

<div>id: {{ hero.number }}</div>
<div>name: <input [(ngModel)]="hero.name" /></div>

<div class="buttons">
  <app-styled-button (click)="location.back()">Back</app-styled-button>
  <app-styled-button (click)="heroService.updateHero(hero)">
    Save
  </app-styled-button>
</div>
} @else {
<div class="title">Hero not found!</div>
}
```

`hero-details.component.css`

```css
.title {
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.buttons {
  margin-top: 1rem;
  display: flex;
  gap: 0.5rem;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/1cd8d5a4a7831b4588c8dd7c2f465dee4f84b3e1)

## Hero verwijderen

Voorzie functionaliteit om een hero te verwijderen in de service. Koppel deze functionaliteit in de `hero-list`-component en in de `hero-details`-component.

`hero.service.ts`

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  // hier nog code

  deleteHero(hero: Hero) {
    this.heroes = this.heroes.filter((h) => h.number !== hero.number);
    if (this.selectedHero?.number === hero.number) {
      this.selectedHero = null;
    }
  }
}
```

Aangezien het mogelijk is dat de hero die verwijderd wordt de `selectedHero` is, moet er gecontroleerd worden of het de `selectedHero` is die verwijderd wordt. Indien dit het geval is, moet de `selectedHero` op `null` gezet worden.

Voorzie een knop om een hero te verwijderen in `hero-details`.

`hero-details.component.html`

```html
@if (hero) {
<div class="title">{{ hero.name }} details!</div>

<div>id: {{ hero.number }}</div>
<div>name: <input [(ngModel)]="hero.name" /></div>

<div class="buttons">
  <app-styled-button (click)="location.back()">Back</app-styled-button>
  <app-styled-button (click)="heroService.updateHero(hero)">
    Save
  </app-styled-button>
  <!-- Voeg de knop toe en koppel de deleteHero functie van de service -->
  <app-styled-button (click)="heroService.deleteHero(hero)">
    Delete
  </app-styled-button>
</div>
} @else {
<div class="title">Hero not found!</div>
}
```

Voorzie een knop om een hero te verwijderen in `hero-list`.

`hero-list.component.css`

```css
// hier nog code
.buttons {
  display: flex;
  gap: 0.5rem;
}
```

`hero-list.component.html`

```html
<div class="title">My Heroes</div>

<!-- hier nog code -->

@if (heroService.selectedHero) {
<div class="title">
  {{ heroService.selectedHero.name | uppercase }} is my hero
</div>
<div class="buttons">
  <app-styled-button
    (click)="router.navigate(['heroes', heroService.selectedHero.number])"
  >
    Details
  </app-styled-button>
  <app-styled-button (click)="heroService.deleteHero(heroService.selectedHero)">
    Delete
  </app-styled-button>
</div>
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/207cf835a724079f8ee1c95bdd9f1d8f4a9840ec)

## UX verbeteren - kleuren van knoppen

De knoppen hebben momenteel allemaal dezelfde styling. Om de gebruiker van meer context te voorzien zullen we de `save`-knop een "primary" kleur geven en de `delete`-knop een "negative" kleur.

`styled-button.component.css`

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

.primary {
  background-color: #1976d2;
  color: #ffffff;
}

.primary:hover {
  background-color: #1565c0;
  color: #ffffff;
}

.negative {
  background-color: #c10015;
  color: #ffffff;
}

.negative:hover {
  background-color: #9b0012;
  color: #ffffff;
}
```

`styled-button.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component, EventEmitter, Input, Output } from "@angular/core";

@Component({
  selector: "app-styled-button",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./styled-button.component.html",
  styleUrl: "./styled-button.component.css",
})
export class StyledButtonComponent {
  @Output() click = new EventEmitter<void>();
  @Input() type: "default" | "primary" | "negative" = "default";
}
```

`styled-button.component.html`

```html
<button
  [ngClass]="{
    primary: type === 'primary',
    negative: type === 'negative'
  }"
  (click)="click.emit()"
>
  <ng-content></ng-content>
</button>
```

De `@Input()`-property is nieuw. Dit voorziet een `input` (in andere frameworks ook wel ooit `prop` genoemd) voor de component. Dit zorgt ervoor dat er een waarde meegegeven kan worden aan de custom component op de locatie waar deze gebruikt wordt.

Wijzig de knop in de `hero-list`-pagina.

```html
<app-styled-button
  [type]="'negative'"
  (click)="heroService.deleteHero(heroService.selectedHero)"
>
  Delete
</app-styled-button>
```

De `@Input() type` kan via `attribute binding` een waarde toegekend krijgen. In dit geval wordt de string `'negative'` toegekend aan de `type`-property.

Wijzig de knoppen in de `hero-details`-pagina.

```html
<!-- hier nog code -->
<div class="buttons">
  <app-styled-button (click)="location.back()">Back</app-styled-button>
  <app-styled-button [type]="'primary'" (click)="heroService.updateHero(hero)">
    Save
  </app-styled-button>
  <app-styled-button [type]="'negative'" (click)="heroService.deleteHero(hero)">
    Delete
  </app-styled-button>
</div>
<!-- hier nog code -->
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/98b6848bf7eb1121750c244b7bb9cdfb704b8df1)

## UX verbeteren - navigatie

Op de `hero-details`-pagina, wanneer een hero verwijdert of opgeslagen wordt, willen we dat de gebruiker terug naar de vorige pagina genavigeerd wordt.

`hero-details.component.html`

```html
<!-- hier nog code -->
<div class="buttons">
  <app-styled-button (click)="location.back()">Back</app-styled-button>

  <!-- location.back() is toegevoegd -->
  <app-styled-button
    [type]="'primary'"
    (click)="heroService.updateHero(hero); location.back()"
  >
    Save
  </app-styled-button>

  <!-- location.back() is toegevoegd -->
  <app-styled-button
    [type]="'negative'"
    (click)="heroService.deleteHero(hero); location.back()"
  >
    Delete
  </app-styled-button>
</div>
<!-- hier nog code -->
```

Dit werkt, maar het is niet gewenst om te veel logica in de template te hebben. Verplaats de logica naar de `TypeScript`-code.
`hero-details.component.ts`

```ts
import { CommonModule, Location } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: undefined | Hero;

  constructor(
    private route: ActivatedRoute,
    public location: Location,
    private heroService: HeroService // wijzig van public naar private - dit wordt nu niet meer gebruikt in de template
  ) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        this.hero = this.heroService.findHero(id);
      }
    });
  }

  // de saveHero-functie bevat de twee statements die in de template stonden
  saveHero(hero: Hero) {
    this.heroService.updateHero(hero);
    this.location.back();
  }

  // de deleteHero-functie bevat de twee statements die in de template stonden
  deleteHero(hero: Hero) {
    this.heroService.deleteHero(hero);
    this.location.back();
  }
}
```

Gebruik de functies in de template.

`hero-details.component.html`

```html
<div class="buttons">
  <app-styled-button (click)="location.back()">Back</app-styled-button>
  <app-styled-button [type]="'primary'" (click)="saveHero(hero)">
    Save
  </app-styled-button>
  <app-styled-button [type]="'negative'" (click)="deleteHero(hero)">
    Delete
  </app-styled-button>
</div>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/52a1294db3aa19e585784b3e97faf12a23525ed1)

## Hero toevoegen

Om een nieuwe hero toe te voegen, moeten er verschillende dingen gedaan worden:

- Er moet een route voorzien worden om de pagina te tonen.
- Er moet een functie voorzien worden om de held toe te voegen aan de lijst in de service.
- Er moet een pagina gemaakt worden waar de gegevens ingevoerd kunnen worden.
- Er moet een knop toegevoegd worden aan de `hero-list`-pagina om naar de nieuwe pagina te navigeren.

### Route

`app.routes.ts`

```ts
import { Routes } from "@angular/router";
import { MainLayoutComponent } from "./layouts/main-layout/main-layout.component";

export const routes: Routes = [
  {
    path: "",
    component: MainLayoutComponent,
    children: [
      {
        path: "",
        loadComponent: () =>
          import("./pages/dashboard/dashboard.component").then(
            (c) => c.DashboardComponent
          ),
      },
      {
        path: "heroes",
        loadComponent: () =>
          import("./pages/hero-list/hero-list.component").then(
            (c) => c.HeroListComponent
          ),
      },
      // voeg heroes/add toe
      {
        path: "heroes/add",
        loadComponent: () =>
          import("./pages/hero-add/hero-add.component").then(
            (c) => c.HeroAddComponent
          ),
      },
      {
        path: "heroes/:id",
        loadComponent: () =>
          import("./pages/hero-details/hero-details.component").then(
            (c) => c.HeroDetailsComponent
          ),
      },
    ],
  },
];
```

### Service

Merk op dat enkel de naam van de hero wordt doorgegeven bij het aanmaken van een nieuwe held. De `number`-property wordt niet doorgegeven, maar deze wordt bepaald op basis van de reeds bestaande heroes.

`hero.service.ts`

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  // hier nog code

  addHero(name: string) {
    const maxNumber = Math.max(...this.heroes.map((h) => h.number));
    const newHero = { number: maxNumber + 1, name } satisfies Hero;
    this.heroes.push(newHero);
  }
}
```

### Pagina

`hero-add.component.ts`

```ts
import { CommonModule, Location } from "@angular/common";
import { Component } from "@angular/core";
import { HeroService } from "../../services/hero.service";
import { FormsModule } from '@angular/forms';
import { StyledButtonComponent } from '../../components/styled-button/styled-button.component';

@Component({
  selector: "app-hero-add",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./hero-add.component.html",
  styleUrl: "./hero-add.component.css",
})
export class HeroAddComponent {
  name: string = "";

  constructor(
    public location: Location,
    private heroService: HeroService
  ) {}

  saveHero(name: string) {
    this.heroService.addHero(name);
    this.location.back();
  }
}
```

`hero-add.component.html`

```html
<div class="title">Add hero</div>

<div>name: <input [(ngModel)]="name" /></div>

<div class="buttons">
  <app-styled-button (click)="location.back()">Back</app-styled-button>
  <app-styled-button [type]="'primary'" (click)="saveHero(name)">
    Save
  </app-styled-button>
</div>
```

`hero-add.component.css`

```css
.title {
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.buttons {
  margin-top: 1rem;
  display: flex;
  gap: 0.5rem;
}
```

Het is nu mogelijk om manueel naar `/heroes/add` te navigeren.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/917eec6c8e2ec9859af10ee34c1ad212931f6a89)

### Knop toevoegen hero-list

`hero-list.component.html`

```html
<div class="title">My Heroes</div>

<!-- hier nog code -->

<app-styled-button
  class="new-hero-button"
  [type]="'primary'"
  (click)="router.navigate(['heroes/add'])"
>
  New
</app-styled-button>

@if (heroService.selectedHero) {
<!-- hier nog code  -->
}
```

`hero-list.component.css`

```css
/* hier nog code */

.new-hero-button {
  display: block; // Angular gebruikt standaard display: inline-block voor custom components
  margin-top: 1rem;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/34bcd7661fe88acc0dc1295aa3ca97bea4cade45)

### Bug - dubbele hero

Bij het uittesten van de functionaliteit, zal je merken dat er niet één, maar twee heroes worden toegevoegd. Dit komt omdat de `@Output() click` dezelfde naam heeft als de default `click`-event van een `<button>`. Er wordt dus twee keer gereageerd op de `click`-event.

Dit kan opgelost worden door de `@Output() click` een andere naam te geven.

`styled-button.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component, EventEmitter, Input, Output } from "@angular/core";

@Component({
  selector: "app-styled-button",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./styled-button.component.html",
  styleUrl: "./styled-button.component.css",
})
export class StyledButtonComponent {
  @Output() onClick = new EventEmitter<void>(); // wijzig van click naar onClick
  @Input() type: "default" | "primary" | "negative" = "default";
}
```

`styled-button.component.html`

```html
<button
  [ngClass]="{
    primary: type === 'primary',
    negative: type === 'negative'
  }"
  (click)="onClick.emit()"
>
  <ng-content></ng-content>
</button>
```

Wijzig `click.emit()` naar `onClick.emit()`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/bc0152bc40d9273fc91ea8c84145edfd06da1208)

## Local Storage

Om te zorgen dat de data niet verloren gaat wanneer de pagina wordt herladen, kunnen we gebruik maken van `localStorage`.

Aangezien de service de locatie is waar de data beheerd wordt, is dit de plaats waar we de data zullen opslaan en laden.

`hero.service.ts`

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  // hier nog code

  updateHero(hero: Hero) {
    const index = this.heroes.findIndex((h) => h.number === hero.number);
    if (index !== -1) {
      this.heroes[index] = structuredClone(hero);
    }
    this.saveHeroes(); // sla de heroes op in localStorage
  }

  deleteHero(hero: Hero) {
    this.heroes = this.heroes.filter((h) => h.number !== hero.number);
    if (this.selectedHero?.number === hero.number) {
      this.selectedHero = null;
    }
    this.saveHeroes(); // sla de heroes op in localStorage
  }

  addHero(name: string) {
    const maxNumber = Math.max(...this.heroes.map((h) => h.number));
    const newHero = { number: maxNumber + 1, name } satisfies Hero;
    this.heroes.push(newHero);
    this.saveHeroes(); // sla de heroes op in localStorage
  }

  // Voeg de saveHeroes-functie toe, private, omdat deze enkel gebruikt wordt in de service, nooit vanuit een andere component
  private saveHeroes() {
    localStorage.setItem("heroes", JSON.stringify(this.heroes));
  }

  // Voeg de loadHeroes-functie toe, public (default), omdat deze aangeroepen wordt vanuit de app.component.ts
  loadHeroes() {
    const heroes = localStorage.getItem("heroes");
    if (heroes) {
      this.heroes = JSON.parse(heroes);
    }
  }
}
```

Deze wijzigingen zorgen ervoor dat alle wijzigingen ook opgeslagen worden in `localStorage`. Om te zorgen dat we de laatste toestand terug ingeladen wordt bij het herladen van de pagina, moeten we de `loadHeroes`-functie aanroepen bij het initialiseren van de applicatie.

`app.component.ts`

```ts
import { Component, OnInit } from "@angular/core";
import { CommonModule } from "@angular/common";
import { RouterOutlet } from "@angular/router";
import { HeroService } from "./services/hero.service";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent implements OnInit {
  constructor(private heroService: HeroService) {}

  ngOnInit(): void {
    this.heroService.loadHeroes();
  }
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/c5eba83079b7960f453c0ea0d9c104a326b6d58d)

## Conclusie

Een service wordt gebruikt om data te beheren. Deze data kan gebruikt worden als `global state` in de applicatie.

Doordat alle logica rondom `heroes` nu in een aparte service/composable zit, is het in de toekomst makkelijker om bijvoorbeeld een echte API te gebruiken, de implementatie kan op één plaats gewijzigd worden.
