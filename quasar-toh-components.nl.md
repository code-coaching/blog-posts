---
postUuid: 5d65c5d7-736e-4505-a28d-65cd6f60341b
title: Quasar Tour of Heroes - Componenten
slug: quasar-toh-componenten
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De beginsituatie is een bestaande Quasar-project met een layout componentent en placeholder pagina's. De eindsituatie is een Quasar-project waarin de pagina's voorzien zijn van componenten.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-styling.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-layouts-and-pages)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-components.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-components)

## Componenten

Alles binnen Vue is een component. Aangezien Quasar een Vue-framework is, zijn alle componenten ook Vue-componenten. In de applicatie is al een component te zien voor de layout, genaamd `MainLayout.vue`. Er zijn ook al componenten voor de verschillende pagina's.

De verschillende pagina's zijn:

- Error404.vue - 404-pagina, standaard aanwezig in een nieuw Quasar-project.
- Dashboard.vue - dit is een pagina waarop de `top heroes` getoond zullen worden.
- HeroList.vue - dit is een pagina waarop de lijst met `heroes` getoond zullen worden.
- HeroDetail.vue - dit is een pagina waarop de details van een `hero` getoond zullen worden.

Een layout is een component en bevat HTML-elementen en andere componenten die op alle pagina's aanwezig zijn.

Een pagina is een component en bevat HTML-elementen en andere componenten die op een specifieke pagina aanwezig zijn.

Tijdens dit artikel zullen er nieuwe componenten worden toegevoegd aan de applicatie.

## Pagina stylen: Dashboard

![dashboard](/img/blog/quasar-toh-dashboard.png)

Wijzig de inhoud van `src/pages/Dashboard.vue`:

```html
<template>
  <div>Top Heroes</div>

  <div>Narco</div>
  <div>Bombasto</div>
  <div>Celeritas</div>
  <div>Magneta</div>
</template>
```

![Quasar dashboard elementen](/img/blog/quasar-toh-dashboard-elements.png)

De elementen zijn aanwezig, maar de styling komt niet overeen.

Voeg een `style`-tag toe aan de Dashboard-pagina:

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

<style lang="scss" scoped>
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

    &:hover {
      background-color: #eeeeee;
      color: #5f7d8c;
      cursor: pointer;
    }
  }
</style>
```

Merk op dat er een `container element` is toegevoegd rondom de `hero`-elementen.

![Quasar Tour of Heroes Dashboard Styling](/img/blog/quasar-toh-dashboard-styling.png)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/e4c7e2534940826a43dd00b5aee2373659fcb8c7..cb681e49138851e87ffa8e39c7fa9c6955c0f1a3)

## Pagina stylen: HeroList

![HeroList](/img/blog/quasar-toh-hero-list.png)

Wijzig de inhoud van `src/pages/HeroList.vue`:

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

![Quasar hero list elementen](/img/blog/quasar-toh-hero-list-elements.png)

Style de elementen, voeg waar nodig extra elementen toe om het gewenste resultaat te bekomen.

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

<style lang="scss" scoped>
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
    background-color: darken(#eeeeee, 10%);
    background-color: #eeeeee;
    cursor: pointer;
    color: #8d8d8d;
    border-radius: 0.5rem;

    &:hover {
      background-color: #cfd8dc;
      color: white;
      margin-left: 0.25rem;
    }
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

![Quasar Tour of Heroes styled hero list](/img/blog/quasar-toh-hero-list-styled.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/cb681e4..ee30bd8)

## Pagina uitbreiden: HeroList

Om te zorgen dat een hero geselecteerd kan worden, moet er een `click handler` toegevoegd worden aan elke hero. Aangezien met de huidige code, er tien aparte elementen zijn die een hero voorstellen, zouden er tien `click handlers` nodig zijn.

Om dit te vermijden, kan er gebruik gemaakt worden van een `v-for`-loop in de `template`-sectie, gecombineerd met een lijst van heroes.

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div class="hero" v-for="(hero, index) in heroes" :key="index">
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
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

      return {
        heroes,
      };
    },
  });
</script>

<style lang="scss" scoped>
  /* No changes! */
</style>
```

Belangrijk in de template is het toevoegen van de `v-for`-loop. Dit wordt gedaan op het element dat herhaald moet worden. Om te zorgen dat `change detection` optimaal werkt, moet er een `key`-attribuut gebruikt worden. Dit zorgt ervoor dat elk element uniek geïdentificeerd wordt.

Er is een `<script lang="ts"></script>` toegevoegd. `lang="ts"` betekent dat er TypeScript gebruikt wordt. Er wordt gebruikgemaakt van de `defineComponent`-functie. Deze functie maakt het mogelijk om een component te definiëren. De `setup`-functie is de functie die wordt aangeroepen als de component wordt aangemaakt. In de `setup`-functie wordt de `heroes`-lijst gedefinieerd, met daarin een object met de properties `number` en `name`. Elk object stelt een hero voor.

Variabelen die gebruikt worden in de `template`-sectie, moeten in de `return`-statement van de `setup`-functie staan. Enkel de variabelen die hier staan, zijn beschikbaar in de template.

### Een hero selecteren

![Klik hero in lijst](/img/blog/quasar-toh-click-hero-in-list.png)

Om te zorgen dat een hero geselecteerd kan worden, zijn er enkele dingen nodig:

- een `click handler` op elk hero element, dit is een functie die wordt aangeroepen als er op een hero wordt geklikt
- een `selectedHero`-variabele die de geselecteerde hero zal bevatten

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      class="hero"
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div>{{ selectedHero }}</div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      let selectedHero;

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

      function onClickHero(hero: { number: number; name: string }) {
        selectedHero = hero;
      }

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>

<style lang="scss" scoped>
  /* No changes! */
</style>
```

Herinner: `selectedHero` is een variabele gedefinieerd met het keyword `let`. Dit is nodig aangezien de variabele op een later moment een andere waarde toegekend krijgt.

De `click handler` wordt toegvoegd door middel van de `@click`-directive. Deze directive wordt gebruikt om een functie aan te roepen als er op een element wordt geklikt. De functie die wordt aangeroepen wordt gedefinieerd in de `setup`-functie.

De functie die wordt aangeroepen wordt gedefinieerd als `onClickHero`, met als argument een hero. Deze functie wordt aangeroepen als er op een hero wordt geklikt.

In de functie wordt de `selectedHero`-variabele gewijzigd. Deze variabele wordt gebruikt in de template om de geselecteerde hero te tonen `{{ selectedHero }}`.

MAAR bij het uittesten van deze code, zal de `selectedHero`-variabele niet gewijzigd worden in de template. Dit is omdat de `selectedHero`-variabele een `ref`-variabele moet zijn. Een `ref`-variabele is een variabele die een referentie naar een element bevat. Deze referentie wordt gebruikt door Vue om te weten dat de waarde van de variabele gewijzigd is en dus dat deze variabele vernieuwd moet worden in de template.

### Gebruik van `ref`-variabelen

Een korte uitleg omtrent `ref`-variabelen. Vue voorziet een functie genaamd `ref`, deze moet geïmporteerd worden `import { ref } from "vue";`. Deze functie accepteert eender welke waarde als argument en zal een object teruggeven met de waarde toegekend aan de property genaamd `value`.

```ts
import { ref } from "vue";

let someVariable; // value is undefined
someVariable = "John Duck"; // value is "John Duck"

const someRef = ref(); // value is { value: undefined }
const someOtherRef = ref("John Duck"); // value is { value: "John Duck" }
someOtherRef.value = "John - The - Duck"; // value is { value: "John - The - Duck" }
```

Enkele verschillen die op te merken zijn, een `ref`-variabele kan altijd toegekend worden aan een variabele aangemaakt met keyword `const`. Dit is omdat de `ref()`-functie een object teruggeeft, vervolgens wordt altijd een property van dit object (genaamd `value`) gebruikt om een nieuwe waarde toe te kennen. De variabele zelf krijgt dus geen nieuwe waarde toegekend, aangezien het nog altijd hetzelfde object is.

Doordat het object gewijzigd wordt dat in de gaten gehouden wordt door Vue, weet Vue dat deze waarde gewijzigd is en kan vervolgens `change detected`-events versturen naar de template.

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";

  export default defineComponent({
    setup() {
      // let selectedHero = undefined;
      const selectedHero = ref();

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

      function onClickHero(hero: { number: number; name: string }) {
        selectedHero.value = hero;
      }

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>
```

Merk op dat `selectedHero` een `ref`-variabele is geworden. Dit is een object waarin de waarde wordt bijgehouden in de property `value`. Om de waarde te wijzigen, moet dus de `.value`-property gebruikt worden. Dit is te zien in de `onClickHero`-functie.

Probeer opnieuw deze code uit en merk op dat de `selectedHero`-variabele nu wél gewijzigd wordt in de template na het selecteren van een hero.

Waarom moet de `heroes`-array niet gewijzigd worden naar een `ref`-variabele? Dit is niet nodig, omdat de lijst van heroes niet gewijzigd wordt. Indien de lijst van heroes wel zou wijzigen, bijvoorbeeld door een nieuwe hero toe te voegen door op een knop te klikken, dan moet deze lijst wel gewijzigd worden.

De `heroes`-array **moet** niet gewijzigd worden naar een `ref`-variabele, maar het **mag** wel gewijzigd worden naar een `ref`-variabele. Een simpele regel om te volgen is _maak elke variabele die gebruikt wordt in de template, een `ref`-variabele_. Op deze manier wordt er gezorgd dat elke variabele die gebruikt wordt in de template `reactive` is. Vue zal altijd een up-to-date waarde tonen voor elke `reactive variabele` in de template.

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      function onClickHero(hero: { number: number; name: string }) {
        selectedHero.value = hero;
      }

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>
```

Merk op dat er niks wijzigt in de `template`. Vue weet automatisch dat een variabele een `ref`-variabele is en zal automatisch de `.value` property gebruiken om de waarde van de variabele te tonen in de template.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/ee30bd8..be91f2b)

![Ref-variabele in HeroList](/img/blog/quasar-toh-selected-hero-ref.png)

Een kleine refactoring van de code om een `interface` toe te voegen en de definitie van de `onClickHero`-functie te wijzigen naar een anonieme arrow functie.

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>
```

De functie is gewijzigd van een named functie.

```ts
function onClickHero(hero: { number: number; name: string }) {
  selectedHero.value = hero;
}
```

Naar een anonieme arrow functie toegekend aan een variabele genaamd `onClickHero`.

```ts
const onClickHero = (hero: { number: number; name: string }) => {
  selectedHero.value = hero;
};
```

Dit wijzigt niks aan de functionaliteit van de functie. Het is simpelweg een voorkeur qua syntax.

De interface is toegevoegd om het complexe type `Hero` voor te stellen. Deze interface is niet nodig, maar het is een goede manier om de code leesbaarder te maken.

Voor het toevoegen van de Hero-interface:

```ts
const onClickHero = (hero: { number: number; name: string }) => {
  selectedHero.value = hero;
};
```

Na het toevoegen van de Hero-interface:

```ts
interface Hero {
  number: number;
  name: string;
}

const onClickHero = (hero: Hero) => {
  selectedHero.value = hero;
};
```

Het voordeel van deze TypeScript interface, is dat overal waar een Hero-object wordt gebruikt, kan de interface gebruikt worden om het type te definiëren.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/be91f2b..37b8e4a)

### Stylen van selectedHero

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      class="hero"
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div v-if="selectedHero">
    <div class="title">{{ selectedHero.name }} is my hero</div>
    <button>Details</button>
  </div>
</template>
```

Merk op dat de `v-if`-directive gebruikt wordt. Dit zorgt ervoor dat het element en alle child-elementen enkel getoond worden als de variabele `selectedHero` `truthy` is. Of in andere woorden, als de variabele een waarde heeft, wordt het element getoond. Zonder de `v-if`-directive zou de applicatie een error tonen, aangezien de `.name`-property van de variabele `selectedHero` niet bestaat, aangezien `selectedHero` op dat moment nog `undefined` is.

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
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      :class="{
        'hero--active': hero.number === selectedHero?.number,
      }"
      class="hero"
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div v-if="selectedHero">
    <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
    <button>Details</button>
  </div>
</template>
```

De gewijzigde SCSS-class `hero--active`:

```scss
.hero {
  display: flex;
  max-width: 10rem;
  background-color: darken(#eeeeee, 10%);
  background-color: #eeeeee;
  cursor: pointer;
  color: #8d8d8d;
  border-radius: 0.5rem;

  &:hover {
    background-color: #cfd8dc;
    color: white;
    margin-left: 0.25rem;
  }

  &--active {
    background-color: #cfd8dc;
    color: white;
    margin-left: 0.25rem;
  }
}
```

```scss
.hero {
  &:hover {
  }

  &--active {
  }
}
```

Is gelijk aan de volgende CSS:

```css
.hero {
}

.hero:hover {
}

.hero--active {
}
```

Wanneer er nu een hero geselecteerd is, wordt de class `hero--active` toegevoegd aan het element.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/37b8e4a..bb8d8ce)

![Styling van geselecteerde hero](/img/blog/quasar-toh-selected-hero-styling.png)

#### Naam hero in hoofdletters

Een manier om de naam in hoofdletters te tonen is om de naam in een element te plaatsen en vervolgens via css de naam in hoofdletters te plaatsen. In deze tutorial wordt er echter voor gekozen om de naam te wijzigen met behulp van een JavaScript-functie.

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      :class="{
        'hero--active': hero.number === selectedHero?.number,
      }"
      class="hero"
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div v-if="selectedHero">
    <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
    <button>Details</button>
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      const upperCase = (str: string) => str.toUpperCase();

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
      };
    },
  });
</script>
```

De functie `upperCase` wordt aangemaakt in de `setup`-functie. De functie wordt toegevoegd aan de `return`-statement zodat deze beschikbaar is in de template. De functie wordt toegevoegd aan `{{ selectedHero.name }}` zodat de naam in hoofdletters wordt weergegeven: `{{ upperCase(selectedHero.name) }}`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/bb8d8ce..6b1f836)

Aangezien `upperCase` een functie is die eender welke string in hoofdletters weergeeft, is het ideaal om de functie in een aparte locatie te plaatsen zodat de functie op verschillende plaatsen geïmporteerd kan worden.

In deze tutorial wordt gekozen om de functie te plaatsen in een file genaamd `utils.ts` in de `components`-map.

`components/utils.ts`:

```ts
const upperCase = (str: string) => str.toUpperCase();

export { upperCase };
```

Importeren van de `upperCase`-functie in de `HeroList`-component:

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";
  import { upperCase } from "components/utils";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
      };
    },
  });
</script>
```

Merk op dat de `upperCase`-functie nu geïmporteerd wordt en niet langer gedefinieerd wordt in de `setup`-functie. Merk ook op dat de `upperCase`-functie nog steeds in de `return`-statement van de `setup`-functie wordt geplaatst zodat deze beschikbaar is in de template.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/6b1f836..6b8760c)

#### Toevoegen details-knop

Aangezien de knop hetzelfde gestyled moet worden als de reeds bestaande knoppen in `MainLayout.vue`, kan de styling daar gekopieerd worden en vervolgens geplakt worden in de `style`-tag van `HeroList.vue`.

```scss
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
```

Dit is een snelle oplossing, maar het zorgt echter voor een probleem. Wat als de `background-color` niet langer `#eeeeee` moet zijn, maar `deeppink`? Dan moet dit zowel in de `HeroList.vue` als in de `MainLayout.vue` gewijzigd worden.

Om dit te voorkomen, kan er een aparte component gemaakt worden waarin de knop én de styling wordt geplaatst.

`components/StyledButton.vue`

```html
<template>
  <button>Details</button>
</template>

<style lang="scss" scoped>
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

De custom component kan geïmporteerd worden in de `HeroList.vue`:

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";
  import { upperCase } from "components/utils";
  import StyledButton from "components/StyledButton.vue"; // 1.

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton, // 2.
    },
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
      };
    },
  });
</script>
```

Gebruiken van de custom component:

```html
<div v-if="selectedHero">
  <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>

  <!-- Option 1 -->
  <styled-button></styled-button>

  <!-- Option 2 -->
  <styled-button />

  <!-- Option 3 -->
  <StyledButton></StyledButton>

  <!-- Option 4 -->
  <StyledButton />
</div>
```

De custom component kan op verschillende manieren gebruikt worden.

Als `kebab-case`-syntax, met begin- en end-tag:

- `<styled-button></styled-button>`

Als `kebab-case`-syntax, self-closing tag:

- `<styled-button />`

Als `PascalCase`-syntax, met begin- en end-tag:

- `<StyledButton></StyledButton>`

Als `PascalCase`-syntax, self-closing tag:

- `<StyledButton />`

Binnen deze tutorial wordt gekozen voor `PascalCase`-syntax. Indien mogelijk de self-closing tag, indien er content tussen de begin- en end-tag zit, dan wordt `PascalCase`-syntax met begin- en end-tag gebruikt.

```html
<div v-if="selectedHero">
  <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
  <StyledButton />
</div>
```

Het probleem met de huidige `StyledButton`, is dat het hardcoded de tekst `Details` bevat. Maar in de `MainLayout.vue`-component moet er `Dashboard` en `Heroes` worden weergegeven.

Een normaal `button`-element in html toont de tekst tussen een begin- en end-tag. Om dit te bekomen in de custom component, kan er een `slot`-tag gebruikt worden.

`components/StyledButton.vue`

```html
<template>
  <button>
    <slot></slot>
  </button>
</template>
```

Of met een self closing slot-tag:

```html
<template>
  <button>
    <slot />
  </button>
</template>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/6b8760c..c33f42b)

Het is nu mogelijk om de knop ook te gebruiken in de `MainLayout.vue`-component. De styling van de `button` kan uit de `style`-tag van de `MainLayout.vue`-component gehaald worden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/c33f42b..d323783)

Wanneer op de `Details`-knop geklikt wordt, moet er genavigeerd worden naar de route `/heroes/:id`. Waar :id de `number`-property van de geselecteerde hero is.

`pages/HeroList.vue`

template

```html
<StyledButton @click="onDetailsClick()">Details</StyledButton>
```

script-tag

```html
<script lang="ts">
  import { defineComponent, ref, Ref } from "vue";
  import { upperCase } from "components/utils";
  import { useRouter } from "vue-router";
  import StyledButton from "components/StyledButton.vue";
  import { ROUTE_NAMES } from "src/router/routes";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const router = useRouter();
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      const onDetailsClick = () => {
        void router.push({
          name: ROUTE_NAMES.HERO_DETAILS,
          params: {
            id: selectedHero.value.number,
          },
        });
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
        onDetailsClick,
      };
    },
  });
</script>
```

In JavaScript zou deze code werken, maar omdat er gebruikgemaakt wordt van TypeScript worden er een aantal fouten getoond:

```sh
Compiled with problems:X

ERROR in src/pages/HeroList.vue:66:11

@typescript-eslint/no-unsafe-assignment: Unsafe assignment of an `any` value.
    64 |         name: ROUTE_NAMES.HERO_DETAILS,
    65 |         params: {
  > 66 |           id: selectedHero.value.number,
       |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    67 |         },
    68 |       });
    69 |     };


ERROR in src/pages/HeroList.vue:66:15

@typescript-eslint/no-unsafe-member-access: Unsafe member access .number on an `any` value.
    64 |         name: ROUTE_NAMES.HERO_DETAILS,
    65 |         params: {
  > 66 |           id: selectedHero.value.number,
       |               ^^^^^^^^^^^^^^^^^^^^^^^^^
    67 |         },
    68 |       });
    69 |     };
```

Het probleem is dat `selectedHero` een `ref`-variabele is. Deze is standaard van het type `any`.

```ts
import { ref } from "vue";

const selectedHero = ref();
```

Is gelijk aan:

```ts
import { ref, Ref } from "vue";

const selectedHero: Ref<any> = ref();
```

Aangezien gekend is wat er in de `ref`-variabele komt, namelijk een hero, kan de interface gebruikt worden om aan te geven dat de `ref`-variabele een `Hero`-object zal bevatten.

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero> = ref();
```

Mooi, opgelost. Toch? Nee! Er is een nieuwe compilatiefout. De fout geeft aan dat de `ref`-variabele enkel een `Hero`-object kan bevatten, maar er wordt ook `undefined` aan toegekend.

```sh
Compiled with problems:

ERROR in src/pages/HeroList.vue:43:11

TS2322: Type 'Ref<Hero | undefined>' is not assignable to type 'Ref<Hero>'.
  Type 'Hero | undefined' is not assignable to type 'Hero'.
    Type 'undefined' is not assignable to type 'Hero'.
    41 |   setup() {
    42 |     const router = useRouter();
  > 43 |     const selectedHero: Ref<Hero> = ref();
       |           ^^^^^^^^^^^^
    44 |
    45 |     const heroes = ref([
    46 |       { number: 11, name: 'Mr. Nice' },
```

Er zijn twee mogelijkheden om deze fout te verhelpen.

De eerste optie is om `undefined` toe te voegen als mogelijk type van deze `ref`-variabele.

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero | undefined> = ref();
```

Het nadeel van deze aanpak is dat er altijd een `?`-teken gebruikt moet worden bij het aanspreken van een property van het object in de `.value`-property, aangezien dat deze mogelijks `undefined` is.

```ts
console.log(selectedHero.value?.number);
console.log(selectedHero.value?.name);
```

De tweede optie is om de `ref`-variabele te definiëren als `Ref<Hero>` en de ref zelf te `type casten` naar `Ref<Hero>`.

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero> = <Ref<Hero>>ref();
```

Of de alternatieve `type cast`-syntax:

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero> = ref() as Ref<Hero>;
```

Nu kan een property van het object in de `.value`-property aangesproken worden zonder het `?`-teken.

```ts
console.log(selectedHero.value.number);
console.log(selectedHero.value.name);
```

Om te zorgen dat het altijd duidelijk is wat het exacte type is van de waarde in de `ref`-variabele. Is het slim om altijd een type toe te kennen aan de `ref`-variabele.

```html
<script lang="ts">
  import { defineComponent, ref, Ref } from "vue";
  import { upperCase } from "components/utils";
  import { useRouter } from "vue-router";
  import StyledButton from "components/StyledButton.vue";
  import { ROUTE_NAMES } from "src/router/routes";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const router = useRouter();
      const selectedHero: Ref<Hero> = ref() as Ref<Hero>;

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      const onDetailsClick = () => {
        void router.push({
          name: ROUTE_NAMES.HERO_DETAILS,
          params: {
            id: selectedHero.value.number,
          },
        });
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
        onDetailsClick,
      };
    },
  });
</script>
```

Bij het uittesten van de code, klik op de hero `Magneta` en klik vervolgens op de `Details`-knop. Verifieer dat de url van de browser gewijzigd is naar `/heroes/15`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/d323783..92e2080)

## Pagina stylen: HeroDetails

Eerst worden alle elementen voorzien die nodig zijn om de details van een hero te tonen.

```html
<template>
  <div v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref, Ref } from "vue";
  import StyledButton from "components/StyledButton.vue";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const hero: Ref<Hero> = ref() as Ref<Hero>;

      return {
        hero,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
    font-size: 1.5rem;
    color: grey;
    font-weight: bold;

    margin-top: 1rem;
    margin-bottom: 1rem;
  }
</style>
```

Als er nu een hero geselecteerd wordt, of wanneer er manueel naar `/heroes/:id` genavigeerd wordt (bv. `/heroes/15`), dan wordt de `HeroDetails`-pagina getoond. Maar er is niks te zien in de browser. Om de elementen te kunnen stylen, kan er tijdelijk een hero hard-coded toegevoegd worden.

```ts
const hero: Ref<Hero> = ref({ number: 15, name: "Magneta" }) as Ref<Hero>;
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/92e2080..0a1e9d0)

![Hero Details - setup](/img/blog/quasar-toh-hero-details-initial.png)

Voor er verdergegaan wordt met het stylen van de pagina, moet er gekeken worden naar hetgeen er gedupliceerd is op deze pagina ten opzichte van de pagina `HeroList`. Gedupliceerde dingen zijn kandidaat om apart gezet te worden zodat het herbruikbaar is.

### Refactor Hero Interface

De `Hero`-interface is gekopieerd en geplakt vanuit de `HeroList`-pagina.

```ts
interface Hero {
  number: number;
  name: string;
}
```

Stel dat er een extra property bijkomt in de `Hero`-interface, dan zou dit gewijzigd moeten worden in zowel de `HeroList`-pagina als de `HeroDetails`-pagina. Dit lijkt misschien niet erg, maar denk eens na wat er gedaan moet worden als deze interface in tien verschillende componenten gebruikt wordt.

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
import { Hero } from "src/components/models";
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/0a1e9d0..b341f6a)

### Refactor .title class

Ook de class `.title` wordt ondertussen meerdere keren gebruikt.

`src/layouts/MainLayout.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
}
```

`src/pages/Dashboard.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}
```

`src/pages/HeroDetails.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

`src/pages/HeroList.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

De gemeenschappelijke properties van deze class kunnen verplaatst worden naar de globale stylesheet `src/css/app.scss`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/b341f6a..9029358)

### Heroes - onBeforeMount - url parameter useRoute

Om te zorgen dat de correcte hero getoond kan worden, gaat er gekeken worden naar de url parameter genaamd `id`. Vervolgens zal de hero gefilterd worden uit de lijst van heroes, die in dit artikel gekopieerd zal worden vanuit de `HeroList`-pagina, in een later artikel zal de lijst van heroes verplaatst worden naar een `service` zodat er maar één unieke lijst van heroes bestaat.

Allereerst wordt de `heroes`-lijst gekopieerd en geplakt vanuit de `HeroList`-pagina.

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

Om aan de routeparameter te kunnen, moet de `useRoute`-composable gebruikt worden. Dit is een functie die een route-object teruggeeft, beschikbaar via `vue-router`.

```ts
import { useRoute } from "vue-router";

export default defineComponent({
  setup() {
    const route = useRoute();

    console.log(route.params.id); // Visiting /heroes/15, this will log 15
  },
});
```

Vervolgens wordt er gebruik gemaakt van de `onBeforeMount`-hook, dit is een `lifecycle hook`. Deze hook wordt aangeroepen nadat Vue klaar is met de reactive data op te zetten, maar voordat er DOM-elementen zijn aangemaakt.

```ts
import { defineComponent, onBeforeMount, ref, Ref } from "vue";
import StyledButton from "components/StyledButton.vue";
import { Hero } from "components/models";
import { useRoute } from "vue-router";

export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const route = useRoute();
    const hero: Ref<Hero> = ref() as Ref<Hero>;

    onBeforeMount(() => {
      const { id } = route.params;
      if (id) {
        const matchingHero = heroes.value.find((h) => h.number === +id);
        if (matchingHero) hero.value = matchingHero;
      }
    });

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

    return {
      hero,
    };
  },
});
```

Merk op dat `const hero = ref() as Ref<Hero>` niet langer de hard-coded hero bevat. Deze wordt nu toegekend op basis van de `id`-parameter. Test de code op twee manieren. De eerste manier is om via het dashboard naar de lijst van heroes te gaan, daar op een hero te klikken en vervolgens op de knop `Details` te klikken. De tweede manier is om manueel de route te bezoeken, bijvoorbeeld `/heroes/15`. Probeer ook om manueel een id van een hero in te geven die niet voorkomt in de lijst, bijvoorbeeld `/heroes/69`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/9029358..3eef809)

### Hero niet gevonden

Om de gebruiker van betere feedback te voorzien bij het invoeren van een hero die niet bestaat, zal een `Hero not found!` (hero niet gevonden!)-melding getoond worden als de `hero`-ref leeg is. Dit wordt gedaan door gebruik te maken van de `v-else`-directive. Een alternatief is om de `v-if`-directive te gebruiken met de `!`-operator.

```html
<div v-if="hero">Hero exists!</div>
<div v-else class="title">Hero not found!</div>
```

Alternatief met v-if:

```html
<div v-if="!hero" class="title">Hero not found!</div>
```

Toegepast in de code:

```html
<template>
  <div v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </div>

  <div v-else class="title">Hero not found!</div>
</template>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/3eef809..5deb200)

### Stylen en koppelen van back-knop

Om de knop correct te plaatsen wordt er een class toegevoegd aan de knop. Om te zorgen dat de knop terug navigeert naar de `HeroList`-pagina wanneer er op geklikt wordt, moet er een `click handler` gekoppeld worden.

```html
<StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
```

```scss
.back-button {
  margin-top: 1rem;
}
```

```ts
const moveBack = () => void router.push({ name: ROUTE_NAMES.HERO_LIST });
```

Vergeet niet om de `moveBack`-functie in de `return`-statement te plaatsen zodat deze beschikbaar is in de `template`-tag.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/5deb200..d01060c)

### Two way binding - v-model

Momenteel wordt er gebruik gemaakt van `v-bind:value`, ofwel de verkorte syntax `:value` om de waarde toe te kennen aan de input. Dit is `one way binding`, de input krijgt een waarde toegekend, maar indien de waarde gewijzigd wordt, wordt dit niet doorgevoerd naar de `hero`-ref. Om `two way binding` te bekomen, moet gebruik gemaakt worden van `v-model`.

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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/d01060c..caa059b)

## Conclusie

Een pagina kan opgesplitst worden in herbruikbare componenten.

Het is oké als componenten niet op voorhand gemaakt worden, maar toegevoegd worden wanneer er opgemerkt wordt dat er identieke blokken code zijn doorheen de code base.

Het is belangrijk om potentiële componenten te identificeren. Hoe meer ervaring, hoe beter het identificeren van componenten zal gaan.
