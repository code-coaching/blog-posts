---
postUuid: 595d1a0c-ea32-46a6-9153-854d8052be5e
title: Quasar Tour of Heroes - API
slug: quasar-toh-api
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
youtubeIds:
  - FnoMmbL9wGo
  - oBRbrYbrBLY
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een composable/service. De eindsituatie is een Quasar-project waarin de data gekoppeld zit aan een API en waar authenticatie nodig is.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-service-functions.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-services-composables)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-api-finished.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-api)

## API

Een `API` (Application Programming Interface) is een interface waarmee een applicatie kan communiceren met een andere applicatie. In dit geval, zal de frontendapplicatie communiceren met de backendapplicatie.

De backendapplicatie wordt voorzien door Code Coaching. Registreer een gratis account op [https://code-coaching.dev](https://code-coaching.dev/#/sign-up), deze account zal gebruikt worden om in te loggen en om de data van de heroes te beheren op de server.

## Axios

Om HTTP-requests te doen, kan er gebruikgemaakt worden van de `fetch API` van de browser. Er bestaan ook alternatieve libraries zoals `axios` om deze functionaliteit te implementeren.

In deze tutorial wordt gekozen voor `axios`.

### Axios dependency

Installeer de `axios dependency` in de `package.json`:

```sh
npm install axios
```

### Quasar boot file

Binnen Quasar is er een concept van `boot files`, dit zijn bestanden die uitgevoerd worden voor het opstarten van de applicatie. Er moeten twee dingen gebeuren om een boot file te kunnen gebruiken:

Maak een file aan in de `src/boot` directory, de naam die wordt gebruikt in deze tutorial is `axios.ts`.

`src/boot/axios.ts`

```ts
import axios, { AxiosInstance } from "axios";

const serverUrl = "https://api.code-coaching.dev";
const api = axios.create({ baseURL: serverUrl });

export { api };
```

Voeg de naam van deze boot file toe aan de array `boot` in quasar.conf.js:

```js
module.exports = configure(function (ctx) {
  return {
    // https://quasar.dev/quasar-cli/supporting-ts
    supportTS: {
      tsCheckerConfig: {
        eslint: {
          enabled: true,
          files: './src/**/*.{ts,tsx,js,jsx,vue}',
        },
      },
    },

    // https://quasar.dev/quasar-cli/prefetch-feature
    // preFetch: true,

    // app boot file (/src/boot)
    // --> boot files are part of "main.js"
    // https://quasar.dev/quasar-cli/boot-files
    boot: ['axios'],

    // MORE PROPERTIES HERE
```

Dit is **niet** de volledige code, dit voorbeeld is zodat het duidelijk is wat er in de boot file moet gebeuren.

### Gebruiken van de Axios API

Voeg een nieuwe pagina toe genaamd `Login.vue` aan de `src/pages` directory.

`src/pages/Login.vue`

```html
<template>
  <input type="email" />
  <input type="password" />
  <button @click="login()">Login</button>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { api } from "boot/axios";

  export default defineComponent({
    setup() {
      const login = () => {
        api
          .post("/authentication", {
            strategy: "local",
            email: "info+quasar@code-coaching.dev",
            password: "SuperSecretPassword",
          })
          .then(console.log)
          .catch((error) => {
            console.error(error);
            alert("Login failed");
          });
      };

      return {
        login,
      };
    },
  });
</script>
```

Korte uitleg bij bovenstaande code:
Wanneer op de knop `Login` wordt geklikt, dan wordt de `login()` functie aangeroepen. Deze functie zal een `post` request doen naar de API, naar het `/authentication` endpoint.
De data die meegestuurd wordt als body van de request is:

```json
{
  "strategy": "local",
  "email": "info+quasar@code-coaching.dev",
  "password": "SuperSecretPassword"
}
```

Indien de request succesvol verlopen is **dan** (Engels: **then**) wordt de `console.log` functie aangeroepen met de data van de respons. `.then(console.log)` is een kortere syntax voor `.then(response => console.log(response))`.

Indien de request mislukt, dan wordt de **catch** (Nederlands: opvangen) uitgevoerd. Hier wordt de error gelogd naar de console via `console.error`, dit is hetzelfde als `console.log`, maar zal de tekst in het rood tonen. Ook zal er een bericht getoond worden aan de gebruiker om te laten weten dat het inloggen mislukt is.

Toevoegen van de pagina aan de `src/router/routes.ts` file:

```ts
export const ROUTE_NAMES = {
  DASHBOARD: "Dashboard",
  HERO_LIST: "HeroList",
  HERO_DETAILS: "HeroDetails",
  HERO_ADD: "HeroAdd",
  LOGIN: "Login",
};
```

De `LOGIN` is nieuw. Voeg ook de route toe aan de lijst van routes in `src/router/routes.ts`:

```ts
// more code here
  {
    path: '/',
    component: () => import('layouts/MainLayout.vue'),
    children: [
      {
        name: ROUTE_NAMES.DASHBOARD,
        path: '',
        component: () => import('pages/Dashboard.vue'),
      },
      {
        name: ROUTE_NAMES.HERO_LIST,
        path: '/heroes',
        component: () => import('pages/HeroList.vue'),
      },
      {
        name: ROUTE_NAMES.HERO_ADD,
        path: '/heroes/add',
        component: () => import('pages/HeroAdd.vue'),
      },
      {
        name: ROUTE_NAMES.HERO_DETAILS,
        path: '/heroes/:id',
        component: () => import('pages/HeroDetails.vue'),
      },
      {
        name: ROUTE_NAMES.LOGIN,
        path: '/login',
        component: () => import('pages/Login.vue'),
      }
    ],
  },
// more code here
```

Wijzig het hardcoded e-mailadres en wachtwoord naar jouw e-mailadres en wachtwoord (**niet committen!**) - dit is om een test uit te voeren of de API goed gekoppeld is.

```ts
const login = () => {
  api
    .post("/authentication", {
      strategy: "local",
      email: "here@your-email.com",
      password: "HERE_THE_PASSWORD",
    })
    .then(console.log)
    .catch((error) => {
      console.error(error);
      alert("Login failed");
    });
};
```

Wijzig `here@your-email.com` naar het e-mailadres waarmee je een account geregistreerd hebt op [Code Coaching](https://code-coaching.dev/#/sign-up).

Wijzig `HERE_THE_PASSWORD` naar het wachtwoord dat gebruikt is om de account te registreren.

(Om de **catch** te testen, vul een fout wachtwoord in.)

De test:

- Start de applicatie op.
- Open de console (F12 - of rechtermuisklik `Inspecteren`).
- Ga naar `/#/login` in de url en klik op de knop `Login`.

Bekijk de console (of de response van de netwerkcall).

Er wordt een heel groot object teruggestuurd, het belangrijke om hier uit te begrijpen is dat er een `accessToken` wordt teruggestuurd.

```json
{
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6ImFjY2VzcyJ9.eyJpYXQiOjE2NTI0ODk4NDAsImV4cCI6MTY1MjU3NjI0MCwiYXVkIjoiaHR0cHM6Ly9jb2RlLWNvYWNoaW5nLmRldiIsImlzcyI6IkNvZGUgQ29hY2hpbmciLCJzdWIiOiI2MThkOWI3ZDI3NjNjYjRjMzU1MmZiOGYiLCJqdGkiOiJkMTcwODcwNS1iNmI0LTQ3YWQtYWQ1OS0yYzQ3ZDE3YThhYjkifQ.wXScLJbN1z4laatH5Bp10ehkk2uKOsEpasozo-Wrff1",
    "authentication": {},
    "user": {}
  },
  "status": 201,
  "statusText": "Created",
  "headers": {
    "content-type": "application/json; charset=utf-8"
  },
  "config": {},
  "request": {}
}
```

Merk op: Er is data verwijderd om het object overzichterlijker te maken.

Deze `accessToken` is 1 week geldig en kan gebruikt worden om verzoeken te sturen naar de API met de ingelogde gebruiker. Aangezien deze token jouw persoonlijke gebruiker voorstelt, is het dus belangrijker om deze **niet** te delen met anderen.

Dit soort token is een **JWT** token. Dit staat voor **J**SON **W**eb **T**oken, het is een standaard die veel gebruikt wordt om authenticatie te voorzien.

Aangezien deze token nodig is om de gebruiker te authentificeren, moet deze opgeslagen worden in de browser om vervolgens aan elk verzoek toe te voegen waarvoor authenticatie nodig is. Dit kan door het op te slaan in een `Cookie` of door deze toe te voegen aan `Local Storage`. Dit wordt later in deze tutorial besproken.

Voeg two way binding toe aan de inputvelden zodat de gebruikersnaam en het wachtwoord niet langer hardcoded in de code staan.

```html
<template>
  <input type="email" v-model="user.email" />
  <input type="password" v-model="user.password" />
  <button @click="login()">Login</button>
</template>

<script lang="ts">
  import { defineComponent, reactive } from "vue";
  import { api } from "boot/axios";

  export default defineComponent({
    setup() {
      const user = reactive({
        email: "",
        password: "",
      });

      const login = () => {
        api
          .post("/authentication", {
            strategy: "local",
            email: user.email,
            password: user.password,
          })
          .then(console.log)
          .catch((error) => {
            console.log(error);
            alert("Login failed");
          });
      };

      return {
        login,
        user,
      };
    },
  });
</script>
```

Merk op: In principe is het niet nodig om het `user` object `reactive` te maken. Dit is enkel nodig wanneer de view deze data zou gebruiken, zodat Vue weet dat de data gewijzigd moet worden. Het kan echter geen kwaad om de `state` van de component `reactive` te maken. Stel dat `{{ user.email }}` wordt toegevoegd in de view, dan zal Vue automatisch de waarde updaten in de view.

Kleine wijziging in `app.scss`, momenteel wordt de styling enkel toegepast op inputs van type `text`, maar er zijn inputs zonder type en inputs met het type `email` en `password` in deze applicatie, wijzig de selector zodat het van toepassing is op alle `input`-elementen.

Voor wijziging:

```scss
body,
input[type="text"],
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
```

Na wijziging:

```scss
body,
input,
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/71730add0a6fc46a7d97eb0de0043313a7bcf690)

## JWT opslaan in Local Storage

Er is ondertussen code om in te loggen. Bij het invullen van de juiste gegevens zien we een respons waarin een `accessToken` wordt teruggestuurd. Deze `accessToken` is een `JSON Web Token (JWT)`, om deze op een later tijdstip terug aan te kunnen, moet deze ergen opgeslagen worden.

In deze tutorial, aangezien er enkel een string opgeslagen moet worden, zal er gebruikgemaakt worden van de Local Storage API die standaard aanwezig is, deze werkt standaard met strings.

Indien er meer data opgeslagen wordt van verschillende datatypes, bijvoorbeeld nummers of booleans of datums... dan is het interessant om de [Quasar LocalStorage Plugin](https://quasar.dev/quasar-plugins/web-storage) te gebruiken, deze zal automatisch de opgeslagen string in Local Storage omvormen naar het juiste datatype.

`Login.vue`

```js
const login = () => {
  api
    .post("/authentication", {
      strategy: "local",
      email: user.email,
      password: user.password,
    })
    .then((result: { data: { accessToken: string } }) => {
      const accessToken = result.data.accessToken;
      if (accessToken) localStorage.setItem("accessToken", accessToken);
    })
    .catch((error) => {
      console.log(error);
      alert("Login failed");
    });
};
```

Waar er eerst `.then(console.log)` stond, wat de verkorte syntax is voor `.then((result) => console.log(result))`, staat er nu `.then((result: { data: { accessToken: string }}) => {})`. Dit is omdat er gebruik wordt gemaakt van TypeScript. Hier wordt aangegeven dat `result` een complex type is. Namelijk een object met een property genaamd `data`. En Data is opnieuw een object met een property genaamd `accessToken` van het type `string`.

Vervolgens wordt de accessToken aan een variabele `accessToken` toegekend. Dan wordt er gekeken of deze variabele `truthy` is (dus niet undefined of een lege string). Indien de variabele een waarde heeft, wordt deze toegevoegd aan Local Storage.

```ts
const accessToken = result.data.accessToken;
if (accessToken) localStorage.setItem("accessToken", accessToken);
```

Merk op: localStorage wordt nergens geïmporteerd, dit is hetzelfde als `window.localStorage`, het `window` object is altijd aanwezig in de browser zonder enige import te moeten doen, hierop bestaat onder andere localStorage als property om de Local Storage API te gebruiken.

Start de applicatie opnieuw en log opnieuw in met de gebruikersnaam en wachtwoord waarmee een account geregistreerd is op Code Coaching. Bekijk na het inloggen de inhoud van Local Storage.

Firefox
![Local Storage Firefox](/img/blog/quasar-toh-firefox-localstorage.png)

Chrome
![Local Storage Chrome](/img/blog/quasar-toh-chrome-localstorage.png)

## Authentication composable/service

Om te zorgen dat de authenticatie van een gebruiker niet per component geïmplementeerd moet worden, zal deze in een composable/service geïmplementeerd worden.

Maak een nieuw bestand aan in `src/services` genaamd `auth.service.ts`.

```ts
import { api } from "src/boot/axios";
import { readonly, ref, computed } from "vue";

interface User {
  _id: string;
}

const authenticatedUser = ref({} as User);

const useAuth = () => {
  const isAuthenticated = computed(
    () => authenticatedUser.value._id !== undefined
  );

  const tryToAuthenticate = () => {
    const accessToken = localStorage.getItem("accessToken");
    if (accessToken) {
      api
        .post("/authentication", {
          strategy: "jwt",
          accessToken,
        })
        .then((result: { data: { accessToken: string; user: User } }) => {
          console.log(result);
          authenticatedUser.value = result.data.user;
        })
        .catch(() => {
          localStorage.removeItem("accessToken");
          authenticatedUser.value = {} as User;
        });
    }
  };

  const login = (email: string, password: string): Promise<void> => {
    return api
      .post("/authentication", {
        strategy: "local",
        email,
        password,
      })
      .then((result: { data: { accessToken: string; user: User } }) => {
        const accessToken = result.data.accessToken;
        if (accessToken) {
          localStorage.setItem("accessToken", accessToken);
          authenticatedUser.value = result.data.user;
          console.log(result.data.user);
          alert("Login successful");
        }
      })
      .catch((error) => {
        console.log(error);
        alert("Login failed");
      });
  };

  const logout = () => {
    localStorage.removeItem("accessToken");
    authenticatedUser.value = {} as User;
  };

  return {
    authenticatedUser: readonly(authenticatedUser),
    isAuthenticated: readonly(isAuthenticated),
    tryToAuthenticate,
    login,
    logout,
  };
};

export { useAuth };
```

Bovenstaande code in meer detail:

```ts
interface User {
  _id: string;
}
```

Dit is een TypeScript interface dat een User object omschrijft. Momenteel is er enkel `_id` aan toegevoegd, in een latere stap wordt er gekeken naar hoe het volledige `model` omschrijven kan worden. Uiteindelijk zal de interface dan ook verplaatst worden naar `src/components/models.ts`.

```ts
const isAuthenticated = computed(
  () => authenticatedUser.value._id !== undefined
);
```

Er wordt gebruik gemaakt van de `computed` functie, afkomstig uit Vue `import { computed } from "vue";`. Deze functie geeft een computed property terug, dit zal de waarde van de computed property automatisch updaten als de waarde van de property wijzigt. Concreet, als `authenticatedUser.value._id` wijzigt, zal de waarde van `isAuthenticated` automatisch gewijzigd worden.

`tryToAuthenticate` haalt de accessToken uit Local Storage en probeert deze te gebruiken om de gebruiker in te loggen. Als dit lukt, wordt de gebruiker aan de `authenticatedUser` variabele toegekend (dit zal vervolgens dus de `isAuthenticated` computed property updaten). Indien dit niet lukt, betekent dit dat de token niet langer geldig is, vervolgens wordt de `authenticatedUser` variabele een leeg object toegekend en wordt de `accessToken` uit Local Storage verwijderd. Er zal opnieuw manueel ingelogd moeten worden.

`login` bevat de logica die eerst in de `Login.vue`-pagina stond gedefinieerd. Met als extra dat opnieuw `authenticatedUser` een waarde wordt toegekend indien de login succesvol is en er is ook een `alert()` toegevoegd. De `console.log` blijft tijdelijk staan, hier gaan we kijken wat het model is dat terugkomt van de server.

`logout` verwijdert de accessToken uit Local Storage en zet de `authenticatedUser` variabele op `{}` (dit zal de `isAuthenticated` computed property updaten). Merk op dat er een type casting gedaan wordt `{} as User`, een alternatieve syntax is `<User>{}`. Dit is nodig omdat `{}` niet voldoet aan de interface `User`, maar een user zonder \_id wordt gezien als een niet-authenticated user, dus is er een type casting nodig.

Elke variabele en functie die gebruikt moet kunnen worden, wordt teruggeven als object met named properties.

```ts
return {
  authenticatedUser: readonly(authenticatedUser),
  isAuthenticated: readonly(isAuthenticated),
  tryToAuthenticate,
  login,
  logout,
};
```

Dit stelt ons in staat om de functies en variabelen aan te roepen via destructuring. Bijvoorbeeld:

```ts
import { useAuth } from "src/services/auth.service";

const { login, logout, isAuthenticated } = useAuth();
```

De variabelen waarvan we enkel de waarde willen uitlezen, worden teruggegeven als readonly properties. Dit zorgt ervoor dat de waarde niet gewijzigd kan worden buiten de service.

```ts
return {
  authenticatedUser: readonly(authenticatedUser),
  isAuthenticated: readonly(isAuthenticated),
};
```

### Wijzigingen Login.vue

Het is niet langer de api die rechtstreeks aangesproken wordt, maar de `auth.service` die gebruikt wordt om de gebruiker in te loggen.

```html
<template>
  <input type="email" v-model="user.email" />
  <input type="password" v-model="user.password" />
  <button @click="login()">Login</button>
</template>

<script lang="ts">
  import { defineComponent, reactive } from "vue";
  import { useAuth } from "src/services/auth.service";

  export default defineComponent({
    setup() {
      const { login: authenticate } = useAuth();
      const user = reactive({
        email: "",
        password: "",
      });

      const login = () => {
        authenticate(user.email, user.password).finally(() => {
          user.email = "";
          user.password = "";
        });
      };

      return {
        login,
        user,
      };
    },
  });
</script>
```

Er is een `name collision` (Nederlands: `naam-conflict`) gevonden tussen de `login` functie die uit `useAuth()` gehaald wordt en de bestaande `login` functie binnen het component. Om te zorgen dat er geen conflict is, wordt de `login` functie uit `useAuth()` hernoemd naar `authenticate`.

```ts
const { login: authenticate } = useAuth();
```

Dit kan gelezen worden als: `const authenticate = useAuth().login;`.
Ofwel: plaats de definitie van de `login` functie uit `useAuth()` in een nieuwe variabele genaamd `authenticate`.
Wanneer `authenticate()` wordt aangeroepen, wordt eigenlijk de `login` functie uit `useAuth()` aangeroepen.

Merk ook op dat de `.then` en `.catch` worden afgehandeld in de auth service. Er wordt vervolgens gebruik gemaakt van `.finally` om de `user.email` en `user.password` te resetten na het inloggen. De callback functie die meegegeven wordt aan `.finally()`, wordt aangeroepen zodra de Promise succesvol is (na de `.then`) of als er een fout opgetreden is (na de `.catch`).

### Wijzigen MainLayout.vue

Er wordt een `header` toegevoegd aan de `MainLayout.vue`-pagina. Deze balk/header zal altijd zichtbaar zijn op elke pagina, vandaar de toevoeging in de `MainLayout.vue`-pagina.

Hierin worden twee knoppen getoond, de `Login` en `Logout`.
Er wordt gebruikgemaakt van de `v-if` directive en de `v-else` directive. Een alternatief is om twee keer een `v-if` directive te gebruiken, één keer met `v-if="!isAuthenticated"` en één keer met `v-if="isAuthenticated"`. De `isAuthenticated` is de computed property afkomstig uit de service, deze zal `true` zijn als de gebruiker is ingelogd en `false` als de gebruiker niet is ingelogd.

```html
<template>
  <div class="header">
    <StyledButton v-if="!isAuthenticated" @click="navigate(ROUTE_NAMES.LOGIN)">
      Login
    </StyledButton>
    <StyledButton v-else @click="logout()">Logout</StyledButton>
  </div>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>
    <div class="button-container">
      <StyledButton @click="navigate(ROUTE_NAMES.DASHBOARD)">
        Dashboard
      </StyledButton>
      <StyledButton @click="navigate(ROUTE_NAMES.HERO_LIST)">
        Heroes
      </StyledButton>
    </div>

    <router-view></router-view>
  </div>
</template>
```

In de `script`-tag wordt er gebruik gemaakt van de `lifecycle hook` genaamd `onBeforeMount`. De code in de callback functie die wordt toegekend als parameter van deze hook wordt uitgevoerd net voordat de component gemount wordt. Dus net voordat de component wordt gemount (wordt toegevoegd aan de DOM), wordt de functie `tryToAuthenticate` aangeroepen, dit is ook functionaliteit die voorzien wordt in de auth service.

```html
<script lang="ts">
  import { defineComponent, onBeforeMount } from "vue";
  import { useRouter } from "vue-router";
  import { ROUTE_NAMES } from "../router/routes";
  import StyledButton from "src/components/StyledButton.vue";
  import { useAuth } from "src/services/auth.service";

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const router = useRouter();
      const { tryToAuthenticate, isAuthenticated, logout } = useAuth();

      onBeforeMount(() => {
        tryToAuthenticate();
      });

      const navigate = (name: string) => {
        // void router.push({ name: 'Dashboard' }); Navigate to Dashboard
        // void router.push({ name: 'HeroList' }); Navigate to HeroList
        void router.push({ name });
      };

      return {
        navigate,
        ROUTE_NAMES,
        isAuthenticated,
        logout,
      };
    },
  });
</script>
```

De styling van de header is toegevoegd.

```html
<style lang="scss" scoped>
  .layout-container {
    margin: 2rem;
  }

  .button-container {
    display: flex;
    gap: 0.25rem;
  }

  .header {
    display: flex;
    justify-content: flex-end;
    padding: 0.5rem;
    background-color: #333;
  }
</style>
```

### De authenticatie uittesten

Start de applicatie op. Indien er al een `accessToken` aanwezig is, wordt er een poging gedaan om de gebruiker te authentificeren. Indien de token vervallen is, wordt de token verwijderd.

Indien de token geldig was, zal de gebruiker worden geauthentificeerd, als dit gelukt is zal er rechtsboven een knop `Logout` te zien zijn.

Indien de token niet geldig was, zal de gebruiker niet ingelogd zijn, als dit het geval is zal er rechtsboven een knop `Login` te zien zijn.

Indien je ingelogd bent, klik op `Logout`.

Klik op `Login`, dit zal de gebruiker naar de login pagina navigeren.

Vul hier een geldige login van Code Coaching in en klik op `Login` naast de invoervelden.

Indien dit gelukt is zal er een alert verschijnen dat het gelukt is, de knop zal automatisch wijzigen van `Login` naar `Logout`. Dit betekent ook dat overal waar de auth service gebruikt wordt, de `isAuthenticated` computed property ter beschikking is om te bepalen of een gebruiker ingelogd is of niet.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d4a6dbc1da66786732ca59bcea0e97ae9a87490f)

## Login pagina stylen

De `Login.vue`-pagina ziet er momenteel een beetje raar uit. Voeg wat styling toe om het er beter uit te laten zien.

Maak gebruik van `StyledButton` i.p.v. het normale `button`-element. Plaats wrapper elementen/container elementen om de plaatsing van de inputvelden en de knop te verbeteren.

```html
<template>
  <div class="login-container">
    <div class="login-fields">
      <input type="email" v-model="user.email" />
      <input type="password" v-model="user.password" />
    </div>
    <StyledButton @click="login()">Login</StyledButton>
  </div>
</template>

<script lang="ts">
  import { defineComponent, reactive } from "vue";
  import { useAuth } from "src/services/auth.service";
  import StyledButton from "src/components/StyledButton.vue";

  export default defineComponent({
    components: { StyledButton },
    setup() {
      const { login: authenticate } = useAuth();
      const user = reactive({
        email: "",
        password: "",
      });
      const login = () => {
        authenticate(user.email, user.password).finally(() => {
          user.email = "";
          user.password = "";
        });
      };
      return {
        login,
        user,
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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/0b03ce0068d659f1b4a485a03c8116b3e3546146)

## User model

Momenteel zit de `interface` van de `User` in de auth service. Dit wordt ook wel het `model van de User` genoemd.

### User model definiëren

Aangezien enkel de `_id` property gebruikt wordt, is het voldoende om een interface te hebben met enkel deze property. Maar er bestaan veel meer properties op het User-object dat terugkomt van de server.

Log opnieuw in op de applicatie en bekijk in de console naar het `User`-object dat uitgelogd is.

Een voorbeeld van het `User`-object:

```js
{
  "_id": "618d9b7d2763cb4c3552fb69",
  "email": "john@duck.com",
  "permissions": [
    "vip"
  ],
  "verifyExpires": null,
  "verifyToken": null,
  "isVerified": true,
  "createdAt": "2021-11-11T22:38:53.147Z",
  "updatedAt": "2022-05-11T12:42:29.214Z",
  "__v": 0,
  "preferences": {
    "lang": "nl-NL",
    "themeSettings": {
      "dark": {
        "isDark": true,
        "name": "SYNTHWAVE",
        "colors": {
          "primary": "#ff7edb",
          "secondary": "#b893ce8f",
          "accent": "#9c27b0",
          "positive": "#20615b",
          "negative": "#a21232",
          "info": "#3e35a8",
          "warning": "#c1a54d",
          "background": "#262335",
          "text": "#ffd9f4",
          "sidebar": "#241b2f"
        }
      },
      "light": {
        "isDark": false,
        "name": "QUASAR",
        "colors": {
          "primary": "#1976d2",
          "secondary": "#26a69a",
          "accent": "#9c27b0",
          "positive": "#21ba45",
          "negative": "#c10015",
          "info": "#31ccec",
          "warning": "#f2c037",
          "background": "#fff",
          "text": "#121212",
          "sidebar": "#fff"
        }
      },
      "darkMode": "DARK"
    }
  },
  "resetExpires": null,
  "resetToken": null,
  "profileImage": {
    "url": "uploads/618d9b7d2763cb4c3552fb8f/profile.png"
  },
  "bannerImage": {
    "url": "uploads/618d9b7d2763cb4c3552fb8f/banner.png"
  },
  "oauth": {
    "github": {
      "verified": true,
      "id": "43420049",
      "login": "bartduisters",
      "token": "gho_M9qnNaO7XOqFuh8lWTKxEBoTMdARun2BZAaa"
    },
    "discord": {
      "verified": true,
      "username": "Bart Duisters",
      "discriminator": "6969",
      "id": "330377159174520869"
    }
  },
  "name": {
    "firstName": "John",
    "lastName": "Duck",
    "showName": true
  }
}
```

Aangezien enkel de `_id` gebruikt wordt momenteel, is het niet nodig om extra properties al toe te voegen aan de interface. Maar stel dat de applicatie uitgebreid wordt en dat er ook gebruik gemaakt gaat worden van de `email`, de `name`, de `permissions` etc. dan kunnen deze toegevoegd worden aan de interface. Idealiter is er API-documentatie waar terug te vinden is wat de mogelijke properties zijn en wat de mogelijke waarden zijn die aan de properties kunnen worden toegewezen. Indien deze API-documentatie niet beschikbaar is, dan kunnen deze properties en waarden afgeleid worden uit de respons.

```ts
interface User {
  _id: string;
  email: string;
  permissions: Array<string>;
  isVerified: boolean;
  name: {
    firstName: string;
    lastName: string;
    showName: boolean;
  };
}
```

Belangrijk als frontender om te weten, dingen zoals `permissions` en `isVerified` zijn beschikbaar. Maar **alle** data die beschikbaar is in de frontend, is beschikbaar voor de gebruiker in de browser en kan dus gewijzigd worden. Het is oké om deze data in de frontend te gebruiken om conditioneel bepaalde routes wel of niet te tonen of om bepaalde niet-belangrijke content wel of niet te tonen. Maar stel dat er bepaalde content is die enkel gezien mag worden door mensen die én geverifieerd zijn én de permission `vip` hebben. Dan zal deze content altijd opgehaald moeten worden via de backend en de backend zal apart de user moeten ophalen en verifiëren dat deze de juiste rechten heeft.

Een voorbeeldje, stel dat er een vip area is, die enkel te zien is door mensen die `vip` als permission hebben. Dan is het mogelijk om het user object te manipuleren in de frontend/browser, waardoor de vip area zichtbaar wordt voor iemand die de permission niet heeft in de database. Dit is niet wat een gewone gebruiker zal doen, maar dit is wel iets dat een developer of iemand met slechte bedoeling kan doen. Hier kan je niks aan wijzigen. Wat is dan wel belangrijk? Stel dat er in de vip area betaalde tutorials te zien zijn. Dan is het belangrijk dat deze betaalde tutorials alleen opgehaald en getoond worden, als de authenticated user ook effectief `vip` heeft in de database.

Op de backend zal dus het volgende gedaan moeten worden:

- de `accessToken` wordt uitgelezen
- de user gegevens uit de database worden opgehaald
- de data uit de database zal gebruikt worden om te kijken dat `vip` aanwezig is in de `permissions` array

In het kort: **Vertrouw nooit op de data die aanwezig is in de frontend!**

### User model verplaatsen

Aangezien het `User` model hoogst waarschijnlijk op meerdere plaatsen gebruikt zal worden, wordt deze alvast verplaatst naar `src/components/models.ts`.

```ts
export interface User {
  _id: string;
  email: string;
  permissions: Array<string>;
  isVerified: boolean;
  name: {
    firstName: string;
    lastName: string;
    showName: boolean;
  };
}
```

Merk op: `export` - zodat deze op andere locaties gebruikt kan worden via `import`.

Bovenaan in `auth.service.ts`

```ts
import { User } from "src/components/models";
```

Verwijder de `console.log` die niet langer nodig is uit de `.then`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/ac8a284dedc66445a8c2a12596e94a7b9854074d)

## Hero service koppelen met API

Tot nu toe werd er hard-coded data gebruikt in de `Hero service`. Maar, met de account van Code Coaching is het mogelijk om de `Hero` data te beheren op je account, via een API.

### Uitleg werking van API

Hoe de API werkt. Elke API call naar `/heroes` heeft authenticatie nodig.

Indien de `Authorization token` verlopen is, zal dit de response zijn:

```json
{
  "name": "NotAuthenticated",
  "message": "jwt expired",
  "code": 401,
  "className": "not-authenticated",
  "data": {
    "name": "TokenExpiredError",
    "message": "jwt expired",
    "expiredAt": "2022-04-03T18:11:58.000Z"
  },
  "errors": {}
}
```

Indien de `Authorization token` niet aanwezig is, zal dit de response zijn:

```json
{
  "name": "NotAuthenticated",
  "message": "Not authenticated",
  "code": 401,
  "className": "not-authenticated",
  "errors": {}
}
```

Ervan uitgaande dat de `Authorization token` correct is, zal dit de response zijn van volgende API calls:

#### GET (opvragen)

Ophalen van alle Heroes.

`HTTP GET https://api.code-coaching.dev/heroes`

De respons:

```json
{
  "total": 0,
  "limit": 10,
  "skip": 0,
  "data": []
}
```

Deze gebruiker heeft momenteel geen heroes.

Ophalen van een specifieke Hero.

Probeer na het aanmaken (POST) van een hero onderstaande API call te doen:

`HTTP GET https://api.code-coaching.dev/heroes/:id`
Waarbij `:id` de `_id` van de hero is.
Bij het voorbeeld dat aangemaakt wordt bij de `POST` hieronder, is dit:
`HTTP GET https://api.code-coaching.dev/heroes/62882f786f3b143ad3a37558`

De respons:

```json
{
  "_id": "628832a96f3b143ad3a37568",
  "belongsTo": "61c1078285ca9a7473b14aac",
  "id": "69",
  "name": "Superwoman",
  "createdAt": "2022-05-21T00:30:33.261Z",
  "updatedAt": "2022-05-21T00:30:33.261Z",
  "__v": 0
}
```

#### POST (aanmaken)

`HTTP POST https://api.code-coaching.dev/heroes`

De body die meegestuurd wordt:

```json
{
  "name": "Superman",
  "id": 69
}
```

De respons:

```json
{
  "_id": "62882f786f3b143ad3a37558",
  "belongsTo": "61c1078285ca9a7473b14aac",
  "id": "69",
  "name": "Superman",
  "createdAt": "2022-05-21T00:16:56.587Z",
  "updatedAt": "2022-05-21T00:16:56.587Z",
  "__v": 0
}
```

Een `Hero` met dezelfde `name` en dezelfde `id` aanmaken is mogelijk, maar `_id` zal verschillen, hierdoor is elke Hero, zelfs die met dezelfde `name` en dezelfde `id`, toch uniek.

Merk op: er wordt `69` als nummer meegestuurd in de body, maar de waarde die terugkomt is `"69"`, een string.

Merk op: het model van de backend is verschillend van het model in de frontend.

Huidige frontend model:

```ts
interface Hero {
  name: string;
  number: number;
}
```

Model van de backend:

```ts
interface Hero {
  _id: string;
  id: string;
  name: string;
  belongsTo: string;
  createdAt: string;
  updatedAt: string;
  __v: number;
}
```

Dit lijkt misschien een raar model door `_id` en `id`, maar `_id` is een unieke identifier die wordt toegekend door de backend. `id` is een nummer dat gekozen wordt door de gebruiker tijdens het aanmaken van een hero via de frontend, dit is wat overeenkomt met `number` in het frontend model. Het type van de property `number` is `number` en het type van `id` is `string`. Dit zal gerefactord moeten worden in de frontend (later in deze tutorial). `id` is **niet uniek**, `_id` is **wel uniek**.

`belongsTo` is de `_id` van de gebruiker die de hero heeft aangemaakt.

`createdAt` is een timestamp in stringformaat van de tijd waarop de hero is aangemaakt.
`updatedAt` is een timestamp in stringformaat van de tijd waarop de hero het laatst is aangepast.

`__v` is een versienummer van MongoDB, de databasetechnologie die gebruikt wordt.

#### PATCH (wijzigen)

Vaak wordt PUT gebruikt om data te wijzigen, maar hierbij wordt het volledige object overschreven.
Met PATCH is het mogelijk om specifieke properties te wijzigen.

`HTTP PATCH https://api.code-coaching.dev/heroes/:id`

Waarbij `:id` de `_id` van de hero is die gewijzigd moet worden., bijvoorbeeld ervan uitgaande dat de naam van de hero die aangemaakt is in de POST call gewijzigd moet worden:
`HTTP PATCH https://api.code-coaching.dev/heroes/62882f786f3b143ad3a37558`

De body die meegestuurd wordt:

```json
{
  "name": "Superwoman"
}
```

De respons:

```json
{
  "_id": "62882f786f3b143ad3a37558",
  "belongsTo": "61c1078285ca9a7473b14aac",
  "id": "69",
  "name": "Superwoman",
  "createdAt": "2022-05-21T00:16:56.587Z",
  "updatedAt": "2022-05-21T00:24:01.908Z",
  "__v": 0
}
```

Merk op: De `name` is gewijzigd naar `Superwoman` en automatisch is `updatedAt` ook gewijzigd, voor de rest is alles identiek.

#### DELETE (verwijderen)

`HTTP DELETE https://api.code-coaching.dev/heroes/:id`

Waarbij `:id` de `_id` van de hero is die gewijzigd moet worden., bijvoorbeeld ervan uitgaande dat de naam van de hero die aangemaakt is in de POST call gewijzigd moet worden:
`HTTP DELETE https://api.code-coaching.dev/heroes/62882f786f3b143ad3a37558`

De respons:

```json
{
  "_id": "62882f786f3b143ad3a37558",
  "belongsTo": "61c1078285ca9a7473b14aac",
  "id": "69",
  "name": "Superwoman",
  "createdAt": "2022-05-21T00:16:56.587Z",
  "updatedAt": "2022-05-21T00:24:01.908Z",
  "__v": 0
}
```

### Koppelen API in hero.service.ts

Aangezien het nu mogelijk is om de heroes te `persisteren` (permanent op te slaan), is dit exact wat er gaat gebeuren!

De lijst van heroes die nu hard-coded staat, zal vervangen worden door een lijst van heroes die opgehaald wordt uit een database via de API.

Een hero wijzigen, zal de hero in de database wijzigen.

Een hero verwijderen, zal effectief de hero verwijderen uit de database.

Een hero toevoegen, zal een nieuwe hero toevoegen aan de database.

#### Lijst van heroes ophalen

Momenteel is er een array met hard-coded heroes.

```ts
const heroes = ref([
  { number: 11, name: "Mr. Nice" },
  { number: 12, name: "Narco" },
  { number: 13, name: "Bombasto" },
  { number: 14, name: "Celeritas" },
  { number: 15, name: "Magneta" },
  { number: 16, name: "RubberMan" },
  { number: 17, name: "Dynama" },
  { number: 18, name: "Dr IQ" },
  { number: 19, name: "Magma" },
  { number: 20, name: "Tornado" },
]);
```

Er zal een functie toegevoegd worden aan de `hero.service.ts` om de lijst van heroes op te halen.

Er zijn twee opties om met Promises te werken, één optie is om met `.then` te werken.

Dit kan gelezen worden als 'de code belooft dat de lijst van heroes opgehaald zal worden, als de data terugkomt, **dan** (`.then`) wordt het codeblok van de callback functie uitgevoerd'.

```ts
const getHeroes = () => {
  void api.get("/heroes").then((result: { data: Array<Hero> }) => {
    console.log("result", result);
    heroes.value = result.data;
  });
};
```

Een tweede manier is om met `async/await` te werken.

De functie krijgt het `async` keyword en in het codeblok van de functie kan vervolgens `await` gebruikt worden.

Dit kan gelezen worden als 'ik wacht met het uitvoeren van de rest van de code tot de data terug is gekomen'.

```ts
const getHeroes = async () => {
  const result = await api.get("/heroes");
  console.log("result", result);
  heroes.value = result.data as Array<Hero>;
};
```

Voeg deze functie toe aan het return statement van de `useHeroes` functie en gebruik deze in `MainLayout.vue`.

`MainLayout.vue` script tag

```ts
import { defineComponent, onBeforeMount } from "vue";
import { useRouter } from "vue-router";
import { ROUTE_NAMES } from "../router/routes";
import StyledButton from "src/components/StyledButton.vue";
import { useAuth } from "src/services/auth.service";
import { useHeroes } from "src/services/hero.service";

export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const router = useRouter();
    const { tryToAuthenticate, isAuthenticated, logout } = useAuth();
    const { getHeroes } = useHeroes();

    onBeforeMount(() => {
      tryToAuthenticate();
      void getHeroes();
    });

    const navigate = (name: string) => {
      // void router.push({ name: 'Dashboard' }); Navigate to Dashboard
      // void router.push({ name: 'HeroList' }); Navigate to HeroList
      void router.push({ name });
    };

    return {
      navigate,
      ROUTE_NAMES,
      isAuthenticated,
      logout,
    };
  },
});
```

Start de applicatie en log in, refresh, kijk naar de console of naar de netwerktab. De `getHeroes`-functie wordt aangeroepen zodra de applicatie opstart (`onBeforeMount` in `MainLayout.vue`). Dit zal een HTTP call doen naar de server, maar er zal geen resultaat teruggegeven worden.

Resultaat in de netwerktab van de `/heroes` call.

```json
{
  "name": "NotAuthenticated",
  "message": "Not authenticated",
  "code": 401,
  "className": "not-authenticated",
  "errors": {}
}
```

#### Authorization header toevoegen

De `getHeroes`-functie heeft de `accessToken` uit Local Storage nodig om de API te kunnen aanroepen met authentificatie.

```ts
const getHeroes = async () => {
  const accessToken = localStorage.getItem("accessToken") || "";

  const result = await api.get("/heroes", {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  });

  console.log("result", result);
};
```

`const accessToken = localStorage.getItem("accessToken") || "";` de syntax `|| ""` zorgt ervoor dat er een lege string wordt toegekend als de `accessToken` niet bestaat.

Zorg dat je ingelogd bent, refresh en kijk opnieuw naar de console of de netwerktab. Nu komt er wél data terug. De respons zal er voor iedereen anders uitzien.

```json
{
  "total": 1,
  "limit": 10,
  "skip": 0,
  "data": [
    {
      "_id": "628832a96f3b143ad3a37568",
      "belongsTo": "61c1078285ca9a7473b14aac",
      "id": "69",
      "name": "Superwoman",
      "createdAt": "2022-05-21T00:30:33.261Z",
      "updatedAt": "2022-05-21T00:30:33.261Z",
      "__v": 0
    }
  ]
}
```

Uit de respons is af te leiden dat paginatie beschikbaar is. Dit wordt in deze tutorial niet besproken. Maar het is belangrijk om te weten dat er maximum 10 heroes teruggegeven worden (`"limit": 10`). Indien er meer dan 10 heroes zijn aangemaakt in de applicatie, verwijder dan een paar heroes.

#### TypeScript axios

Aangezien er met TypeScript gewerkt wordt en omdat het gewenst is om auto-completion te krijgen op het resultaat, is het nodig om de typings toe te voegen.

```ts
const getHeroes = async () => {
  const accessToken = localStorage.getItem("accessToken") || "";

  const result = await api.get<Paged<Hero>>("/heroes", {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  });

  console.log(result);
};
```

Het is mogelijk om het type van de response toe te voegen aan de api call.

```ts
api.get<Paged<Hero>>();
```

Het type omschrijft het type van de waarde die toegekend zal worden aan de `data` property van de axios response.

`Paged` is een interface die toegevoegd is bovenaan de `hero.service.ts` file.

```ts
interface Paged<T> {
  total: number;
  limit: number;
  skip: number;
  data: Array<T>;
}
```

Hier wordt gebruik gemaakt van een `Generic` (`<T>`).
Enkele voorbeelden ter verduidelijking:

```ts
const exampleString: Paged<string>;
```

Dit betekent dat de `data` property een array van strings zal bevatten.

```ts
const exampleHero: Paged<Hero>;
```

Dit betekent dat de `data` property een array van `Hero`-objecten zal bevatten.

Het resultaat van de respons is een `AxiosResponse`, dit is op zich ook een generic, onderstaande typing maakt duidelijk wat het eigenlijke type is van de respons.

```ts
const result: AxiosResponse<Paged<Hero>>;
```

Dit betekent dat de `data` property van de AxiosResponse van het type `Paged` zal zijn. `Paged<Hero>` betekent dat de `data` property van de paged respons een array van `Hero`-objecten zal bevatten.

Bijvoorbeeld:

```json
{
  "config": {},
  "headers": {},
  "request": {},
  "status": 200,
  "statusText": "OK",
  "data": {
    "total": 1,
    "limit": 10,
    "skip": 0,
    "data": [
      {
        "_id": "628832a96f3b143ad3a37568",
        "belongsTo": "61c1078285ca9a7473b14aac",
        "id": "69",
        "name": "Superwoman",
        "createdAt": "2022-05-21T00:30:33.261Z",
        "updatedAt": "2022-05-21T00:30:33.261Z",
        "__v": 0
      }
    ]
  }
}
```

Merk op: `config`, `headers` en `request` zijn normaal objecten met properties, deze zijn leeg gelaten om het voorbeeld korter te houden.

Dit betekent, dat de array met heroes bekomen kan worden door `result.data.data`.
Ken de array toe aan de `ref`-variabele `heroes`.

```ts
const getHeroes = async () => {
  const accessToken = localStorage.getItem("accessToken") || "";

  const result = await api.get<Paged<Hero>>("/heroes", {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  });
  console.log(result.data.data);

  heroes.value = result.data.data;
};
```

Bevestig in de console dat de array wordt uitgeprint. Indien er nog geen heroes zijn aangemaakt, zal de array leeg zijn.

Deze wijziging heeft enkele bugs geïntroduceerd. Om deze bugs te tonen, zal eerst de `addHero`-functie gewijzigd worden.

#### Hero toevoegen

```ts
const addHero = (name: string) => {
  let maxNumber = Math.max(...heroes.value.map((h) => h.number));
  if (maxNumber === -Infinity) maxNumber = 0;

  const newHero = { number: maxNumber + 1, name };

  const accessToken = localStorage.getItem("accessToken") || "";

  heroes.value.push(newHero);

  api
    .post<Hero>(
      "/heroes",
      {
        ...newHero,
        id: newHero.number,
      },
      {
        headers: {
          Authorization: `Bearer ${accessToken}`,
        },
      }
    )
    .then((result) => {
      console.log(result);
    })
    .catch((error) => {
      console.log(error);
      alert("Error adding hero");
      const index = heroes.value.findIndex((e) => e === newHero);
      heroes.value.splice(index, 1);
    });
};
```

Er wordt gebruik gemaakt van een techniek genaamd `naïve update` om te zorgen dat de gebruiker meteen de nieuwe hero kan zien. Indien er iets misgaat, waardoor de hero niet aan de database is toegevoegd, dan zal deze naïve update teruggedraaid worden en de gebruiker wordt op de hoogte gebracht dat er iets mis is gegaan via een alert.

Merk op dat er een wijziging is gebeurd:

```ts
const maxNumber = Math.max(...heroes.value.map((h) => h.number));
```

```ts
let maxNumber = Math.max(...heroes.value.map((h) => h.number));
if (maxNumber === -Infinity) maxNumber = 0;
```

Voorheen was het niet mogelijk dat er géén heroes waren. Nu dit wel mogelijk is, zal `Math.max()` evalueren naar `-Infinity`. Dit wordt opgevangen door de regel:

```ts
if (maxNumber === -Infinity) maxNumber = 0;
```

Start de applicatie, zorg dat je ingelogd bent en voeg twee heroes toe.

Het lijkt te werken.

![Add Hero - werkende](/img/blog/quasar-toh-add-hero-working.png)

Maar er is nog een bug, refresh de browser.

![Add Hero - niet werkende](/img/blog/quasar-toh-add-hero-not-working.png)

Beide heroes lijken geselecteerd en het nummer wordt niet getoond.

```html
<div
  :class="{
    'hero--active': hero.number === selectedHero?.number,
  }"
  class="hero"
  v-for="(hero, index) in heroes"
  :key="index"
  @click="onClickHero(hero)"
>
  <span class="hero-number">{{ hero.number }}</span>
  <span class="hero-name">{{ hero.name }}</span>
</div>
```

Dit komt doordat de class `hero--active` wordt toegevoegd wanneer `hero.number === selectedHero?.number` evalueert naar `true`. Doordat `.number` geen property is op het model dat van de backend terugkomt, zal `.number` `undefined` zijn. Aangezien er nooit een hero geselecteerd is, zal `selectedHero?.number` ook `undefined` zijn.

Om dit op te lossen, zal het model dat terugkomt van de backend, omgevormd moeten worden naar het model dat in de frontend gebruikt wordt.

Voeg een interface toe aan `src/components/models.ts`:

```ts
export interface BackendHero {
  _id: string;
  id: string;
  name: string;
}
```

Gebruik deze interface i.p.v. de interface `Hero` bij de API calls.

```ts
const getHeroes = async () => {
  const accessToken = localStorage.getItem("accessToken") || "";

  const result = await api.get<Paged<BackendHero>>("/heroes", {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  });
  console.log(result.data.data);

  heroes.value = result.data.data.map((hero: BackendHero): Hero => {
    return {
      ...hero,
      number: hero.id,
    };
  });
};
```

De `.map()` itereert over de array van `BackendHero`-objecten en vormt elk object om naar het `Hero`-object dat gebruikt wordt in de frontend.

Dit geeft een TypeScript error.

```sh
ERROR in src/services/hero.service.ts:45:9
TS2322: Type 'string' is not assignable to type 'number'.
    43 |       return {
    44 |         ...hero,
  > 45 |         number: hero.id
       |         ^^^^^^
    46 |       }
    47 |     });
    48 |   }
```

Bij het koppelen van de backend wordt duidelijk dat het frontendmodel niet overeenkomt met de werkelijkheid. Refactor het frontendmodel.

```ts
export interface Hero {
  number: string;
  name: string;
}
```

Bij het compileren van de applicatie worden er nu een heel reeks andere errors opgegooid.

Wijzig het type en de initialisatie van de heroes ref.

```ts
const heroes: Ref<Array<Hero>> = ref([]);
```

Wijzig het type van de parameter `findHero` en verwijder de type casting naar number.

Voor de wijziging:

```ts
const findHero = (id: number): Hero | undefined => {
  const matchingHero = heroes.value.find((h) => h.number === +id);
  if (matchingHero) return { ...matchingHero };
};
```

Na de wijziging:

```ts
const findHero = (id: string): Hero | undefined => {
  const matchingHero = heroes.value.find((h) => h.number === id);
  if (matchingHero) return { ...matchingHero };
};
```

Voeg type casting toe in het codeblok van de `addHero` functie.

Voor de wijziging:

```ts
let maxNumber = Math.max(...heroes.value.map((h) => h.number));
if (maxNumber === -Infinity) maxNumber = 0;

const newHero = { number: maxNumber + 1, name };
```

Na de wijziging:

```ts
let maxNumber = Math.max(...heroes.value.map((h) => +h.number));
if (maxNumber === -Infinity) maxNumber = 0;

const number = (maxNumber + 1).toString();
const newHero = { number, name };
```

#### Hero verwijderen

Uit de uitleg van de werking van de API is gekend dat er met `_id` gewerkt moet worden om een hero te verwijderen. Voeg dit toe aan het frontend model.

```ts
export interface Hero {
  _id?: string;
  number: string;
  name: string;
}
```

Properties met een `?` zijn optionele properties. Aangezien `_id` niet gekend is wanneer een naïve update gedaan wordt in `addHero`, moet deze property nog toegevoegd worden wanneer `addHero` succesvol is uitgevoerd.

Voor de wijziging:

```ts
heroes.value.push(newHero);

api
  .post<BackendHero>(
    "/heroes",
    {
      ...newHero,
      id: newHero.number,
    },
    {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    }
  )
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(error);
    alert("Error adding hero");
    const index = heroes.value.findIndex((e) => e === newHero);
    heroes.value.splice(index, 1);
  });
```

Na de wijziging:

```ts
heroes.value.push(newHero);
const index = heroes.value.length - 1;

api
  .post<BackendHero>(
    "/heroes",
    {
      ...newHero,
      id: newHero.number,
    },
    {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    }
  )
  .then((result) => {
    console.log(result);
    heroes.value[index] = { ...result.data, number: result.data.id };
  })
  .catch((error) => {
    console.log(error);
    alert("Error adding hero");
    heroes.value.splice(index, 1);
  });
```

API koppelen in `deleteHero`.

```ts
const deleteHero = (hero: Hero) => {
  if (hero._id) {
    const index = heroes.value.findIndex((h) => h._id === hero._id);
    heroes.value.splice(index, 1);

    const accessToken = localStorage.getItem("accessToken") || "";

    api
      .delete(`/heroes/${hero._id}`, {
        headers: {
          Authorization: `Bearer ${accessToken}`,
        },
      })
      .catch(() => {
        heroes.value.splice(index, 0, hero);
        alert("Error deleting hero");
      });
  }
};
```

Merk op dat er opnieuw gebruik gemaakt wordt van een `naïve update`. De hero wordt verwijderd voor de API call gedaan wordt, de index wordt bijgehouden. Indien de call niet lukt, zal de hero teruggeplaatst worden op dezelfde plaats en de gebruiker wordt geïnformeerd via een alert.

#### Hero updaten

```ts
const editHero = (hero: Hero) => {
  if (hero._id) {
    const accessToken = localStorage.getItem("accessToken") || "";

    const index = heroes.value.findIndex((h: Hero) => h._id === hero._id);
    const oldHero = heroes.value[index];
    heroes.value[index] = hero;

    api
      .delete(`/heroes/${hero._id}`, {
        headers: {
          Authorization: `Bearer ${accessToken}`,
        },
      })
      .catch(() => {
        heroes.value[index] = oldHero;
        alert("Error updating hero");
      });
  }
};
```

Opnieuw wordt er gebruik gemaakt van een `naïve update`. De oude hero wordt bijgehouden, indien de API call niet lukt, wordt de oude hero teruggeplaatst en de gebruiker wordt geïnformeerd via een alert.

Verwijder alle `console.log` statements uit de `hero.service.ts`.

Nog een laatste kleine wijziging in `HeroList.vue`, wijzig `'hero--active': hero.number === selectedHero.number,` naar`'hero--active': hero._id === selectedHero?._id,`. Aangezien het mogelijk is om een hero aan te maken met hetzelfde `number` (niet in deze applicatie, maar stel dat de API wordt aangeroepen via Postman of via een terminal), wordt nu de unieke `_id` gebruikt om te bepalen of een hero geselecteerd is.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/a42d394dd98865d0e1e2422a9b5202adc4d9b79e)

**BUG**: De bug wordt opgemerkt op het einde van de tutorial en wordt ook daar opgelost. In het kort: `api.delete` moet `api.patch` zijn. En als tweede parameter moet de `hero` meegegeven worden. De commit met de bugfix: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92e3f6614601db5bd23bb60cdf3397aede6c9aff)

### API calls refactoren

Bij elke call naar de API moet er een `headers` object meegegeven worden. Dit object bevat de `Authorization` header met de access token. Dit is momenteel geïmplementeerd in elke methode die een call naar de API doet.

Refactor dit naar een aparte functie bovenaan het bestand.

```ts
const getRequestConfig = (): AxiosRequestConfig => {
  const accessToken = localStorage.getItem("accessToken") || "";

  return {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  };
};
```

Gebruik deze functie in elke methode die een call naar de API doet. Verwijder de code die afgehandeld wordt door deze functie.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/9e1e5bde3b4ec94823677e80c25b4de261e048a2)

## UX improvements

### Login redirect

Doordat de volledige applicatie nu afhankelijk is van ingelogd zijn, is het een goed idee om de gebruiker te redirecten naar de login pagina indien de applicatie bezocht wordt en er nog geen gebruiker is ingelogd.

In `auth.service.ts`, refactor de `tryToAuthenticate`-functie:

```ts
const tryToAuthenticate = (): Promise<void> => {
  return new Promise((resolve, reject) => {
    const accessToken = localStorage.getItem("accessToken");
    if (accessToken) {
      api
        .post("/authentication", {
          strategy: "jwt",
          accessToken,
        })
        .then((result: { data: { accessToken: string; user: User } }) => {
          authenticatedUser.value = result.data.user;
          resolve();
        })
        .catch(() => {
          localStorage.removeItem("accessToken");
          authenticatedUser.value = {} as User;
          reject();
        });
    } else {
      reject();
    }
  });
};
```

Refactor in `MainLayout.vue`:

```ts
onBeforeMount(() => {
  tryToAuthenticate()
    .then(() => {
      void getHeroes();
    })
    .catch(() => {
      void router.push({ name: ROUTE_NAMES.LOGIN });
    });
});
```

Indien er geen gebruiker is ingelogd, of wanneer de token niet geldig is, zal de promise een `reject()` doen, dit wordt opgevangen in de `.catch()`.
Indien er wel een gebruiker is ingelogd, zal de promise een `resolve()` doen, dit wordt opgevangen in de `.then()`.

Wanneer de gebruiker is ingelogd, wordt de `getHeroes()`-functie aangeroepen om de lijst met heroes op te halen.

Wanneer de gebruiker niet is ingelogd, wordt de gebruiker doorgestuurd naar de loginpagina.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/52b52c88734a774f73b196d9fcca5c7a54570111)

### Dashboard redirect

Na het inloggen op de inlogpagina, moet de gebruiker doorgestuurd worden naar het dashboard.

Refactor de `login`-functie in `auth.service.ts`:

```ts
const login = (email: string, password: string): Promise<void> => {
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
          localStorage.setItem("accessToken", accessToken);
          authenticatedUser.value = result.data.user;
          resolve();
        } else {
          alert("Login failed");
          reject();
        }
      })
      .catch((error) => {
        console.log(error);
        alert("Login failed");
        reject();
      });
  });
};
```

Voeg de redirect toe in `Login.vue`:

```ts
const login = () => {
  authenticate(user.email, user.password)
    .then(() => {
      void router.push({ name: ROUTE_NAMES.DASHBOARD });
    })
    .finally(() => {
      user.email = "";
      user.password = "";
    });
};
```

De `router` en `ROUTE_NAMES` moeten geïmporteerd worden.

Voeg een `watch` toe aan `MainLayout.vue`:

```ts
watch(isAuthenticated, (newValue) => {
  if (newValue) {
    void getHeroes();
  } else {
    void router.push({ name: ROUTE_NAMES.LOGIN });
  }
});
```

De `watch` functie zal de property `isAuthenticated` in de gaten houden (`watchen`). Wanneer deze wijzigt wordt de `callback`-functie worden uitgevoerd, hier wordt de nieuwe waarde van de variabele doorgegeven als parameter.

Bij het uittesten van de applicatie kan opgemerkt worden dat na de redirect van het dashboard, de top heroes niet getoond worden. Om dit op te lossen is een `computed` property nodig.

In `hero.service.ts`:

Voor de wijziging:

```ts
const topHeroes = heroes.value.slice(0, 4);
```

Na de wijziging:

```ts
const topHeroes = computed(() => heroes.value.slice(0, 4));
```

`computed` wordt geïmporteerd via `vue`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/64f213936c79cd89ce3fe6ad89569585f6900145)

### Auth guard

Doordat de applicatie nu afhankelijk is van ingelogd zijn, is het mogelijk om de gebruiker naar de loginpagina te redirecten indien de gebruiker niet is ingelogd en toch een pagina opvraagt waar de gebruiker wél ingelogd moet zijn.

`MainLayout.ts`

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
  },
  { immediate: true }
);

// onBeforeMount(() => {
//   tryToAuthenticate()
//     .then(() => {
//       void getHeroes();
//     })
//     .catch(() => {
//       void router.push({ name: ROUTE_NAMES.LOGIN });
//     });
// });
```

De logica van `onBeforeMount` wordt verplaatst naar een `watch`. De `watch` houdt de property `route.path` in de gaten. Wanneer deze wijzigt wordt de `callback`-functie uitgevoerd. Wanneer de gebruiker nog niet is ingelogd, wordt de `tryToAuthenticate()`-functie aangeroepen. Merk ook de `{ immediate: true }`-parameter op, dit zorgt ervoor dat het ook meteen wordt uitgevoerd wanneer de applicatie wordt ingeladen, dit vervangt de `onBeforeMount`-functionaliteit.

In tegenstelling tot de `watch` die een ref-variabele in de gaten houdt (de `watch`-functie die `isAuthenticated` in de gaten houdt), is het nodig om `() => route.path` te gebruiken wanneer geneste properties in de gaten moeten gehouden worden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/627f1c75393a3ac46bd2514e962d3c0f048e5254)

## Bugfix

Bij het uittesten van alle functionaliteit, is er een bug op te merken.

- Voeg een hero toe
- Klik op de hero in de lijst
- Klik op 'details'
- Wijzig de naam
- Klik op `save`

Wat er verwacht wordt, is dat de hero wordt opgeslagen met de nieuwe naam.
Wat er effectief gebeurt, is dat de hero wordt verwijderd.

Bij het kijken naar de code die instaat voor het opslaan van de wijziging aan een hero, is het snel duidelijk:

`hero.service.ts`

```ts
const editHero = (hero: Hero) => {
  if (hero._id) {
    const index = heroes.value.findIndex((h: Hero) => h._id === hero._id);
    const oldHero = heroes.value[index];
    heroes.value[index] = hero;

    api.delete(`/heroes/${hero._id}`, getRequestConfig()).catch(() => {
      heroes.value[index] = oldHero;
    });
  }
};
```

Er staat `api.delete()`, dit is exact wat er gebeurt. Wat er moet gebeuren is `api.patch()`. Bij `patch` is het ook nodig om het nieuw object mee te sturen.

```ts
const editHero = (hero: Hero) => {
  if (hero._id) {
    const index = heroes.value.findIndex((h: Hero) => h._id === hero._id);
    const oldHero = heroes.value[index];
    heroes.value[index] = hero;

    api.patch(`/heroes/${hero._id}`, hero, getRequestConfig()).catch(() => {
      heroes.value[index] = oldHero;
    });
  }
};
```

De les die hier uit geleerd kan worden, is dat het uiterst belangrijk is om elke functionaliteit te testen na implementeren. Het is ook belangrijk om te begrijpen dat mensen fouten maken, bugs zijn onderdeel van development. In dit geval kwam de bug naar voren tijdens `QA` (**Q**uality **A**ssurance).

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92e3f6614601db5bd23bb60cdf3397aede6c9aff)

## Conclusie

Er komt meer bij kijken wanneer de data beheerd wordt op een externe locatie. In deze tutorial wordt aangeleerd hoe een API gekoppeld kan worden aan een Quasar-applicatie met Axios. Doordat de externe locatie ook authenticatie nodig heeft, is er gekeken naar de mogelijkheid om een gebruiker in te loggen.

Composables/Services verzamelen alle functionaliteit rondom bepaalde data. Bijvoorbeeld, alle acties die een gebruiker kan uitvoeren op Hero data steekt in de `hero.service.ts`-file. Doordat deze functionaliteit in de `hero.service.ts`-file staat, is het relatief makkelijk om de implementatie te wijzigen. In deze tutorial is zijn de functies gewijzigd naar functies die de wijzigingen doen via de API. Er is ook gekeken naar `naïve updates` om de data direct te wijzigen voor de gebruiker.
