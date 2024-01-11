---
postUuid: c5667cc4-5904-4480-93ac-2ad4ee222313
title: Vue Tour of Heroes - Layouts en Pagina's
slug: vue-toh-layouts-en-paginas
tags:
  - Vue
  - Tour of Heroes
categories:
  - Frontend
youtubeIds:
  - ool_PAuG3RQ
---

In dit artikel wordt de layout opgezet en gaan de verschillende pagina's toegevoegd worden van de Tour of Heroes-applicatie. De beginsituatie is een nieuw Vue-project. De eindsituatie is een Vue-project waarin de layout is opgezet en waar er pagina's toegevoegd zijn.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

Beginsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/Begin)

Eindsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/angular-toh-layouts-and-pages)

## Layouts en Pagina's

Binnen het project wordt er gesproken over Layouts en Pagina's. In feite is er geen verschil, het zijn beide Vue-componentent. Waarom dan het onderscheid? Dit is om een duidelijk onderscheid te maken tussen een component die elementen bevat die zichtbaar moeten zijn op elke pagina en componenten die specifiek zijn aan een pagina.

Een layout is een component waarin elementen worden toegevoegd die zichtbaar moeten zijn op elke pagina. Bijvoorbeeld een header, footer, navigatie, etc. De layout voorziet ook een element waarin de inhoud van een andere pagina getoond zal worden.

Een pagina, ook wel een view genoemd, is een component waarin elementen worden toegevoegd die specifiek zijn aan een pagina. Bijvoorbeeld een lijst van blogberichten, één specifiek blogbericht, etc.

## Vue component

Een Vue-component bestaat uit één bestand:

- `ComponentNaam.vue` - een Vue-component

Een Vue-component is een 'Single File Component', dit bestand bestaat uit drie delen:

- `<template>` - de HTML
- `<script>` - de JavaScript
- `<style>` - de CSS

In dit project wordt er gebruik gemaakt worden van TypeScript `<script lang="ts">` en er zal gebruik gemaakt worden van de `script setup` syntax `<script setup lang="ts">`. Dit is een nieuwe syntax die geïntroduceerd is in Vue 3. Deze syntax zorgt ervoor dat er minder boilerplate code nodig is.

## Layout identificeren

Het resultaat waar naartoe gewerkt wordt doorheen verschillende artikels:
![Tour of Heroes basic](https://angular.io/generated/images/guide/toh/toh-anim.gif)

In de onderstaande afbeelding is het navigatiediagram terug te vinden, hierop zijn de verschillende pagina's te zien. De pijlen duiden aan welke knop naar welke pagina verwijst. Bijvoorbeeld, klikken op de knop met `View Details` zal ervoor zorgen dat de pagina met de details van de geselecteerde hero wordt getoond.

![Tour of Heroes navigatiediagram](/img/blog/quasar-toh-nav-diagram.png)

Om te bepalen wat de layout is, kan er gekeken worden naar hetgeen op elke pagina terug te vinden is. Bij het bekijken van het bovenstaande navigatiediagram, kan gezien worden dat het bovenste gedeelte (de titel en de twee knoppen) hetzelfde is op alle pagina's.

![Tour of Heroes navigatiediagram met aanduiding van layout](/img/blog/quasar-toh-nav-diagram-layout.png)

Het doel van dit artikel is om de layout toe te voegen en de pagina's klaar te zetten, zonder de inhoud van de pagina's al te implementeren.

## Layout toevoegen

Voeg een nieuwe map toe waarin layouts bijgehouden worden `src/layouts`. Voeg hieraan een Vue-component toe `src/layouts/MainLayout.vue`. Maak dit manueel aan of gebruik het onderstaande commando in de root van het project om de layout te maken.

```sh
mkdir src/layouts && touch src/layouts/MainLayout.vue
```

De inhoud van `src/layouts/MainLayout.vue`:

```html
<template>
  <div>Tour of Heroes</div>
  <button>Dashboard</button>
  <button>Heroes</button>
</template>
```

## Pagina's en routing toevoegen

In het navigatiediagram is te zien dat er drie verschillende pagina's zijn.

1. `Dashboard`, met de `Top Heroes`.
1. `Heroes`, met `My Heroes` waar een lijst van heroes te zien is.
1. `Details`, met de details van een `Hero`.

Om te zorgen dat deze pagina's gekoppeld kunnen worden aan de router, moeten deze eerst aangemaakt worden. In de map `src/views` worden de pagina's toegevoegd. Voeg ze manueel toe of gebruik het onderstaande commando in de root van het project om de pagina's te maken.

```sh
touch src/views/DashboardView.vue && touch src/views/HeroListView.vue && touch src/views/HeroDetailsView.vue
```

De inhoud van `src/views/DashboardView.vue`:

```html
<template>
  <div>Dashboard works!</div>
</template>
```

De inhoud van `src/views/HeroListView.vue`:

```html
<template>
  <div>Hero list works!</div>
</template>
```

De inhoud van `src/views/HeroDetailsView.vue`:

```html
<template>
  <div>Hero details works!</div>
</template>
```

Om te zorgen dat de pagina's bekeken kunnen worden, moeten deze gekoppeld worden aan de router. In `src/router/index.ts` wordt de router geconfigureerd. Dit is een lijst van routes. Een route is een URL die bezocht kan worden en bepaalt welke component/pagina getoond wordt.

Inhoud van `src/router/index.ts`:

```ts
import { createRouter, createWebHistory } from "vue-router";

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "MainLayout",
      component: () => import("@/layouts/MainLayout.vue"),
      children: [
        {
          path: "",
          name: "dashboard",
          component: () => import("@/views/DashboardView.vue"),
        },
        {
          path: "heroes",
          name: "hero-list",
          component: () => import("@/views/HeroListView.vue"),
        },
        {
          path: "heroes/:id",
          name: "hero-details",
          component: () => import("@/views/HeroDetailsView.vue"),
        },
      ],
    },
  ],
});

export default router;
```

Start de applicatie op om te weten hoe het resultaat eruitziet:

```sh
npm run dev
```

In de console wordt de URL getoond waar de applicatie op draait. Standaard is de URL `http://localhost:5173/`.

Bezoek de volgende routes door de URL in de adresbalk te wijzigen:

- `http://localhost:5173/` (DashboardView.vue)
- `http://localhost:5173/heroes` (HeroListView.vue)
- `http://localhost:5173/heroes/1` (HeroDetailsView.vue)

Wat valt er op? De inhoud van de pagina's wordt niet getoond! Dit kan opgelost worden door aan te geven waar de children getoond moeten worden in `src/layouts/MainLayout.vue`:

```html
<script setup lang="ts">
  import { RouterView } from "vue-router";
</script>

<template>
  <div>Tour of Heroes</div>
  <button>Dashboard</button>
  <button>Heroes</button>

  <!-- Dit is waar de children getoond worden -->
  <RouterView />
</template>
```

`<RouterView />` is toegevoegd aan de template. Dit is functionaliteit die voorzien wordt door `vue-router`. Dit wordt geïmporteerd in de `<script>`-tag.

Bezoek opnieuw de drie verschillende routes in de adresbalk. Nu is de inhoud van de pagina's wel te zien!

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/5ffd28f6c6bdb366aa84da766d0b8a33222b47ea)

## Navigatie toevoegen

Het is niet de bedoeling om altijd manueel de URL in de adresbalk in te typen om de pagina's te bekijken. Het zou handig zijn als er op de knoppen geklikt kan worden om te navigeren tussen de verschillende pagina's.

Voeg `useRouter` toe aan `src/layouts/MainLayout.vue`.

```html
<script setup lang="ts">
  import { RouterView, useRouter } from "vue-router";

  const router = useRouter();
</script>

<template>
  <div>Tour of Heroes</div>
  <button @click="router.push({ name: 'dashboard' })">Dashboard</button>
  <button @click="router.push({ name: 'hero-list' })">Heroes</button>

  <RouterView />
</template>
```

Merk op dat de `name` van de route gebruikt wordt om te navigeren. Dit is de `name` die gegeven is aan de route in `src/router/index.ts`.

![Routing demo](/img/blog/quasar-toh-routing.gif)

De huidige versie in de browser zal verschillen van hetgeen je ziet in de afbeelding, het belangrijkste is dat de navigatie werkt. De styling wordt later toegevoegd.

Merk op dat het voorbeeld er een `#` aanwezig is in de URL. Dit kan bekomen worden door de `createWebHashHistory()` te gebruiken. Dit zorgt ervoor dat er geen server-side wijzigingen nodig zijn om de `Single Page Application` te laten werken.

Om de `createWebHashHistory()` te gebruiken, moet deze toegevoegd worden aan de configuratie van de router.

`src/router/index.ts`

```ts
import { createRouter, createWebHashHistory } from "vue-router"; // importeer createWebHashHistory

const router = createRouter({
  history: createWebHashHistory(), // gebruik createWebHashHistory
  routes: [
    {
      path: "/",
      name: "MainLayout",
      component: () => import("@/layouts/MainLayout.vue"),
      children: [
        {
          path: "",
          name: "dashboard",
          component: () => import("@/views/DashboardView.vue"),
        },
        {
          path: "heroes",
          name: "hero-list",
          component: () => import("@/views/HeroListView.vue"),
        },
        {
          path: "heroes/:id",
          name: "hero-details",
          component: () => import("@/views/HeroDetailsView.vue"),
        },
      ],
    },
  ],
});

export default router;
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/74aaf44383f537579af68356a2efbbe8d04491b4)

## Styling toevoegen

Als de applicatie in de browser vergeleken wordt met het navigatiediagram, is te zien dat de styling van de titel en de knoppen nog niet overeenkomen.

![Tour of Heroes navigatiediagram met aanduiding van layout](/img/blog/quasar-toh-nav-diagram-layout.png)

Er moeten enkele `containerelementen` toegevoegd worden om de styling van de layout en de knoppen te vergemakkelijken.

Voeg CSS toe aan `src/layouts/MainLayout.vue`.

```html
<script setup lang="ts">
  import { RouterView, useRouter } from "vue-router";

  const router = useRouter();
</script>

<template>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>

    <div class="button-container">
      <button @click="router.push({ name: 'dashboard' })">Dashboard</button>
      <button @click="router.push({ name: 'hero-list' })">Heroes</button>
    </div>

    <RouterView />
  </div>
</template>

<style scoped>
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
  }

  button:hover {
    background-color: #e6e6e6;
    color: #0096e8;
    cursor: pointer;
  }
</style>
```

Merk op: De `style`-tag is toegevoegd aan het Vue-component en het `scoped`-attribuut is toegevoegd aan de `style`-tag. Dit zorgt ervoor dat de styling enkel van toepassing is op het huidige component.

Dit lijkt al meer op het navigatiediagram!

![Styled routing demo](/img/blog/quasar-toh-styling.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/15f83d5192ffeaa2b2454838b1e511089b1a4d8b)

## Conclusie

Een layout bevat elementen die op elke route/pagina zichtbaar zijn.

Een pagina wordt gekoppeld via de `children`-property van de layout in het `src/router/index.ts`-bestand. Een `child` wordt ingeladen in het `<RouterView />`-element.
