---
postUuid: 92284887-59b6-4ef0-b400-0867e0a98150
title: Quasar Tour of Heroes - Layouts en Pagina's
slug: quasar-toh-layouts-en-paginas
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt de layout opgezet en gaan de verschillende pagina's toegevoegd worden van de Tour of Heroes-applicatie. De beginsituatie is een nieuw Quasar-project. De eindsituatie is een Quasar-project waarin de layout is opgezet en waar er pagina's toegevoegd zijn.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/Begin)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-layouts-and-pages)

## Layouts en Pagina's

Binnen Quasar wordt er gesproken over Layouts en Pagina's. In feite is er geen verschil, het zijn beide Vue-componentent. Waarom dan het onderscheid? Dit is om een duidelijk onderscheid te maken tussen een component die elementen bevat die zichtbaar moeten zijn op elke pagina en componenten die specifiek zijn aan een pagina.

Een layout is een component waarin elementen worden toegevoegd die zichtbaar moeten zijn op elke pagina. Bijvoorbeeld een header, footer, navigatie, etc. De layout voorziet ook een element waarin de inhoud van een andere pagina getoond zal worden.

Een pagina is een component waarin elementen worden toegevoegd die specifiek zijn aan een pagina. Bijvoorbeeld een lijst van blogberichten, één specifiek blogbericht, etc.

## Layout identificeren

Het resultaat waar naartoe gewerkt wordt doorheen verschillende artikels:
![Tour of Heroes basic](https://angular.io/generated/images/guide/toh/toh-anim.gif)

In de onderstaande afbeelding is het navigatiediagram terug te vinden, hierop zijn de verschillende pagina's te zien. De pijlen duiden aan welke knop naar welke pagina verwijst. Bijvoorbeeld, klikken op de knop met `View Details` zal ervoor zorgen dat de pagina met de details van de geselecteerde hero wordt getoond.

![Tour of Heroes navigatiediagram](/img/blog/quasar-toh-nav-diagram.png)

Om te bepalen wat de layout is, kan er gekeken worden naar hetgeen op elke pagina terug te vinden is. Bij het bekijken van het bovenstaande navigatiediagram, kan gezien worden dat het bovenste gedeelte (de titel en de twee knoppen) hetzelfde is op alle pagina's.

![Tour of Heroes navigatiediagram met aanduiding van layout](/img/blog/quasar-toh-nav-diagram-layout.png)

Het doel van dit artikel is om de layout toe te voegen en de pagina's klaar te zetten, zonder de inhoud van de pagina's al te implementeren.

## Layout toevoegen

Maak een bestand genaamd `MainLayout.vue` aan in `src/layouts/`. Voeg dit bestand manueel toe of gebruik onderstaand commando in de root van het project.

```sh
touch src/layouts/MainLayout.vue
```

De inhoud van dit bestand is:

```html
<template>
  <div>Tour of Heroes</div>
  <button>Dashboard</button>
  <button>Heroes</button>

  <router-view></router-view>
</template>
```

## Pagina's en routing toevoegen

In het navigatiediagram is te zien dat er drie verschillende pagina's zijn.

1. `Dashboard`, met de `Top Heroes`.
1. `Heroes`, met `My Heroes` waar een lijst van heros te zien is.
1. `Details`, met de details van een `Hero`.

Om te zorgen dat deze pagina's gekoppeld kunnen worden aan de router, moeten deze eerst aangemaakt worden. In de map `src/pages` worden de pagina's toegevoegd. Voeg ze manueel toe of gebruik het onderstaande commando in de root van het project om de pagina's te maken.

```sh
touch src/pages/Dashboard.vue && touch src/pages/HeroList.vue
&& touch src/pages/HeroDetails.vue
```

De inhoud van `src/pages/Dashboard.vue` is:

```html
<template>
  <h1>Dashboard works!</h1>
</template>
```

De inhoud van `src/pages/HeroList.vue` is:

```html
<template>
  <h1>Hero list works!</h1>
</template>
```

De inhoud van `src/pages/HeroDetails.vue` is:

```html
<template>
  <h1>Hero details works!</h1>
</template>
```

Om te zorgen dat de pagina's bekeken kunnen worden, moeten deze gekoppeld worden aan de router. In de `src/router/routes.ts` wordt de router geconfigureerd. Dit is een lijst van routes. Een route is een URL die bezocht kan worden en bepaalt welke component/pagina getoond wordt.

Inhoud van `src/router/routes.ts`:

```js
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [
      { path: "", component: () => import("pages/Dashboard.vue") },
      { path: "heroes", component: () => import("pages/HeroList.vue") },
      { path: "heroes/:id", component: () => import("pages/HeroDetails.vue") },
    ],
  },

  /*
   * English: The path below will capture all routes that are not defined,
   * leave it as last route.
   * Nederlands: Het pad hieronder zal alle routes die niet gedefinieerd zijn opvangen,
   * laat het als laatste route staan.
   */
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Start de applicatie op om te weten hoe het resultaat eruitziet:

```sh
npm run dev
```

In de console wordt de URL getoond waar de applicatie op draait. Standaard is de URL `http://localhost:8080`.

Bezoek de volgende routes door de URL in de adresbalk te wijzigen:

- `http://localhost:8080/` (Dashboard)
- `http://localhost:8080/heroes` (HeroList)
- `http://localhost:8080/heroes/1` (HeroDetails)

Wat valt er op? De inhoud van de pagina's wordt niet getoond! Dit kan opgelost worden door één extra lijn toe te voegen aan `src/layouts/MainLayout.vue`:

```html
<template>
  <div>Tour of Heroes</div>
  <button>Dashboard</button>
  <button>Heroes</button>

  <router-view></router-view>
</template>
```

`<router-view></router-view>` is nieuw. Dit is een slot in de layout waarin de gekoppelde component van de router ingeladen wordt.

Bezoek opnieuw de drie verschillende routes in de adresbalk. Nu is de inhoud van de pagina's wel te zien!

## Navigatie toevoegen

Het is niet de bedoeling om altijd manueel de URL in de adresbalk in te typen om de pagina's te bekijken. Het zou handig zijn als er op de knoppen geklikt kan worden om te navigeren tussen de verschillende pagina's.

Stap 1: Voeg een script tag toe aan `src/layouts/MainLayout.vue`. Er wordt gebruikgemaakt van de Composition API en TypeScript.

`src/layouts/MainLayout.vue`

```html
<template>
  <div>Tour of Heroes</div>
  <button>Dashboard</button>
  <button>Heroes</button>

  <router-view></router-view>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      return {};
    },
  });
</script>
```

Stap 2: Importeer Vue Router.

`src/layouts/MainLayout.vue` - template wordt niet getoond, maar is nog steeds aanwezig.

```html
<script lang="ts">
  import { defineComponent } from "vue";
  import { useRouter } from "vue-router";

  export default defineComponent({
    setup() {
      const router = useRouter();

      return {};
    },
  });
</script>
```

Stap 3: Voeg een functie toe aan `src/layouts/MainLayout.vue` om routing/navigatie af te handelen.

```html
<script lang="ts">
  import { defineComponent } from "vue";
  import { useRouter } from "vue-router";

  export default defineComponent({
    setup() {
      const router = useRouter();

      const navigate = (path: string) => {
        // void router.push('/'); Navigate to Dashboard
        // void router.push('/heroes'); Navigate to HeroList
        void router.push(path);
      };

      return {};
    },
  });
</script>
```

Stap 4: Maak de functie bruikbaar in de template door het toe te voegen aan de return statement. Koppel deze aan het click event van de knoppen.

```html
<template>
  <div>Tour of Heroes</div>
  <button @click="navigate('/dashboard')">Dashboard</button>
  <button @click="navigate('/heroes')">Heroes</button>

  <router-view></router-view>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useRouter } from "vue-router";

  export default defineComponent({
    setup() {
      const router = useRouter();

      const navigate = (path: string) => {
        // void router.push('/'); Navigate to Dashboard
        // void router.push('/heroes'); Navigate to HeroList
        void router.push(path);
      };

      return {
        navigate,
      };
    },
  });
</script>
```

![Routing demo](img/blog/quasar-toh-routing.gif)

## Styling toevoegen

Als de applicatie in de browser vergeleken wordt met het navigatiediagram, is te zien dat de styling van de titel en de knoppen nog niet overeenkomen.

![Tour of Heroes navigatiediagram met aanduiding van layout](/img/blog/quasar-toh-nav-diagram-layout.png)

Er moeten enkele `containerelementen` toegevoegd worden om de styling van de layout en de knoppen te vergemakkelijken.

Voeg een style tag toe aan `src/layouts/MainLayout.vue` en koppel de CSS aan de HTML-elementen:

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

## Refactoren voor makkelijker onderhoud

De code werkt, prima! Maar het is niet makkelijk om de code te onderhouden. De navigatie is afhankelijk van de route die gedefinieerd is in de `src/router/routes.ts`-bestand. Stel dat de route voor de lijst van Heroes plots gewijzigd moet naar `/hero-list`. Wat voor impact heeft dit op de code?

De wijziging zou doorgevoord moeten worden in `src/router/routes.ts` en op elke locatie in de code waar er rechtstreeks genavigeerd wordt naar de route. In de code van dit artikel, is dit enkel in `MainLayout.vue`. Maar stel dat het project al wat uitgebreider is, dan kan dit ook op tien locaties voorkomen.

Om dit aan te pakken, gaat er gebruikgemaakt worden van `named routes` (Nederlands: routes met een naam).

`src/router/routes.ts`

```js
// ...
  {
    path: '/',
    component: () => import('layouts/MainLayout.vue'),
    children: [
      {
        name: 'Dashboard', // This is new
        path: '',
        component: () => import('pages/Dashboard.vue')
      },
      {
        name: 'HeroList', // This is new
        path: '/heroes',
        component: () => import('pages/HeroList.vue')
      },
      {
        name: 'HeroDetails', // This is new
        path: '/heroes/:id',
        component: () => import('pages/HeroDetails.vue')
      }
    ],
  },
// ...
```

`src/layouts/MainLayout.vue`

```html
<template>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>
    <div class="button-container">
      <button @click="navigate('Dashboard')">Dashboard</button>
      <button @click="navigate('HeroList')">Heroes</button>
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
        // void router.push({ name: 'Dashboard' }); Navigate to Dashboard
        // void router.push({ name: 'HeroList' }); Navigate to HeroList
        void router.push({ name: path });
      };

      return {
        navigate,
      };
    },
  });
</script>

<!-- ... -->
```

Gefeliciflopstaart! De code is nu makkelijker te onderhouden. Stel dat de lijst met heroes getoond moet worden op een andere route, bijvoorbeeld `/hero-list`, dan is dit mogelijk door enkel het `path` te wijzigen in `src/router/routes.ts`.

De applicatie doet functioneel nog exact hetzelfde. Dit kan geverifieerd worden door de applicatie opnieuw uit te testen in de browser.

Er is nog een verbetering mogelijk, niet qua onderhoud, maar om bugs door middel van typfouten te voorkomen. Momenteel wordt `Dashboard` en `HeroList` manueel toegevoegd op elke locatie waar er verwezen wordt naar deze `named route`. De laatste verbetering die toegevoegd wordt in dit artikel, is het toevoegen van een object dat alle `named routes` bevat.

Bovenaan in `src/router/routes.ts` staat nu:

```js
export const ROUTE_NAMES = {
  DASHBOARD: "Dashboard",
  HERO_LIST: "HeroList",
  HERO_DETAILS: "HeroDetails",
};
```

`export` omdat het ook in andere bestanden gebruikt gaat worden.

Wijzig de referenties naar de `named routes` in `src/router/routes.ts`:

```js
import { RouteRecordRaw } from "vue-router";

export const ROUTE_NAMES = {
  DASHBOARD: "Dashboard",
  HERO_LIST: "HeroList",
  HERO_DETAILS: "HeroDetails",
};

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [
      {
        name: ROUTE_NAMES.DASHBOARD, // This is new
        path: "",
        component: () => import("pages/Dashboard.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_LIST, // This is new
        path: "/heroes",
        component: () => import("pages/HeroList.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_DETAILS, // This is new
        path: "/heroes/:id",
        component: () => import("pages/HeroDetails.vue"),
      },
    ],
  },

  /*
   * English: The path below will capture all routes that are not defined,
   * leave it as last route.
   * Nederlands: Het pad hieronder zal alle routes die niet gedefinieerd zijn opvangen,
   * laat het als laatste route staan.
   */
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Wijzig de referenties naar de `named routes` in `src/layouts/MainLayout.vue`:

```html
<template>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>
    <div class="button-container">
      <!-- English: Use the ROUTE_NAMES object -->
      <!-- Nederlands: Gebruik het ROUTE_NAMES object -->
      <button @click="navigate(ROUTE_NAMES.DASHBOARD)">Dashboard</button>
      <button @click="navigate(ROUTE_NAMES.HERO_LIST)">Heroes</button>
    </div>

    <router-view></router-view>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useRouter } from "vue-router";
  import { ROUTE_NAMES } from "../router/routes"; // This is new

  export default defineComponent({
    setup() {
      const router = useRouter();

      const navigate = (path: string) => {
        // void router.push({ name: 'Dashboard' }); Navigate to Dashboard
        // void router.push({ name: 'HeroList' }); Navigate to HeroList
        void router.push({ name: path });
      };

      return {
        navigate,

        // English: This is new, this makes the object available in the template
        // Dutch: Dit is nieuw, dit maakt het object beschikbaar in de template
        ROUTE_NAMES,
      };
    },
  });
</script>

<!-- ... -->
```

Opnieuw is er functioneel niks gewijzigd. De applicatie doet nog steeds exact hetzelfde. Dit kan geverifieerd worden door de applicatie opnieuw uit te testen in de browser.

Nog één kleine wijziging. De functie `navigate` heeft een parameter genaamd `path`, maar wat er eigenlijk in de parameter genaamd `path` komt, is de `name` van de route. Omdat omschrijvende variabelen en functies zorgen voor makkelijker te begrijpen code, wordt `path` gewijzigd naar `name`.

```js
const navigate = (name: string) => {
  // void router.push({ name: 'Dashboard' }); Navigate to Dashboard
  // void router.push({ name: 'HeroList' }); Navigate to HeroList
  void router.push({ name: name });
};
```

Als `key` en `value` dezelfde naam hebben, is het voldoende om enkel de `key` te gebruiken. De code wordt dus gewijzigd naar:

```js
const navigate = (name: string) => {
  // void router.push({ name: 'Dashboard' }); Navigate to Dashboard
  // void router.push({ name: 'HeroList' }); Navigate to HeroList
  void router.push({ name });
};
```

Hetzelfde is ook al terug te zien bij de `return statement` van de `setup()`-functie.

```js
return {
  navigate,
  ROUTE_NAMES,
};
```

Is hetzelfde als:

```js
return {
  navigate: navigate,
  ROUTE_NAMES: ROUTE_NAMES,
};
```

## Conclusie

Een layout bevat elementen die op elke route/pagina zichtbaar zijn.
Een pagina wordt gekoppeld via de `children`-property van de layout in het `src/router/routes.ts`-bestand. Een `child` wordt ingeladen in het `<router-view></router-view>`-element.

Het is oké om code werkende te krijgen en vervolgens te refactoren om het in de toekomst makkelijker te kunnen onderhouden. Een voorbeeld uit dit artikel is de keuze voor `named routes` in plaats van `paths`.

Het is oké om code te refactoren om te zorgen dat de code `DRY` is. DRY staat voor `Don't Repeat Yourself`, ofwel `Dit mag niet herhaald worden`. Een andere verwoording is: zorg voor een `single point of failure`. Dit betekent dat het wijzigen van de code maar op één locatie moet gebeuren en enkel hier kan de code kapot gaan door een verkeerde wijziging. Een voorbeeld uit dit artikel is de keuze om de `named routes` in een object te plaatsen en dit object overal te gebruiken waar een verwijzing nodig is naar een `named route`.

Duidelijke benamingen voor zowel functies als variabelen helpen om code te lezen en te begrijpen. Een voorbeeld uit dit artikel is de keuze om `path` te wijzigen naar `name`.
