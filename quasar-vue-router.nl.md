---
title: Quasar layouts - Quasar-pagina - Vue Router
slug: quasar-layouts-quasar-pagina-vue-router
keywords:
  - Quasar
  - Vue
  - Vue Router
  - Vue 3
modules:
  - Jaar 2
---

Zet een nieuw Quasar-project op om stap voor stap mee te volgen.

## Wat is Vue Router

Vue router is een `library` gemaakt om een Vue-applicatie te voorzien van een `router`. Een `router` is een oplossing om in een `SPA (Single Page Applications)` verschillende routes te voorzien.

Bijvoorbeeld `/` om de hoofdpagina te tonen en `/admin` om een adminpagina te tonen.

In Quasar wordt Vue Router opgezet in `src/router/index.ts`.

src/router/index.ts (de automatisch gegenereerde commentaar is verwijderd)

```ts
import { route } from "quasar/wrappers";
import {
  createMemoryHistory,
  createRouter,
  createWebHashHistory,
  createWebHistory,
} from "vue-router";
import routes from "./routes";

export default route(function () {
  const createHistory = process.env.SERVER
    ? createMemoryHistory
    : process.env.VUE_ROUTER_MODE === "history"
    ? createWebHistory
    : createWebHashHistory;

  const Router = createRouter({
    scrollBehavior: () => ({ left: 0, top: 0 }),
    routes,

    history: createHistory(
      process.env.MODE === "ssr" ? void 0 : process.env.VUE_ROUTER_BASE
    ),
  });

  return Router;
});
```

Het is niet nodig om iets te wijzigen in dit bestand, Wat belangrijk is om te begrijpen, is dat de Vue Router hier wordt aangemaakt:

```ts
// ...
const Router = createRouter({
  scrollBehavior: () => ({ left: 0, top: 0 }),
  routes,

  history: createHistory(
    process.env.MODE === "ssr" ? void 0 : process.env.VUE_ROUTER_BASE
  ),
});
// ...
```

De Router wordt aangemaakt, met als `routes` de routes die geïmporteerd worden vanuit `src/router/router.ts`.

src/router/router.ts

```ts
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [{ path: "", component: () => import("pages/Index.vue") }],
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Er zijn twee routes gedefinieerd. De onderste route met als `path` (Nederlands: pad) `/:catchAll(.*)*`, zal als component `pages/Error404.vue` inladen.

Om dit uit te testen, start het project:

```sh
quasar dev
```

Ga naar `http://localhost:8080/#/voorbeeld`.

Een pagina wordt getoond, deze wordt ingeladen vanuit `src/pages/Error404.vue`. Dit komt doordat `/voorbeeld` geen gekende route is, bijgevolg is er een match met `/:catchAll(.*)*`. Deze route laadt `src/pages/Error404.vue`.

src/pages/Error404.vue

```html
<template>
  <div
    class="fullscreen bg-blue text-white text-center q-pa-md flex flex-center"
  >
    <div>
      <div style="font-size: 30vh">404</div>

      <div class="text-h2" style="opacity: 0.4">Oops. Nothing here...</div>

      <q-btn
        class="q-mt-xl"
        color="white"
        text-color="blue"
        unelevated
        to="/"
        label="Go Home"
        no-caps
      />
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "Error404",
  });
</script>
```

Verwijder alles in de template tag en wijzig het naar simpele HTML. In de `<template>`-tag, kan alle HTML-kennis toegepast worden.

src/pages/Error404.vue na wijzigen van `<template>`-tag.

```html
<template>
  <h1>404</h1>
  <div>Pagina niet gevonden!</div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "Error404",
  });
</script>
```

Bezoek opnieuw `http://localhost:8080/#/voorbeeld`.

Voeg nu een `<style>`-tag toe en gebruik CSS-kennis om de elementen te stylen.

src/pages/Error404.vue na toevoegen van `<style>`-tag.

```html
<template>
  <div id="page404">
    <div id="content-wrapper">
      <h1>404</h1>
      <div id="sub-text">Pagina niet gevonden!</div>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "Error404",
  });
</script>

<style lang="scss" scoped>
  #page404 {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;

    background-color: deeppink;
    color: white;
  }

  #content-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #sub-text {
    font-size: 3em;
  }
</style>
```

Bekijk de wijzigingen op [GitHub](https://github.com/code-coaching/blog-posts-examples/compare/53e9e05..981d873?diff=split).

Het resultaat lijkt veel op wat er eerst stond. Maar dit keer zonder hulp-classes (bv. `fullscreen`, `q-pa-md`) van Quasar te gebruiken en zonder custom componenten (bv. `<q-btn />`) van Quasar te gebruiken.

Quasar voorziet custom componenten die gebruikt kunnen worden, dit wordt behandeld in een ander artikel.
Quasar voorziet hulp-classes die gebruikt kunnen worden, dit wordt behandeld in een ander artikel.

Het is geen verplichting om de componenten of hulp-classes te gebruiken, het is perfect mogelijk om met de kennis van HTML in de `<template>`-tag te werken en met de kennis van CSS in de `<style>`-tag te werken.

Enkel de `<script>`-tag blijft nog over, hier wordt in dit artikel niks mee gedaan. Dit is de plaats waarin de JavaScript- / TypeScript-kennis gebruikt kan worden.

## Pages

Alle `.vue`-bestanden zijn componenten. Ook pagina's. Voeg een nieuw bestand toe aan de map `src/pages` genaamd `First.vue`. Voeg een `<template>`-tag en een `<script>`-tag toe in het bestand. Aangezien er met TypeScript gewerkt wordt, wordt het `lang`-attribuut met als waarde `ts` toegevoegd op de `<script>`-tag.

src/pages/First.vue

```html
<template>
  <h1>De pagina First.vue werkt!</h1>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      console.log("First.vue: Dit wordt in de console getoond!");
    },
  });
</script>
```

In de `<script>`-tag wordt een nieuwe component gedefinieerd met de functie `defineComponent()`. Deze functie accepteert een configuratie-object. In dit configuratie-object wordt gebruikgemaakt van de functie `setup() {}`.

Wanneer de component getoond wordt in de browsr, zal de `setup`-functie uitgevoerd worden. In dit voorbeeld zal er iets in de console geprint worden. Vervolgens zal de inhoud van de `<template>`-tag getoond worden in de browser.

### Pagina inladen via router

Voeg een nieuwe route toe in `src/router/routes.ts`.

src/router/router.ts

```ts
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [{ path: "", component: () => import("pages/Index.vue") }],
  },

  {
    path: "/first",
    component: () => import("pages/First.vue"),
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Bezoek `http://localhost:8080/#/first` in de webbrowser. De inhoud van de `<template>`-tag wordt getoond. Bekijk de console van de browser, hier is `First.vue: Dit wordt in de console getoond!` terug te vinden.

Maak een tweede pagina aan genaamd `Second.vue`, met dezelfde inhoud. Wijzig `First.vue` in de template en de console.log naar `Second.vue`. Zorg ervoor dat deze pagina ingeladen wordt via de router op `/second`. Bezoek vervolgens zowel `/#/first` als `/#/second` in de webbrowser. Bekijk de console van de webbrowser en kijk hoe de inhoud van de pagina wijzigt.

Bekijk de wijzigingen op [GitHub](https://github.com/code-coaching/blog-posts-examples/compare/981d873..ddf3a38?diff=split).

## Layouts

Stel dat er een vaste header en footer getoond moet worden op beide pagina's. Dan is een oplossing om dit toe te voegen op beide pagina's. Maar stel dat er iets moet wijzigen in de footer, dan zou dit op beide plaatsn gewijzigd moeten worden. Er is een manier om te zorgen dat dit maar op één plaats moet. Door gebruik te maken van `<router-view />`.

Binnen Quasar wordt dit gedaan in een component in de map `src/layouts`. Voeg een nieuw bestand toe in `src/layouts`, genaamd `PrimaryLayout.vue`.

src/layouts/PrimaryLayout.vue

```html
<template>
  <header>Code Coaching</header>

  <router-view />

  <footer>
    <a href="mailto:info@code-coaching.dev">info@code-coaching.dev</a>
  </footer>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      console.log("PrimaryLayout setup wordt uitgevoerd!");
    },
  });
</script>

<style lang="scss" scoped>
  header {
    background-color: $primary;
    position: sticky;
    top: 0;
    padding: 1em;
    font-size: 1.5em;
    color: white;
  }

  footer {
    background-color: $secondary;
    position: fixed;
    bottom: 0;
    padding: 1em;
    width: 100%;
  }

  footer a {
    text-decoration: none;
    color: white;
  }
</style>
```

Opmerking: Er wordt gebruikgemaakt van SCSS-variabelen gedefinieerd door Quasar in `src/css/quasar.variables.scss`. Door gebruik te maken van de variabelen in plaats van hard-coded kleurcodes, is het makkelijk om de volledige website er anders uit te laten zien door op één locatie de kleur te wijzigen.

Wanneer deze component wordt ingeladen, dan zal de `setup`-functie uitgevoerd worden en `PrimaryLayout setup wordt uitgevoerd!` zal in de console geprint worden. De inhoud van de `<template>`-tag zal in de webbrowser getoond worden. De `<router-view />`-tag is de locatie waarin andere componenten getoond kunnen worden. Dit wordt duidelijker met de veranderingen in `src/router/routes.ts`.

### Layouts inladen via router

src/router/routes.ts

```ts
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/PrimaryLayout.vue"),
    children: [
      {
        path: "first",
        component: () => import("pages/First.vue"),
      },
      {
        path: "second",
        component: () => import("pages/Second.vue"),
      },
    ],
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Merk op dat er nu twee routes zijn, `"/"` en `"/:catchAll(.*)*"`. Bij de eerste route wordt als component, het `PrimaryLayout.vue`-component ingeladen. Er worden ook `children` (Nederlands: kinderen) gedefinieerd. In dit voorbeeld zijn er twee routes gedefinieerd:

1. `"first"`, waarbij als component `First.vue` wordt ingeladen.
2. `"second"`, waarbij als component `Second.vue` wordt ingeladen.

Hoe dit gelezen kan worden is dat bij het bezoeken van `/first`, zowel het component op `/` wordt ingeladen (`PrimaryLayout.vue`) en dat het component op `first` wordt ingeladen (`First.vue`).

In de `<template>`-tag van `PrimaryLayout.vue` staat het volgende:

```html
<template>
  <header>Code Coaching</header>

  <router-view />

  <footer>
    <a href="mailto:info@code-coaching.dev">info@code-coaching.dev</a>
  </footer>
</template>
```

Door `http://localhost:8080/#/first` te bezoeken, wordt `PrimaryLayout.vue` ingeladen (door de match op `"/"` in `src/router/routes.ts`). Vervolgens wordt er gekeken naar de routes in `children`, hier is een match op `"first"`. Het component `First.vue` wordt ingeladen in de `<router-view />`-tag.

Hetzelfde geldt bij het bezoeken van `http://localhost:8080/#/second`. `PrimaryLayout.vue` wordt ingeladen, vervolgens wordt `Second.vue` ingeladen in de `<router-view />`-tag.

Het resultaat is dat de header en de footer altijd zichtbaar zijn en dat het andere component getoond wordt op de plaats waar de `<router-view />`-tag staat.

Meerdere layouts zijn handig om bijvoorbeeld een layout te voorzien voor de pagina's die dienen voor de gewone bezoeker en een andere layout te voorzien voor de pagina's die dienen voor administrators van de website.

Bekijk de wijzigingen op [GitHub](https://github.com/code-coaching/blog-posts-examples/compare/ddf3a38..e08f465?diff=split).

## Conclusie

Vue Router voorziet functionaliteit om verschillende paden/routes te voorzien in een Vue-applicatie. Met de `<router-view />`-tag is het mogelijk om een locatie te voorzien waarin andere componenten getoond kunnen worden. Dit is mogelijk door in `src/router/routes.ts` `children` toe te voegen voegen aan een bestaande route.

Quasar voorziet een projectstructuur om componenten die een layout voorstellen te plaatsen in de map `src/layouts` en om componenten die een pagina voorstellen te plaatsen in de map `src/pages`.

Code terug te vinden op [GitHub](https://github.com/code-coaching/blog-posts-examples/tree/quasar-vue-router)!
