---
postUuid: 5b038d37-dca5-4e50-ba8b-bdafc8cded2b
title: Quasar Tour of Heroes - Theme
slug: quasar-toh-theme
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

Om deze tutorial te kunnen volgen is een gratis [Code Coaching](https://code-coaching.dev/) account nodig.

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een composable/service. Een API met authentificatie is gekoppeld om de Hero data te beheren. Er wordt gebruikt gemaakt van Quasar componenten en plugins. De eindsituatie is exact hetzelfde qua functionaliteit, maar het zal mogelijk zijn om verschillende themas te gebruiken.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-quasarify.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-quasarify)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie]()

Eindsituatie: [Source code]()

## Voorbeeldpagina met dark mode toggle

Voeg een extra pagina toe `Example.vue`, deze is beschikbaar op `/example`. Deze pagina maakt géén gebruik van `MainLayout.vue`.

`src/router/routes.ts`

```ts
import { RouteRecordRaw } from "vue-router";

export const ROUTE_NAMES = {
  DASHBOARD: "Dashboard",
  HERO_LIST: "HeroList",
  HERO_DETAILS: "HeroDetails",
  HERO_ADD: "HeroAdd",
  LOGIN: "Login",
  EXAMPLE: "Example", // new
};

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
      {
        name: ROUTE_NAMES.LOGIN,
        path: "/login",
        component: () => import("pages/Login.vue"),
      },
    ],
  },

  // new
  {
    name: ROUTE_NAMES.EXAMPLE,
    path: "/example",
    component: () => import("pages/Example.vue"),
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

`src/pages/Example.vue`

```html
<template>
  <div class="example-page">
    <StyledButton @click="toggleDarkMode()">Toggle Dark Mode</StyledButton>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useQuasar } from "quasar";
  import StyledButton from "components/StyledButton.vue";

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const $q = useQuasar();

      const toggleDarkMode = () => $q.dark.set($q.dark.isActive ? false : true);

      return {
        toggleDarkMode,
      };
    },
  });
</script>

<style lang="scss">
  .example-page {
    margin: 1rem;
  }
</style>
```

Wijzigen van de authentificatiefunctionaliteit in `MainLayout.vue`.

Voor de wijziging:

```ts
watch(
  () => route.path,
  (newValue) => {
    if (newValue !== ROUTE_NAMES.LOGIN) {
      if (!isAuthenticated.value) {
        tryToAuthenticate()
          .then(() => {
            void getHeroes();
          })
          .catch(() => {
            void router.push({ name: ROUTE_NAMES.LOGIN });
          });
      }
    }
  }
);
```

Deze functionaliteit werkt niet 100% correct, wanneer route.path wijzigt, wordt de nieuwe waarde beschikbaar via `newValue`. Stel dat er naar `/login` genavigeerd wordt, dan bevat `newValue` de string `/login`. De conditionele statement `if (newValue !== ROUTE_NAMES.LOGIN)` vergelijkt `/login` met `Login`, dit zal dus evalueren naar true want `/login` is niet gelijk aan `Login`. Het is dus niet `route.path` maar `route.name` die gebruikt moet worden in de conditionele statement.

Na de wijziging:

```ts
watch(
  () => route.path,
  () => {
    const routesWithoutAuth = [ROUTE_NAMES.LOGIN, ROUTE_NAMES.EXAMPLE];

    if (route.name) {
      if (!routesWithoutAuth.includes(route.name.toString())) {
        if (!isAuthenticated.value) {
          tryToAuthenticate()
            .then(() => {
              void getHeroes();
            })
            .catch(() => {
              void router.push({ name: ROUTE_NAMES.LOGIN });
            });
        }
      }
    }
  },
  { immediate: true }
);
```

Wanneer het pad van de route wijzigt, wordt gekeken of de nieuwe routenaam niet in de array `routesWithoutAuth` staat. Als de naam niet in deze array staat, moet er wél gecheckt worden of de gebruiker is ingelogd.

Voor de loginpagina én voor de voorbeeldpagina is géén authenticatie nodig.

Om testen of het werkt, start de applicatie en ga manueel naar `/example` in de browser. Als je op de knop drukt, zal de dark mode aangezet worden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/9b7174b0c589200a4faaf4e51f7209fc473a40aa)

## Componenten op voorbeeldpagina

Om een beter beeld te krijgen van de impact van de dark mode op de componenten, worden componenten die gebruikt worden in de voorbeeldpagina gekoppeld. Hier kunnen ook componenten worden toegevoegd die niet gebruikt worden in de applicatie om op voorhand te testen of het werkt.

```html
<template>
  <div class="example-page">
    <StyledButton @click="toggleDarkMode()">Toggle Dark Mode</StyledButton>

    <div class="component">StyledButton</div>
    <div v-for="option in quasarButtonOptions" :key="option">
      <div class="option">{{ option }}</div>
      <div class="elements">
        <StyledButton v-bind:[option]="true">Default</StyledButton>
        <StyledButton
          v-bind:[option]="true"
          v-for="color in quasarColors"
          :key="color"
          :color="color"
        >
          {{ color ? color : 'default' }}
        </StyledButton>
      </div>
    </div>

    <div class="component">QInput</div>
    <div v-for="option in quasarInputOptions" :key="option">
      <div class="option">{{ option }}</div>
      <div class="elements">
        <q-input outlined v-bind:[option]="true">Default</q-input>
        <q-input
          outlined
          v-bind:[option]="true"
          v-for="color in quasarColors"
          :key="color"
          :color="color"
          :placeholder="option"
        >
          {{ color ? color : 'default' }}
        </q-input>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useQuasar } from "quasar";
  import StyledButton from "components/StyledButton.vue";

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const $q = useQuasar();

      const toggleDarkMode = () => $q.dark.set($q.dark.isActive ? false : true);

      const quasarColors = [
        "primary",
        "secondary",
        "accent",
        "positive",
        "negative",
        "info",
        "warning",
      ];

      const quasarButtonOptions = [
        "primary",
        "negative"
        "flat",
        "unelevated",
        "push",
        "glossy",
        "fab",
        "fab-mini",
        "loading",
        "disable",
      ];

      const quasarInputOptions = [
        "filled",
        "outlined",
        "borderless",
        "standout",
        "hide-bottom-space",
        "rounded",
        "square",
        "dense",
        "disable",
        "readonly",
        "loading",
      ];

      return {
        toggleDarkMode,
        quasarColors,
        quasarInputOptions,
        quasarButtonOptions,
      };
    },
  });
</script>

<style lang="scss">
  .example-page {
    margin: 1rem;
  }

  .component {
    font-size: 1.2rem;
    margin-top: 1rem;
  }

  .option {
    font-size: 1.1rem;
  }

  .elements {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
</style>
```

`v-bind:[option]="true"` zorgt ervoor dat de `option` wordt toegekend. Stel dat er `'rounded'` in de vierkante haakjes staat, dan wordt eigenlijk `v-bind:rounded="true"` toegekend, wat hetzelfde is als `:rounded="true"`, wat hetzelfde is als `rounded`.

Bij QInput wordt standaard al het attribuut `outlined` toegekend op de eerste input bij elke optie, aangezien er in de applicatie met deze variant gewerkt wordt, dit geeft een beter beeld hoe een `square` en `rounded` inputveld eruit ziet.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/e8b708fb7dc341a668c68c64eacfbf980c254159)

**Let op**: Later in deze tutorial wordt er een `v-model` toegevoegd aan de `q-input` componenten [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/a46d972b41ac33ac21a4c12f24f6e6c10d25d42c).

## Theme service

Het is mogelijk om overal waar de dark mode gewijzigd moet worden, gebruik te maken van `useQuasar` en vervolgens de `$q.dark.set()`-functie aan te roepen. Maar, om te zorgen dat deze afhankelijkheid maar op één plaats wordt gebruikt, zal dit afgehandeld worden in een service. Dit maakt het in de toekomst mogelijk om de dark mode te wijzigen op een totaal andere manier, die niet afhankelijk is van de Quasar-plugin. Dit geeft ook de mogelijkheid om makkelijker extra functionaliteit rondom themes te kunnen toevoegen op één locatie.

`src/services/theme/theme.service.ts`

```ts
import { Dark } from "quasar";

const useTheme = () => {
  const toggleDarkMode = () => Dark.set(Dark.isActive ? false : true);

  return {
    toggleDarkMode,
  };
};

export { useTheme };
```

Wijzig de code in `src/pages/Example.vue` zodat deze gebruik maakt van de nieuwe theme.service.

Voor de wijziging:

```ts
// More code here
import { useQuasar } from 'quasar';

export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const $q = useQuasar();

    const toggleDarkMode = () => $q.dark.set($q.dark.isActive ? false : true);
    // More code here
  }
}
```

Na de wijziging:

```ts
// More code here
import { useTheme } from 'src/services/theme/theme.service';

export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const { toggleDarkMode } = useTheme();
    // More code here
  }
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/6a5f48c39fa78aaf8ffb637c87b58120f5b84273)

## Dark toggle component

Voeg functionaliteit toe aan de theme service om de toestand van de dark mode op te vragen.

```ts
import { Dark } from "quasar";
import { computed } from "vue";

const useTheme = () => {
  const toggleDarkMode = () => Dark.set(Dark.isActive ? false : true);
  const isDarkActive = computed(() => Dark.isActive);

  return {
    toggleDarkMode,
    isDarkActive,
  };
};

export { useTheme };
```

Maak een nieuwe component genaamd `DarkToggle.vue` in `src/services/theme/`.

`src/services/theme/DarkToggle.vue`

```html
<template>
  <q-btn
    round
    outline
    :icon="!isDarkActive ? 'dark_mode' : 'light_mode'"
    @click="toggleDarkMode()"
  />
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useTheme } from "src/services/theme/theme.service";

  export default defineComponent({
    setup() {
      const { toggleDarkMode, isDarkActive } = useTheme();

      return {
        toggleDarkMode,
        isDarkActive,
      };
    },
  });
</script>
```

Deze component maakt gebruik van de functie `toggleDarkMode` en de computed property `isDarkActive` uit de theme service. De reden dat er gekozen wordt voor QBtn i.p.v. StyledButton is omdat de theme service gemaakt wordt vanuit het idee dat het in een ander Quasar project gebruikt zou kunnen worden. Aangezien elk Quasar project wél toegang heeft tot QBtn, maar niet tot StyledButton, wordt er gekozen voor QBtn.

Deze component heeft een dubbel doel. Het eerste doel is om een kant-en-klare knop te voorzien waarmee de dark mode kan worden aangezet of uitgezet. Het tweede doel is dat deze knop als voorbeeld dient hoe de theme service gecombineerd kan worden met elementen.

## Toevoegen v-model aan q-input

Tijdens het schrijven van deze tutorial is er een nieuwe versie van Volar uitgekomen, waarbij een kleine wijziging in `tsconfig.json` nodig is. Door deze wijziging wordt TypeScript ook gevalideerd in de template tag. Hierdoor werd duidelijk dat `v-model` miste op elke `q-input` component.

De wijziging met deze kleine aanpassing: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/a46d972b41ac33ac21a4c12f24f6e6c10d2542c)

## Quasar standaard theming

Voeg elementen toe om de kleuren van de Quasar-variabelen te tonen.

```html
<div class="colors">
  <div
    v-for="color in quasarColors"
    :key="color"
    class="color"
    :class="`bg-${color}`"
  >
    {{ color }}
  </div>
</div>
```

![Default Theme Colors](/img/blog/quasar-toh-theme-colors.png)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/1493fb0ff8c65995454185d1075d5d78b28f3d85)

Om de kleuren te wijzigen, kunnen de SCSS-variabelen in `src/css/quasar.variables.scss` gewijzigd worden.

```scss
$primary: #156473;
$secondary: #195c55;
$accent: #5e096d;

$dark: #1d1d1d;

$positive: #00791c;
$negative: #f80723;
$info: #00d4ff;
$warning: #ffbb00;
```

![Custom Theme Colors](/img/blog/quasar-toh-theme-custom-colors.png)

Nog een andere combinatie van kleuren.

```scss
$primary: #e2ba41;
$secondary: #9e4724;
$accent: #252cae;

$dark: #1d1d1d;

$positive: #255c08;
$negative: #a2074a;
$info: #2e79e8;
$warning: #ee8e19;
```

![Custom Theme Colors](/img/blog/quasar-toh-theme-custom-colors-2.png)

### Dark mode

Wanneer er getoggled wordt naar de dark mode, dan wordt de achtergrond gewijzigd naar een donkere kleur. Bij het kijken naar de namen van de SCSS-variabelen, lijkt het alsof `$dark` de kleur van de achtergrond zal wijzigen.

Wanneer dit uitgetest wordt, wordt de achtergrondkleur helemaal niet gewijzigd. Bij het bekijken van de CSS in de inspector, valt iets op. Elke SCSS-variabele is ook beschikbaar als CSS-variabele.

`$primary` is beschikbaar als `--q-primary`.
`$secondary` is beschikbaar als `--q-secondary`.
...

![Dark Mode Color](/img/blog/quasar-toh-theme-dark-color.png)

Bij het spelen met de kleuren, valt op dat `--q-dark-page` de kleur van de achtergrond wijzigt wanneer dark mode actief is.

![Dark Mode Custom Color](/img/blog/quasar-toh-theme-dark-custom-color.png)

Vanuit de redenering dat `$primary` beschikbaar is als `--q-primary`, kan afgeleid worden dat `$dark-page` een waarde toegekend kan worden die vervolgens beschikbaar is als `--q-dark-page`.

```scss
$primary: #1976d2;
$secondary: #26a69a;
$accent: #9c27b0;

$dark: #1d1d1d;
$dark-page: deeppink;

$positive: #21ba45;
$negative: #c10015;
$info: #31ccec;
$warning: #f2c037;
```

![Dark Mode Custom Color 2](/img/blog/quasar-toh-theme-dark-custom-color-deeppink.png)

Merk op: `$dark-page` is in latere versies van Quasar standaard aanwezig.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/ddc4dd6db92efabdada1749c53b61abf94b24671)

Een tip om specifiek voor light of dark mode te stylen, in `app.scss`:

```scss
/* Light and Dark specific styles */
body {
  // light mode
  background-color: white;
}

body.body--dark {
  // dark mode
  background-color: black;

  img {
    // make image less bright in dark mode
    filter: brightness(0.8);
  }
}
```

## Custom Themes

Het voordeel van de aanpak met SCSS-variabelen, is dat het makkelijk is om de kleuren te wijzigen en zo de volledig look van de Quasar-applicatie te wijzigen.

Het nadeel is dat dit betekent dat de kleuren toegekend moeten worden voordat de compilatie naar de eindcode gedaan wordt.

De gewenste functionaliteit is dat er tussen verschillende themes gewisseld kan worden. Om dit te bekomen is het nodig om de CSS variabelen te wijzigen. Voorzie functionaliteit om de CSS variabelen te wijzigen.

`src/services/theme/theme.service.ts`

```ts
import { Dark, setCssVar } from "quasar";
import { computed } from "vue";

interface Theme {
  isDark: boolean;
  name: string;
  colors: {
    primary: string;
    secondary: string;
    accent: string;

    positive: string;
    negative: string;
    info: string;
    warning: string;

    dark: string;
    "dark-page": string;
    [key: string]: string;
  };
}

const useTheme = () => {
  const toggleDarkMode = () => Dark.set(Dark.isActive ? false : true);
  const isDarkActive = computed(() => Dark.isActive);

  const themes: Array<Theme> = [
    {
      isDark: false,
      name: "Quasar",
      colors: {
        primary: "#1976d2",
        secondary: "#26a69a",
        accent: "#9c27b0",

        positive: "#21ba45",
        negative: "#c10015",
        info: "#31ccec",
        warning: "#f2c037",

        dark: "#1d1d1d",
        "dark-page": "#121212",
      },
    },
    {
      isDark: true,
      name: "Quasar",
      colors: {
        primary: "#1976d2",
        secondary: "#26a69a",
        accent: "#9c27b0",

        positive: "#21ba45",
        negative: "#c10015",
        info: "#31ccec",
        warning: "#f2c037",

        dark: "#1d1d1d",
        "dark-page": "#121212",
      },
    },
    {
      isDark: true,
      name: "Synthwave",
      colors: {
        primary: "#ff7edb",
        secondary: "#b893ce8f",
        accent: "#9c27b0",

        positive: "#20615b",
        negative: "#a21232",
        info: "#3e35a8",
        warning: "#c1a54d",

        dark: "#241b2f",
        "dark-page": "#262335",
      },
    },
  ];

  const toggleTheme = (theme: Theme) => {
    Dark.set(theme.isDark);

    const colorKeys = Object.keys(theme.colors);
    colorKeys.forEach((key) => setCssVar(key, theme.colors[key]));
  };

  return {
    toggleDarkMode,
    isDarkActive,
    themes,
    toggleTheme,
  };
};

export { useTheme, Theme };
```

`setCssVar` is een Quasar-functie en zal een CSS-variabele aanmaken met de `key`, prefixed met `--q-`.

Er is een TypeScript interface voorzien om de structuur van een theme te beschrijven. Merk op dat er `[key: string]: string;` aanwezig is in het color object van de interface, dit betekent 'eender welke key is mogelijk'. Dit wordt momenteel nog niet gebruikt, maar kan later gebruikt worden om eender welke kleur toe te voegen, die vervolgens beschikbaar zal zijn als CSS-variabele `--q-color-name`, waarbij `color-name` de naam van de property zou zijn.

Bijvoorbeeld:

```js
const theme = {
  isDark: false,
  name: "Quasar",
  colors: {
    primary: "#1976d2",
    secondary: "#26a69a",
    accent: "#9c27b0",

    positive: "#21ba45",
    negative: "#c10015",
    info: "#31ccec",
    warning: "#f2c037",

    dark: "#1d1d1d",
    "dark-page": "#121212",

    "extra-color": deeppink, // --q-extra-color
    brand: "#6943ab", // --q-brand
  },
};
```

`src/services/theme/ThemeToggle.vue`

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/cca5606c07cdb49d2e56420c4191378675a61354)

### Nadeel van deze aanpak

Doordat de CSS-variabelen kunnen wijzigen, is het niet mogelijk om SCSS-functies te gebruiken.

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    background-color: darken($primary, 5%);
    color: white;
  }
}
```

Bij het inladen van de applicatie wordt eerst de default theme toegepast, deze heeft als `$primary`, de blauwe kleur `#1976d2`. Deze is beschikbaar als de CSS-variabele `--q-primary`, het is `--q-primary` dat gebruikt wordt als waarde voor de `background-color`.

Bij `&:hover` wordt de `background-color` overschreven `darken($primary, 5%)`. Dit betekent dat de kleur `#1976d2` 5% donkerder wordt gemaakt, het resultaat van deze berekening is de kleur die toegekend wordt aan de `background-color`.

Wanneer vervolgens `--q-primary` wordt gewijzigd, wordt wél de achtergrondkleur van de knop gewijzigd naar de nieuwe kleur. Maar de achtergrondkleur 'on hover' heeft nog altijd het 5% donkerder blauw als waarde. Dit is te zien in onderstaand voorbeeld.

![SCSS function](/img/blog/quasar-toh-theme-scss-variable.gif)

Om dit te vermijden, mag geen gebruik gemaaktw orden van SCSS-functies. Gebruik in plaats hiervan, gewone CSS.

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    background-color: darken($primary, 5%);
    color: white;
  }
}
```

Lichter maken kan door gebruik te maken van `opacity`:

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    opacity: 0.9; // between 0 and 1
    color: white;
  }
}
```

Donkerder maken kan door gebruik te maken van een brightness filter:

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    filter: brightness(0.9); // 0.9 = 10% darker
    color: white;
  }
}
```

Lichter maken kan in principe ook door gebruik te maken van de brightness filter:

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    filter: brightness(1.1); // 1.1 = 10% lighter
    color: white;
  }
}
```

Aangezien we de default button gewijzigd hebben door een QBtn, kan alle hover state verwijderd worden uit de custom styling. Quasar handelt dit al correct af achter de schermen!

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/de910a5006e094d3a3cda361d64279dec5adc924

### Uitbesteden aan Quasar

Momenteel wordt primary en negative gezet via props. Quasar voorziet al een manier om QBtn van primary en negative styling te voorzien. Door het zetten van de `color` property.

Voor de wijziging:

```html
<StyledButton primary>Primary</StyledButton>

<StyledButton secondary>Secondary</StyledButton>
```

Na de wijziging:

```html
<StyledButton color="primary">Primary</StyledButton>

<StyledButton color="secondary">Secondary</StyledButton>
```

Alle props/emits die bestaan op de gewone Quasar component, kunnen ook direct gebruikt worden op de wrappercomponent.

Enkel de custom styling blijft nog over in de wrappercomponent.

```html
<template>
  <q-btn>
    <slot />
  </q-btn>
</template>

<style lang="scss" scoped>
  button {
    background-color: #eeeeee;
    border-radius: 0.25rem;
    padding: 0.25rem 0.5rem;
    color: #567868;
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/7bce768224595005f9a2073f453d12b906c29e8e)

## Quasar defaults

Het gewenste resultaat is dat de eindgebruiker zelf kan beslissen hoe de applicatie eruit ziet. Er zullen enkele dingen gewijzigd worden om dit te bereiken, zo zal er geen wrapper component meer zijn genaamd `StyledButton`, maar zal de component `QBtn` gebruikt worden. De `ButtonGroup` wordt refactort naar een `FlexWrap` component, wat op veel plaatsen gebruikt zal worden.

![Example Page](/img/blog/quasar-toh-theme-example-page.gif)

### Refactor ButtonGroup

Refactor `ButtonGroup`, hernoem naar `FlexWrap`. Deze component is een wrapper voor alle elementen die `display: flex;` nodig hebben met `flex-wrap: wrap;` (hierdoor zullen elementen automatisch naar een nieuwe lijn springen indien er geen plaats meer is op de huidige lijn). Standaard wordt een `gap: 1rem;` toegepast. Via een property `dense` kan de `gap` verminderd worden naar `0.5rem;`. Via een property `column` kan de `flex-direction` veranderd worden naar `column;`.

```html
<template>
  <div
    class="flex-wrap"
    :class="{
      'flex-wrap--dense': dense,
       'flex-wrap--column': column,
     }"
  >
    <slot></slot>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  export default defineComponent({
    props: {
      dense: {
        type: Boolean,
        default: false,
      },
      column: {
        type: Boolean,
        default: false,
      },
    },
  });
</script>

<style lang="scss" scoped>
  .flex-wrap {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    &--dense {
      gap: 0.5rem;
    }
    &--column {
      flex-direction: column;
    }
  }
</style>
```

Let op: overal waar `FlexWrap` gebruikt wordt, zal deze ook geïmporteerd moeten worden en vervolgens toegevoegd worden aan de property `components` van het configuratie-object dat meegegeven wordt aan `defineComponent`.

```js
import FlexWrap from "src/components/FlexWrap.vue";

export default defineComponent({
  components: {
    FlexWrap,
  },
  setup() {
    // ...
  },
});
```

Wijzig alle elementen waarop momenteel een `flex-wrap: wrap;` gebruikt wordt, naar `FlexWrap`. Vervolgens kunnen de CSS properties die afgehandeld worden door de `FlexWrap` component verwijderd worden van de elementen waarop dit eerst apart werd toegevoegd. Indien er geen CSS properties meer overblijven, kan de volledige CSS-selector verwijderd worden.

Merk op: Het element met class `.top-heroes` wordt niet gewijzigd.

`MainLayout.vue`:

- `.button-container` (selector kan verwijderd worden)

`Example.vue`:

- `.top-row` (selector kan verwijderd worden)
- elk element met de class `.colors` (verwijder `display`, `flex-wrap` en `gap` properties
- elk element met de class `.elements` (verwijder `display`, `flex-wrap` en `gap` properties)

`HeroAdd.vue`:

- `ButtonGroup` wordt gewijzigd naar `FlexWrap`

`Herodetails.vue`:

- `ButtonGroup` wordt gewijzigd naar `FlexWrap`

`HeroList.vue`:

- element met de class `.hero-list` (selector kan verwijderd worden)
- `ButtonGroup` wordt gewijzigd naar `FlexWrap`

`Login.vue`:

- element met class `.login-container` (verwijder `display`, `flex-wrap` en `gap` properties), voeg aan deze `FlexWrap` ook de property `column` toe

### Refactor StyledButton

Om te zorgen dat auto complete van props en emits behouden kan blijven, zal overal gebruikgemaakt worden van `QBtn` i.p.v. de wrappercomponent `StyledButton`. Verwijder het bestand `StyledButton.vue`, overal waar deze gebruikt wordt, verwijder je de import regel, verwijd je de component uit de `components` property en vervolgens wijzig je `StyledButton` in de html naar `q-btn`. De default styling waar de wrappercomponent voor gebruikt werd, is niet meer nodig. Dit wordt kort hierna vervangen door een configuratie object dat on the fly de styling aanpast.

StyledButton komt voor op de volgende plaatsen:

- `MainLayout.vue`
- `Example.vue`
- `HeroAdd.vue`
- `HeroDetails.vue`
- `HeroList.vue`
- `Login.vue`

### Component API

Maak een nieuw bestand aan waarin de component API wordt gedefinieerd, hierin zullen alle properties en de waarde van de properties worden bijgehouden. Zowel van de eigen componenten (geplaatst in het `customApi` object) als van de Quasar componenten (geplaatst in het `quasarApi` object) zullen de properties worden gedefinieerd.

Voorzie functionaliteit om de waarden van de properties te `getten` en `setten`. Voorzie functionaliteit om een object terug te geven waarvan elke key de naam van de property is en de value zal de waarde zijn die hoort bij de property.

`src/services/theme/api.ts`

```ts
import { ref, Ref } from "vue";

interface Property {
  description?: string;
  value?: boolean;
}

export type ComponentApi = Record<string, Record<string, Property>>;

const customApi = {
  FlexWrap: {
    dense: {
      value: false,
    },
  },
};

const quasarApi = {
  QBtn: {
    outline: { value: false },
    flat: { value: false },
    unelevated: { value: false },
    rounded: { value: false },
    push: { value: false },
    glossy: { value: false },
    fab: { value: false },
    "fab-mini": { value: false },
    dense: { value: false },
    ripple: { value: false },
    round: { value: false },
  },
  QCheckbox: { dense: { value: false } },
  QColor: {
    square: { value: false },
    flat: { value: false },
    bordered: { value: false },
  },
  QInput: {
    filled: { value: false },
    outlined: { value: false },
    borderless: { value: false },
    standout: { value: false },
    "hide-bottom-space": { value: false },
    rounded: { value: false },
    square: { value: false },
    dense: { value: false },
    "item-aligned": { value: false },
  },
};

const componentApi: Ref<ComponentApi> = ref({
  ...customApi,
  ...quasarApi,
});

const syncProps = ref(false);

const getDefaults = (property: string) => {
  const defaultProps: Record<string, boolean> = {};
  const prop = componentApi.value[property];
  if (prop) {
    const propertyNames = Object.keys(prop);
    propertyNames.forEach((propertyName) => {
      defaultProps[propertyName] =
        !!componentApi.value[property][propertyName].value;
    });
  }
  return defaultProps;
};

const getPropertyValue = (component: string, property: string) => {
  return componentApi.value[component][property].value;
};

const setMatchingProperties = (property: string, newValue: boolean) => {
  const componentNames = Object.keys(componentApi.value);
  componentNames.forEach((componentName) => {
    const component = componentApi.value[componentName];
    const properties = Object.keys(component);
    properties.forEach((prop) => {
      let matches = [prop];

      const OUTLINED_PROPERTIES = ["outline", "outlined"];
      if (OUTLINED_PROPERTIES.includes(prop)) matches = OUTLINED_PROPERTIES;

      if (matches.includes(property)) {
        matches.forEach((propertyName) => {
          const matchingProperty =
            componentApi.value[componentName][propertyName];
          if (matchingProperty) matchingProperty.value = newValue;
        });
      }
    });
  });
};

const setPropertyValue = (
  component: string,
  property: string,
  syncProps = false
) => {
  const newValue = !getPropertyValue(component, property);
  if (syncProps) setMatchingProperties(property, newValue);
  else componentApi.value[component][property].value = newValue;
};

export {
  componentApi,
  syncProps,
  getDefaults,
  getPropertyValue,
  setPropertyValue,
};
```

Importeer de API en de functionaliteit om de API te wijzigen in `src/services/theme/theme.service.ts` en zorg ervoor de de functionaliteit opnieuw teruggegeven wordt, zodat deze uiteindelijk via `useTheme()` gebruikt kunnen worden.

```ts
import { Dark, LocalStorage, setCssVar, Notify } from "quasar";
import { computed, Ref, ref, watch } from "vue";
import {
  ComponentApi,
  componentApi,
  getDefaults,
  getPropertyValue,
  setPropertyValue,
  syncProps,
} from "./api";

interface Theme {
  isDark: boolean;
  name: string;
  colors: {
    primary: string;
    secondary: string;
    accent: string;

    positive: string;
    negative: string;
    info: string;
    warning: string;

    dark: string;
    "dark-page": string;
    [key: string]: string;
  };
  custom?: boolean;
}

const themes: Array<Theme> = [
  {
    isDark: false,
    name: "Quasar",
    colors: {
      primary: "#1976d2",
      secondary: "#26a69a",
      accent: "#9c27b0",

      positive: "#21ba45",
      negative: "#c10015",
      info: "#31ccec",
      warning: "#f2c037",

      dark: "#1d1d1d",
      "dark-page": "#121212",
    },
  },
  {
    isDark: true,
    name: "Quasar",
    colors: {
      primary: "#1976d2",
      secondary: "#26a69a",
      accent: "#9c27b0",

      positive: "#21ba45",
      negative: "#c10015",
      info: "#31ccec",
      warning: "#f2c037",

      dark: "#1d1d1d",
      "dark-page": "#121212",
    },
  },
  {
    isDark: true,
    name: "Synthwave",
    colors: {
      primary: "#ff7edb",
      secondary: "#b893ce",
      accent: "#9c27b0",

      positive: "#20615b",
      negative: "#a21232",
      info: "#3e35a8",
      warning: "#c1a54d",

      dark: "#241b2f",
      "dark-page": "#262335",
    },
  },
];

const activeTheme: Ref<Theme> = ref(themes[0]);

const useTheme = () => {
  const toggleDarkMode = () =>
    (activeTheme.value.isDark = !activeTheme.value.isDark);
  const isDarkActive = computed(() => Dark.isActive);

  const toggleTheme = (theme: Theme) => {
    activeTheme.value = theme;
    Dark.set(theme.isDark);

    const colorKeys = Object.keys(theme.colors);
    colorKeys.forEach((key) => setCssVar(key, theme.colors[key]));
  };

  watch(activeTheme, (theme: Theme) => toggleTheme(theme), { deep: true });

  const saveCustomTheme = () => {
    LocalStorage.set("theme", {
      ...activeTheme.value,
      name: "Custom",
      custom: true,
    });
    LocalStorage.set("properties", componentApi.value);
    Notify.create({ message: "Theme saved", color: "positive" });

    const customTheme = themes.findIndex((t) => t.name === "Custom");
    themes[customTheme] = activeTheme.value;
  };

  const loadCustomTheme = () => {
    const theme = LocalStorage.getItem("theme") as Theme;
    if (theme) {
      if (!themes.find((t) => t.name === theme.name)) themes.push(theme);
      activeTheme.value = theme;
    }
    const properties = LocalStorage.getItem("properties") as ComponentApi;
    if (properties) {
      Object.keys(properties).forEach((key) => {
        componentApi.value[key] = properties[key];
      });
    }
  };

  return {
    isDarkActive,
    themes,
    componentApi,
    activeTheme,
    syncProps,
    toggleDarkMode,
    toggleTheme,
    getDefaults,
    getPropertyValue,
    setPropertyValue,
    saveCustomTheme,
    loadCustomTheme,
  };
};

export { useTheme, Theme };
```

`themes` en `activetheme` zijn verplaatst naar buiten de `useTheme`-functie. Dit zorgt ervoor dat overal verwezen wordt naar dezelfde variabelen.

De implementatie van `toggleDarkMode` is gewijzigd, dit wijzigt nu de `isDark`-waarde van de `activeTheme`-variabele.

Er is een `watch` op de `activeTheme`-variabele toegevoegd, dit zorgt ervoor dat wanneer de waarde van één van de properties van de `activeTheme`-variabele wijzigt, de `toggleTheme`-functie wordt aangeroepen. Door `{ deep: true }` zal ook de wijziging van geneste properties opgemerkt worden.

In de `toggleTheme`-functie wordt nu ook de `activeTheme` gewijzigd.

Er is functionaliteit voorzien om de keuze van themakleuren en de keuze van default properties op te slaan in LocalStorage en om deze vervolgens terug in te laden.

Om te zorgen dat een component de gekozen default properties gebruikt, zal elke component gekoppeld moeten worden via `v-bind="getDefaults('componentName')"`. Waar `componentName` de naam van de component is. Bijvoorbeeld:

```html
<q-btn v-bind="getDefaults('QBtn')">Example</q-btn>
<q-input v-bind="getDefaults('QInput')">Example</q-input>
<FlexWrap v-bind="getDefaults('FlexWrap')">Example</FlexWrap>
```

## Andere wijzigingen

`DarkToggle` is nu ook gekoppeld in `MainLayout.vue`, om te zorgen dat de knop altijd zichtbaar is, is er een extra prop toegevoegd.

`src/services/theme/DarkToggle.vue`

```html
<template>
  <q-btn
    round
    outline
    :class="{ 'text-white': forceLight }"
    :icon="!isDarkActive ? 'dark_mode' : 'light_mode'"
    @click="toggleDarkMode()"
  />
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useTheme } from "src/services/theme/theme.service";

  export default defineComponent({
    props: {
      forceLight: {
        type: Boolean,
        default: false,
      },
    },
    setup() {
      const { toggleDarkMode, isDarkActive } = useTheme();

      return {
        toggleDarkMode,
        isDarkActive,
      };
    },
  });
</script>
```

Er is ook een route toegevoegd om naar de settings/example page te gaan.

Alle wijzigingen:
[GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d660166880690aa52273742b21aef9a51fab111e)

## API Extractor

Om te helpen met het opzetten van het configuratie-object om de properties van de Quasar-componenten te beheren, is er een bestand toegevoegd genaamd `api-extractor.js`, deze is terug te vinden in de map `api-extractor` en bevat ook een `README.md` met hoe dit gebruikt kan worden.

De file kan bekeken worden via [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/47ce8f0d937ee545b3d4adee04d53c53540a06b3).

## Conclusie

Doordat er een wijziging is in de verwachting hoe de applicatie werkt, kan het zijn dat eerdere belissingen niet meer de correcte beslissingen zijn. Dit zorgt voor refactorwerk om te voldoen aan de nieuwe verwachtingen. Werken met verschillende themas op gebied van kleuren kan redelijk eenvoudig. Elke component wijzigen op basis van de keuze van de gebruiker is iets ingewikkelder en vereist extra werk van de developer, maar geeft meer vrijheid aan de gebruiker. Het is een goed idee om bij custom components gelijkaardige properties te voorzien die aanwezig zijn in het framework dat gebruikt wordt, in dit geval Quasar. Dit is als voorbeeld toegevoegd bij de `FlexWrap`-component.
