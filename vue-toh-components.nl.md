---
postUuid: 59df7c59-2dda-4d33-99f1-e463ac520bca
title: Vue Tour of Heroes - Componenten
slug: vue-toh-componenten
tags:
  - Vue
  - Tour of Heroes
categories:
  - Frontend
youtubeIds:
  - gehx4a_3LRk
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De beginsituatie is een bestaand Vue-project met een layoutcomponent en placeholderpagina's. De eindsituatie is een Vue-project waarin de pagina's voorzien zijn van componenten.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-styling.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/angular-toh-layouts-and-pages)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-components.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/vue-toh-components)

## Componenten

Componenten zijn essentiële bouwstenen van een Vue-applicatie. In de applicatie is al een component te zien voor de layout, genaamd `MainLayout.vue`. Er zijn ook al componenten voor de verschillende pagina's.

De verschillende pagina's zijn:

- DashboardView.vue - dit is een pagina waarop de `top heroes` getoond zullen worden.
- HeroListView.vue - dit is een pagina waarop de lijst met `heroes` getoond zullen worden.
- HeroDetailView.vue - dit is een pagina waarop de details van een `hero` getoond zullen worden.

Een layout is een component en bevat HTML-elementen en andere componenten die op alle pagina's aanwezig zijn.

Een pagina is een component en bevat HTML-elementen en andere componenten die op een specifieke pagina aanwezig zijn.

Tijdens dit artikel zullen er nieuwe componenten worden toegevoegd aan de applicatie.

## Pagina stylen: dashboard

![dashboard](/img/blog/quasar-toh-dashboard.png)

Wijzig de inhoud van `src/views/DashboardView.vue`:

```html
<template>
  <div>Top Heroes</div>

  <div>Narco</div>
  <div>Bombasto</div>
  <div>Celeritas</div>
  <div>Magneta</div>
</template>
```

![Dashboard elementen](/img/blog/quasar-toh-dashboard-elements.png)

De elementen zijn aanwezig, maar de styling komt niet overeen.

Voeg classes toe aan de elmenten om deze te kunnen stylen.

`DashboardView.vue`

```html
<template>
  <div class="title">Top Heroes</div>

  <div class="top-heroes">
    <div class="top-hero">Narco</div>
    <div class="top-hero">Bombasto</div>
    <div class="top-hero">Celeritas</div>
    <div class="top-hero">Magneta</div>
  </div>
</template>

<style scoped>
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
</style>
```

Merk op dat er een `container element` is toegevoegd rondom de `hero`-elementen.

![Tour of Heroes Dashboard Styling](/img/blog/quasar-toh-dashboard-styling.png)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/69501d8b48df06caece7505eda79692ce700b9aa)

## Pagina stylen: hero-list

![HeroList](/img/blog/quasar-toh-hero-list.png)

Wijzig de inhoud van `HeroListView.vue`:

```html
<template>
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
</template>
```

![hero list elementen](/img/blog/quasar-toh-hero-list-elements.png)

Style de elementen, voeg waar nodig extra elementen toe om het gewenste resultaat te bekomen.

`HeroListView.vue`

```html
<template>
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
</template>

<style scoped>
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
</style>
```

![Tour of Heroes styled hero list](/img/blog/quasar-toh-hero-list-styled.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/00da00968f0b2c0f29d7a4eb7af14d77eb1950da)

## Pagina uitbreiden: HeroList

Om te zorgen dat een hero geselecteerd kan worden, moet er een `click handler` toegevoegd worden aan elke hero. Aangezien met de huidige code, er tien aparte elementen zijn die een hero voorstellen, zouden er tien `click handlers` nodig zijn.

Om dit te vermijden, kan er gebruik gemaakt worden van een `v-for`-loop in de `template`, gecombineerd met een lijst van heroes.

`HeroListView.vue`

```html
<script setup lang="ts">
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
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div v-for="(hero, index) in heroes" :key="index" class="hero">
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Hier wordt gebruikgemaakt van een `v-for`-loop. Dit is onderdeel van Vue. De `key` wordt gebruikt om te zorgen dat Vue beter weet welk specifiek element gewijzigd moet worden. De `key` moet uniek zijn binnen de `v-for`-loop.

In het `<script>` wordt de `heroes`-lijst gedefinieerd, deze array bevat objecten met de properties `number` en `name`. Elk object stelt een hero voor.

Properties van de class zijn beschikbaar in de template/html.

### Een hero selecteren

![Klik hero in lijst](/img/blog/quasar-toh-click-hero-in-list.png)

Om te zorgen dat een hero geselecteerd kan worden, zijn er enkele dingen nodig:

- een `click handler` op elk hero element, dit is een functie die wordt aangeroepen als er op een hero wordt geklikt
- een `selectedHero`-variabele die de geselecteerde hero zal bevatten

```html
<script setup lang="ts">
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

  let selectedHero: null | { number: number; name: string } = null;

  const onClickHero = (hero: { number: number; name: string }) => {
    selectedHero = hero;
  };
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
      class="hero"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div>{{ selectedHero }}</div>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Merk op: Dit werkt **niet** in de browser. Test dit door op een hero te klikken. Er zal geen `selectedHero` getoond worden. Dit wijzigen we in de volgende stap.

De `click handler` wordt toegvoegd via `@click`. Dit wordt gebruikt om een functie aan te roepen wanneer er op een element wordt geklikt. De functie die wordt aangeroepen wordt gedefinieerd in de `<script>`-tag van dit component.

De functie die wordt aangeroepen wordt gedefinieerd als `onClickHero`, met als argument een hero. Deze functie wordt aangeroepen als er op een hero wordt geklikt.

In de functie wordt aan de `selectedHero`-variabele een nieuwe waarde toegekend. Deze variabele wordt gebruikt in de template om de geselecteerde hero te tonen `{{ selectedHero }}`.

Merk op dat de `selectedHero` **niet** automatisch geüpdatet wordt in de browser bij het selecteren van een hero. Dit komt omdat Vue niet weet dat de `selectedHero`-variabele gewijzigd is. Om dit op te lossen, kan er gebruik gemaakt worden van de `reactive`-functionaliteit van Vue.

```html
<script setup lang="ts">
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

  /**
   * const i.p.v. let - een `ref` is een object met een property `value`
   * Het is de property `value` die we gaan wijzigen, niet de variabele zelf
   */
  const selectedHero: Ref<null | { number: number; name: string }> = ref(null);

  const onClickHero = (hero: { number: number; name: string }) => {
    /**
     * Ken een nieuwe waarde toe aan de property `value` van de `ref`
     * Hierdoor weet Vue dat de `ref` gewijzigd is en zal de template opnieuw renderen
     */
    selectedHero.value = hero;
  };
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
      class="hero"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div>{{ selectedHero }}</div>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Merk op dat een `ref`-variabele gebruikt wordt. Een `ref` is een object met een property `value`. Wanneer we de waarde van een `ref` gebruiken in de `<script>`-tag, dan gebruiken we de property `value` van de `ref`. In de template wordt dit automatisch afgehandeld door Vue, vandaar dat je in de template `{{ selectedHero }}` ziet staan en niet `{{ selectedHero.value }}`.

Start de applicatie opnieuw, selecteer een hero en merk dat de `selectedHero` getoond wordt in de browser.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/00da00968f0b2c0f29d7a4eb7af14d77eb1950da)

![selectedHero-variabele in HeroList](/img/blog/quasar-toh-selected-hero-ref.png)

Een kleine refactoring van de code om een `interface` toe te voegen.

`HeroListView.vue`

```html
<script setup lang="ts">
  import { ref, type Ref } from "vue";

  /**
   * Een interface is TypeScript-functionaliteit om een complex type te definiëren
   */
  interface Hero {
    number: number;
    name: string;
  }

  // Gebruik de Hero interface
  const heroes: Array<Hero> = [
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

  // Gebruik de Hero interface
  const selectedHero: Ref<null | Hero> = ref(null);

  // Gebruik de Hero interface
  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

De interface is toegevoegd om het complexe type `Hero` voor te stellen. Deze interface is niet nodig, maar het is een goede manier om de code leesbaarder te maken.

Voor het toevoegen van de Hero-interface:

Het voordeel van deze TypeScript interface, is dat overal waar een Hero-object wordt gebruikt, kan de interface gebruikt worden om het type aan te duiden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/aec70518655c4a0eb4ac16c24c08ef60ce2d82db)

### Stylen van selectedHero

`HeroListView.vue`

```html
<script setup lang="ts">
  // Hier de bestaande code
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
      class="hero"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <template v-if="selectedHero">
    <div class="title">{{ selectedHero.name }} is my hero</div>
    <button>Details</button>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Merk op dat de `v-if`-statement op een `template`-tag gebruikt wordt. Dit zorgt ervoor dat er geen extra element wordt toegevoegd aan de DOM, de elementen in de `template`-tag worden rechtstreeks toegevoegd aan de DOM.

De elementen zijn aanwezig, de styling is nog niet in orde.

![elementen geselecteerde hero](/img/blog/quasar-toh-selected-hero-elements.png)

Nu de geselecteerde hero gekend is, moet de styling van de gekozen hero gewijzigd worden. De naam van de geselecteerde hero moet getoond worden in allemaal hoofdletters. De knop moet dezelfde styling krijgen als de al bestaande knoppen.

#### Styling van active hero

Toevoegen van een dynamische class:

```html
<div
  :class="{
    'hero--active': hero.number === selectedHero?.number
  }"
></div>
```

Merk op dat er een `:class` gebruikt wordt om variabelen te kunnen gebruiken in het toekennen van een class. Dit kan gelezen worden als "de class `hero--active` wordt toegevoegd aan dit element als de statement `truthy` is.

De statement `hero.number === selectedHero?.number` evalueert naar `true` als het nummer van de hero waarover geïtereerd wordt gelijk is aan het nummer van de geselecteerde hero.

Er wordt gebruik gemaakt van de `?.`-operator. Deze operator zorgt ervoor dat `.number` enkel gelezen wordt als `selectedHero` een waarde heeft. Indien `selectedHero` `undefined` is, wordt `.number` niet gelezen en evalueert de statement `hero.number === selectedHero?.number` naar `false`.

`HeroListView.vue`

```html
<script setup lang="ts">
  // Hier de bestaande code
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
      class="hero"
      :class="{
        'hero--active': hero.number === selectedHero?.number
      }"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <template v-if="selectedHero">
    <div class="title">{{ selectedHero.name }} is my hero</div>
    <button>Details</button>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */

  .hero--active {
    background-color: #cfd8dc;
    color: white;
    margin-left: 0.25rem;
  }
</style>
```

Wanneer er nu een hero geselecteerd is, wordt de class `hero--active` toegevoegd aan het element.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/1251e29d82f55f5cfcc1cdd3ace9f0ebeffc9c2f)

![Styling van geselecteerde hero](/img/blog/quasar-toh-selected-hero-styling.png)

#### Naam hero in hoofdletters

Een manier om de naam in hoofdletters te tonen is om de naam in een element te plaatsen en vervolgens via css de naam in hoofdletters te plaatsen. In deze tutorial wordt er echter voor gekozen om de naam te wijzigen met behulp van een functie.

`HeroListView.vue`

```html
<script setup lang="ts">
  import { ref, type Ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  const heroes: Array<Hero> = [
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

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  /**
   *  Voeg een functie toe om de naam van een hero in hoofdletters te tonen
   */
  const uppercase = (text: string) => text.toUpperCase();
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
      class="hero"
      :class="{
        'hero--active': hero.number === selectedHero?.number
      }"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <template v-if="selectedHero">
    <!-- Gebruik de functie -->
    <div class="title">{{ uppercase(selectedHero.name) }} is my hero</div>
    <button>Details</button>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Momenteel wordt er een functie gebruikt om de naam van de geselecteerde hero in hoofdletters te tonen. Maar dit zou ook een totaal andere functie kunnen zijn die de data transformeert. Beeld je in dat de naam van de hero achterstevoren getoond moet worden. Dan kan er een functie geschreven worden die de naam achterstevoren toont.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/abe5078a9206720d61330e143d6d99c67e8527d2)

#### Toevoegen details-knop

Aangezien de knop hetzelfde gestyled moet worden als de reeds bestaande knoppen in `MainLayout.vue`, kan de styling daar gekopieerd worden en vervolgens geplakt worden in de `HeroListView.vue`.

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

Dit is een snelle oplossing, maar het zorgt echter voor een probleem. Wat als de `background-color` niet langer `#eeeeee` moet zijn, maar `deeppink`? Dan moet dit zowel in de `HeroListView.vue` als in `MainLayout.vue` gewijzigd worden.

Om dit te voorkomen, kan er een aparte component gemaakt worden waarin de knop én de styling wordt geplaatst.

```sh
touch src/components/StyledButton.vue
```

`StyledButton.vue`

```html
<template>
  <button>Details</button>
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
</style>
```

De custom component kan geïmporteerd worden in `HeroListView.vue`:

```html
<script setup lang="ts">
  import { ref, type Ref } from "vue";
  import StyledButton from "@/components/StyledButton.vue"; // importeer de component

  // Hier de bestaande code
</script>

<template>
  <!-- Hier de bestaande html -->

  <template v-if="selectedHero">
    <div class="title">{{ uppercase(selectedHero.name) }} is my hero</div>
    <!-- gebruik de component -->
    <StyledButton></StyledButton>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

De reden dat we `<StyledButton></StyledButton>` moeten gebruiken in de html, komt door de variabele `StyledButton` waarin de component geïmporteerd wordt. Stel dat we dit zouden wijzigen naar `import HihiHaha from "@/components/StyledButton.vue";`, dan zouden we `<HihiHaha></HihiHaha>` moeten gebruiken in de html. Vaak wordt er gekozen om de naam van de component te gebruiken als naam van de variabele waarin de component geïmporteerd wordt, dit zorgt voor minder verwarring.

Het probleem met de huidige `StyledButton`, is dat het hardcoded de tekst `Details` bevat. Maar in de `MainLayout.vue` moet er `Dashboard` en `Heroes` worden weergegeven.

Een normaal `button`-element in html toont de tekst tussen een begin- en end-tag. Om dit te bekomen in de custom component, kan er gebruik gemaakt worden van `content projection`. Dit doen we door een `<slot>`-element toe te voegen aan de template van de custom component.

`StyledButton.vue`

```html
<button>
  <slot />
</button>
```

`HeroListView.vue`

Het is nu mogelijk om de tekst tussen begin- en end-tag te plaatsen.

```html
<script setup lang="ts">
  import { ref, type Ref } from "vue";
  import StyledButton from "@/components/StyledButton.vue";

  // Hier de bestaande code
</script>

<template>
  <!-- Hier de bestaande html -->

  <template v-if="selectedHero">
    <div class="title">{{ uppercase(selectedHero.name) }} is my hero</div>
    <!-- Plaats de tekst tussen de begin- en end-tag -->
    <StyledButton>Details</StyledButton>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/ac6d0087ce80e0a747e012822233ecac2088f724)

Het is nu mogelijk om de knop ook te gebruiken in `MainLayout.vue`. De styling van de `button` kan uit de `style`-tag verwijderd worden.

`MainLayout.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue"; // importeer de component
  import { RouterView, useRouter } from "vue-router";

  const router = useRouter();
</script>

<template>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>

    <div class="button-container">
      <!-- Gebruik de component -->
      <StyledButton @click="router.push({ name: 'dashboard' })">
        Dashboard
      </StyledButton>
      <!-- Gebruik de component -->
      <StyledButton @click="router.push({ name: 'hero-list' })">
        Heroes
      </StyledButton>
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

  // verwijder de styling van de button
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/706f9bc97738bc0c03738a85fcdccfba681c0643)

Wanneer op de `Details`-knop geklikt wordt, moet er genavigeerd worden naar de route `/heroes/:id`. Waar :id de `number`-property van de geselecteerde hero is.

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { ref, type Ref } from "vue";
  import { useRouter } from "vue-router"; // importeer useRouter

  const router = useRouter(); // Zorg dat de router gebruikt kan worden

  // Hier de bestaande code
</script>

<template>
  <!-- Hier de bestaande html -->

  <template v-if="selectedHero">
    <div class="title">{{ uppercase(selectedHero.name) }} is my hero</div>
    <!-- Gebruik de router -->
    <StyledButton
      @click="router.push({ name: 'hero-details', params: { id: selectedHero?.number } })"
    >
      Details
    </StyledButton>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Merk op dat er `selectedHero?.number` staat, dit zou eigenlijk niet nodig moeten zijn, aangezien we de knop enkel tonen als er een hero geselecteerd is. Maar de IDE deed moeilijk, dus is er voor gekozen om de `?.`-operator te gebruiken om de IDE tevreden te stellen.

Wanneer het `click` event wordt uitgestuurd, dan wordt er genavigeerd naar de route `/heroes/:id`, waarbij `:id` gelijk is aan de `number`-property van de geselecteerde hero.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/a3f8f87a7c1d64ce479e6b46ed0a3f9de49e732f)

## Pagina stylen: hero-details

Voorzie een property om de hero in op te slaan. Importeer de `StyledButton`-component zodat deze gebruikt kan worden in de template. Voorzie elementen in de template om de details van een hero te tonen.

Aangezien er opnieuw een complex object wordt toegekend, voorzien we een interface.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { ref, type Ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  const hero: Ref<Hero | null> = ref(null);
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </template>
</template>
```

Via `Property Binding` wordt de waarde van `hero.name` toegekend aan de `value` van de `input`-tag.

Als er nu een hero geselecteerd wordt, of wanneer er manueel naar `/heroes/:id` genavigeerd wordt (bv. `/heroes/15`), dan wordt de `hero-details`-pagina getoond. Maar er is niks te zien in de browser. Om de elementen te kunnen stylen, kan er tijdelijk een hero hard-coded toegevoegd worden.

```html
<script setup lang="ts">
import StyledButton from '@/components/StyledButton.vue';
import { ref, type Ref } from 'vue';

interface Hero {
  number: number;
  name: string;
}

const hero: Ref<Hero | null> = ref({ number: 15, name: 'Magneta' });
</script>

<temmplate>
  <!-- Hier de bestaande html -->
</template>
```

Nu de hero zichtbaar is, kan de styling toegevoegd worden.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { ref, type Ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  const hero: Ref<Hero | null> = ref({ number: 15, name: "Magneta" });
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </template>
</template>

<style scoped>
  .title {
    font-size: 1.5rem;
    color: grey;
    font-weight: bold;
    margin-top: 1rem;
    margin-bottom: 1rem;
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/140e63b2c8d7e927f0b52d4afc2cb635941e7ad9)

![Hero Details - setup](/img/blog/quasar-toh-hero-details-initial.png)

Voor er verdergegaan wordt met het stylen van de pagina, moet er gekeken worden naar hetgeen er gedupliceerd is op deze pagina ten opzichte van de pagina `hero-list`. Gedupliceerde dingen zijn kandidaat om apart gezet te worden zodat het herbruikbaar is.

### Refactor Hero Interface

De `Hero`-interface is gedupliceerd vanuit de `hero-list`-pagina.

```ts
interface Hero {
  number: number;
  name: string;
}
```

Stel dat er een extra property bijkomt in de `Hero`-interface, dan zou dit gewijzigd moeten worden in zowel de `hero-list`-pagina als de `hero-details`-pagina. Dit lijkt misschien niet erg, maar denk eens na wat er gedaan moet worden als deze interface in tien verschillende componenten gebruikt wordt.

Dit wordt opgelost door de interface in `src/components/models.ts` te plaatsen. Vanuit dit bestand wordt de interface geëxporteerd en vervolgens wordt de interface geïmporteerd in alle bestanden waar deze interface gebruikt wordt.

`src/components/models.ts`:

```ts
export interface Hero {
  number: number;
  name: string;
}
```

Wijzig de bestaande `Hero`-interface in de twee plaatsen waar het gebruikt wordt in de componenten, naar een import die verwijst naar de interface in `src/components/models.ts`.

```ts
import type { Hero } from "@/components/models";
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/a6cb1fe52d1120df6ee29a387d9047cc7a6dc8a5)

### Refactor .title class

Ook de class `.title` wordt ondertussen meerdere keren gebruikt.

`MainLayout.vue`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
}
```

`DashboardView.vue`

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

`HeroDetailsView.vue`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

`HeroListView.vue`

```css
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

De gemeenschappelijke properties van deze class kunnen verplaatst worden naar de globale stylesheet `src/assets/main.css`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/2fc370193397e80ea6d9b4887a25d858fad4e788)

### Heroes - url parameter

Om te zorgen dat de correcte hero getoond kan worden, gaat er gekeken worden naar de url parameter genaamd `id`. Vervolgens zal de hero gefilterd worden uit de lijst van heroes, die in dit artikel gekopieerd zal worden vanuit de `hero-list`-pagina, in een later artikel zal de lijst van heroes verplaatst worden naar een `service` zodat er maar één unieke lijst van heroes bestaat.

`src/views/HeroDetailsView.vue`
```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute } from "vue-router";

  const route = useRoute();

  const hero: Ref<Hero | null> = ref(null); // start met null
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

  /**
   * Gebruik de `onMounted`-hook om de hero te filteren uit de lijst van heroes.
   * Dit wordt uitgevoerd wanneer de component gemount wordt (wanneer de component getoond wordt)
   */
  onMounted(() => {
    const heroId = Number(route.params.id); // Een param is altijd een string, dus moet deze geconverteerd worden naar een number
    hero.value = heroes.find((hero) => hero.number === heroId) ?? null; // ?? null zorgt ervoor dat er een null wordt toegekend als de hero niet gevonden wordt
  });
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </template>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/716349e0287e84a1650b239bd91b04ba102e63cd)

### Hero niet gevonden

Om de gebruiker van betere feedback te voorzien bij het invoeren van een hero die niet bestaat, zal een `Hero not found!` (hero niet gevonden!)-melding getoond worden als de `hero` `null` is. Dit wordt gedaan door gebruik te maken van `v-else`. Een alternatief is om een `v-if` te gebruiken met de `!`-operator.

```html
<template>
  <template v-if="hero">Hero gevonden</template>
  <template v-else>Hero niet gevonden</template>
</template>
```

Alternatief met @if:

```html
<template>
  <template v-if="hero">Hero gevonden</template>
  <template v-if="!hero">Hero niet gevonden</template>
</template>
```

Toegepast in de code:

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  // Hier de bestaande code
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
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
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/9b97c59b4fa1980256dd85e597f04c7cf301ca52)

### Stylen en koppelen van back-knop

Om de knop te stylen wordt er een class toegevoegd. Om te zorgen dat de knop terug navigeert naar de vorige pagina, wordt er gebruik gemaakt van de `router`-functionaliteit.

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router"; // importeer useRouter

  const route = useRoute();
  const router = useRouter(); // zorg dat de router gebruikt kan worden

  const hero: Ref<Hero | null> = ref(null);
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

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = heroes.find((hero) => hero.number === heroId) ?? null;
  });
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <!-- Voeg een class toe en voeg een click handler toe -->
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

  /* Voeg de styling voor de .back-button toe */
  .back-button {
    margin-top: 1rem;
  }
</style>
```

`router.go(-1)` zorgt ervoor dat er één pagina terug genavigeerd wordt.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/62bcebffede9e9be5ce8f44c325c9483146891ee)

### Two way binding - v-model

Momenteel wordt er gebruik gemaakt van `:value` om een waarde toe te kennen aan de input. Dit is `one way binding`, de input krijgt een waarde toegekend, maar indien de waarde gewijzigd wordt, wordt dit niet doorgevoerd naar de `hero`-property. Om `two way binding` te bekomen, moet gebruik gemaakt worden van `v-model`.

`v-model` kent de waarde van de `hero`-ref toe aan de input, maar zal ook de waarde van `hero`-ref updaten wanneer de inputwaarde gewijzigd wordt.

One way binding `:value`

```html
<div>name: <input :value="hero.name" /></div>
```

![One way binding](/img/blog/quasar-toh-one-way-binding.gif)

Two way binding `v-model`

```html
<div>name: <input v-model="hero.name" /></div>
```

![Two way binding](/img/blog/quasar-toh-two-way-binding.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/d05f64d7efb26ce1655f45d4c8fa47c2231db038)

## Conclusie

Een pagina kan opgesplitst worden in herbruikbare componenten.

Het is oké als componenten niet op voorhand gemaakt worden, maar toegevoegd worden wanneer er opgemerkt wordt dat er identieke blokken code zijn doorheen de codebase.

Het is belangrijk om potentiële componenten te identificeren. Hoe meer ervaring, hoe beter het identificeren van componenten zal gaan.

Potentiële componenten binnen deze applicatie zijn bijvoorbeeld: De heroes die op het dashboard getoond worden. De heroes die in de lijst getoond worden, ... Aangezien deze momenteel nog maar op één plaats gebruikt worden, is het niet nodig om deze componenten te maken. Maar het is wel belangrijk om te weten dat deze componenten gemaakt kunnen worden.
