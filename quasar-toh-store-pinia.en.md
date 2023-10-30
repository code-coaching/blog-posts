---
postUuid: c8147e86-aa6d-488a-864a-b7a45701b67f
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

A free [Code Coaching](https://code-coaching.dev/) account is required to follow this tutorial.

This article will start from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The pages have been provided with elements, data and components. The data has been moved to a composable/service. An API with authentication is linked to manage the Hero data. Quasar components and plugins are used. Functionality is provided that allows the user to control what the application looks like. Multiple languages are also provided. The final situation is functionally exactly the same as the initial situation, but a store (Pinia) will be used instead of a service/composable to manage global data.

The code can be obtained by downloading a zip of the source code.

Translated with www.DeepL.com/Translator (free version)

Starting situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-i18n)

End situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-store-pinia)

## What is a store

A store is essentially a globally available object that holds data that can be used throughout the application.

Within Vue, there used to be `Vuex`, a `state management pattern`. But nowadays `Pinia` is used.

## @quasar/app-webpack

Since this project was created with a version of Quasar CLI when only webpack was used, there will be a `devDependency` in the `package.json` for `@quasar/app`. In order for Quasar to generate stores using Pinia, the project will need to be upgraded. A new project will have a `devDependency` for `@quasar/app-webpack`.

### Quasar CLI

To ensure that the project can be upgraded, the latest Quasar CLI will first be installed globally.

```sh
npm install -g quasar-cli
```

### Upgrade Quasar project

```sh
quasar upgrade
```

The output of this command looks like this:

```sh

  Quasar CLI · Gathering information with npm...

 @quasar/extras: 1.12.5 → 1.14.3
 quasar: 2.5.5 → 2.7.5
 @quasar/app-webpack: 3.3.3 → 3.5.7

 See https://quasar.dev/start/release-notes for release notes.
 Run "quasar upgrade -i" to do the actual upgrade.
```

Note: `@quasar/app` will be changed to `@quasar/app-webpack`.

As stated, the upgrade can be performed via `quasar upgrade -i`.

```sh
quasar upgrade -i
```

Verify that `@quasar/app-webpack` is present in the `package.json` of the project. It can be found in the `devDependencies` of the `package.json`.

## Pinia

Install Pinia.

```sh
npm install pinia
```

## Adding a store

```sh
quasar new store hero.store
```

A new folder called `stores` is created with a file named `index.ts`, in it the Pinia store is created. There is also a file named `hero.store.ts`, in it an example is created.

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

The `defineStore` function accepts two parameters. The first parameter is the name of the store (in this case `counter`). The second parameter is an object with the following properties:

- `state`: an anonymous arrow function that contains the state/data of the store.
- `getters`: a code block in which the getters for the store are created. Getters are functions that read data from the state and possibly transform it.
- actions`: a code block in which the actions for the store are created. Actions are functions that execute changes to the state.

## Migrate Hero service to Pinia store

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

The name `heroes` is the name of the store, it will be shown in `Vue Devtools` (this will be covered later in the article).

The `ref` variable from the service are now properties in the `state`.

The `computed` variable `topHeroes` is now a `getter`.

All functions are now `actions`. The state that is changed to `actions` is called via `this.`. Also note that `.value` is no longer used since they are no longer `ref` variables.

## Use the hero store

Wherever the `Hero Service` is used, the `Hero Store` will now be used.

`src/layouts/MainLayout.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-22686b16c14b601ef5e6e4de40acc8234bf4f4ccbe2d5856e3bb7a0b77da6afc)

`src/pages/Dashboard.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-22686b16c14b601ef5e6e4de40acc8234bf4f4ccbe2d5856e3bb7a0b77da6afc)
Note that the getter `topHeroes` is assigned via a `computed`.

`src/pages/HeroAdd.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-a110a93e8b518e304f94e987672018d02c72ea77460abad18b17c030be974c75)

`src/pages/HeroDetails.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-7c304e04b030014039db1daf3899f99287a3b67483a6224e5fd373179c719b9b)

`src/pages/HeroList.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08#diff-b6065c77e2882be1c27b77a08ac03f6a1ca5b9e178f314e685d92fd498e960b9)
Note that both `heroes` and `selectedHero` are assigned via a `computed`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92c0e8639edde45f6254ad3a6047513cd4706b08)

## Vue Devtools

Install the [Vue Devtools](https://devtools.vuejs.org/guide/installation.html) for the browser of your choice.

Pinia stores can be viewed in the `Vue Devtools` tab.

After installing, the `Vue Devtools` is available as a tab in the same location where the `console` and the `network` tab are found.

![Vue Devtools](/img/blog/quasar-toh-vue-devtools-pinia.png)
In the `Vue Devtools`, go to the `Pinia` tab.

![Vue Devtools heroes](/img/blog/quasar-toh-vue-devtools-store.png)
The chosen name of the store, `heroes`, is circled in red. Since the store itself is called `heroes` and there is also a property in the state with the same name, this can cause confusion.

Refactor the store to a new name.

The change: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/8d21c8575602de8eca77cf89e237ad43c05c587e)

This does not change anything functionally, but it helps to recognize the store in the `Vue Devtools`.
![Vue Devtools heroStore](/img/blog/quasar-toh-vue-devtools-hero-store.png)

When viewing the `heroStore`, all the details of the store can be viewed. Both the `state` and the `getters` can be viewed.
![Vue Devtools heroes store](/img/blog/quasar-toh-vue-devtools-hero-store-details.png)

When clicking on a hero, which changes the `selectedHero`, the change can be viewed in the `Vue Devtools`.

![Vue Devtools selected hero](/img/blog/quasar-toh-vue-devtools-selected-hero.png)
![Vue Devtools selected hero 2](/img/blog/quasar-toh-vue-devtools-selected-hero-2.png)

`time travel`, when selecting `Timeline` it is possible to view the changes of the store (and other Vue actions) through time.

## Migrate Auth service to Pinia store

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

Wherever the `Auth Service` is used, the `Auth Store` will now be used.

`src/layouts/MainLayout.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/4cf0b5b99ae36b7236dd5a21d2a7eb97e2babc03#diff-22686b16c14b601ef5e6e4de40acc8234bf4f4ccbe2d5856e3bb7a0b77da6afc)
Note that the `getter` is passed to the template via a `computed`.
Note that the `action` is passed to the template via an anonymous arrow function.

`src/pages/Login.vue`: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/4cf0b5b99ae36b7236dd5a21d2a7eb97e2babc03#diff-78ff024242ee805990cc729c4b0fe76e134328f5387bf1c8b383c1eb4d4c986d)
Note that first there was a rename of the `login` function to `authenticate` from the Auth Service. This is no longer the case, the `login` function from the `authStore` is called directly on the `authStore`.

In the `devtools` you can now see two stores: the `authStore` and the `heroStore`.

![Auth Store](/img/blog/quasar-toh-vue-devtools-auth-store.gif)

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/4cf0b5b99ae36b7236dd5a21d2a7eb97e2babc03)

## Conclusion

Because all data was already handled separately in a `service`/`composable`, it is easy to migrate to an alternative way of managing data. In this case the migration to using a store via Pinia.

This migration can be done in parts, for example it is possible to migrate one service at a time, until eventually all services are migrated. It is also perfectly okay not to migrate all services to a store.

The advantage of the Pinia store is that the state can be viewed in `Vue Devtools`. The `timeline` allows `time travel` by viewing the state of the application through time. There is no need for `.value` because there is no use of `ref` variables.

The disadvantage of the Pinia store is that there are additional `gotchas`. For example, `getters` must always still be wrapped in a `computed` when passing it to the template (with the service/composable, this was simply a `computed` variable in the service itself). `actions` must be passed to the template via anonymous arrow functions.

The store is simply a way to manage data. If there is no need to be able to see the data in the `Vue Devtools`, it is not necessarily necessary to use a library like Pinia to manage the data.
