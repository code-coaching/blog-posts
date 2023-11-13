---
postUuid: 14087005-64c1-4349-9f98-124c26563d2b
title: Angular Tour of Heroes - Layouts en Pagina's
slug: angular-toh-layouts-en-paginas
tags:
  - Angular
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt de layout opgezet en gaan de verschillende pagina's toegevoegd worden van de Tour of Heroes-applicatie. De beginsituatie is een nieuw Angular-project. De eindsituatie is een Angular-project waarin de layout is opgezet en waar er pagina's toegevoegd zijn.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

Beginsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/Begin)

Eindsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-layouts-and-pages)

## Layouts en Pagina's

Binnen het project wordt er gesproken over Layouts en Pagina's. In feite is er geen verschil, het zijn beide Angular-componentent. Waarom dan het onderscheid? Dit is om een duidelijk onderscheid te maken tussen een component die elementen bevat die zichtbaar moeten zijn op elke pagina en componenten die specifiek zijn aan een pagina.

Een layout is een component waarin elementen worden toegevoegd die zichtbaar moeten zijn op elke pagina. Bijvoorbeeld een header, footer, navigatie, etc. De layout voorziet ook een element waarin de inhoud van een andere pagina getoond zal worden.

Een pagina is een component waarin elementen worden toegevoegd die specifiek zijn aan een pagina. Bijvoorbeeld een lijst van blogberichten, één specifiek blogbericht, etc.

## Angular component

Een Angular-component bestaat uit verschillende bestanden:

- `[een-naam].component.ts` - TypeScript-code
- `[een-naam].component.html` - HTML
- `[een-naam].component.css` - CSS
- `[een-naam].component.spec.ts` - Testcode

`[een-naam]` is de naam van de component. Bijvoorbeeld `main-layout` of `dashboard`.

In deze tutorial worden de testen niet gebruikt, de `spec.ts`-bestanden worden niet aangemaakt. Je zal dus per component drie bestanden terugvinden (`ts`, `html` en `css`).

## Layout identificeren

Het resultaat waar naartoe gewerkt wordt doorheen verschillende artikels:
![Tour of Heroes basic](https://angular.io/generated/images/guide/toh/toh-anim.gif)

In de onderstaande afbeelding is het navigatiediagram terug te vinden, hierop zijn de verschillende pagina's te zien. De pijlen duiden aan welke knop naar welke pagina verwijst. Bijvoorbeeld, klikken op de knop met `View Details` zal ervoor zorgen dat de pagina met de details van de geselecteerde hero wordt getoond.

![Tour of Heroes navigatiediagram](/img/blog/quasar-toh-nav-diagram.png)

Om te bepalen wat de layout is, kan er gekeken worden naar hetgeen op elke pagina terug te vinden is. Bij het bekijken van het bovenstaande navigatiediagram, kan gezien worden dat het bovenste gedeelte (de titel en de twee knoppen) hetzelfde is op alle pagina's.

![Tour of Heroes navigatiediagram met aanduiding van layout](/img/blog/quasar-toh-nav-diagram-layout.png)

Het doel van dit artikel is om de layout toe te voegen en de pagina's klaar te zetten, zonder de inhoud van de pagina's al te implementeren.

## Layout toevoegen

Gebruik de Angular CLI om een component aan te maken. De layout wordt toegevoegd in de map `src/app/layouts`. Voer het onderstaande commando uit in de root van het project.

```sh
ng g c layouts/main-layout --skip-tests
```

De inhoud van `src/app/layouts/main-layout.component.html` is:

```html
<div>Tour of Heroes</div>
<button>Dashboard</button>
<button>Heroes</button>
```

## Pagina's en routing toevoegen

In het navigatiediagram is te zien dat er drie verschillende pagina's zijn.

1. `Dashboard`, met de `Top Heroes`.
1. `Heroes`, met `My Heroes` waar een lijst van heroes te zien is.
1. `Details`, met de details van een `Hero`.

Om te zorgen dat deze pagina's gekoppeld kunnen worden aan de router, moeten deze eerst aangemaakt worden. In de map `src/app/pages` worden de pagina's toegevoegd. Voeg ze manueel toe of gebruik het onderstaande commando in de root van het project om de pagina's te maken.

```sh
ng g c pages/dashboard --skip-tests && ng g c pages/hero-list --skip-tests && ng g c pages/hero-details --skip-tests
```

Om te zorgen dat de pagina's bekeken kunnen worden, moeten deze gekoppeld worden aan de router. In de `src/app.routes.ts` wordt de router geconfigureerd. Dit is een lijst van routes. Een route is een URL die bezocht kan worden en bepaalt welke component/pagina getoond wordt.

Inhoud van `src/app.routes.ts`:

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

Start de applicatie op om te weten hoe het resultaat eruitziet:

```sh
npm run start
```

In de console wordt de URL getoond waar de applicatie op draait. Standaard is de URL `http://localhost:4200/`.

Bezoek de volgende routes door de URL in de adresbalk te wijzigen:

- `http://localhost:4200/` (dashboard.component.ts)
- `http://localhost:4200/heroes` (hero-list.component.ts)
- `http://localhost:4200/heroes/1` (hero-details.component.ts)

Wat valt er op? De inhoud van de pagina's wordt niet getoond! Dit kan opgelost worden door één extra lijn toe te voegen aan `src/app/layouts/main-layout.component.html`:

```html
<template>
  <div>Tour of Heroes</div>
  <button>Dashboard</button>
  <button>Heroes</button>

  <router-outlet></router-outlet>
</template>
```

`<router-outlet></router-outlet>` is nieuw. Dit is een slot in de layout waarin de gekoppelde component van de router ingeladen wordt.

In `src/app/layouts/main-layout.component.ts` wordt `RouterOutlet` toegevoegd aan de imports.

```ts
import { Component } from "@angular/core";
import { CommonModule } from "@angular/common";
import { RouterOutlet } from "@angular/router"; // Dit is nieuw

@Component({
  selector: "app-main-layout",
  standalone: true,
  imports: [CommonModule, RouterOutlet], // RouterOutlet is toegevoegd
  templateUrl: "./main-layout.component.html",
  styleUrl: "./main-layout.component.css",
})
export class MainLayoutComponent {}
```

Bezoek opnieuw de drie verschillende routes in de adresbalk. Nu is de inhoud van de pagina's wel te zien!

Toevoegen van `dev` script aan `package.json` om de applicatie te starten:

```json
"scripts": {
    "ng": "ng",
    "start": "ng serve",
    "dev": "ng serve", // Dit is nieuw
    "build": "ng build",
    "watch": "ng build --watch --configuration development",
    "test": "ng test"
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/Begin..7088b7f60c0386bd471f70f0b2a500571248f336)

## Navigatie toevoegen

Het is niet de bedoeling om altijd manueel de URL in de adresbalk in te typen om de pagina's te bekijken. Het zou handig zijn als er op de knoppen geklikt kan worden om te navigeren tussen de verschillende pagina's.

Stap 1: Voeg de `Router` toe aan `src/app/layouts/main-layout/main-layout.component.ts`.

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { Router, RouterOutlet } from "@angular/router";

@Component({
  selector: "app-main-layout",
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: "./main-layout.component.html",
  styleUrl: "./main-layout.component.css",
})
export class MainLayoutComponent {
  constructor(public router: Router) {}
}
```

Merk op: `public router: Router` is toegevoegd aan de constructor. Dit zorgt ervoor dat de `Router` beschikbaar is in de component. Doordat `public` gebruikt wordt, is de `Router` ook beschikbaar in de template. Dit concept wordt `dependency injection` genoemd.

Stap 2: Gebruik de `Router` in de template om te navigeren.

`src/app/layouts/main-layout/main-layout.component.html`

```html
<div>Tour of Heroes</div>
<button (click)="router.navigate([''])">Dashboard</button>
<button (click)="router.navigate(['heroes'])">Heroes</button>

<router-outlet></router-outlet>
```

![Routing demo](img/blog/quasar-toh-routing.gif)

De huidige versie in de browser zal verschillen van hetgeen je ziet in de afbeelding, het belangrijkste is dat de navigatie werkt. De styling wordt later toegevoegd.

Merk op dat het voorbeeld er een `#` aanwezig is in de URL. Dit kan bekomen worden door de `HashLocationStrategy` te gebruiken. Dit zorgt ervoor dat er geen server-side wijzigingen nodig zijn om de `Single Page Application` te laten werken.

Om de `HashLocationStrategy` te gebruiken, moet deze toegevoegd worden aan de configuratie van de router.

```ts
import { ApplicationConfig } from "@angular/core";
import { provideRouter, withHashLocation } from "@angular/router"; // Import withHashLocation

import { routes } from "./app.routes";

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes, withHashLocation())], // Voeg withHashLocation toe
};
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/7088b7f60c0386bd471f70f0b2a500571248f336..6e0d53a94edd4bc6c3f6ae19fa15921dd060afbc)

## Styling toevoegen

Als de applicatie in de browser vergeleken wordt met het navigatiediagram, is te zien dat de styling van de titel en de knoppen nog niet overeenkomen.

![Tour of Heroes navigatiediagram met aanduiding van layout](/img/blog/quasar-toh-nav-diagram-layout.png)

Er moeten enkele `containerelementen` toegevoegd worden om de styling van de layout en de knoppen te vergemakkelijken.

Voeg CSS toe aan `src/app/layouts/main-layout.component.css`.

```html
<template>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>
    <div class="button-container">
      <button @click="navigate('/')">Dashboard</button>
      <button @click="navigate('/heroes')">Heroes</button>
    </div>

    <router-view></router-view>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useRouter } from "vue-router";

  export default defineComponent({
    setup() {
      const router = useRouter();

      const navigate = (path: string) => {
        // void router.push('/); Navigate to Dashboard
        // void router.push('/heroes'); Navigate to HeroList
        void router.push(path);
      };

      return {
        navigate,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
    font-size: 1.5rem;
    color: grey;
    font-weight: bold;
  }

  .layout-container {
    margin: 2rem;
  }

  .button-container {
    display: flex;
    gap: 0.25rem;
  }

  button {
    background-color: #eeeeee;
    border-radius: 0.25rem;
    font-weight: 500;
    border: none;
    padding: 0.25rem 0.5rem;
    color: #567868;

    &:hover {
      background-color: darken(#eeeeee, 10%);
      color: #0096e8;
      cursor: pointer;
    }
  }
</style>
```

Dit lijkt al meer op het navigatiediagram!

![Styled routing demo](/img/blog/quasar-toh-styling.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/compare/6e0d53a94edd4bc6c3f6ae19fa15921dd060afbc..bca6449ed4c410206717b3b3fc46a19a3cabad1f)

## Conclusie

Een layout bevat elementen die op elke route/pagina zichtbaar zijn.

Een pagina wordt gekoppeld via de `children`-property van de layout in het `src/app.routes.ts`-bestand. Een `child` wordt ingeladen in het `<router-outlet></router-outlet>`-element.

Een service kan gebruikt worden door deze te `injecteren` in de constructor van een component. Wanneer deze `public` is, kan deze ook gebruikt worden in de template.
