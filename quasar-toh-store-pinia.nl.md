---
postUuid: 74ee7748-5459-49b0-b5b9-9d0f7b9c861a
title: Quasar Tour of Heroes - Store (Pinia)
slug: quasar-toh-store-pinia
tags:
  - Quasar
  - Vue
  - Tour of Heroes
  - Pinia
categories:
  - Frontend
---

Om deze tutorial te kunnen volgen is een gratis [Code Coaching](https://code-coaching.dev/) account nodig.

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een composable/service. Een API met authentificatie is gekoppeld om de Hero data te beheren. Er wordt gebruikt gemaakt van Quasar componenten en plugins. Er is functionaliteit voorzien waarmee de gebruiker zelf kan bepalen hoe de applicatie eruit ziet. Ook zijn er meerdere talen voorzien. De eindsituatie is functioneel gezien exact hetzelfde als de beginsituatie, maar er zal gebruik gemaakt worden van een store (Pinia) i.p.v. een service/composable om globale data te beheren.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-i18n)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-store-pinia)

## Wat is een store

Een store is in essentie een object dat globaal beschikbaar is waarin data bijgehouden wordt die doorheen heel de applicatie gebruikt kan worden.

Binnen Vue was er eerst `Vuex`, een `state management pattern` (Nederlands: patroon om data te beheren). Maar tegenwoordig wordt `Pinia` gebruikt.

## @quasar/app-webpack

Aangezien dit project is aangemaakt met een versie van Quasar CLI toen enkel webpack gebruikt werd, zal er in de `package.json` een `devDependency` zijn voor `@quasar/app`. Om Quasar stores te laten genereren die gebruikmaken van Pinia, zal het project geüpgrade moeten worden. Een nieuw project zal een `devDependency` hebben voor `@quasar/app-webpack`.

### Quasar CLI

Om te zorgen dat het project geüpgrade kan worden, zal eerst de laatste Quasar CLI globaal geïnstalleerd worden.

```sh
npm install -g quasar-cli
```

### Upgrade Quasar project

```sh
quasar upgrade
```

De output van dit commando ziet er als volgt uit:

```sh

  Quasar CLI · Gathering information with npm...

 @quasar/extras: 1.12.5 → 1.14.3
 quasar: 2.5.5 → 2.7.5
 @quasar/app-webpack: 3.3.3 → 3.5.7

 See https://quasar.dev/start/release-notes for release notes.
 Run "quasar upgrade -i" to do the actual upgrade.
```

Merk op: `@quasar/app` wordt gewijzigd naar `@quasar/app-webpack`.

Zoals vermeld staat, kan de upgrade uitgevoerd worden via `quasar upgrade -i`.

```sh
quasar upgrade -i
```

Verifieer dat `@quasar/app-webpack` aanwezig is in de `package.json` van het project. Deze is terug te vinden in de `devDependencies` van de `package.json`.

## Pinia

Installeren van Pinia.

```sh
npm install pinia
```

## Een store toevoegen

```sh
quasar new store hero.store
```

Er wordt een nieuwe map gemaakt genaamd `stores` met een bestand met de naam `index.ts`, hierin wordt de Pinia store gemaakt. Er is ook een bestand met de naam `hero.store.ts`, hierin wordt een voorbeeld aangemaakt.

```ts
import { defineStore } from "pinia";

export const useCounterStore = defineStore("counter", {
  state: () => ({
    counter: 0,
  }),

  getters: {
    doubleCount(state) {
      return state.counter * 2;
    },
  },

  actions: {
    increment() {
      this.counter++;
    },
  },
});
```

De `defineStore`-functie accepteert twee parameters. De eerste parameter is de naam van de store (in dit geval `counter`). De tweede parameter is een object met de volgende properties:

- `state`: een anonieme arrow-functie die de state/data van de store bevat.
- `getters`: een codeblok waarin de getters voor de store gemaakt worden. Getters zijn functies die data uit de state lezen en mogelijks transformeren.
- `actions`: een codeblok waarin de actions voor de store gemaakt worden. Actions zijn functies die wijzigingen aan de state uitvoeren.

## Hero service migreren naar Pinia store

`src/stores/hero.store.ts`

```ts
import { AxiosRequestConfig } from "axios";
import { defineStore } from "pinia";
import { Notify } from "quasar";
import { api } from "src/boot/axios";
import { BackendHero, Hero } from "src/components/models";
import { useI18n } from "src/boot/i18n";
const { t } = useI18n();

interface Paged<T> {
  total: number;
  limit: number;
  skip: number;
  data: Array<T>;
}

const getRequestConfig = (): AxiosRequestConfig => {
  const accessToken = localStorage.getItem("accessToken") || "";

  return {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  };
};

export const useHeroesStore = defineStore("heroes", {
  state: () => ({
    heroes: [] as Array<Hero>,
    selectedHero: {} as Hero,
  }),

  getters: {
    topHeroes: (state) => state.heroes.slice(0, 4),
  },

  actions: {
    async getHeroes() {
      const result = await api.get<Paged<BackendHero>>(
        "/heroes",
        getRequestConfig()
      );

      this.heroes = result.data.data.map((hero: BackendHero): Hero => {
        return {
          ...hero,
          number: hero.id,
        };
      });
    },
    editHero(hero: Hero) {
      if (hero._id) {
        const index = this.heroes.findIndex((h: Hero) => h._id === hero._id);
        const oldHero = this.heroes[index];
        this.heroes[index] = hero;

        api
          .patch(`/heroes/${hero._id}`, hero, getRequestConfig())
          .then(() =>
            Notify.create({
              message: t("services.hero.hero-updated"),
              color: "positive",
            })
          )
          .catch(() => {
            Notify.create({
              message: t("services.hero.hero-update-failed"),
              color: "negative",
            });
            this.heroes[index] = oldHero;
          });
      }
    },

    findHero(id: string): Hero | undefined {
      const matchingHero = this.heroes.find((h) => h.number === id);
      if (matchingHero) return { ...matchingHero };
    },

    deleteHero(hero: Hero) {
      if (hero._id) {
        const index = this.heroes.findIndex((h) => h._id === hero._id);
        this.heroes.splice(index, 1);

        api
          .delete(`/heroes/${hero._id}`, getRequestConfig())
          .then(() =>
            Notify.create({
              message: t("services.hero.hero-deleted"),
              color: "positive",
            })
          )
          .catch(() => {
            Notify.create({
              message: t("services.hero.failed-to-delete-hero"),
              color: "negative",
            });
            this.heroes.splice(index, 0, hero);
          });
      }
    },

    addHero(name: string) {
      let maxNumber = Math.max(...this.heroes.map((h) => +h.number));
      if (maxNumber === -Infinity) maxNumber = 0;

      const number = (maxNumber + 1).toString();
      const newHero = { number, name };

      this.heroes.push(newHero);
      const index = this.heroes.length - 1;

      api
        .post<BackendHero>(
          "/heroes",
          {
            ...newHero,
            id: newHero.number,
          },
          getRequestConfig()
        )
        .then((result) => {
          Notify.create({
            message: t("services.hero.hero-added"),
            type: "positive",
          });
          this.heroes[index] = { ...result.data, number: result.data.id };
        })
        .catch(() => {
          Notify.create({
            message: t("services.hero.error-adding-hero"),
            type: "negative",
          });
          const index = this.heroes.findIndex((e) => e === newHero);
          this.heroes.splice(index, 1);
        });
    },

    resetSelectedHero() {
      this.selectedHero = {} as Hero;
    },
    setSelectedHero(hero: Hero) {
      this.selectedHero = hero;
    },
  },
});
```

De naam `heroes` is de naam van de store, deze zal getoond worden in `Vue Devtools` (dit wordt later in het artikel behandeld).

De `ref`-variabele uit de service zijn nu properties in de `state`.

De `computed`-variabele `topHeroes` is nu een `getter`.

Alle functies zijn nu `actions`. De state die gewijzigd wordt in `actions` wordt aangesproken via `this.`. Merk ook op dat er niet langer gebruikgemaakt wordt van `.value` aangezien het niet langer `ref`-variabelen zijn.

## Hero store gebruiken

Overal waar de `Hero Service` gebruikt wordt, zal nu de `Hero Store` gebruikt worden.

`src/layouts/MainLayout.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-22686b16c14b601ef5e6e4de40acc8234bf4f4ccbe2d5856e3bb7a0b77da6afc)

`src/pages/Dashboard.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-22686b16c14b601ef5e6e4de40acc8234bf4f4ccbe2d5856e3bb7a0b77da6afc)
Merk op dat de getter `topHeroes` toegekend wordt via een `computed`.

`src/pages/HeroAdd.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-a110a93e8b518e304f94e987672018d02c72ea77460abad18b17c030be974c75)

`src/pages/HeroDetails.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-7c304e04b030014039db1daf3899f99287a3b67483a6224e5fd373179c719b9b)

`src/pages/HeroList.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-b6065c77e2882be1c27b77a08ac03f6a1ca5b9e178f314e685d92fd498e960b9)
Merk op dat zowel `heroes` als `selectedHero` toegekend worden via een `computed`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08)

## Vue Devtools

Installeer de [Vue Devtools](https://devtools.vuejs.org/guide/installation.html) voor de browser naar keuze.

Pinia stores zijn te bekijken in de `Vue Devtools`-tab.

Na het installeren is de `Vue Devtools` als tab beschikbaar op dezelfde locatie waar de `console` en de `netwerktab` te vinden zijn.

![Vue Devtools](/img/blog/quasar-toh-vue-devtools-pinia.png)
Ga in de `Vue Devtools` naar de `Pinia`-tab.

![Vue Devtools heroes](/img/blog/quasar-toh-vue-devtools-store.png)
De gekozen naam van de store, `heroes`, is in het rood omcirkeld. Aangezien de store zelf `heroes` noemt en er ook een property in de state is met dezelfde naam, kan dit voor verwarring zorgen.

Refactor de store naar een nieuwe naam.

De wijziging: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/8d21c8575602de8eca77cf89e237ad43c05c587e)

Dit wijzigt niks functioneel, maar helpt bij het herkennen van de store in de `Vue Devtools`.
![Vue Devtools heroStore](/img/blog/quasar-toh-vue-devtools-hero-store.png)

Bij het bekijken van de `heroStore` kunnen alle details van de store bekeken worden. Zowel de `state` als de `getters` zijn te bekijken.
![Vue Devtools heroes store](/img/blog/quasar-toh-vue-devtools-hero-store-details.png)

Bij het aanklikken van een hero, waardoor de `selectedHero` gewijzigd wordt, kan de wijziging bekeken worden in de `Vue Devtools`.

![Vue Devtools selected hero](/img/blog/quasar-toh-vue-devtools-selected-hero.png)
![Vue Devtools selected hero 2](/img/blog/quasar-toh-vue-devtools-selected-hero-2.png)

`time travel`, bij het selecteren van `Timeline` is het mogelijk om de wijzigingen van de store (en andere Vue acties) doorheen de tijd te bekijken.

## Auth service migreren naar Pinia store

```ts
import { defineStore } from "pinia";
import { Notify } from "quasar";
import { api } from "src/boot/axios";
import { User } from "src/components/models";
import { useI18n } from "src/boot/i18n";
const { t } = useI18n();

export const useAuthStore = defineStore("authStore", {
  state: () => ({
    authenticatedUser: {} as User,
  }),

  getters: {
    isAuthenticated: (state) => state.authenticatedUser._id !== undefined,
  },

  actions: {
    tryToAuthenticate(): Promise<void> {
      return new Promise((resolve, reject) => {
        const accessToken = localStorage.getItem("accessToken");
        if (accessToken) {
          api
            .post("/authentication", {
              strategy: "jwt",
              accessToken,
            })
            .then((result: { data: { accessToken: string; user: User } }) => {
              this.authenticatedUser = result.data.user;
              resolve();
            })
            .catch(() => {
              localStorage.removeItem("accessToken");
              this.authenticatedUser = {} as User;
              reject();
            });
        } else {
          reject();
        }
      });
    },
    login(email: string, password: string): Promise<void> {
      return new Promise((resolve, reject) => {
        api
          .post("/authentication", {
            strategy: "local",
            email,
            password,
          })
          .then((result: { data: { accessToken: string; user: User } }) => {
            const accessToken = result.data.accessToken;
            if (accessToken) {
              Notify.create({
                message: t("services.auth.logged-in"),
                color: "positive",
              });
              localStorage.setItem("accessToken", accessToken);
              this.authenticatedUser = result.data.user;
              resolve();
            } else {
              Notify.create({
                message: t("services.auth.login-failed"),
                color: "negative",
              });
              reject();
            }
          })
          .catch(() => {
            Notify.create({
              message: t("services.auth.login-failed"),
              color: "negative",
            });
            reject();
          });
      });
    },
    logout() {
      Notify.create({
        message: t("services.auth.logged-out"),
        color: "positive",
      });
      localStorage.removeItem("accessToken");
      this.authenticatedUser = {} as User;
    },
  },
});
```

Overal waar de `Auth Service` gebruikt wordt, zal nu de `Auth Store` gebruikt worden.

`src/layouts/MainLayout.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/4cf0b5b99ae36b7236dd5a21d2a7eb97e2babc03#diff-22686b16c14b601ef5e6e4de40acc8234bf4f4ccbe2d5856e3bb7a0b77da6afc)
Merk op dat de `getter` doorgegeven wordt aan de template via een `computed`.
Merk op dat de `action` doorgegeven wordt aan de template via een anonieme arrow-functie.

`src/pages/Login.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/4cf0b5b99ae36b7236dd5a21d2a7eb97e2babc03#diff-78ff024242ee805990cc729c4b0fe76e134328f5387bf1c8b383c1eb4d4c986d)
Merk op dat er eerst een rename gebeurde van de functie `login` naar `authenticate` vanuit de Auth Service. Dit is niet langer het geval, de `login`-functie van de `authStore` wordt direct op de `authStore` aangeroepen.

In de `devtools` zijn nu twee stores te zien: de `authStore` en de `heroStore`.

![Auth Store](/img/blog/quasar-toh-vue-devtools-auth-store.gif)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/4cf0b5b99ae36b7236dd5a21d2a7eb97e2babc03)

## Conclusie

Doordat alle data al apart werd afgehandeld in een `service`/`composable`, is het makkelijk om te migreren naar een alternatieve manier van databeheer. In dit geval de migratie naar het gebruiken van een store via Pinia.

Deze migratie kan in delen gedaan worden, het is bijvoorbeeld mogelijk om één service per keer te migreren, totdat uiteindelijk alle services gemigreerd zijn. Het is ook perfect oké om niet alle services om te zetten naar een store.

Het voordeel van de Pinia store is dat de state in `Vue Devtools` bekeken kan worden. De `timeline` staat `time travel` toe door de toestand van de applicatie doorheen de tijd te bekijken. Er is geen nood aan `.value` omdat er geen gebruik gemaakt wordt van `ref`-variabelen.

Het nadeel van de Pinia store is dat er extra `gotchas` zijn. Zo moeten `getters` altijd nog in een `computed` gewrapped worden bij het doorgeven aan de template (bij de service/composable was dit simpelweg een `computed`-variabele in de service zelf). `actions` moeten via anonieme arrow-functies doorgegeven worden aan de template.

De store is simpelweg een manier om data te beheren. Indien er geen nood is aan de data te kunnen zien in de `Vue Devtools`, is het niet per se nodig om een library zoals Pinia te gebruiken voor het beheer van de data.
