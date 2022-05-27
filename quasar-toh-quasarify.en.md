---
postUuid: 45ec8eb7-709c-4554-8f5a-aad6753d2f0e
title: Quasar Tour of Heroes - Quasarify
slug: quasar-toh-quasarify
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

A free [Code Coaching](https://code-coaching.dev/) account is required to follow this tutorial.

This article will start from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The pages have been provided with elements, data and components. The data has been moved to a composable/service. An API with authentication is linked to manage the Hero data. The final situation is a Quasar project where the functionality is identical, but Quasar components and plugins are used.

The code can be obtained by downloading a `zip` from the `Source code`.

The situation that is used as a starting point is as follows:

![begin situation](/img/blog/quasar-toh-service-functions.gif)

Begin situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-api)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![end situation](/img/blog/quasar-toh-quasarify.gif)

End situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-quasarify)

## QInput

Currently, plain HTML inputs are used. Quasar provides a QInput component. On it exist different attributes that can be used, for example in this tutorial `outlined` and `dense` are chosen. The component also accepts a `label` as a prop.

### Before

![input](/img/blog/quasar-toh-default-inputs-1.png)
![input](/img/blog/quasar-toh-default-inputs-2.png)
![input](/img/blog/quasar-toh-default-inputs-3.png)

### After

![input](/img/blog/quasar-toh-qinputs-1.png)
![input](/img/blog/quasar-toh-qinputs-2.png)
![input](/img/blog/quasar-toh-qinputs-3.png)

Due to the default behavior of QInput, it appears too wide. To limit this, a `width` is set on the container (as is already done on the login page).

![input](/img/blog/quasar-toh-qinputs-width-1.png)
![input](/img/blog/quasar-toh-qinputs-width-2.png)

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/2d5ee808234d8aefe9827490cbf6f1f20aad8575)

### Validation

Quasar provides functionality to add validation on input fields.

Change the div around the inputs to a `q-form`. Provide a `ref` variable in which to place an object of type `QForm`. Link the `formRef` to `q-form`.

Then place `rules` on the `<q-input>` elements. This is a prop to which an array of validation rules is passed.

For ease of use, `greedy` is added to the `<q-form>`. This ensures that when `.validate()` is called on the form, all fields are validated at once. We also chose to add `lazy-rules` to `<q-input>`, this ensures that a field is validated only after the field loses focus.

![input validation](/img/blog/quasar-toh-qinput-validation.gif)

```html
<template>
  <div class="login-container">
    <q-form class="login-fields" ref="formRef" greedy>
      <q-input
        outlined
        dense
        label="email"
        type="email"
        v-model="user.email"
        :rules="emailRules"
        lazy-rules
      />
      <q-input
        outlined
        dense
        label="password"
        type="password"
        v-model="user.password"
        :rules="passwordRules"
        lazy-rules
      />
    </q-form>
    <StyledButton @click="login()">Login</StyledButton>
  </div>
</template>

<script lang="ts">
  import { defineComponent, reactive, ref } from "vue";
  import { useAuth } from "src/services/auth.service";
  import StyledButton from "src/components/StyledButton.vue";
  import { useRouter } from "vue-router";
  import { ROUTE_NAMES } from "src/router/routes";
  import { QForm } from "quasar";

  export default defineComponent({
    components: { StyledButton },
    setup() {
      const router = useRouter();
      const { login: authenticate } = useAuth();
      const user = reactive({
        email: "",
        password: "",
      });

      const login = () => {
        void formRef.value.validate().then((valid) => {
          if (valid) {
            authenticate(user.email, user.password)
              .then(() => {
                void router.push({ name: ROUTE_NAMES.DASHBOARD });
              })
              .finally(() => {
                user.email = "";
                user.password = "";
              });
          }
        });
      };

      const formRef = ref({} as QForm);

      const required =
        (s?: string) =>
        (v: string): boolean | string =>
          !!v || (s ? `${s} is required` : "Required");

      const emailFormat =
        () =>
        (v: string): boolean | string =>
          /.+@.+\..+/.test(v) || "Invalid email format";

      const minCharacters =
        (n: number) =>
        (v: string): boolean | string =>
          v.length >= n || `Must be at least ${n} characters`;

      const emailRules = [required("Email"), emailFormat()];
      const passwordRules = [required("Password"), minCharacters(8)];

      return {
        login,
        user,

        formRef,
        emailRules,
        passwordRules,
      };
    },
  });
</script>

<style lang="scss">
  .login-container {
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 20rem;
  }

  .login-fields {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
</style>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d2012bfc70a8dc28061922794133ff5fe6103915)

## Composable for validation

Currently, validation has been added only in the login page. But validation can (and will) occur in multiple places. So it's a good idea to put these validation methods in a composable right away.

To repeat, composable/service contains `reusable code'. Within this tutorial series, composables with global state are called services. Composables without global state are called composables. This makes it possible, simply by looking at the extension of a file, to know whether it is a composable with global state or a composable without global state.

`src/services/auth.service.ts`: contains global state
`src/services/hero.service.ts`: contains global state
`src/services/validator.composable.ts`: contains no global state

Global state is the variable above the function. So in these examples above `useAuth` and `useHero`.

Create the file `src/services/validator.composable.ts`.

```ts
const useValidators = () => {
  const required =
    (s?: string) =>
    (v: string): boolean | string =>
      !!v || (s ? `${s} is required` : "Required");

  const emailFormat =
    () =>
    (v: string): boolean | string =>
      /.+@.+\..+/.test(v) || "Invalid email format";

  const minCharacters =
    (n: number) =>
    (v: string): boolean | string =>
      v.length >= n || `Must be at least ${n} characters`;

  const emailRules = [required("Email"), emailFormat()];
  const passwordRules = [required("Password"), minCharacters(8)];

  return {
    required,
    emailFormat,
    minCharacters,
    emailRules,
    passwordRules,
  };
};

export { useValidators };
```

Both the validation functions and the array of validation functions for specific types of fields are returned. It is assumed that every email field has the same validation, that every password field has the same validation. If there were any differing fields, a new array of separate values can be created with the appropriate validation functions for these fields in the component. This approach is used for the other two inputs.

Use this composable in the login page.

`src/pages/login.vue`

```ts
import { defineComponent, reactive, ref } from "vue";
import { useAuth } from "src/services/auth.service";
import StyledButton from "src/components/StyledButton.vue";
import { useRouter } from "vue-router";
import { ROUTE_NAMES } from "src/router/routes";
import { QForm } from "quasar";
import { useValidators } from "src/services/validator.composable"; // new

export default defineComponent({
  components: { StyledButton },
  setup() {
    const router = useRouter();
    const { login: authenticate } = useAuth();
    const { emailRules, passwordRules } = useValidators(); // new
    const user = reactive({
      email: "",
      password: "",
    });

    const login = () => {
      void formRef.value.validate().then((valid) => {
        if (valid) {
          authenticate(user.email, user.password)
            .then(() => {
              void router.push({ name: ROUTE_NAMES.DASHBOARD });
            })
            .finally(() => {
              user.email = "";
              user.password = "";
            });
        }
      });
    };

    const formRef = ref({} as QForm);

    return {
      login,
      user,

      formRef,
      emailRules,
      passwordRules,
    };
  },
});
```

Use this composable in the pages with the other inputs as well. Again, provide a `<q-form>` with a ref and the attribute `greedy`. Place `rules` on the `<q-input>` and place the attribute `lazy-rules` on the `<q-input>`.

`src/pages/HeroAdd.vue`

```html
<template>
  <q-form class="add-container" ref="formRef" greedy>
    <div class="title">Add hero</div>

    <q-input
      outlined
      dense
      label="name"
      v-model="name"
      :rules="[required()]"
      lazy-rules
    />

    <ButtonGroup class="button-group">
      <StyledButton @click="moveBack()">Back</StyledButton>
      <StyledButton primary @click="saveHero()">Save</StyledButton>
    </ButtonGroup>
  </q-form>
</template>

<script lang="ts">
  import { defineComponent, ref } from "vue";
  import StyledButton from "components/StyledButton.vue";
  import ButtonGroup from "components/ButtonGroup.vue";
  import { useRouter } from "vue-router";
  import { useHeroes } from "src/services/hero.service";
  import { useValidators } from "src/services/validator.composable";
  import { QForm } from "quasar";

  export default defineComponent({
    components: {
      StyledButton,
      ButtonGroup,
    },
    setup() {
      const router = useRouter();
      const name = ref("");
      const { addHero } = useHeroes();
      const { required } = useValidators();

      const moveBack = () => void router.go(-1);
      const saveHero = () => {
        void formRef.value.validate().then((valid) => {
          if (valid) {
            addHero(name.value);
            moveBack();
          }
        });
      };

      const formRef = ref({} as QForm);

      return {
        name,
        moveBack,
        saveHero,
        required,
        formRef,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .add-container {
    width: 20rem;
  }

  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .button-group {
    margin-top: 1rem;
  }
</style>
```

`src/pages/HeroDetails.vue`

```html
<template>
  <q-form v-if="hero" class="details-container" ref="formRef" greedy>
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <q-input
      outlined
      dense
      label="name"
      v-model="hero.name"
      :rules="[required()]"
      lazy-rules
    />

    <ButtonGroup class="button-group">
      <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
      <StyledButton primary @click="saveHero()">Save</StyledButton>
      <StyledButton negative @click="removeHero()">Delete</StyledButton>
    </ButtonGroup>
  </q-form>

  <div v-else class="title">Hero not found!</div>
</template>

<script lang="ts">
  import { defineComponent, onBeforeMount, ref, Ref } from "vue";
  import StyledButton from "components/StyledButton.vue";
  import ButtonGroup from "components/ButtonGroup.vue";
  import { Hero } from "components/models";
  import { useRoute, useRouter } from "vue-router";
  import { useHeroes } from "src/services/hero.service";
  import { useValidators } from "src/services/validator.composable";
  import { QForm } from "quasar";

  export default defineComponent({
    components: {
      StyledButton,
      ButtonGroup,
    },
    setup() {
      const route = useRoute();
      const router = useRouter();
      const hero: Ref<Hero> = ref() as Ref<Hero>;
      const { editHero, findHero, deleteHero } = useHeroes();
      const { required } = useValidators();

      onBeforeMount(() => {
        const { id } = route.params;
        if (id) {
          const matchingHero = findHero(id.toString());
          if (matchingHero) hero.value = matchingHero;
        }
      });

      const moveBack = () => void router.go(-1);
      const saveHero = () => {
        void formRef.value.validate().then((valid) => {
          if (valid) {
            editHero(hero.value);
            moveBack();
          }
        });
      };
      const removeHero = () => {
        deleteHero(hero.value);
        moveBack();
      };

      const formRef = ref({} as QForm);

      return {
        hero,
        moveBack,
        saveHero,
        removeHero,
        required,
        formRef,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .details-container {
    width: 20rem;
  }

  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .button-group {
    margin-top: 1rem;
  }
</style>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/90e991e46aafb67dd821356a9e87d1a93b36b17e)

## QBtn

Currently a `StyledButton` component is used, in this a regular `<button>` element is used. Change this to a `<q-btn>` and see what happens next.

![QBtn](/img/blog/quasar-toh-qbtn.gif)

In the first part, the `button` can be seen, it is clicked but this is not noticeable. After changing to `q-btn` suddenly there is a click animation and additional styling such as a shadow. All by simply changing `button` to `q-btn`.

`src/components/StyledButton.vue`

Before changes:

```html
<button
  :class="{
      primary: primary,
      negative: negative,
    }"
>
  <slot />
</button>
```

After changes:

```html
<q-btn
  :class="{
      primary: primary,
      negative: negative,
    }"
>
  <slot />
</q-btn>
```

Next, the heroes in the `HeroList.vue` can be seen as buttons, even though they are currently just `div` elements. Make this a `q-btn` component and change some styling so that the button looks correct again.

`src/pages/HeroList.vue`

```html
<div class="hero-list">
  <q-btn
    :class="{
        'hero--active': hero._id === selectedHero?._id,
      }"
    class="hero"
    v-for="(hero, index) in heroes"
    :key="index"
    @click="onClickHero(hero)"
  >
    <span class="hero-number">{{ hero.number }}</span>
    <span class="hero-name">{{ hero.name }}</span>
  </q-btn>
</div>
```

```scss
.title {
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
  padding: 0; // new
  align-items: flex-start; // new

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

.new-hero-button {
  margin-top: 1rem;
}
```

![QBtn Hero List](/img/blog/quasar-toh-qbtn-hero-list.gif)

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/f0b6b18d6d8f5cf2a5ded566f5e86ff2dc1ef62d)

## QNotify Plugin

Currently, an alert is used to provide feedback to the user when something goes wrong. This will be changed to the `QNotify` component. Notifications will also be added to successful actions.

### Add in quasar.conf.js

```js
{
  // ...
    framework: {
      config: {
        notify: {
          timeout: 3000,
        },
      },

      plugins: [
        'Notify'
      ],
    },
  // ...
}
```

Adding the `notify` object to the `config` object changes the default values. In this example, the default timeout is changed from 5000ms (5 seconds) to 3000ms (3 seconds).

To provide the user with better feedback when creating, modifying or deleting a hero, a notification is displayed. Both when the call is successful (`.then`) and when there is an error (`.catch`), a notification is shown.

`src/services/hero.service.ts`

```ts
import { AxiosRequestConfig } from "axios";
import { api } from "src/boot/axios";
import { BackendHero, Hero } from "src/components/models";
import { readonly, Ref, ref, computed } from "vue";
import { Notify } from "quasar"; // new

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

const heroes: Ref<Array<Hero>> = ref([]);

const selectedHero = ref({} as Hero);

const useHeroes = () => {
  const topHeroes = computed(() => heroes.value.slice(0, 4));

  const getHeroes = async () => {
    const result = await api.get<Paged<BackendHero>>(
      "/heroes",
      getRequestConfig()
    );

    heroes.value = result.data.data.map((hero: BackendHero): Hero => {
      return {
        ...hero,
        number: hero.id,
      };
    });
  };

  const editHero = (hero: Hero) => {
    if (hero._id) {
      const index = heroes.value.findIndex((h: Hero) => h._id === hero._id);
      const oldHero = heroes.value[index];
      heroes.value[index] = hero;

      api
        .patch(`/heroes/${hero._id}`, hero, getRequestConfig())
        .then(
          () => Notify.create({ message: "Hero updated", color: "positive" }) // new
        )
        .catch(() => {
          Notify.create({ message: "Hero update failed", color: "negative" }); // changed
          heroes.value[index] = oldHero;
        });
    }
  };

  const findHero = (id: string): Hero | undefined => {
    const matchingHero = heroes.value.find((h) => h.number === id);
    if (matchingHero) return { ...matchingHero };
  };

  const deleteHero = (hero: Hero) => {
    if (hero._id) {
      const index = heroes.value.findIndex((h) => h._id === hero._id);
      heroes.value.splice(index, 1);

      api
        .delete(`/heroes/${hero._id}`, getRequestConfig())
        .then(
          () => Notify.create({ message: "Hero deleted", color: "positive" }) // new
        )
        .catch(() => {
          Notify.create({
            message: "Failed to delete hero",
            color: "negative",
          }); // changed
          heroes.value.splice(index, 0, hero);
        });
    }
  };

  const addHero = (name: string) => {
    let maxNumber = Math.max(...heroes.value.map((h) => +h.number));
    if (maxNumber === -Infinity) maxNumber = 0;

    const number = (maxNumber + 1).toString();
    const newHero = { number, name };

    heroes.value.push(newHero);
    const index = heroes.value.length - 1;

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
        Notify.create({ message: "Hero added", type: "positive" }); // new
        heroes.value[index] = { ...result.data, number: result.data.id };
      })
      .catch(() => {
        Notify.create({ message: "Error adding hero", type: "negative" }); // changed
        const index = heroes.value.findIndex((e) => e === newHero);
        heroes.value.splice(index, 1);
      });
  };

  const resetSelectedHero = () => (selectedHero.value = {} as Hero);
  const setSelectedHero = (hero: Hero) => (selectedHero.value = hero);

  return {
    heroes: readonly(heroes),
    selectedHero: readonly(selectedHero),
    topHeroes,
    editHero,
    findHero,
    deleteHero,
    resetSelectedHero,
    addHero,
    setSelectedHero,
    getHeroes,
  };
};

export { useHeroes };
```

![QNotify](/img/blog/quasar-toh-notify.gif)

Do the same for the auth service.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/605526c48d074c812d0a677abaa4c573b71a47e5)

## Conclusion

By using components and plugins present in Quasar, it is easy to quickly get a good looking application with subtle click animations, good styling and easy to use notifications.

Find even more components and plugins provided by Quasar in the [Quasar Docs](https://quasar.dev/). Want a detailed example in video format? Visit the site created by the always cheerful, always enthusiastic Luke Diebold [Quasar Component Videos](https://quasarcomponents.com/).
