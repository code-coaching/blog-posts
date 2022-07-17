---
postUuid: 74ee7748-5459-49b0-b5b9-9d0f7b9c861a
title: Quasar Tour of Heroes - I18n
slug: quasar-toh-i18n
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

A free [Code Coaching](https://code-coaching.dev/) account is required to follow this tutorial.

This article will start from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The pages have been provided with elements, data and components. The data has been moved to a composable/service. An API with authentication is linked to manage the Hero data. Quasar components and plugins are used. Functionality is provided that allows the user to decide what the application looks like. The end situation is exactly the same in terms of functionality, but it will be possible to use different languages.

The code can be obtained by downloading a zip from the source code.

The situation that will be used as a starting point is as follows:

![starting situation](/img/blog/quasar-toh-theme-example-page.gif)

Starting situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-quasarify)

The final situation being worked towards is as follows:

![end situation](/img/blog/quasar-toh-i18n-finished.gif)

End situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-i18n)

## vue-i18n

The term `i18n` stands for `internationalization`. the `i` and the `n` refer to the first and last letters of the word. The `18` refers to the number of letters between the `i` and the `n`.

The library `vue-i18n` provides a way to handle translations.

### Installing

Installing `vue-i18n`.

```sh
npm install vue-i18n
```

### Translation files

Create a new folder in the `src` folder, named `i18n`. In the new folder, provide a folder `en-US` and `nl-NL`. In each folder provide an `index.ts` file.

The structure:

```sh
src/
- i18n/
  - en-US/
    - index.ts
  - nl-NL/
    - index.ts
```

Contents of `src/i18n/en-US/index.ts`:

```ts
export default {
  failed: "Action failed",
  success: "Action was successful",
};
```

Contents of `src/i18n/nl-NL/index.ts`:

```ts
export default {
  failed: "Actie mislukt",
  success: "Actie is gelukt",
};
```

Contents of `src/i18n/index.ts`:

```ts
import enUS from "./en-US";
import nlNL from "./nl-NL";

export default {
  "en-US": enUS,
  "nl-NL": nlNL,
};
```

### Boot file

In the `boot` folder, add a file called `i18n.ts`.

Contents of `src/boot/i18n.ts`:

```ts
import messages from "src/i18n";
import { createI18n } from "vue-i18n";

const i18n = createI18n({
  legacy: false,
  locale: "en-US",
  messages,
});

const useI18n = () => {
  // eslint-disable-next-line @typescript-eslint/unbound-method
  const { t, te, tm, rt, d, n, ...globalApi } = i18n.global;

  return {
    t: t.bind(i18n),
    te: te.bind(i18n),
    tm: tm.bind(i18n),
    rt: rt.bind(i18n),
    d: d.bind(i18n),
    n: n.bind(i18n),
    ...globalApi,
  };
};

export { i18n, useI18n };
```

By binding the functions `t`, `te`, `tm`, `rt`, `d` and `n` to the returned object, it is only necessary to use the `eslint-disable-next-line` once. If this is not done, in every file where `const { t } = useI18n()` is used, the line `// eslint-disable-next-line @typescript-eslint/unbound-method` will have to be used.

### Use translations

In `Example.vue`:

```html
<template>
  <div class="example-page">
    <FlexWrap v-bind="getDefaults('FlexWrap')">
      <q-btn round outline icon="home" @click="navigate(ROUTE_NAMES.HOME)" />
      <DarkToggle />
      <q-btn @click="toggleDarkMode()">Toggle Dark Mode</q-btn>
      <ThemeToggle />
    </FlexWrap>

    <div class="section">Translations</div>
    <FlexWrap class="elements">
      <div class="translation">
        {{ t('failed') }}
      </div>
      <div class="translation">
        {{ t('success') }}
      </div>
    </FlexWrap>

    <!-- More code here -->
</template>

<script lang="ts">
// More imports here
import { useI18n } from 'boot/i18n';

export default defineComponent({
  setup() {
    // More code here

    const { t } = useI18n();

    // More code here

    return {
      t
    }
  }
});
</script>

<style lang="scss">
  /* More styling here */

  .translation {
  outline: 1px solid var(--q-primary);
  padding: 0.5rem;
}
</style>
```

`useI18n` is imported from the `boot/i18n` file, and not from `vue-i18n`. The translation function `t` is placed in the return object so that it is available in the template.

In the template, the translation string is passed as a parameter to the `t` function.

To confirm that the translations work, the hard-coded string `en-US` can be changed to `nl-NL` in the file `src/boot/i18n.ts`:

```ts
const i18n = createI18n({
  legacy: false,
  locale: "nl-NL", // en-US
  messages,
});
```

This changes the default language of the application.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/f6eedc43f4f2aeda77fdece525e81b239191a3f5)

## Service

Create a new folder in `services`, named `i18n`. In the folder, create two files `language.service.ts` and `LanguageToggle.vue`.

`src/services/i18n/language.service.ts`:

```ts
import { LocalStorage } from "quasar";
import { useI18n } from "boot/i18n";
import { ref } from "vue";
const { locale } = useI18n();

interface Language {
  nativeWord: string;
  locale: string;
}

const languages: Array<Language> = [
  { nativeWord: "English", locale: "en-US" },
  { nativeWord: "Nederlands", locale: "nl-NL" },
];

const activeLanguage = ref(languages[0]);

const useLanguage = () => {
  const toggleLanguage = (language: Language) => {
    setLanguage(language);
    saveLanguage(language);
  };

  const setLanguage = (language: Language) => {
    activeLanguage.value = language;
    locale.value = language.locale;
  };

  const saveLanguage = (language: Language) => {
    LocalStorage.set("language", language);
    Notify.create({ message: "Language saved", color: "positive" });
  };

  const loadLanguage = () => {
    const language = LocalStorage.getItem("language") as Language;
    if (language) setLanguage(language);
  };

  return {
    languages,
    activeLanguage,
    toggleLanguage,
    setLanguage,
    loadLanguage,
  };
};

export { useLanguage, Language };
```

We keep track of the active language in the `activeLanguage` ref, which is initialized with the first language in the `languages` array. The `languages` array contains the native language word and the locale for the language.

A `toggleLanguage` function is provided, it will be associated with the `ToggleLanguage` component. This function will call `setLanguage` and `saveLanguage`.

`setLanguage` is in charge of changing the active language and locale.

`saveLanguage` is in charge of saving the language in `LocalStorage`. The string shown in the notification will be translated later.

`loadLanguage` is in charge of loading the language from `LocalStorage` and it will then also call `setLanguage`.

`src/services/i18n/LanguageToggle.vue`:

```html
<template>
  <q-btn-dropdown flat icon="translate" cover>
    <q-list>
      <q-item
        clickable
        v-close-popup
        v-for="(language, index) in languages"
        :key="index"
        @click="toggleLanguage(language)"
      >
        <q-item-section>
          <q-item-label class="language-label">
            <span>{{ language.nativeWord }}</span>
          </q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
  </q-btn-dropdown>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { useLanguage } from "./language.service";

  export default defineComponent({
    setup() {
      const { languages, toggleLanguage } = useLanguage();

      return {
        languages,
        toggleLanguage,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .language-label {
    display: flex;
    justify-content: space-between;
    gap: 1rem;
  }
</style>
```

This button can be used to change the language. This will be available in the preview page, which also serves as the settings page.

`src/pages/Example.vue`

```html
<template>
  <div class="example-page">
    <FlexWrap v-bind="getDefaults('FlexWrap')">
      <q-btn round outline icon="home" @click="navigate(ROUTE_NAMES.HOME)" />
      <DarkToggle />
      <q-btn @click="toggleDarkMode()">Toggle Dark Mode</q-btn>
      <ThemeToggle />
      <LanguageToggle />
    </FlexWrap>
   <!-- more html  -->
</template>

<script lang="ts">
  // more imports
import LanguageToggle from 'src/services/i18n/LanguageToggle.vue';

export default defineComponent({
  components: {
    // more components
    LanguageToggle
},
  setup() {
    const { loadLanguage } = useLanguage();

    loadCustomTheme();
    // more code
  },
});
</script>

<!-- scss here -->
```

![i18n](/img/blog/quasar-toh-i18n.gif)

Make sure the language is also loaded into `MainLayout.vue`.

```html
<script lang="ts">
  // more imports
  import { useLanguage } from "src/services/i18n/language.service";

  export default defineComponent({
    setup() {
      // more code
      const { loadLanguage } = useLanguage();
      loadLanguage();
    },
  });
</script>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/c0bcd2bbaf74192f12ca9d1d2e234614c50be2e6)

## Translations

There are several ways to handle the translations. One way is to provide translations on a page-by-page basis.

`src/i18n/en-US/index.ts`

```ts
export default {
  failed: "Action failed",
  success: "Action was successful",
  pages: {
    dashboard: {
      title: "Top Heroes",
    },
  },
};
```

`src/i18n/en-US/index.ts`

```ts
export default {
  failed: "Actie mislukt",
  success: "Actie is gelukt",
  pages: {
    dashboard: {
      title: "Tophelden",
    },
  },
};
```

Using the translation in `src/pages/Dashboard.vue`:

```html
<template>
  <div class="title">{{ t('pages.dashboard.title') }}</div>

  <!-- more html -->
</template>

<script lang="ts">
  // more imports
  import { useI18n } from "src/boot/i18n";

  export default defineComponent({
    setup() {
      // more code
      const { t } = useI18n();

      // more code

      return {
        t,
        // more code
      };
    },
  });
</script>

<!-- styling -->
```

Change the language on the settings page and confirm that it works.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/c03905a466f4ae2e0fa695237d893894e79820c4)

A tip regarding translations: use a string instead of a nested object.

Before the change:

```ts
export default {
  failed: "Action failed",
  success: "Action was successful",
  pages: {
    dashboard: {
      title: "Top Heroes",
    },
  },
};
```

After the change:

```ts
export default {
  failed: "Action failed",
  success: "Action was successful",
  "pages.dashboard.title": "Top Heroes",
};
```

Often third party translation software works with key/value pairs, but not with nested objects. This avoids the need for a transformation later. It also ensures that the file uses fewer lines, a nested object that has two levels is five lines long. A string is one line long.

This also ensures that the string used in the page matches the translation in the translation file. So it is easier to find the exact key by doing a global search.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/b2915654751c5a12676e89fdca4b6b1a4875f340)

## Translations

In each file where translations are required, the following is done:

```ts
import { useI18n } from "src/boot/i18n";

export default defineComponent({
  setup() {
    const { t } = useI18n();

    return {
      t,
    };
  },
});
```

Then the translation can be done in the template.

If the translation is separate in the html, it can be done via:

```html
{{ t('hier-de-vertaal-key') }}
```

If the translation is assigned to an attribute, it can be done via (example in `src/pages/HeroDetails.vue`):

```html
<input :label="t('hier-de-vertaal-key')" />
```

In services, `t` can also be used:

```ts
// more imports
import { useI18n } from "src/boot/i18n";
const { t } = useI18n();
// more code

const useLanguage = () => {
  // more code

  const saveLanguage = (language: Language) => {
    LocalStorage.set("language", language);
    Notify.create({
      message: t("services.language.language-saved"),
      color: "positive",
    });
  };

  // more code

  return {
    // more code
  };
};

export { useLanguage };
```

In `src/services/validator.composable.ts` you can find an example of how a variable can be passed along to a translation.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/cdfa8f84d3798d648c716962dbd425dbbb995c2f)

## Conclusion

Translations make the application more accessible to a wider audience. To add translations afterwards can take some time. Even if you only support one language, it is smart to work with translations from the beginning. If you ever need to add a language, it is possible to just add an extra file instead of having to add all translations to the code.
