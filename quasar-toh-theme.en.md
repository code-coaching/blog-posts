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

A free [Code Coaching](https://code-coaching.dev/) account is required to follow this tutorial.

This article will start from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The pages have been provided with elements, data and components. The data has been moved to a composable/service. An API with authentication is linked to manage the Hero data. Quasar components and plugins are used. The final situation is exactly the same in terms of functionality, but it will be possible for the user to set their own theme.

The code can be obtained by downloading a zip from the source code.

The situation that will be used as a starting point is as follows:

![starting situation](/img/blog/quasar-toh-quasarify.gif)

Starting situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-quasarify)

The final situation being worked towards is as follows:

![end situation](/img/blog/quasar-toh-theme-example-page.gif)

End situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-theme)

## Example page with dark mode toggle

Add an additional page `Example.vue`, this is available at `/example`. This page does not use the `MainLayout.vue`.

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

Modify the authentication functionality in `MainLayout.vue`.

Before the change:

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

This functionality does not work 100% correctly, when route.path changes, the new value becomes available through `newValue`. Suppose navigating to `/login`, `newValue` contains the string `/login`. The conditional statement `if (newValue !== ROUTE_NAMES.LOGIN)` compares `/login` to `Login`, this will therefore evaluate to true because `/login` is not equal to `Login`. So it is not `route.path` but `route.name` that should be used in the conditional statement.

After the change:

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

When the path of the route changes, we check if the new route name is not in the array `routesWithoutAuth`. If the name is not in this array, it is necessary to check whether the user is logged in.

For the login page and for the example page no authentication is needed.

To test whether it works, start the application and manually go to `/example` in the browser. If you press the button, the dark mode will be enabled.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/9b7174b0c589200a4faaf4e51f7209fc473a40aa)

## Components on example page

To get a better idea of the impact of the dark mode on the components, components will be wired in to the example page. Here you can also add components that are not used in the application to test in advance if it works.

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

`v-bind:[option]="true"` causes the `option` to be assigned. Suppose there is `'rounded'` in the square brackets, then actually `v-bind:rounded="true"` is assigned, which is the same as `:rounded="true"`, which is the same as `rounded`.

In QInput, by default the attribute `outlined` is already assigned on the first input at each option, since the application works with this variant, this gives a better idea of what a `square` and `rounded` input field looks like.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/e8b708fb7dc341a668c68c64eacfbf980c254159)

**Please note**: Later in this tutorial, an `v-model` will be added to the `q-input` components [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/a46d972b41ac33ac21a4c12f24f6e6c10d25d42c).

## Theme service

It is possible to use `useQuasar` wherever the dark mode needs to be changed and then call the `$q.dark.set()` function. But, to ensure that this dependency is only used in one place, this will be handled in a service. This will make it possible in the future to change the dark mode in a completely different way that does not depend on the Quasar plugin. This will also allow for easier addition of additional functionality around themes in one location.

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

Change the code in `src/pages/Example.vue` so that it uses the new theme.service.

Before the change:

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

After the change:

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

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/6a5f48c39fa78aaf8ffb637c87b58120f5b84273)

## Dark toggle component

Add functionality to the theme service to query the state of the dark mode.

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

Create a new component called `DarkToggle.vue` in `src/services/theme/`.

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

This component uses the `toggleDarkMode` function and the computed property `isDarkActive` from the theme service. The reason for choosing QBtn instead of StyledButton is that the theme service is created with the idea that it could be used in another Quasar project. Since every Quasar project has access to QBtn but not to StyledButton, QBtn is chosen.

This component has a dual purpose. The first purpose is to provide a ready-made button that can be used to turn on or off the dark mode. The second purpose is for this button to serve as an example of how the theme service can be combined with elements.

## Add v-model to q-input

While writing this tutorial, a new version of Volar has been released, requiring a small change in `tsconfig.json`. This change also validates TypeScript in the template tag. This made it clear that `v-model` was missing on every `q-input` component.

The change with this small modification: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/a46d972b41ac33ac21a4c12f24f6e6c10d2542c)

## Quasar default theming

Add elements to show the colors of the Quasar variables.

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

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/1493fb0ff8c65995454185d1075d5d78b28f3d85)

To change the colors, the SCSS variables in `src/css/quasar.variables.scss` can be changed.

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

Yet another combination of colors.

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

When toggled to dark mode, the background is changed to a dark color. When looking at the names of the SCSS variables, it looks like `$dark` will change the background color.

When testing this out, the background color is not changed at all. When looking at the CSS in the inspector, something stands out. Every SCSS variable is also available as a CSS variable.

`$primary` is available as `--q-primary`.
`$secondary` is available as `--q-secondary`.
...

![Dark Mode Color](/img/blog/quasar-toh-theme-dark-color.png)

When playing with the colors, notice that `--q-dark-page` changes the color of the background when dark mode is active.

![Dark Mode Custom Color](/img/blog/quasar-toh-theme-dark-custom-color.png)

From the reasoning that `$primary` is available as `--q-primary`, it can be derived that `$dark-page` can be assigned a value that is then available as `--q-dark-page`.

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

Note: `$dark-page` is present by default in later versions of Quasar.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/ddc4dd6db92efabdada1749c53b61abf94b24671)

A tip to style specifically for light or dark mode, in `app.scss`:

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

The advantage of the approach with SCSS variables, is that it is easy to change the colors and thus change the complete look of the Quasar application.

The disadvantage is that this means that the colors must be assigned before the compilation to the final code is done.

The desired functionality is to be able to switch between different themes. To achieve this it is necessary to change the CSS variables. Provide functionality to change the CSS variables.

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

`setCssVar` is a Quasar function and will create a CSS variable with the `key`, prefixed with `--q-`.

A TypeScript interface is provided to describe the structure of a theme. Note that there is `[key: string]: string;` present in the color object of the interface, this means 'any key is possible'. This is not currently used, but can be used later to add any color, which will then be available as a CSS variable `--q-color-name`, where `color-name` would be the name of the property.

For example:

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

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/cca5606c07cdb49d2e56420c4191378675a61354)

### Disadvantage of this approach

Because the CSS variables can change, it is not possible to use SCSS functions.

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

When loading the application the default theme is applied first, it has as `$primary`, the blue color `#1976d2`. This is available as the CSS variable `--q-primary`, it is `--q-primary` that is used as the value for the `background-color`.

At `&:hover` the `background-color` is overwritten `darken($primary, 5%)`. This means that the color `#1976d2` is made 5% darker, the result of this calculation is the color assigned to the `background-color`.

When `--q-primary` is changed, the background color of the button will be changed to the new color. But the background color `on hover' still has the 5% darker blue as its value. This can be seen in the example below.

![SCSS function](/img/blog/quasar-toh-theme-scss-variable.gif)

To avoid this, do not use SCSS functions. Instead, use plain CSS.

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

Lightening can be done by using `opacity`:

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

Making it darker can be done by using a brightness filter:

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

Brightening is also possible in principle by using the brightness filter:

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

Since we changed the default button by a QBtn, all hover state can be removed from the custom styling. Quasar already handles this correctly behind the scenes!

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/de910a5006e094d3a3cda361d64279dec5adc924

### Outsourcing to Quasar

Currently, primary and negative is set via props. Quasar already provides a way to provide QBtn with primary and negative styling. By setting the `color` property.

Before the change:

```html
<StyledButton primary>Primary</StyledButton>

<StyledButton secondary>Secondary</StyledButton>
```

After the change:

```html
<StyledButton color="primary">Primary</StyledButton>

<StyledButton color="secondary">Secondary</StyledButton>
```

All props/emits that exist on the regular Quasar component can also be used directly on the wrapper component.

Only the custom styling remains in the wrapper component.

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

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/7bce768224595005f9a2073f453d12b906c29e8e)

## Quasar defaults

The desired result is that the end user can decide what the application looks like. A few things will be changed to achieve this, for example there will no longer be a wrapper component called `StyledButton`, but the component `QBtn` will be used. The `ButtonGroup` will be refactored to a `FlexWrap` component, which will be used in many places.

![Example Page](/img/blog/quasar-toh-theme-example-page.gif)

### Refactor ButtonGroup

Refactor `ButtonGroup`, rename to `FlexWrap`. This component is a wrapper for all elements that require `display: flex;` with `flex-wrap: wrap;` (this will cause elements to automatically jump to a new line if there is no more room on the current line). By default, a `gap: 1rem;` is applied. Via a property `dense` the `gap` can be reduced to `0.5rem;`. Via a property `column`, the `flex-direction` can be changed to `column;`.

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

Note: wherever `FlexWrap` is used, it will also need to be imported and then added to the `components` property of the configuration object passed along to `defineComponent`.

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

Change all elements on which `flex-wrap: wrap;` is currently used to `FlexWrap`. Then the CSS properties handled by the `FlexWrap` component can be removed from the elements on which this was first added separately. If there are no CSS properties left, the entire CSS selector can be removed.

Notice: The element with class `.top-heroes` is not modified.

`MainLayout.vue`:

- `.button-container` (selector can be removed)

`Example.vue`:

- `.top-row` (selector can be removed)
- each element with the class `.colors` (remove `display`, `flex-wrap` and `gap` properties)
- each element with the class `.elements` (remove `display`, `flex-wrap` and `gap` properties)
  `HeroAdd.vue`:

- `ButtonGroup` is changed to `FlexWrap`

`Herodetails.vue`:

- `ButtonGroup` is changed to `FlexWrap`

`HeroList.vue`:

- element with the class `.hero-list` (selector can be removed)
- `ButtonGroup` is changed to `FlexWrap`

`Login.vue`:

- element with the class `.login-container` (remove `display`, `flex-wrap` and `gap` properties), add to this `FlexWrap` also the property `column`

### Refactor StyledButton

To ensure that auto complete of props and emits can be preserved, `QBtn` will be used everywhere instead of the wrapper component `StyledButton`. Delete the file `StyledButton.vue`, wherever it is used, delete the import line, delete the component from the `components` property and then change `StyledButton` in the html to `q-btn`. The default styling that the wrapper component was used for is no longer needed. This will be replaced shortly after this by a configuration object that adjusts the styling on the fly.

StyledButton appears in the following places:

- `MainLayout.vue`
- `Example.vue`
- `HeroAdd.vue`
- `HeroDetails.vue`
- `HeroList.vue`
- `Login.vue`

### Component API

Create a new file defining the component API, this will hold all properties and the value of the properties. The properties of both the custom components (placed in the `customApi` object) and the Quasar components (placed in the `quasarApi` object) will be defined.

Provide functionality to `get` and `set` the values of the properties. Provide functionality to return an object whose each key is the name of the property and the value will be the value belonging to the property.

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

Import the API and the functionality to change the API into `src/services/theme/theme.service.ts` and make sure the functionality is returned again so that they can eventually be used via `useTheme()`.

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

`themes` and `activetheme` have been moved outside the `useTheme` function. This ensures that the same variables are referenced throughout.

The implementation of `toggleDarkMode` has been changed, this now changes the `isDark` value of the `activeTheme` variable.

A `watch` on the `activeTheme` variable has been added, this ensures that when the value of one of the properties of the `activeTheme` variable changes, the `toggleTheme` function is called. By `{ deep: true }` the change of nested properties will also be noticed.

The `toggleTheme` function will now also change the `activeTheme`.

Functionality is provided to save the choice of theme colors and the choice of default properties to LocalStorage and then to load them back in.

To ensure that a component uses the chosen default properties, each component will need to be linked via `v-bind="getDefaults('componentName')"`. Where `componentName` is the name of the component. For example:

```html
<q-btn v-bind="getDefaults('QBtn')">Example</q-btn>
<q-input v-bind="getDefaults('QInput')">Example</q-input>
<FlexWrap v-bind="getDefaults('FlexWrap')">Example</FlexWrap>
```

## Other changes

`DarkToggle` is now also linked in `MainLayout.vue`, to make sure the button is always visible, an extra prop has been added.

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

A route to get to the settings/example page has also been added.

All changes:
[GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d660166880690aa52273742b21aef9a51fab111e)

## API Extractor

To help with setting up the configuration object to manage the properties of the Quasar components, a file called `api-extractor.js` has been added, it can be found in the `api-extractor` folder and also contains a `README.md` with how to use it.

The file can be viewed at [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/47ce8f0d937ee545b3d4adee04d53c53540a06b3).

## Conclusion

Because there is a change in the expectation of how the application works, previous decisions may no longer be the correct decisions. This causes refactoring work to meet the new expectations. Working with different themes in terms of colors can be fairly easy. Changing each component based on the user's choice is a bit more complicated and requires extra work from the developer, but gives more freedom to the user. It is a good idea to provide custom components with similar properties that are present in the framework that is used, in this case Quasar. This is added as an example for the `FlexWrap` component.
