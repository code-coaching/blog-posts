---
  title: Quasar Themes
  keywords: ["Quasar", "Vue", "Theme", "SCSS", "CSS"]
  modules: ['']
---

Code can be found on [GitHub](https://github.com/code-coaching/blog-posts-examples/tree/quasar-themes)!

At the end of this blog it is possible to apply multiple themes to a Quasar project through a theme switcher.

## Generate a new Quasar project

In the directory where the project should be generated, execute:

```sh
quasar create .
```

Choose the default option for everything. Except for the last one, pick `Yes, use NPM`.

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

## What Quasar provides out of the box

### CSS variables

In `src/css/quasar.variables.scss` there are a couple of variables defined.

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

This are made available `CSS variables`, which can be seen in the css viewed in the browser.

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

Note: `--q-dark-page` is set automatically by Quasar.

### Body classes

When using a light theme, the class `body--light` is placed on the body tag.

```html
<body class="desktop no-touch body--light"></body>
```

When using a dark theme, the class `body--dark` is placed on the body tag.

```html
<body class="desktop no-touch body--dark"></body>
```

### Dark mode plugin

In `src/css/pages/Index.vue`, use the plugin to see the `body--light` and `body--dark` functionality at work.

```vue
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

Open up the inspector and take a look at the body tag.

Now change `$q.dark.set(false)` into `$q.dark.set(true)`.

Take a look at the body tag again.

### setCssVar utility

Quasar also provides a way to set CSS variables. This is just a function that sets CSS variables and prepends the names with `--q-`.

As an example `setCssVar("primary", "#FF0000");` would create a variable called `--q-primary` that has the color `#FF0000` which is red.

## Creating a theme switcher

```vue
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

In the `template` tag a button with a dropdown is added. When any of the themes is clicked, `applyTheme(theme)` will execute.

As seen in the `script` tag, the `applyTheme` function will use the information from the theme object to determine if Quasar should handle it as a dark theme (`body--dark`) or as a light theme (`body--light`):

```js
$q.dark.set(theme.isDark);
```

It will set all the necessary variables that Quasar uses by default in all of the components:

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

It will set the `selectedTheme` ref to the chosen theme:

```js
selectedTheme.value = theme;
```

At the end of the `script` tag a theme is applied:

```js
applyTheme(themes[0]);
```

To have the dark theme as a default `applyTheme(themes[1])` or to have Synthwave as a default `applyTheme(themes[2])`.

## Optimizing the code

Of course, this is code of a mad man:

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

This can be replace by:

```js
const applyTheme = (theme) => {
  $q.dark.set(theme.isDark);

  Object.keys(theme).forEach((key) => setCssVar(key, theme[key]));

  setCssVar("dark", theme["background"]);
  setCssVar("dark-page", theme["background"]);

  selectedTheme.value = theme;
};
```

This way, if there ever is a property added to a theme object, it will automatically become available as a CSS variable.

Note: The variables in `src/css/quasar.variables.scss` are no longer used, since these variables are handled by the theme switcher.

## Conclusion

Quasar already has some utilities in place for light and dark themes. Quasar also has some predefined CSS variables. It is possible to leverage these utilities (Dark plugin, setCssVar) and the knowledge about Quasar to create a theme switcher that handles both light and dark themes.