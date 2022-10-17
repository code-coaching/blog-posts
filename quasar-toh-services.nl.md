---
postUuid: 32e8808f-e109-4b0f-ab7a-afe0fb3422db
title: Quasar Tour of Heroes - Composables & Services
slug: quasar-toh-composables-en-services
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De beginsituatie is een bestaande Quasar-project met een layout componenent en werkende pagina's. De eindsituatie is een Quasar-project waarin de data verplaatst is naar services.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-components.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-components)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-service-functions.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-services-composables)

## Composables & Services

Een composable is een verzameling van herbruikbare functionaliteit. Dit wordt ook wel eens een service genoemd.

Momenteel is er een "probleem" met de `hero`-data. De lijst met `hero`-objecten wordt meerdere keren gedefinieerd in verschillende componenten.

Zowel in de pagina `HeroDetails` als in de pagina `HeroList` wordt de lijst met `hero`-objecten gedefinieerd:

```ts
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
```

Dit is een "probleem". Wanneer er een nieuwe hero wordt toegevoegd, verwijderd of gewijzigd dan moet dit op alle pagina's gebeuren.

## Hero Service

Maak een service aan om de `hero`-data te beheren. Gebruik de service vervolgens op de locatie waar de lijst met `hero`-objecten gebruikt wordt.

Bij het aanmaken van de service is het belangrijk om te begrijpen of een variabele scoped of globaal is. Om dit te demonstreren wordt er gebruik gemaakt van een kleinere lijst.

Scoped binnen de functie:

```ts
import { ref } from "vue";

const useHeroes = () => {
  const heroes = ref([
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
  ]);

  return {
    heroes,
  };
};

export { useHeroes };
```

In het geval van een variabele in de scope van de functie, zal **overal een nieuwe lijst** met `hero`-objecten worden gemaakt.

Globale scope:

```ts
import { ref } from "vue";

const heroes = ref([
  { number: 11, name: "Mr. Nice" },
  { number: 12, name: "Narco" },
]);

const useHeroes = () => {
  return {
    heroes,
  };
};

export { useHeroes };
```

In het geval van een variabele in de globale scope van het bestand, zal **overal dezelfde lijst** met `hero`-objecten worden gebruikt.

Aangezien de lijst met `hero`-objecten overal moet wijzen naar dezelfde lijst, zal in de code gebruik gemaakt worden van een `ref`-variabele in de globale scope.

Maak een nieuwe map aan in de `src`-map met de naam `services`. In deze map voorzie een service/composable met de naam `hero.service.ts`.

In het bestand wordt de `hero`-data gedefinieerd:

```ts
import { ref } from "vue";

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

const useHeroes = () => {
  return {
    heroes,
  };
};

export { useHeroes };
```

Vervolgens wordt de service geïmporteerd op de plaatsen waar momenteel een hard-coded lijst met `hero`-objecten wordt gebruikt.

Vervang de hard-coded data in zowel `HeroList.vue` als `HeroDetails.vue` door de data uit de service.

Verwijder de hard-coded lijst:

```ts
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
```

Voeg de import van de service toe:

```ts
import { useHeroes } from "src/services/hero.service";
```

Gebruik de heroes uit de service:

```ts
const { heroes } = useHeroes();
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/f4ff9c923fb9964540e686e0de23e6773705fb63)

## Selected Hero

De `selectedHero` wordt momenteel alleen gebruikt in de pagina `HeroList`. Het is dus niet per se nodig om deze te verplaatsen naar de service. Maar het is wel handig om alle data rondom heroes te verplaatsen naar de service, op deze manier kan er altijd gewerkt worden via de service als er met `hero`-data gewerkt wordt.

Verplaats de `selectedHero` naar de service:

```ts
import { Hero } from "src/components/models";
import { ref, Ref } from "vue";

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

const selectedHero: Ref<Hero> = ref() as Ref<Hero>;

const useHeroes = () => {
  return {
    heroes,
    selectedHero,
  };
};

export { useHeroes };
```

Hierbij is het belangrijk om te beslissen of de variabele globaal of scoped is. Indien de variabele scoped is in de functie, zal er per component waar de service gebruikt wordt een unieke `selectedHero` zijn. Indien de variabele globaal is, zal overal dezelfde `selectedHero` worden gebruikt.

Met de keuze die hierboven gemaakt wordt, zal er dus één selectedHero zijn voor het hele project. Overal waar `useHeroes()` gebruikt wordt, waaruit vervolgens de `selectedHero` wordt gehaald, zal deze dezelfde waarde hebben.

In de `HeroList.vue` wordt de `selectedHero` geïmporteerd vanuit de service:

```ts
const { heroes, selectedHero } = useHeroes();
```

Bevestig dat het selecteren van een hero in de lijst nog altijd werkt op de `HeroList`-pagina.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/9e03cce4f20713f838f8c09cd35eadcbad7fb4eb)

## Top Heroes

Momenteel worden de top heroes getoond in de `Dashboard`-pagina. Dit is momenteel enkel hard-coded data in de template van de `Dashboard`-pagina. Maar, dit zijn de namen van `hero`-objecten die in de lijst staan in de service.

Om te zorgen dat er gebruik gemaakt kan worden van de service, moet er een `script`-tag toegevoegd worden. Hierin wordt de service geïmporteerd.

```html
<template>
  <div class="title">Top Heroes</div>

  <div class="top-heroes">
    <div v-for="(hero, index) in topHeroes" :key="index" class="top-hero">
      {{ hero.name }}
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent, computed } from "vue";
  import { useHeroes } from "src/services/hero.service";

  export default defineComponent({
    setup() {
      const { heroes } = useHeroes();

      const topHeroes = heroes.value.slice(0, 4);

      return {
        topHeroes,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
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

    &:hover {
      background-color: #eeeeee;
      color: #5f7d8c;
      cursor: pointer;
    }
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/8e735ac022ca8e24321d0cfddf619ebb6062c059)

### Verplaatsen topHeroes naar de service

Er is besproken dat het handig is om alle data rondom heroes te verplaatsen naar de service. De `topHeroes`-property is momenteel toegevoegd in de `Dashboard`-pagina. Deze wordt verplaats naar de service:

```ts
import { Hero } from "src/components/models";
import { ref, Ref } from "vue";

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

const selectedHero: Ref<Hero> = ref() as Ref<Hero>;

const useHeroes = () => {
  const topHeroes = heroes.value.slice(0, 4);

  return {
    heroes,
    selectedHero,
    topHeroes,
  };
};

export { useHeroes };
```

Merk op dat de `topHeroes`-variabele in de scope van de functie staat. Een simpele regel om te hanteren is om `state` in de globale scope te plaatsen en afgeleide waarden in de scope van de functie te plaatsen. Ook functies die de `state` in de globale scope wijzigen, kunnen in de scope van de functie geplaatst worden.

Het `script`-gedeelte van de `Dashboard`-pagina wordt aangepast:

```ts
import { defineComponent } from "vue";
import { useHeroes } from "src/services/hero.service";

export default defineComponent({
  setup() {
    const { topHeroes } = useHeroes();

    return {
      topHeroes,
    };
  },
});
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/828571746cf61ff4884828d46ebec2311a90d0f7)

## Doorklikken van dashboard naar detail

Voeg een functie toe die naar de detailpagina navigeert wanneer er op een hero wordt geklikt.

In de setup van de `Dashboard`-pagina wordt de functie `navigateToHero` toegevoegd:

```ts
const router = useRouter();

const navigateToHero = (hero: Hero) => {
  void router.push({
    name: ROUTE_NAMES.HERO_DETAILS,
    params: {
      id: hero.number,
    },
  });
};
```

Voer de functie uit wanneer er op een hero wordt geklikt:

```html
<div
  v-for="(hero, index) in topHeroes"
  :key="index"
  class="top-hero"
  @click="navigateToHero(hero)"
>
  {{ hero.name }}
</div>
```

Wanneer er nu op een hero wordt geklikt, dan wordt er naar de `HeroDetails`-pagina genavigeerd. Wanneer er op de `HeroDetails`-pagina op de `back`-knop wordt geklikt, dan wordt er naar de `HeroList`-pagina genavigeerd. Dit is het gewenste gedrag wanneer er vanuit de `HeroList`-pagina naar de `HeroDetails`-pagina wordt gegaan. Dit is **niet** het gewenste gedrag wanneer er vanuit de `Dashboard`-pagina naar de `HeroDetails`-pagina wordt gegaan.

Wijzig de functionaliteit van de `HeroDetails`-pagina:

`HeroDetails.vue`:

```ts
const moveBack = () => void router.go(-1);
```

Probeer opnieuw zowel vanuit de `HeroList` als de `Dashboard`-pagina naar de `HeroDetails`-pagina te navigeren en vervolgens op de `back`-knop te klikken.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/1502f1e7377ae9d9413c63a4f1d431e5012738ec)

## Hero wijzigen

De gewenste functionaliteit is dat er een `save`-knop is die de wijzigingen in de hero opslaat op de detailpagina.

Één van de mogelijkheden is dat de globale `heroes`-variabele wordt aangepast in de `HeroDetails`-pagina`. In plaats van dit te doen, gaat de funtionaliteit rechtstreeks in de service geïmplementeerd worden.

Het idee is dat de globale `ref`-variabelen gebruikt worden om de waarden uit te lezen. Het wijzigen van deze globale `ref`-variabelen wordt gedaan via functies in de service.

`hero.service.ts`:

```ts
const useHeroes = () => {
  const topHeroes = heroes.value.slice(0, 4);

  const editHero = (hero: Hero) => {
    heroes.value = heroes.value.map((h) =>
      h.number === hero.number ? hero : h
    );
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    editHero,
  };
};
```

Koppel de functie in de `HeroDetails`-pagina.

```html
<template>
  <div v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
    <StyledButton class="save-button" @click="saveHero()">Save</StyledButton>
  </div>

  <div v-else class="title">Hero not found!</div>
</template>
```

De tweede `StyledButton` is nieuw.

```js
export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const route = useRoute();
    const router = useRouter();
    const hero: Ref<Hero> = ref() as Ref<Hero>;
    const { heroes, editHero } = useHeroes();

    onBeforeMount(() => {
      const { id } = route.params;
      if (id) {
        const matchingHero = heroes.value.find((h) => h.number === +id);
        if (matchingHero) hero.value = matchingHero;
      }
    });

    const moveBack = () => void router.go(-1);
    const saveHero = () => editHero(hero.value);

    return {
      hero,
      moveBack,
      saveHero,
    };
  },
});
```

### Verwijderen van de two-way binding

Verwijderen van de two-way binding in de `HeroDetails`-pagina. Momenteel wordt de hero uit de lijst gehaald. Doordat JavaScript bij objecten en arrays werkt met `by reference` (Nederlands: via referentie), verwijst de `hero`-ref naar hetzelfde object als het object dat uit de globale `heroes`-lijst wordt gehaald.

Om te zorgen dat dit niet gebeurt, moet de `hero`-ref worden gewijzigd naar een nieuw object. Dit kan gedaan worden door gebruik te maken van de `spread`-operator.

Voor de wijziging:

```ts
onBeforeMount(() => {
  const { id } = route.params;
  if (id) {
    const matchingHero = heroes.value.find((h) => h.number === +id);
    if (matchingHero) hero.value = matchingHero;
  }
});
```

Na de wijziging:

```ts
onBeforeMount(() => {
  const { id } = route.params;
  if (id) {
    const matchingHero = heroes.value.find((h) => h.number === +id);
    if (matchingHero) hero.value = { ...matchingHero };
  }
});
```

Merk op: de `spread`-operator "kopieert" het object, hierdoor is het resultaat een nieuw object, er is geen referentie meer naar het originele object. **Maar** dit werkt maar één niveau diep. Indien het object een property met een object of een array bevat. In die geval zou ook die property via de `spread`-operator gekopieerd moeten worden.

### Verplaatsen van findHero naar de service

De `Hero`-service staat in voor alle functionaliteit van de `Hero`-data. De functionaliteit om een hero te zoeken, wordt verplaatst naar de service.

In de service:

```ts
const findHero = (id: number): Hero | undefined => {
  const matchingHero = heroes.value.find((h) => h.number === +id);
  if (matchingHero) return { ...matchingHero };
};
```

```ts
const { editHero, findHero } = useHeroes();

onBeforeMount(() => {
  const { id } = route.params;
  if (id) {
    const matchingHero = findHero(+id);
    if (matchingHero) hero.value = matchingHero;
  }
});
```

Bij het uittesten van de applicatie, kan de hero niet langer gewijzigd worden door simpelweg de naam te wijzigen en vervolgens op `back` te klikken. Er **moet** op `save` geklikt worden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/ac3a44bc8545ea14a361ec41e60fb779e673683f)

### Mooier stylen van de knoppen

Plaats de knoppen in een wrapper div. Gebruik de wrapper div om de knoppen te positioneren.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/6122975233efc7f642db7b7a85da5d7e8f7b169a)

De Quasar SCSS-variabelen `css/quasar.variables.scss`:

```scss
$primary: #1976d2;
$secondary: #26a69a;
$accent: #9c27b0;

$dark: #1d1d1d;

$positive: #21ba45;
$negative: #c10015;
$info: #31ccec;
$warning: #f2c037;
```

Zijn beschikbaar als CSS-variabelen, dit is te zien in de `inspect`-modus van de browser:

```css
:root {
  --q-primary: #1976d2;
  --q-secondary: #26a69a;
  --q-accent: #9c27b0;
  --q-positive: #21ba45;
  --q-negative: #c10015;
  --q-info: #31ccec;
  --q-warning: #f2c037;
  --q-dark: #1d1d1d;
  --q-dark-page: #121212;
}
```

Deze CSS-variabelen kunnen gebruikt worden in de `style`-tag. De `--q-primary`-variabele is de kleur die gebruik wordt om hoofdacties aan te duiden. Voorzie een manier om aan te geven dat een `StyledButton` gestyled moet worden als een knop die `--q-primary` gebruikt.

Voeg een prop toe aan de `StyledButton`-component:

```html
<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    props: {
      primary: {
        type: Boolean,
        default: false,
      },
    },
  });
</script>
```

Gebruik deze prop om dynamisch een class toe te voegen indien de prop `primary` true is.

```html
<template>
  <button :class="{ primary: primary }">
    <slot />
  </button>
</template>
```

Overschrijf in de `style`-tag de styling indien de class `primary` aanwezig is.

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    background-color: darken($primary, 10%);
    color: white;
  }
}
```

Merk op dat zowel de CSS-variabele `var(--q-primary)` als de SCSS-variabele `$primary` gebruikt kan worden.

Er zijn twee dingen belangrijk om te weten:

- Indien andere SCSS-functies, zoals `darken` gebruikt worden, dan **moet** de SCSS-variant gebruikt worden.
- Indien SCSS-functies gebruikt worden, dan is de kleur niet wijzigbaar door de CSS-variabele aan te passen in de browser. Dit is enkel belangrijk indien er themability voorzien wordt waarvoor geen compilatie nodig is.

De `primary StyledButton` kan nu gebruikt worden. Dit kan door de `primary`-prop te gebruiken:

```html
<StyledButton :primary="true">Save</StyledButton>
```

Of de alternatieve, kortere syntax om een boolean prop te gebruiken:

```html
<StyledButton primary>Save</StyledButton>
```

Dit is de voorkeur voor deze tutorial.

```html
<StyledButton primary class="save-button" @click="saveHero()">
  Save
</StyledButton>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/b82ba86c6bdd3f8aa8b423f141d78e80c7444330)

## Hero verwijderen

Voorzie functionaliteit om een hero te verwijderen in de service. Koppel deze functionaliteit in de `HeroList`-component en in de `HeroDetail`-component.

```ts
const deleteHero = (hero: Hero) => {
  heroes.value = heroes.value.filter((h) => h.number !== hero.number);
};
```

Voorzie een knop om een hero te verwijderen. Deze knop moet gestyled kunnen worden als een `StyledButton` met een `negative` styling.

Implementeer de `negative` styling in de `StyledButton`-component.

```html
<template>
  <button
    :class="{
      primary: primary,
      negative: negative,
    }"
  >
    <slot />
  </button>
</template>
```

```ts
export default defineComponent({
  props: {
    primary: {
      type: Boolean,
      default: false,
    },
    negative: {
      type: Boolean,
      default: false,
    },
  },
});
```

```scss
.negative {
  background-color: var(--q-negative);
  color: white;

  &:hover {
    background-color: darken($negative, 5%);
    color: white;
  }
}
```

Gebruik deze knop in de `HeroList`-component.
Voeg een `StyledButton` toe aan de `HeroList`-component met de `negative` prop.

```html
<StyledButton negative @click="onDeleteClick()">Delete</StyledButton>
```

Voeg deze knop ook toe in de `HeroDetail`-component.

```html
<div class="buttons">
  <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
  <StyledButton primary class="save-button" @click="saveHero()">
    Save
  </StyledButton>
  <StyledButton negative @click="removeHero()"> Delete </StyledButton>
</div>
```

Tijdens het toevoegen van deze knop, merken we plots op dat de classes `back-button` en `save-button` niet gebruikt worden. Deze worden verwijderd.

```html
<div class="buttons">
  <StyledButton @click="moveBack()">Back</StyledButton>
  <StyledButton primary @click="saveHero()">Save</StyledButton>
  <StyledButton negative @click="removeHero()">Delete</StyledButton>
</div>
```

Bij het bekijken van de `HeroList`-component, is te zien dat de knoppen tegen elkaar staan. Dit probleem is al opgelost in de `HeroDetail`-component. Om duplicatie van code te vermijden, wordt er een aparte component gemaakt om dit probleem één keer op te lossen. Vervolgens wordt deze component gebruikt op de locaties waar er meerdere knoppen naast elkaar moeten staan.

`src/components/ButtonGroup.vue`

```html
<template>
  <div class="buttons">
    <slot></slot>
  </div>
</template>

<style lang="scss" scoped>
  .buttons {
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Gebruik deze nieuwe component vervolgens op beide locaties.

`HeroDetails.vue`

```html
<ButtonGroup class="button-group">
  <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
  <StyledButton primary @click="saveHero()">Save</StyledButton>
  <StyledButton negative @click="removeHero()">Delete</StyledButton>
</ButtonGroup>
```

Merk op dat er een class `button-group` is toegevoegd om de `margin-top: 1rem;` toe te passen.

`HeroList.vue`

```html
<ButtonGroup>
  <StyledButton @click="onDetailsClick()">Details</StyledButton>
  <StyledButton negative @click="onDeleteClick()">Delete</StyledButton>
</ButtonGroup>
```

Vergeet in beide gevallen niet om de component te importeren en toe te voegen aan de `components`-property.

Koppel de `deleteHero`-functie van de service aan de `delete`-knoppen. De implementatie is in beide gevallen net iets anders.

`HeroDetail.vue`

```ts
const moveBack = () => void router.go(-1);
const saveHero = () => editHero(hero.value);
const removeHero = () => {
  deleteHero(hero.value);
  moveBack();
};
```

Wanneer een hero verwijderd wordt, kan deze niet meer getoond worden. Er wordt vervolgens terug genavigeerd naar de pagina waarvan genavigeerd werd. Dit kan de `Dashboard`-pagina zijn of de `HeroList`-pagina`.

Voeg dezelfde logica toe aan het saven van een hero.

```ts
const moveBack = () => void router.go(-1);
const saveHero = () => {
  editHero(hero.value);
  moveBack();
};
const removeHero = () => {
  deleteHero(hero.value);
  moveBack();
};
```

`HeroList.vue`

```ts
const onDeleteClick = () => {
  deleteHero(selectedHero.value);
  selectedHero.value = {} as Hero;
};
```

Er wordt een casting gedaan van een leeg object naar een `Hero`. Dit is hoe vanaf nu wordt aangetoond dat de `selectedHero` geen `Hero` is. Dit vereist ook een refactoring in de service.

De huidige implementatie maakt van een `ref`-variabele die de waarde `undefined` heeft, een `Hero`. Het probleem hiermee is dat `undefined` nooit opnieuw toegekend kan worden en dus gaat er logica geschreven worden rondom dit principe. Een betere manier is om de `selectedHero`-variabele te initialiseren met een leeg object. Een leeg object kan altijd gecast worden naar een `Hero` en vervolgens is de logica die geschreven wordt altijd bruikbaar.

Voor de wijziging:

```ts
const selectedHero: Ref<Hero> = ref() as Ref<Hero>;
```

Na de wijziging:

```ts
const selectedHero = ref({} as Hero);
```

De explicitie typing is weggehaald. TypeScript geeft impliciet de typing `Ref<Hero>` aan de variabele. Dit kan gezien worden door over de variabele te hoveren in VSCode.

Als de applicatie nu uitgetest wordt, zal er een error zijn in de `HeroList`-component. Dit is omdat een leeg object, gezien wordt als een `truthy`-waarde. De `v-if="selectedhero"` evalueert dus naar `true` en de elementen worden getoond. In deze elementen wordt gebruikgemaakt van `selectedHero.name`. Aangezien deze property gebruikt wordt in de functie `upperCase(selectedHero.name)`, zal er een error zijn.

Wijzig de `v-if`-conditie naar `selectedHero?.name`.

```html
<div v-if="selectedHero?.name">
  <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
  <ButtonGroup>
    <StyledButton @click="onDetailsClick()">Details</StyledButton>
    <StyledButton negative @click="onDeleteClick()">Delete</StyledButton>
  </ButtonGroup>
</div>
```

Dit is een verkorte syntax voor `v-if="selectedHero && selectedHero.name"`.

In plaats van de `selectedHero`-variabele rechtstreeks te wijzigen, zal een functie voorzien worden in de service om deze variabele te wijzigen.

`src/services/hero.service.ts`

```ts
const resetSelectedHero = () => (selectedHero.value = {} as Hero);
```

Gebruik deze functie om de `selectedHero`-variabele te resetten in de `HeroList`-component`.

```ts
const onDeleteClick = () => {
  deleteHero(selectedHero.value);
  resetSelectedHero();
};
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/8fc9fe745568f0dc5b1b8ea0c351e3913142b9f8)

## Hero toevoegen

Om een nieuwe hero toe te voegen, moeten er verschillende dingen gedaan worden:

- Er moet een route voorzien worden om de pagina te tonen.
- Er moet een functie voorzien worden om de held toe te voegen aan de lijst in de service.
- Er moet een pagina gemaakt worden waar de gegevens ingevoerd kunnen worden.
- Er moet een knop toegevoegd worden aan de `HeroList`-pagina om naar de nieuwe pagina te navigeren.

### Route

Het is niet nodig om de route vóór de wildcard route `/heroes/:id` te plaatsen. Dit is een voorkeur, eerste alle expliciete routes definiëren en vervolgens de wildcard route.

```ts
const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [
      {
        name: ROUTE_NAMES.DASHBOARD,
        path: "",
        component: () => import("pages/Dashboard.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_LIST,
        path: "/heroes",
        component: () => import("pages/HeroList.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_ADD,
        path: "/heroes/add",
        component: () => import("pages/HeroAdd.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_DETAILS,
        path: "/heroes/:id",
        component: () => import("pages/HeroDetails.vue"),
      },
    ],
  },

  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];
```

### Service

Merk op dat enkel de naam van de hero wordt doorgegeven bij het aanmaken van een nieuwe held. De `number`-property wordt niet doorgegeven, maar deze wordt bepaald op basis van de reeds bestaande heroes.

```ts
const addHero = (name: string) => {
  const maxNumber = Math.max(...heroes.value.map((h) => h.number));
  const newHero = { number: maxNumber + 1, name };
  heroes.value.push(newHero);
};
```

### HeroAdd.vue

Na het toevoegen van de pagina is het mogelijk om manueel te navigeren naar `/heroes/add`.

```html
<template>
  <div class="title">Add hero</div>

  <div>name: <input v-model="name" /></div>

  <ButtonGroup class="button-group">
    <StyledButton @click="moveBack()">Back</StyledButton>
    <StyledButton primary @click="saveHero()">Save</StyledButton>
  </ButtonGroup>
</template>

<script lang="ts">
  import { defineComponent, ref } from "vue";
  import StyledButton from "components/StyledButton.vue";
  import ButtonGroup from "components/ButtonGroup.vue";
  import { useRouter } from "vue-router";
  import { useHeroes } from "src/services/hero.service";

  export default defineComponent({
    components: {
      StyledButton,
      ButtonGroup,
    },
    setup() {
      const router = useRouter();
      const name = ref("");
      const { addHero } = useHeroes();

      const moveBack = () => void router.go(-1);
      const saveHero = () => {
        addHero(name.value);
        moveBack();
      };

      return {
        name,
        moveBack,
        saveHero,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .button-group {
    margin-top: 1rem;
  }
</style>
```

### Knop toevoegen HeroList.vue

```html
<StyledButton class="new-hero-button" primary @click="onNewClick()">
  New Hero
</StyledButton>
```

```ts
const onNewClick = () => void router.push({ name: ROUTE_NAMES.HERO_ADD });
```

```scss
.new-hero-button {
  margin-top: 1rem;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/e35ae4b4fe9c056afe9bbdb07637f690a7c33d2a)

## readonly

In de service, voeg de `readonly`-functie, voorzien door Vue, toe aan de globale state `heroes` en `selectedHero`.

```ts
import { readonly, ref } from "vue";
```

```ts
return {
  heroes: readonly(heroes),
  selectedHero: readonly(selectedHero),
  topHeroes,
  editHero,
  findHero,
  deleteHero,
  resetSelectedHero,
  addHero,
};
```

Na deze wijziging is er een error in de console wanneer de applicatie opgestart wordt:

```ts
ERROR in src/pages/HeroList.vue:52:20
TS2540: Cannot assign to 'value' because it is a read-only property.
    50 |
    51 |     const onClickHero = (hero: Hero) => {
  > 52 |       selectedHero.value = hero;
       |                    ^^^^^
    53 |     };
    54 |
    55 |     const onDetailsClick = () => {
```

In de `HeroList.vue`-pagina wordt er nog een waarde direct toegekend aan de `selectedHero`-ref.

Om dit te voorkomen, wordt een functie toegevoegd `setSelectedHero`. Deze functie zal vervolgens in de service de `selectedHero`-ref wijzigen.

```ts
const setSelectedHero = (hero: Hero) => (selectedHero.value = hero);
```

Gebruik deze functie in de `HeroList.vue`-pagina.

Voor de wijziging:

```ts
const { heroes, selectedHero, deleteHero, resetSelectedHero } = useHeroes();

const onClickHero = (hero: Hero) => {
  selectedHero.value = hero;
};
```

Na de wijziging:

```ts
const { heroes, selectedHero, deleteHero, resetSelectedHero, setSelectedHero } =
  useHeroes();

const onClickHero = (hero: Hero) => setSelectedHero(hero);
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/3c48e4c30f2c9cc67d595974e3f95109807000da)

## Conclusie

Een service/composable wordt gebruikt om data te beheren. Deze data kan gebruikt worden als `global state` in de applicatie. Om te zorgen dat `global state` niet rechtstreeks gewijzigd kan worden, wordt deze exposed via een `readonly`-functie.

Doordat alle logica rondom `heroes` nu in een aparte service/composable zit, is het in de toekomst makkelijker om bijvoorbeeld een echte API te gebruiken, de implementatie kan op één plaats gewijzigd worden.
