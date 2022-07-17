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

Om deze tutorial te kunnen volgen is een gratis [Code Coaching](https://code-coaching.dev/) account nodig.

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een composable/service. Een API met authentificatie is gekoppeld om de Hero data te beheren. Er wordt gebruikt gemaakt van Quasar componenten en plugins. Er is functionaliteit voorzien waarmee de gebruiker zelf kan bepalen hoe de applicatie eruit ziet. De eindsituatie is exact hetzelfde qua functionaliteit, maar het zal mogelijk zijn om verschillende talen te gebruiken.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-theme-example-page.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-theme)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-i18n-finished.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-i18n)

## vue-i18n

De term `i18n` staat voor `internationalization`. de `i` en de `n` verwijzen naar de eerste en laatste letter van het woord. De `18` verwijst naar het aantal letters tussen de `i` en de `n`.

De library `vue-i18n` voorziet een manier om vertalingen af te handelen.

### Installeren

Installeren van `vue-i18n`.

```sh
npm install vue-i18n
```

### Vertaalbestanden

Maak een nieuwe map aan in de `src`-map, genaamd `i18n`. In de nieuwe map, voorzie een map `en-US` en `nl-NL`. In elke map voorzie een `index.ts`-bestand.

De structuur:

```sh
src/
- i18n/
  - en-US/
    - index.ts
  - nl-NL/
    - index.ts
```

Inhoud van `src/i18n/en-US/index.ts`:

```ts
export default {
  failed: "Action failed",
  success: "Action was successful",
};
```

Inhoud van `src/i18n/nl-NL/index.ts`:

```ts
export default {
  failed: "Actie mislukt",
  success: "Actie is gelukt",
};
```

Inhoud van `src/i18n/index.ts`:

```ts
import enUS from "./en-US";
import nlNL from "./nl-NL";

export default {
  "en-US": enUS,
  "nl-NL": nlNL,
};
```

### Boot file

Voeg in de `boot`-map een bestand toe genaamd `i18n.ts`.

Inhoud van `src/boot/i18n.ts`:

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

Door de functies `t`, `te`, `tm`, `rt`, `d` en `n` te binden aan het teruggegeven object, is het maar één keer nodig om de `eslint-disable-next-line` te gebruiken. Indien dit niet gedaan wordt, zal in elk bestand waar `const { t } = useI18n()` gebruikt wordt, de lijn `// eslint-disable-next-line @typescript-eslint/unbound-method` gebruikt moeten worden.

### Vertalingen gebruiken

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

`useI18n` wordt geïmporteerd vanuit het `boot/i18n`-bestand, en niet vanuit `vue-i18n`. De vertaalfunctie `t` wordt in het return object geplaatst zodat deze ter beschikking is in de template.

In de template wordt de vertaalstring meegegeven als parameter aan de functie `t`.

Om te bevestigen dat de vertalingen werken, kan de hard-coded string `en-US` gewijzigd worden naar `nl-NL` in het bestand `src/boot/i18n.ts`:

```ts
const i18n = createI18n({
  legacy: false,
  locale: "nl-NL", // en-US
  messages,
});
```

Dit wijzigt de standaardtaal van de applicatie.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/f6eedc43f4f2aeda77fdece525e81b239191a3f5)

## Service

Maak een nieuwe map aan in `services`, genaamd `i18n`. In de map worden twee bestanden aangemaakt `language.service.ts` en `LanguageToggle.vue`.

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

We houden de actieve taal bij in de `activeLanguage` ref, die geïnitialiseerd wordt met de eerste taal in de `languages` array. De `languages` array bevat het woord in de moedertaal en de locale voor de taal.

Er wordt een `toggleLanguage`-functie voorzien, deze zal gekoppeld worden aan de `ToggleLanguage`-component. Deze functie roept `setLanguage` en `saveLanguage` aan.

`setLanguage` staat in voor het wijzigen van de actieve taal en de locale.

`saveLanguage` staat in voor het opslaan van de taal in `LocalStorage`. De string die getoond wordt in de notificatie wordt later vertaald.

`loadLanguage` staat in voor het laden van de taal uit `LocalStorage` en deze zal vervolgens ook `setLanguage` aanroepen.

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

Deze knop kan gebruikt worden om de taal te wijzigen. Dit zal beschikbaar zijn in de voorbeeldpagina, die ook dient als instellingenpagina.

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

Zorg dat de taal ook ingeladen wordt in `MainLayout.vue`.

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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/c0bcd2bbaf74192f12ca9d1d2e234614c50be2e6)

## Vertalingen

Er zijn meerdere manieren om de vertalingen af te handelen. Één van de manieren is om de vertalingen per pagina te voorzien.

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

De vertaling gebruiken in `src/pages/Dashboard.vue`:

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

Wijzig de taal op de instellingenpagina en bevestig dat het werkt.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/c03905a466f4ae2e0fa695237d893894e79820c4)

Een tip i.v.m. vertalingen: gebruik een string i.p.v. een genest object.

Voor de wijziging:

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

Na de wijziging:

```ts
export default {
  failed: "Action failed",
  success: "Action was successful",
  "pages.dashboard.title": "Top Heroes",
};
```

Vaak werkt third party vertaalsoftware met key/value pairs, maar niet met geneste objecten. Dit voorkomt dat er later een transformatie nodig is. Ook zorgt dit ervoor dat de file minder regels gebruikt, een genest object dat twee niveaus heeft is vijf regels lang. Een string is één regel lang.

Dit zorgt er ook voor dat de string die gebruikt wordt in de pagina, overeenkomt met de vertaling in het vertaalbestand. Het is dus makkelijker om de exact key terug te vinden door een globale zoekopdracht te doen.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/b2915654751c5a12676e89fdca4b6b1a4875f340)

## Vertalingen

In elk bestand waar vertalingen nodig zijn, wordt het volgend gedaan:

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

Vervolgens kan de vertaling gedaan worden in de template.

Indien de vertaling los in de html staat, kan dit gedaan worden via:

```html
{{ t('hier-de-vertaal-key') }}
```

Indien de vertaling toegekend wordt aan een attribuut, kan dit gedaan worden via (voorbeeld in `src/pages/HeroDetails.vue`):

```html
<input :label="t('hier-de-vertaal-key')" />
```

In services kan ook gebruik gemaakt worden van `t`:

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

In `src/services/validator.composable.ts` is een voorbeeld terug te vinden hoe een variabele meegegeven kan worden aan een vertaling.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/cdfa8f84d3798d648c716962dbd425dbbb995c2f)

## Conclusie

Vertalingen zorgen ervoor dat de applicatie toegankelijker is tot een groter publiek. Om vertalingen achteraf toe te voegen kan enige tijd in beslag nemen. Zelfs als je maar één taal ondersteunt, is het slim om vanaf het begin met vertalingen te werken. Indien er ooit een taal moet toegevoegd worden, moet enkel een extra bestand worden toegevoegd i.p.v. ook nog alle vertalingen toe te moeten voegen in de code.
