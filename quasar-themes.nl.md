---
  title: Quasar Themas
  keywords: ["Quasar", "Vue", "Thema", "SCSS", "CSS"]
  modules: ['']
---
## Nieuw Quasar-project genereren

Voer onderstaande uit in de map waar het project gegenereerd moet worden:

```sh
quasar create .
```

Neem voor alles de standaardoptie. Behalve voor de laatste optie, daar wordt er gekozen voor `Yes, use NPM`.

```sh
  ___
 / _ \ _   _  __ _ ___  __ _ _ __
| | | | | | |/ _` / __|/ _` | '__|
| |_| | |_| | (_| \__ \ (_| | |
 \__\_\\__,_|\__,_|___/\__,_|_|



? Generate project in current directory? Yes
? Project name (internal usage for dev) quasar-themes
? Project product name (must start with letter if building mobile apps) Quasar Themes
? Project description A Quasar example with multiple themes
? Author Bart Duisters <bartduisters@bartduisters.com>
? Pick your CSS preprocessor: SCSS
? Check the features needed for your project: ESLint (recommended)
? Pick an ESLint preset: Prettier
? Continue to install project dependencies after the project has been created? (recommended)
  Yes, use Yarn (recommended)
❯ Yes, use NPM
  No, I will handle that myself
```

## Wat Quasar standaard voorziet

### CSS-variablen

In `src/css/quasar.variables.scss` zijn een aantal variabelen gedefinieerd.

```css
$primary: #1976d2;
$secondary: #26a69a;
$accent: #9c27b0;

$dark: #1d1d1d;

$positive: #21ba45;
$negative: #c10015;
$info: #31ccec;
$warning: #f2c037;
```

Dit wordt beschikbaar gesteld als `CSS-variablen`, dit is te zien in de browser bij het bekijken van de CSS.

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

Opmerking: `--q-dark-page` wordt automatisch gezet door Quasar.

### Body classes

Bij het gebruiken van een licht thema zal de class `body--light` op de body-tag geplaatst worden.

```html
<body class="desktop no-touch body--light"></body>
```

Bij het gebruiken van een donker thema zal de class `body--light` op de body-tag geplaatst worden.

```html
<body class="desktop no-touch body--dark"></body>
```

### Dark mode plugin

Maak in `src/css/pages/Index.vue` gebruik van de plugin om de werking van `body--light` en `body--dark` te zien.

```html
<template>
  <q-page class="flex flex-center">
    Dark active: {{ $q.dark.isActive }}
  </q-page>
</template>

<script>
  import { defineComponent } from "vue";
  import { useQuasar } from "quasar";

  export default defineComponent({
    name: "PageIndex",
    setup() {
      const $q = useQuasar();

      $q.dark.set(false);

      return {
        $q,
      };
    },
  });
</script>
```

Open de `inspector` en bekijk de body-tag.

Wijzig `$q.dark.set(false)` naar `$q.dark.set(true)`.

Bekijk de body-tag opnieuw.

### setCssVar utility

Quasar voorziet ook een manier om CSS-variabelen te zetten. Dit is een functie die een CSS-variabele aanmaakt en zal `--q-` voor de naam zetten.

Bijvoorbeeld `setCssVar("primary", "#FF0000");` zou een variabele genaamd `--q-primary` aanmaken, waarvan de kleur `#FF0000` (rood) is.

## Een themaswitcher maken

```html
<template>
  <q-page class="flex flex-center">
    <q-btn-dropdown
      color="primary"
      v-if="selectedTheme"
      :label="selectedTheme.name"
    >
      <q-list>
        <q-item
          v-for="(theme, index) in themes"
          :key="index"
          clickable
          v-close-popup
          @click="applyTheme(theme)"
        >
          <q-item-section>
            <q-item-label>{{ theme.name }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-btn-dropdown>
  </q-page>
</template>

<script>
  import { defineComponent, ref } from "vue";
  import { useQuasar, setCssVar } from "quasar";

  export default defineComponent({
    name: "PageIndex",
    setup() {
      const $q = useQuasar();

      const themes = [
        {
          isDark: false,
          name: "Quasar",

          primary: "#1976d2",
          secondary: "#26a69a",
          accent: "#9c27b0",

          positive: "#21ba45",
          negative: "#c10015",
          info: "#31ccec",
          warning: "#f2c037",

          background: "#fff",
        },
        {
          isDark: true,
          name: "Quasar Dark",

          primary: "#6d6d6d",
          secondary: "#5d5d5d",
          accent: "#4d4d4d",

          positive: "#20615b",
          negative: "#a21232",
          info: "#3e35a8",
          warning: "#c1a54d",

          background: "#2c2c2c",
        },
        {
          isDark: true,
          name: "Synthwave",

          primary: "#ff7edb",
          secondary: "#b893ce8f",
          accent: "#9c27b0",

          positive: "#20615b",
          negative: "#a21232",
          info: "#3e35a8",
          warning: "#c1a54d",

          background: "#262335",
        },
      ];

      const selectedTheme = ref();

      const applyTheme = (theme) => {
        $q.dark.set(theme.isDark);

        setCssVar("primary", "primary"); // theme["primary"] is the same as theme.primary
        setCssVar("secondary", theme["secondary"]);
        setCssVar("accent", theme["accent"]);

        setCssVar("positive", theme["positive"]);
        setCssVar("negative", theme["negative"]);
        setCssVar("info", theme["info"]);
        setCssVar("warning", theme["warning"]);

        setCssVar("background", theme["background"]);

        setCssVar("dark", theme["background"]);
        setCssVar("dark-page", theme["background"]);

        selectedTheme.value = theme;
      };

      applyTheme(themes[0]);

      return {
        themes,
        selectedTheme,
        applyTheme,
      };
    },
  });
</script>
```

In de`template`-tag is een knop met een dropdown toegevoegd. Wanneer één van de themas wordt aangeklikt, zal de functie `applyTheme(theme)` uitgevoerd worden.

Deze functie is gedefinieerd in de `script`-tag. De functie zal de informatie van het thema-object gebruiken om te bepalen of Quasar het moet behandelen als een donker thema (`body--dark`) of als een licht thema (`body--light`):

```js
$q.dark.set(theme.isDark);
```

Het zal ook alle nodige variabelen aanmaken die Quasar standaard gebruikt in alle componenten:

```js
setCssVar("primary", "#FF0000");
setCssVar("secondary", theme["secondary"]);
setCssVar("accent", theme["accent"]);

setCssVar("positive", theme["positive"]);
setCssVar("negative", theme["negative"]);
setCssVar("info", theme["info"]);
setCssVar("warning", theme["warning"]);

setCssVar("background", theme["background"]);

setCssVar("dark", theme["background"]);
setCssVar("dark-page", theme["background"]);
```

Het zal het gekozen thema toekennen aan de ref `selectedTheme`:

```js
selectedTheme.value = theme;
```

Op het einde van de `script`-tag wordt een thema toegepast:

```js
applyTheme(themes[0]);
```

Om standaard het donkere thema te hebben kan `applyTheme(themes[1])` gebruikt worden. Om standaard Synthwave als thema te hebben kan `applyTheme(themes[2])` gebruiktw orden.

## De code optimaliseren

Natuurlijk is dit code die geschreven is door een krankzinnig persoon:

```js
const applyTheme = (theme) => {
  $q.dark.set(theme.isDark);

  setCssVar("primary", theme["primary"]); // theme["primary"] is the same as theme.primary
  setCssVar("secondary", theme["secondary"]);
  setCssVar("accent", theme["accent"]);

  setCssVar("positive", theme["positive"]);
  setCssVar("negative", theme["negative"]);
  setCssVar("info", theme["info"]);
  setCssVar("warning", theme["warning"]);

  setCssVar("background", theme["background"]);

  setCssVar("dark", theme["background"]);
  setCssVar("dark-page", theme["background"]);

  selectedTheme.value = theme;
};
```

Het kan vervangen worden door:

```js
const applyTheme = (theme) => {
  $q.dark.set(theme.isDark);

  Object.keys(theme).forEach((key) => setCssVar(key, theme[key]));

  setCssVar("dark", theme["background"]);
  setCssVar("dark-page", theme["background"]);

  selectedTheme.value = theme;
};
```

Op deze manier zal elke property die toegevoegd wordt aan het thema-object, autmoatisch beschikbaar zijn als CSS-variabele.

Opmerking: De variabelen in `src/css/quasar.variables.scss` worden niet meer gebruikt. Alle variabelen worden nu afgehandeld door de themaswitcher.

## Conclusie

Quasar biedt al wat functionaliteit aan om met lichte en donkere themas te werken. Quasar heeft ook al voorgedefinieerde CSS-variabelen. Het is mogelijk om met de functionaliteit (Dark plugin, setCssVar) en kennis over hoe Quasar werkt een themaswitcher te maken die overweg kan met zowel lichte als donkere themas.

Code terug te vinden op [GitHub](https://github.com/code-coaching/blog-posts-examples/tree/quasar-themes)!
