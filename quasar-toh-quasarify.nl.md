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
youtubeIds:
  - 4_qJpxI-Fmk
---

Om deze tutorial te kunnen volgen is een gratis [Code Coaching](https://code-coaching.dev/) account nodig.

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een composable/service. Een API met authentificatie is gekoppeld om de Hero data te beheren. De eindsituatie is een Quasar-project waarin de functionaliteit identiek is, maar er wordt gebruik gemaakt van Quasar-componenten en -plugins.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-service-functions.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-api)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-quasarify.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-quasarify)

## QInput

Momenteel wordt er gebruik gemaakt van gewone HTML-inputs. Quasar voorziet een QInput component. Hierop bestaan verschillende attributen die gebruikt kunnen worden, zo wordt er in deze tutorial gekozen voor `outlined` en `dense`. De component accepteert ook een `label` als prop.

### Voor

![input](/img/blog/quasar-toh-default-inputs-1.png)
![input](/img/blog/quasar-toh-default-inputs-2.png)
![input](/img/blog/quasar-toh-default-inputs-3.png)

### Na

![input](/img/blog/quasar-toh-qinputs-1.png)
![input](/img/blog/quasar-toh-qinputs-2.png)
![input](/img/blog/quasar-toh-qinputs-3.png)

Door het standaard gedrag van QInput, komt deze te breed over. Om dit te limiteren wordt er een `width` op de container gezet (zoals al gedaan wordt op de inlogpagina).

![input](/img/blog/quasar-toh-qinputs-width-1.png)
![input](/img/blog/quasar-toh-qinputs-width-2.png)

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/2d5ee808234d8aefe9827490cbf6f1f20aad8575)

### Validatie

Quasar voorziet functionaliteit om validatie te voorzien op inputvelden.

Wijzig de div rondom de inputs naar een `q-form`. Voorzien een `ref`-variabele waarin een object geplaatst wordt van het type `QForm`. Koppel de `formRef` aan `q-form`.

Plaats vervolgens `rules` op de `<q-input>`-elementen. Dit is een prop waaraan een array van validatieregels wordt meegegeven.

Voor gebruiksgemak is `greedy` toegevoegd aan de `<q-form>`. Dit zorgt ervoor dat als `.validate()` wordt aangeroepen op de form, alle velden in één keer worden gevalideerd. Ook is er gekozen om `lazy-rules` toe te voegen aan `<q-input>`, dit zorgt ervoor dat een veld pas wordt gevalideerd nadat het veld focus verliest.

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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d2012bfc70a8dc28061922794133ff5fe6103915)

## Composable voor validatie

Momenteel is er enkel in de loginpagina validatie toegevoegd. Maar validatie kan (en zal) op meerdere plaatsen voorkomen. Het is dus een goed idee om deze validatiemethoden meteen in een composable te plaatsen.

Ter herhaling, composable/service bevat `reusable code` (Nederlands: `herbruikbare code`). Binnen deze tutorialreeks worden composables met globale state, services genoemd. Composables zonder globale state, worden composables genoemd. Hierdoor is het mogelijk door simpelweg naar de extensie te kijken van een bestand om te weten of het gaat om een composable met globale state of een composable zonder globale state.

`src/services/auth.service.ts`: bevat globale state
`src/services/hero.service.ts`: bevat globale state
`src/services/validator.composable.ts`: bevat géén globale state

Globale state zijn de variabelen die boven de functie staan. In deze voorbeelden dus boven `useAuth` en `useHero`.

Maak het bestand `src/services/validator.composable.ts` aan.

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

Zowel de validatiefuncties als de array met validatiefuncties voor specifieke types van velden worden teruggegeven. Er wordt van uitgegaan dat elk e-mailveld dezelfde validatie heeft, dat elk wachtwoordveld dezelfde validatie heeft. Indien er toch afwijkende velden zouden zijn, kan een nieuwe array met aparte waarden worden opgesteld met de juiste validatiefuncties voor deze velden in de component. Deze aanpak wordt gebruikt voor de twee andere inputs.

Gebruik deze composable in de loginpagina.

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

Gebruik deze composable ook in de pagina's met de andere inputs. Zorg opnieuw voor een `<q-form>` met een ref en het attribuut `greedy`. Plaats `rules` op de `<q-input>` en plaats het attribuut `lazy-rules` op de `<q-input>`.

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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/90e991e46aafb67dd821356a9e87d1a93b36b17e)

## QBtn

Momenteel wordt er gebruikgemaakt van een `StyledButton`-component, hierin is een gewoon `<button>`-element gebruikt. Wijzig dit naar een `<q-btn>` en zie wat er nu gebeurt.

![QBtn](/img/blog/quasar-toh-qbtn.gif)

In het eerste deel is de `button` te zien, er wordt op geklikt maar dit is niet te merken. Na het wijzigen naar `q-btn` is er plots een klikanimatie en extra styling zoals een schaduw. Allemaal door simpelweg `button` te wijzigen in `q-btn`.

`src/components/StyledButton.vue`

Voor wijziging:

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

Na wijziging:

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

Vervolgens kunnen de heroes in de `HeroList.vue` gezien worden als knoppen, ook al zijn het momenteel gewoon `div`-elementen. Maak hier een `q-btn`-component van en wijzig wat aan de styling zodat de knop er terug correct uitziet.

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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/f0b6b18d6d8f5cf2a5ded566f5e86ff2dc1ef62d)

## QNotify Plugin

Momenteel wordt er gebruik gemaakt van een alert om feedback te geven aan de gebruiker wanneer er iets misgaat. Dit zal gewijzigd worden naar de `QNotify`-component. Ook zullen er notificaties toegevoegd worden aan de succesvolle acties.

### Toevoegen in quasar.conf.js

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

Door het `notify`-object toe te voegen aan het `config`-object, worden de default waarden gewijzigd. In dit voorbeeld wordt de default timeout van 5000ms (5 seconden) gewijzigd naar 3000ms (3 seconden).

Om de gebruiker van betere feedback te voorzien bij het aanmaken, wijzigen of verwijderen van een hero, wordt er een notificatie getoond. Zowel als de call succesvol is (`.then`) als wanneer er een fout is (`.catch`), wordt er een notificatie getoond.

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

Doe hetzelfde voor de auth service.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/605526c48d074c812d0a677abaa4c573b71a47e5)

## Conclusie

Door componenten en plugins te gebruiken die aanwezig zijn in Quasar, is het makkelijk om snel een goed uitziende applicatie te krijgen met subtiele klikanimaties, goede styling en makkelijk te gebruiken notificaties.

Vind nog meer componenten en plugins aangeleverd door Quasar in de [Quasar Docs](https://quasar.dev/). Wil je een gedetailleerd voorbeeld in videoformaat? Bezoek de site gemaakt door de altijd vrolijke, altijd enthousiaste Luke Diebold [Quasar Component Videos](https://quasarcomponents.com/).
