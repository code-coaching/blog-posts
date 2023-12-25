---
postUuid: b61379fc-6707-4373-9b54-237d52d0ba38
title: Vue Tour of Heroes - Services
slug: vue-toh-services
tags:
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De beginsituatie is een bestaand Vue-project met een layoutcomponenent en werkende pagina's. De eindsituatie is een Vue-project waarin de data verplaatst is naar services.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-components.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/vue-toh-components)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-service-functions.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/vue-toh-services)

## Services

Een service is een manier om data te delen tussen verschillende componenten. Om te zorgen dat de data overal hetzelfde is, wordt er gewerkt met het concept van een `singleton`. Dit is data die slechts één keer bestaat in de applicatie. Een service service kan verwijzen naar globaal beschikbare variabelen, deze variabelen zijn allemaal `singletons`. Een service wordt binnen Vue ook wel een `composable` genoemd.

Momenteel is er een "probleem" met de `hero`-data. De lijst met `hero`-objecten wordt meerdere keren gedefinieerd in verschillende componenten.

Zowel in de pagina `HeroDetailsView` als in de pagina `HeroListView` wordt de lijst met `hero`-objecten gedefinieerd:

```ts
// hier nog code
const heroes = [
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

Maak een map genaamd `services` aan in de `src`-map. Maak in deze map een bestand genaamd `hero.service.ts` aan. Gebruik de service vervolgens op de locatie waar de lijst met `hero`-objecten gebruikt wordt.

```sh
mkdir src/services && touch src/services/hero.service.ts
```

`hero.service.ts`

```ts
const useHeroes = () => {
  return {};
};

export { useHeroes };
```

Dit is een bestand met een functie genaamd `useHeroes`, dit is een conventie binnen Vue. De functie wordt `use` genoemd, gevolgd door de naam van de data die behandeld wordt. In dit geval is dat `Heroes`. De functie noemt dus `useHeroes`.

Momenteel geeft de functie een leeg object terug. Uiteindelijk zal de functie een object teruggeven met de data en functies die nodig zijn om met de data te werken.

Voeg de lijst van heroes toe aan de service:

```ts
const heroes = [
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

const useHeroes = () => {
  return {
    heroes,
  };
};

export { useHeroes };
```

Merk op dat de `heroes` in de `globale scope` geplaatst wordt, buiten de functie. Dit is zodat de `heroes` over heel de applicatie beschikbaar zijn.

Vervolgens moet de service gebruikt worden op de locatie waar de `heroes` gebruikt worden. In dit geval is dat in de `HeroListView`-pagina en in de `HeroDetailsView`-pagina.

Vervang de hard-coded data in zowel `HeroListView.vue` als `HeroDetailsView.vue` door de data uit de service.

Verwijder de hard-coded lijst.

`HeroListView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { ref, type Ref } from "vue";
  import { useRouter } from "vue-router";
  import { useHeroes } from "@/services/hero.service"; // importeer de service

  const router = useRouter();
  const { heroes } = useHeroes(); // haal de heroes uit de service

  const selectedHero: Ref<null | Hero> = ref(null);

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  const uppercase = (text: string) => text.toUpperCase();
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";
  import { useHeroes } from "@/services/hero.service"; // importeer de service

  const route = useRoute();
  const router = useRouter();

  const { heroes } = useHeroes(); // haal de heroes uit de service
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = heroes.find((hero) => hero.number === heroId) ?? null;
  });
</script>

<template>
  <!-- Hier de bestaande html  -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/469fd4e698431feb0baa11e7a6c225ff6dc88f60)

## Selected Hero

De `selectedHero` wordt momenteel alleen gebruikt in de pagina `HeroListView`. Het is dus niet per se nodig om deze te verplaatsen naar de service. Maar het is wel handig om alle data rondom heroes te verplaatsen naar de service, op deze manier kan er altijd gewerkt worden via de service als er met `hero`-data gewerkt wordt.

Verplaats de `selectedHero` naar de service:

```ts
import type { Hero } from "@/components/models";
import { ref, type Ref } from "vue";

const heroes = [
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

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  return {
    heroes,
    selectedHero,
  };
};

export { useHeroes };
```

In de `HeroListView.vue` wordt de `selectedHero` aangesproken via de service.

`HeroListView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { useRouter } from "vue-router";
  import { useHeroes } from "@/services/hero.service";

  const router = useRouter();

  const { heroes, selectedHero } = useHeroes();

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  const uppercase = (text: string) => text.toUpperCase();
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Bevestig dat het selecteren van een hero in de lijst nog altijd werkt op de `HeroListView`-pagina.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/f2f237eb9d55d1599c593a0a754ab0f49739b6f3)

## Top Heroes

Momenteel worden de top heroes getoond in de `DashboardView`-pagina. Dit is momenteel enkel hard-coded data in de template van de `DashboardView`-pagina. Maar, dit moeten de namen zijn van de `hero`-objecten die in de lijst staan in de service.

Voorzie een manier om de "top heroes" te tonen in de `DashboardView`-pagina. We zullen de laatste 4 heroes tonen als "top heroes". Voeg een functie toe aan de service die de laatste 4 heroes teruggeeft.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, type Ref } from "vue";

const heroes = [
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

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  const topHeroes = computed(() => {
    return heroes.slice(-4);
  });

  return {
    heroes,
    selectedHero,
    topHeroes,
  };
};
export { useHeroes };
```

`topHeroes` is een `computed property`, dit is Vue-functionaliteit om een afgeleide waarde te berekenen. De data die overal hetzelfde moet zijn, wordt gedefinieerd in de globale scope. De functies en computed properties die de data behandelen worden gedefinieerd in de scope van de service `useHeroes`.

`DashboardView.vue`

```html
<script setup lang="ts">
  import { useHeroes } from "@/services/hero.service";
  const { topHeroes } = useHeroes();
</script>

<template>
  <div class="title">Top Heroes</div>

  <div class="top-heroes">
    <!-- Voeg een v-for toe i.p.v. de hard-coded data -->
    <div v-for="(hero, index) in topHeroes" :key="index" class="top-hero">
      {{ hero.name }}
    </div>
  </div>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/2a626cd7586271367878b4fe9abc730cd73a02af)

## Doorklikken van dashboard naar detail

Gebruik de router om vanuit de `DashboardView`-pagina naar de `HeroDetailsView`-pagina te navigeren.

`DashboardView.vue`

```html
<script setup lang="ts">
  import { useHeroes } from "@/services/hero.service";
  import { useRouter } from "vue-router"; // importeer de router

  const { topHeroes } = useHeroes();
  const router = useRouter(); // zorg dat de router gebruikt kan worden
</script>

<template>
  <div class="title">Top Heroes</div>

  <div class="top-heroes">
    <!-- Voeg de navigatie toe aan elke hero -->
    <div
      v-for="(hero, index) in topHeroes"
      :key="index"
      class="top-hero"
      @click="
        router.push({
          name: 'hero-details',
          params: { id: hero.number }
        })
      "
    >
      {{ hero.name }}
    </div>
  </div>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Wanneer er nu op een hero wordt geklikt, dan wordt er naar de `HeroDetailsView`-pagina genavigeerd.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/c7814c6a83b99dd7d2c5bfa0a23c24e53e850cdb)

## Hero wijzigen

In principe werkt deze functionaliteit al. Probeer maar!

Klik in het dashboard op een hero, wijzig de naam en klik op `back`. Je zal zien dat de naam van de hero gewijzigd is. Dit komt doordat objecten in JavaScript `by reference` zijn. Dit wilt zeggen dat wanneer er een object wordt aangepast, dat dit object overal aangepast is waar er naar dat object gerefereerd/verwezen wordt.

Dit is echter niet de gewenste functionaliteit. De gewenste functionaliteit is dat er een `save`-knop is op de detailpagina. Wanneer er géén gewijziging gedaan wordt en er wordt op `back` geklikt, dan moet er geen wijziging zijn doorgevoerd. Als er wel een wijziging is gedaan, dan moet er op `save` geklikt worden om de wijziging door te voeren.

Om te zorgen dat de hero niet meer verwijst (by reference) naar het object in de array van heroes in de service, moet er een kopie gemaakt worden van het object. Dit kan gedaan worden door gebruik te maken van `structuredClone`. Er wordt ook gebruik gemaakt van `toRaw`, dit zorgt ervoor dat we de effectieve waarde van de `ref` krijgen, niet de `ref` zelf.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { computed, ref, toRaw, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { heroes } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    const matchingHero = heroes.find((hero) => hero.number === heroId) ?? null; // hou de hero bij in een variabele
    hero.value = structuredClone(toRaw(matchingHero)); // neem een kopie van de hero (structuredClone), zodat er geen referentie meer is naar het originele object
  });
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <StyledButton class="back-button" @click="router.go(-1)">Back</StyledButton>
  </template>
  <template v-else>
    <div class="title">Hero not found!</div>
  </template>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .back-button {
    margin-top: 1rem;
  }
</style>
```

Dit zorgt ervoor dat het object dat aan `hero.value` wordt toegekend een kopie is van het object dat in de array van heroes staat. Wanneer er nu een wijziging wordt gedaan aan `hero.value` (bijvoorbeeld het wijzigen van `hero.name` in de template) dan wordt dit enkel gewijzigd op de kopie, niet op het originele object.

Probeer opnieuw de stappen uit die hierboven beschreven staan. Je zal zien dat de wijziging niet meer wordt doorgevoerd wanneer er op `back` wordt geklikt.

Vervolgens gaan we de logica voor het zoeken van de `matchingHero` verplaatsen naar de service. Dit is een logische stap, aangezien de service verantwoordelijk is voor alle data rondom heroes.

`hero.service.ts`:

```ts
// imports
import { computed, ref, toRaw, type Ref } from "vue";

// hier de bestaande code

const useHeroes = () => {
  // hier nog code

  const findHero = (heroId: number) => {
    const matchingHero = heroes.find((hero) => hero.number === heroId) ?? null;
    return structuredClone(toRaw(matchingHero));
  };

  return {
    // hier nog code
    findHero,
  };
};

export { useHeroes };
```

Koppel de functie in de `HeroDetailsVue`-pagina.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero } = useHeroes(); // destructure findHero uit de service
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = findHero(heroId); // gebruik de functie uit de service
  });
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Voeg een functie toe om de hero te wijzigen in de service.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes = ref([
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
]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  const topHeroes = computed(() => {
    return heroes.slice(-4);
  });

  const findHero = (heroId: number) => {
    const matchingHero = heroes.find((hero) => hero.number === heroId) ?? null;
    return structuredClone(toRaw(matchingHero));
  };

  const updateHero = (hero: Hero) => {
    const index = heroes.findIndex((h) => h.number === hero.number);
    if (index !== -1) {
      heroes[index] = structuredClone(toRaw(hero));
    }
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    findHero,
    updateHero,
  };
};

export { useHeroes };
```

Merk op dat `heroes` nu een `ref` is geworden. Dit is nodig omdat de `updateHero`-functie de `heroes`-array overschrijft. Wanneer dit gebeurt, moet de `reactivity` van Vue opnieuw getriggerd worden. Dit gebeurt automatisch wanneer er een `ref` overschreven wordt, maar niet wanneer er een gewone array overschreven wordt. Het is een goed idee om altijd een `ref` te gebruiken voor data die in heel de applicatie gebruikt wordt.

Voeg een knop toe om de hero te wijzigen in de `hero-details`-pagina. Voorzie ook een div rondom de knoppen om deze te kunnen stylen.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = findHero(heroId);
  });
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <div class="buttons">
      <StyledButton @click="router.go(-1)">Back</StyledButton>
      <StyledButton
        @click="
          updateHero(hero);
          router.go(-1);
        "
      >
        Save
      </StyledButton>
    </div>
  </template>
  <template v-else>
    <div class="title">Hero not found!</div>
  </template>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .buttons {
    margin-top: 1rem;
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Er kunnen meerdere statements gekoppeld worden aan een `@click`-event. In dit geval wordt zowel `updateHero(hero);` als `router.go(-1);` uitgevoerd wanneer er op de knop geklikt wordt. Het is ook mogelijk om een aparte functie aan te maken in de `<script>`-tag waarin beide acties uitgevoerd worden om de template wat leesbaarder te houden. In dit artikel kiezen we ervoor om dit niet te doen.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/f0dc6a9bb9e7efcf88ccb81385e3796fdb3cbcb4)

## Hero verwijderen

Voorzie functionaliteit om een hero te verwijderen in de service. Koppel deze functionaliteit in de `HeroListView`-component en in de `HeroDetailsView`-component.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes = ref([
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
]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const deleteHero = (hero: Hero) => {
    heroes.value = heroes.value.filter((h) => h.number !== hero.number);
    if (selectedHero.value?.number === hero.number) {
      selectedHero.value = null;
    }
  };

  return {
    // hier nog code
    deleteHero,
  };
};

export { useHeroes };
```

Aangezien het mogelijk is dat de hero die verwijderd wordt de `selectedHero` is, moet er gecontroleerd worden of het de `selectedHero` is die verwijderd wordt. Indien dit het geval is, moet de `selectedHero` op `null` gezet worden.

Voorzie een knop om een hero te verwijderen in `HeroDetailsView`.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero, deleteHero } = useHeroes(); // destructure deleteHero uit de service
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = findHero(heroId);
  });
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <div class="buttons">
      <StyledButton @click="router.go(-1)">Back</StyledButton>
      <StyledButton
        @click="
          updateHero(hero);
          router.go(-1);
        "
      >
        Save
      </StyledButton>
      <!-- Koppel deleteHero aan de knop -->
      <StyledButton
        @click="
          deleteHero(hero);
          router.go(-1);
        "
      >
        Delete
      </StyledButton>
    </div>
  </template>
  <template v-else>
    <div class="title">Hero not found!</div>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Voorzie een knop om een hero te verwijderen in `HeroListView`.

`HeroListView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { useHeroes } from "@/services/hero.service";
  import { useRouter } from "vue-router";

  const router = useRouter();

  const { heroes, selectedHero, deleteHero } = useHeroes(); // destructure deleteHero uit de service

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  const uppercase = (text: string) => text.toUpperCase();
</script>

<template>
  <!-- Hier de bestaande html  -->

  <template v-if="selectedHero">
    <div class="title">{{ uppercase(selectedHero.name) }} is my hero</div>
    <div class="buttons">
      <StyledButton
        @click="router.push({ name: 'hero-details', params: { id: selectedHero?.number } })"
      >
        Details
      </StyledButton>
      <StyledButton @click="deleteHero(selectedHero)">Delete</StyledButton>
    </div>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */

  .buttons {
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/dc4e2d319e725ea96fe59c94173ff54757fbd8d0)

## UX verbeteren - kleuren van knoppen

De knoppen hebben momenteel allemaal dezelfde styling. Om de gebruiker van meer context te voorzien zullen we de `save`-knop een "primary" kleur geven en de `delete`-knop een "negative" kleur.

`StyledButton.vue`

```html
<script setup lang="ts">
  defineProps<{ type: "default" | "primary" | "negative" }>();
</script>

<template>
  <button
    :class="{
      primary: type === 'primary',
      negative: type === 'negative'
    }"
  >
    <slot />
  </button>
</template>

<style scoped>
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
</style>
```

`defineProps()` is een Vue macro, macros hoeven niet geïmporteerd te worden. Met `defineProps()` kan aangegeven worden welke `props` er verwacht worden. Een `prop` is een `property` die meegegeven kan worden aan een component. In dit geval wordt er een `prop` verwacht met de naam `type` en de waarde van deze `prop` moet een string zijn met de waarde `"default"`, `"primary"` of `"negative"`.

We kunnen een defaultwaarde meegeven door gebruik te maken van de `withDefaults()`-macro.

```html
<script setup lang="ts">
  withDefaults(defineProps<{ type: "default" | "primary" | "negative" }>(), {
    type: "default",
  });
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Wijzig de knop in de `HeroListView`-pagina.

`HeroListView.vue`

```html
<!-- Hier nog code -->
<StyledButton :type="'negative'" @click="deleteHero(selectedHero)">
  Delete
</StyledButton>
<!-- Hier nog code -->
```

De prop `type` kan via `attribute binding` een waarde toegekend krijgen. In dit geval wordt de string `'negative'` toegekend aan de `type`-property.

Wijzig de knoppen in de `HeroDetailsView`-pagina.

`HeroDetailsView.vue`

```html
<!-- hier nog code -->
<div class="buttons">
  <StyledButton @click="router.go(-1)">Back</StyledButton>
  <StyledButton
    :type="'primary'"
    @click="
      updateHero(hero);
      router.go(-1);
    "
  >
    Save
  </StyledButton>
  <StyledButton
    :type="'negative'"
    @click="
      deleteHero(hero);
      router.go(-1);
    "
  >
    Delete
  </StyledButton>
</div>
<!-- hier nog code -->
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/242a03ee2f9485fe365b4bd0c2365e5f103cc609)

## Hero toevoegen

Om een nieuwe hero toe te voegen, moeten er verschillende dingen gedaan worden:

- Er moet een route voorzien worden om de pagina te tonen.
- Er moet een functie voorzien worden om de held toe te voegen aan de lijst in de service.
- Er moet een pagina gemaakt worden waar de gegevens ingevoerd kunnen worden.
- Er moet een knop toegevoegd worden aan de `hero-list`-pagina om naar de nieuwe pagina te navigeren.

### Route

`src/routes/index.ts`

```ts
import { createRouter, createWebHashHistory } from "vue-router";

const router = createRouter({
  history: createWebHashHistory(),
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
          path: "heroes/add",
          name: "hero-add",
          component: () => import("@/views/HeroAddView.vue"),
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

Merk op: Op dit moment zal er nog een foutmelding zijn omdat de pagina `HeroAddView.vue` nog niet bestaat.

### Service

Merk op dat enkel de naam van de hero wordt doorgegeven bij het aanmaken van een nieuwe held. De `number`-property wordt niet doorgegeven, maar deze wordt bepaald op basis van de reeds bestaande heroes.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes = ref([
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
]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const addHero = (name: string) => {
    const maxNumber = Math.max(...heroes.value.map((h) => h.number));
    const newHero = { number: maxNumber + 1, name } satisfies Hero;
    heroes.value.push(newHero);
  };

  return {
    // hier nog code
    addHero,
  };
};

export { useHeroes };
```

### Pagina

Maak de pagina aan.

```sh
touch src/views/HeroAddView.vue
```

`HeroAddView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { ref } from "vue";
  import { useRouter } from "vue-router";

  const router = useRouter();
  const { addHero } = useHeroes();

  const name = ref("");
</script>

<template>
  <div class="title">Add hero</div>

  <div>name: <input v-model="name" /></div>

  <div class="buttons">
    <StyledButton @click="router.go(-1)">Back</StyledButton>
    <StyledButton
      :type="'primary'"
      @click="
        addHero(name);
        router.go(-1);
      "
    >
      Save
    </StyledButton>
  </div>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .buttons {
    margin-top: 1rem;
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Het is nu mogelijk om manueel naar `/heroes/add` te navigeren.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/e923401e694086c8fd044478267fd861e65d9093)

### Knop toevoegen HeroListView

`HeroListView.vue`

```html
<!-- Hier de script tag -->

<template>
  <div class="title">My Heroes</div>

  <!-- Hier nog code -->

  <StyledButton
    class="new-hero-button"
    :type="'primary'"
    @click="router.push({ name: 'hero-add' })"
  >
    New
  </StyledButton>

  <template v-if="selectedHero">
    <!-- Hier nog code -->
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */

  .new-hero-button {
    margin-top: 1rem;
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/0e0138f23c2f96a424753db29a61f9cfa77a3a19)

### Bug - warning in console

Bij het bekijken van de console, bij het rondklikken in de applicatie, zal er een warning te zien zijn:

```sh
[Vue warn]: Missing required prop: "type"
  at <StyledButton onClick=fn >
  at <MainLayout onVnodeUnmounted=fn<onVnodeUnmounted> ref=Ref< undefined > >
  at <RouterView>
  at <App>
```

We geven een default waarde mee aan de `type`-prop, maar TypeScript heeft dit niet door. Om deze warning te voorkomen, zullen we de `type`-prop als optioneel aanduiden met een `?`.

```html
<script setup lang="ts">
  withDefaults(defineProps<{ type?: "default" | "primary" | "negative" }>(), {
    type: "default",
  });
</script>
```

Merk op dat het `?` is toegevoegd aan de `type`-prop.

Nu heeft Vue door dat de `type`-prop optioneel is en zal er geen warning meer zijn.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/64245f934f27719181c9653a27c6bab194df7b5c)

## Local Storage

Om te zorgen dat de data niet verloren gaat wanneer de pagina wordt herladen, kunnen we gebruik maken van `localStorage`.

Aangezien de service de locatie is waar de data beheerd wordt, is dit de plaats waar we de data zullen opslaan en laden.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes = ref([
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
]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  const topHeroes = computed(() => {
    return heroes.value.slice(-4);
  });

  const findHero = (heroId: number) => {
    const matchingHero =
      heroes.value.find((hero) => hero.number === heroId) ?? null;
    return structuredClone(toRaw(matchingHero));
  };

  const updateHero = (hero: Hero) => {
    const index = heroes.value.findIndex((h) => h.number === hero.number);
    if (index !== -1) {
      heroes.value[index] = structuredClone(toRaw(hero));
    }
    saveHeroes(); // sla de heroes op
  };

  const deleteHero = (hero: Hero) => {
    heroes.value = heroes.value.filter((h) => h.number !== hero.number);
    if (selectedHero.value?.number === hero.number) {
      selectedHero.value = null;
    }
    saveHeroes(); // sla de heroes op
  };

  const addHero = (name: string) => {
    const maxNumber = Math.max(...heroes.value.map((h) => h.number));
    const newHero = { number: maxNumber + 1, name } satisfies Hero;
    heroes.value.push(newHero);
    saveHeroes(); // sla de heroes op
  };

  /**
   * voeg saveHeroes toe
   * saveHeroes wordt enkel binnen de service gebruikt
   * daarom staat deze functie niet in de return van useHeroes
   */
  const saveHeroes = () => {
    localStorage.setItem("heroes", JSON.stringify(heroes.value));
  };

  /**
   * voeg loadHeroes toe
   */
  const loadHeroes = () => {
    const savedHeroes = localStorage.getItem("heroes");
    if (savedHeroes) {
      heroes.value = JSON.parse(savedHeroes);
    }
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    findHero,
    updateHero,
    deleteHero,
    addHero,
    loadHeroes,
  };
};

export { useHeroes };
```

Deze wijzigingen zorgen ervoor dat alle wijzigingen ook opgeslagen worden in `localStorage`. Om te zorgen dat we de laatste toestand terug ingeladen wordt bij het herladen van de pagina, moeten we de `loadHeroes`-functie aanroepen bij het initialiseren van de applicatie.

`App.vue`

```html
<script setup lang="ts">
  import { onMounted } from "vue";
  import { RouterView } from "vue-router";
  import { useHeroes } from "./services/hero.service";

  const { loadHeroes } = useHeroes();

  onMounted(() => {
    loadHeroes();
  });
</script>

<template>
  <RouterView />
</template>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/1c6c8489c56f72af4ff7443f4757a5cc00244579)

## Conclusie

Een service wordt gebruikt om data te beheren. Deze data kan gebruikt worden als `global state` in de applicatie.

Doordat alle logica rondom `heroes` nu in een aparte service/composable zit, is het in de toekomst makkelijker om bijvoorbeeld een echte API te gebruiken, de implementatie kan op één plaats gewijzigd worden.
