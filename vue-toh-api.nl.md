---
postUuid: 246bc7b0-3031-4c76-bffd-673b0965d4dd
title: Vue Tour of Heroes - API
slug: vue-toh-api
tags:
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een service. De eindsituatie is een Vue-project waarin de data gekoppeld zit aan een API en waar authenticatie nodig is.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-service-functions.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/vue-toh-services)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-api-finished.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/vue-tour-of-heroes/releases/tag/vue-toh-api)

## API

Een `API` (Application Programming Interface) is een interface waarmee een applicatie kan communiceren met een andere applicatie. In dit geval, zal de frontendapplicatie communiceren met de backendapplicatie.

De backendapplicatie wordt voorzien door Code Coaching. Registreer een gratis account op [https://code-coaching.dev](https://code-coaching.dev/registreren), deze account zal gebruikt worden om in te loggen en om de data van de heroes te beheren op de server.

## Axios

Standaard is de `Fetch API` beschikbaar in de browser. Deze functionaliteit kan gebruikt worden om data op te halen van een API.

Axios is een library die een aantal extra functionaliteiten toevoegt ten opzichte van de `Fetch API`. Deze library kan gebruikt worden om data op te halen van een API.

In deze tutorial zal Axios gebruikt worden om data op te halen van de API.

## Axios installeren

Axios kan geïnstalleerd worden via `npm`.

```sh
npm install axios
```

## Inloggen via API

Code Coaching voorziet een API om in te loggen. Deze end point is beschikbaar op `https://code-coaching.dev/api/token/login`.

```
POST https://code-coaching.dev/api/token/login
{
  "email": "john@duck.com",
  "password": "hier-jouw-wachtwoord'
}
```

Dit is een `POST` request, de data wordt meegegeven in de body van de request.

Dit kan getest worden via Postman.

## Inlogpagina

Voeg een nieuwe pagina toe aan de applicatie, de `LoginView`-pagina.

```sh
touch src/views/LoginView.vue
```

`LoginView.vue`

```html
<script setup lang="ts">
  import axios from "axios";

  const login = () => {
    axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: "john@duck.com", // wijzig naar jouw e-mailadres
        password: "hier-jouw-wachtwoord", // wijzig naar jouw wachtwoord
      })
      .then((res) => {
        /**
         * Axios zal de data van de respons toekennen aan een property met de naam "data".
         */
        console.log(res); // volledige respons
        console.log(res.data); // data van de respons, dit zal { token: string } bevatten
      });
  };
</script>

<template>
  <button @click="login()">Login</button>
</template>
```

Let op: In deze code worden je e-mailadres en wachtwoord hard-coded in de code toegevoegd. **NIET** committen.

Wanneer op de knop `Login` wordt geklikt, dan wordt de `login()` functie aangeroepen. Deze functie zal een `post` request doen naar de API, naar het `/api/token/login` endpoint.

De data die meegestuurd wordt als body van de request is:

```json
{
  "email": "john@duck.com", // wijzig naar jouw e-mailadres
  "password": "hier-jouw-wachtwoord" // wijzig naar jouw wachtwoord
}
```

`axios.post()` is een `Promise`. Een `Promise` is een object met een `.then`-functie. De `.then` wordt pas uitgevoerd wanneer de data arriveert van de API.

Dit end point zal een object terugsturen dat er als volgt uitziet:

```json
{
  "token": "400|915wnnNpHQuCcQlTDvCC50EX0lMMmjWjIin6QoGc0a225b8d"
}
```

Dit is ook het type dat we meegeven bij `axios.post<{ token: string }>()`.

Belangrijk! Deze token stelt jouw ingelogde gebruiker voor. Net zoals met een wachtwoord is het belangrijk om deze token **niet** te delen met anderen.

Toevoegen van de pagina aan de `src/router/index.ts` file:

```ts
import { createRouter, createWebHashHistory } from "vue-router";

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: "/",
      name: "MainLayout",
      component: () => import("@/layouts/MainLayout.vue"),
      children: [
        {
          path: "",
          name: "dashboard",
          component: () => import("@/views/DashboardView.vue"),
        },
        {
          path: "heroes",
          name: "hero-list",
          component: () => import("@/views/HeroListView.vue"),
        },
        {
          path: "heroes/add",
          name: "hero-add",
          component: () => import("@/views/HeroAddView.vue"),
        },
        {
          path: "heroes/:id",
          name: "hero-details",
          component: () => import("@/views/HeroDetailsView.vue"),
        },
        {
          path: "login",
          name: "login",
          component: () => import("@/views/LoginView.vue"),
        },
      ],
    },
  ],
});

export default router;
```

Ga naar `/login` en klik op de knop `Login`. Open de developer tools en bekijk de console. Hierin staat de data die teruggestuurd wordt van de API. Je zal een object zien met een `token` property en met een `user` property. De `user` property wordt momenteel niet gebruikt.

### Formulier voor inloggen

In plaats van het e-mailadres en het wachtwoord hard-coded in de code te zetten, kan er een formulier gemaakt worden waarin de gebruiker een e-mailadres en wachtwoord kan invullen.

`LoginView.vue`

```html
<script setup lang="ts">
  import axios from "axios";
  import { ref } from "vue";

  const email = ref(""); // maak een ref aan voor het e-mailadres
  const password = ref(""); // maak een ref aan voor het wachtwoord

  const login = () => {
    axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: email.value, // gebruik de waarde van de ref
        password: password.value, // gebruik de waarde van de ref
      })
      .then((res) => {
        /**
         * Axios zal de data van de respons toekennen aan een property met de naam "data".
         */
        console.log(res); // volledige respons
        console.log(res.data); // data van de respons, dit zal { token: string } bevatten
      });
  };
</script>

<template>
  <!-- Gebruik two-way binding voor email en password -->
  <input type="email" v-model="email" />
  <input type="password" v-model="password" />
  <button @click="login()">Login</button>
</template>
```

Kleine wijziging in `main.css`, momenteel wordt de styling enkel toegepast op inputs van type `text`, maar er zijn inputs zonder type en inputs met het type `email` en `password` in deze applicatie, wijzig de selector zodat het van toepassing is op alle `input`-elementen.

Voor wijziging:

```css
body,
input[type="text"],
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
```

Na wijziging:

```css
body,
input,
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/c4155e52f395e36092be3ecf6676c18ea2c1f017)

## Token opslaan in Local Storage

Er is ondertussen code om in te loggen. Bij het invullen van de juiste gegevens zien we een respons waarin een `token` wordt teruggestuurd. Deze `token` stelt de ingelogde gebruiker voor (de gebruiker met het e-mailadres en het wachtwoord dat ingegeven werd), om deze op een later tijdstip terug aan te kunnen, moet deze ergen opgeslagen worden.

We zullen de token opslaan in `Local Storage`. Dit is een API die beschikbaar is in de browser. Deze API kan gebruikt worden om data op te slaan in de browser. Deze data blijft beschikbaar, ook als de pagina herladen wordt of als de browser afgesloten wordt.

`LoginView.vue`

```html
<script setup lang="ts">
  import axios from "axios";
  import { ref } from "vue";

  const email = ref("");
  const password = ref("");

  const login = () => {
    axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: email.value,
        password: password.value,
      })
      .then((res) => {
        const token = res.data.token;
        localStorage.setItem("token", token);
      });
  };
</script>

<template>
  <input type="email" v-model="email" />
  <input type="password" v-model="password" />
  <button @click="login()">Login</button>
</template>
```

Merk op: `localStorage` is een property van het `window` object (`window.localStorage`, of kortweg `localStorage`). Dit object is altijd beschikbaar in de browser, zonder dat er een import nodig is. `localStorage` is een object met een aantal functies, waaronder `setItem` en `getItem`. Deze functies kunnen gebruikt worden om data op te slaan en op te halen uit de Local Storage.

Bezoek opnieuw de loginpagina, vul de juiste gegevens in en klik op `Login`. Bekijk vervolgens de Local Storage in de developer tools van de browser.

Je kan bij Firefox de Local Storage bekijken via de developer tools. Ga naar de `Storage` tab en klik op `Local Storage`. Hierin zal de token opgeslagen zijn.

Je kan bij Chrome de Local Storage bekijken via de developer tools. Ga naar de `Application` tab en klik op `Local Storage`. Hierin zal de token opgeslagen zijn.

Firefox
![Local Storage Firefox](/img/blog/quasar-toh-firefox-localstorage.png)
Er zal `token` staan i.p.v. `accessToken`.

Chrome
![Local Storage Chrome](/img/blog/quasar-toh-chrome-localstorage.png)
Er zal `token` staan i.p.v. `accessToken`.

## Authentication service

Om te zorgen dat de authenticatie van een gebruiker niet per component geïmplementeerd moet worden, zal deze in een service geïmplementeerd worden. Over het algemeen is het een goed idee om `http`-calls in een service te implementeren.

Genereer een nieuwe service.

```sh
touch src/services/auth.service.ts
```

`src/services/auth.service.ts`

```ts
const useAuth = () => {
  return {};
};

export { useAuth };
```

Verplaats de code om in te loggen naar de service.

`src/services/auth.service.ts`

```ts
import axios from "axios";

const useAuth = () => {
  const login = (email: string, password: string) => {
    axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: email,
        password: password,
      })
      .then((res) => {
        const token = res.data.token;
        localStorage.setItem("token", token);
      });
  };

  return { login };
};

export { useAuth };
```

Merk op: de `login()` functie heeft nu twee parameters, `email` en `password`, zodat we deze kunnen meegeven vanuit de loginpagina.
Merk op: de parameters zijn `string`-waarden, het is dus niet langer nodig om `.value` te gebruiken: `email.value` wordt `email`, `password.value` wordt `password`.

`src/services/auth.service.ts`

Aangezien key en value nu dezelfde naam hebben, kan de code herschreven worden naar:

```ts
import axios from "axios";

const useAuth = () => {
  const login = (email: string, password: string) => {
    axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email, // email: email
        password, // password: password
      })
      .then((res) => {
        const token = res.data.token;
        localStorage.setItem("token", token);
      });
  };

  return { login };
};

export { useAuth };
```

Maak gebruik van de `useAuth`-service in de `LoginView`-pagina.

`LoginView.vue`

```html
<script setup lang="ts">
  import { useAuth } from "@/services/auth.service";
  import { ref } from "vue";

  const { login } = useAuth();

  const email = ref("");
  const password = ref("");
</script>

<template>
  <input type="email" v-model="email" />
  <input type="password" v-model="password" />
  <!-- gebruik de login-funcite van useAuth, geef de parameters door -->
  <button @click="login(email, password)">Login</button>
</template>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/484cc5dcafe38f95b4f8db8f47f03379b305404c)

### Resetten van velden

De gewenste situatie is dat de velden enkel resetten als het inloggen gelukt is. Om dit te doen, zullen we de `login()`-functie van de `AuthService` aanpassen.

`LoginView.vue`

```ts
import axios from "axios";

const useAuth = () => {
  const login = (email: string, password: string) => {
    /*
     * Voeg return toe zodat de `Promise` teruggegeven wordt.
     * Hierdoor kunnen we `.then()` aanroepen op de `login()`-functie in de `LoginView.vue`-pagina.
     */
    return axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email,
        password,
      })
      .then((res) => {
        const token = res.data.token;
        localStorage.setItem("token", token);
        /*
         * Geef de respons terug zodat de volgende `.then()` deze kan gebruiken (ook al doen we er niks mee in deze tutorial)
         */
        return res;
      });
  };

  return { login };
};

export { useAuth };
```

`LoginView.vue`

```html
<script setup lang="ts">
  import { useAuth } from "@/services/auth.service";
  import { ref } from "vue";

  const { login } = useAuth();

  const email = ref("");
  const password = ref("");

  /**
   * Voeg de functie authenticate toe.
   * Hierin wordt de login-functie van de AuthService aangeroepen waarop een `.then()` wordt gedaan.
   */
  const authenticate = () => {
    login(email.value, password.value).then(() => {
      email.value = "";
      password.value = "";
    });
  };
</script>

<template>
  <input type="email" v-model="email" />
  <input type="password" v-model="password" />
  <button @click="authenticate()">Login</button>
</template>
```

Merk op dat de parameters van de login-functie van de `AuthService` nu meegegeven worden vanuit de `script`-tag van de `LoginView.vue`-pagina. Hierdoor is het opnieuw nodig om `.value` te gebruiken.

Om in woorden uit te leggen wat er gebeurt:

- Er wordt op de `Login`-button geklikt.
- De `authenticate()`-functie van de `LoginView`-component wordt aangeroepen.
- Er wordt een `.then()` gedaan op de `axios.post()`-functie in de `AuthService`. Hierdoor kunnen we aan de slag met de respons van de `axios.post()`-functie.
- We slaan de token die zich in deze data bevindt op in de Local Storage.
- We geven de respons door aan de volgende `.then()`-functie.
- We komen in de `.then()` van de `login()`-functie van de `AuthService`.
- We resetten de velden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/44761beab7998e08b879b0dfecdf9ccc754ff9f2)

### Inloggen en uitloggen

We kunnen al inloggen, deze functionaliteit is geïmplementeerd in de `AuthService`. We zouden ook graag kunnen uitloggen. Hiervoor zullen we een nieuwe functie toevoegen aan de `AuthService`. Ook willen we weten of de gebruiker ingelogd is of niet. Hiervoor zullen we een afgeleide waarde toevoegen aan de `AuthService`.

`auth.service.ts`

```ts
import axios from "axios";
import { ref } from "vue";

const isAuthenticated = ref(localStorage.getItem("token") !== null);

const useAuth = () => {
  const login = (email: string, password: string) => {
    return axios
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email,
        password,
      })
      .then((res) => {
        const token = res.data.token;
        localStorage.setItem("token", token);
        isAuthenticated.value = true;
        return res;
      });
  };

  const logout = () => {
    localStorage.removeItem("token");
    isAuthenticated.value = false;
  };

  return { login, logout, isAuthenticated };
};

export { useAuth };
```

De `logout()`-functie zal de token verwijderen uit de Local Storage. Hierdoor wordt de gebruiker uitgelogd.

De `isAuthenticated` property wordt geïnitialiseerd met de waarde van de token in de Local Storage. Indien deze waarde `null` is, dan is de gebruiker niet ingelogd. Indien deze waarde niet `null` is, dan is de gebruiker ingelogd. Bij het uitloggen of inloggen wordt deze waarde aangepast.

`MainLayout.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { useAuth } from "@/services/auth.service"; // importeer useAuth
  import { RouterView, useRouter } from "vue-router";

  const router = useRouter();
  /**
   * Destructuring van logout en isAuthenticated uit de useAuth-service.
   */
  const { logout, isAuthenticated } = useAuth();
</script>

<template>
  <!-- Voeg de header toe - koppel Logout en Login knoppen -->
  <!-- Gebruik v-if, v-else op isAuthenticated op te bepalen welke knop getoond moet worden -->
  <div class="header">
    <StyledButton v-if="isAuthenticated" @click="logout()">Logout</StyledButton>
    <StyledButton v-else @click="router.push({ name: 'login' })">
      Login
    </StyledButton>
  </div>
  <div class="layout-container">
    <div class="title">Tour of Heroes</div>

    <div class="button-container">
      <StyledButton @click="router.push({ name: 'dashboard' })">
        Dashboard
      </StyledButton>
      <StyledButton @click="router.push({ name: 'hero-list' })">
        Heroes
      </StyledButton>
    </div>

    <RouterView />
  </div>
</template>

<style scoped>
  .layout-container {
    margin: 2rem;
  }

  .button-container {
    display: flex;
    gap: 0.25rem;
  }

  /* Voeg de .header toe */
  .header {
    display: flex;
    justify-content: flex-end;
    padding: 0.5rem;
    background-color: #333;
  }
</style>
```

Er wordt een `header` toegevoegd aan de "layout"-pagina, `MainLayout.vue`. Deze balk/header zal altijd zichtbaar zijn op elke pagina, vandaar de toevoeging in de `layout`-pagina.

Hierin worden twee knoppen getoond, `Login` en `Logout`.
Er wordt gebruikgemaakt van `v-if` en `v-else`. De `isAuthenticated` is de ref property afkomstig uit de service, deze zal `true` zijn als de gebruiker is ingelogd en `false` als de gebruiker niet is ingelogd.

Nu is te zien dat de balk niet de volledige breedte inneemt en ook dat deze niet bovenaan de pagina staat. Dit komt omdat er nog margin aanwezig is op de `body` van de pagina. Verwijder deze margin.

`src/assets/main.css`

```css
/* Application-wide Styles */
body,
input,
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}

/* Dit is nieuw */
body {
  margin: 0;
}

.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
}

/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/e9617c12a09627f3c10bda26c81ed875311bf5c7)

### De authenticatie uittesten

Start de applicatie op. Indien je ingelogd bent, zal er rechtsboven een knop `Logout` te zien zijn. Indien je niet ingelogd bent, zal er rechtsboven een knop `Login` te zien zijn.

Indien je ingelogd bent, klik op `Logout`.

Klik op `Login`, dit zal de gebruiker naar de loginpagina navigeren.

Vul hier een geldige login van Code Coaching in en klik op `Login` naast de invoervelden. De gebruiker zal ingelogd worden, dit is te zien aan de knop rechtsboven die veranderd is naar `Logout`.

## Loginpagina stylen

De loginpagina ziet er momenteel een beetje raar uit. Voeg wat styling toe om het er beter uit te laten zien.

Maak gebruik van `StyledButton` i.p.v. het normale `button`-element. Plaats wrapper elementen/container elementen om de plaatsing van de inputvelden en de knop te verbeteren.

`LoginView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { useAuth } from "@/services/auth.service";
  import { ref } from "vue";

  const { login } = useAuth();

  const email = ref("");
  const password = ref("");

  const authenticate = () => {
    login(email.value, password.value).then(() => {
      email.value = "";
      password.value = "";
    });
  };
</script>

<template>
  <div class="login-container">
    <div class="login-fields">
      <input type="email" v-model="email" />
      <input type="password" v-model="password" />
    </div>
    <StyledButton @click="authenticate()">Login</StyledButton>
  </div>
</template>

<style scoped>
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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/932eae648d12a929ff8a2f8caddd6a0aff061777)

## Hero service koppelen met API

Tot nu toe werd er hard-coded data gebruikt in de `Hero service`. Maar, met de account van Code Coaching is het mogelijk om de `Hero` data te beheren in een database via een API.

Er wordt vanuitgegaan dat de werking van het `/api/heroes` end point gekend is. Indien dit niet het geval is, bekijk dan deze korte oefening: [CRUD /api/heroes](https://code-coaching.dev/blogs/crud-oefening).

In de oefening wordt er gebruikgemaakt van Postman. En de token die de ingelogde gebruiker voorstelt, wordt aangemaakt via de profielpagina van Code Coaching. In deze tutorial wordt er gebruikgemaakt van de token die opgeslagen is in Local Storage, die bekomen wordt door in te loggen via de loginpagina.

In het kort: Er worden calls gedaan naar `https://code-coaching.dev/api/heroes`. Dit kunnen `GET`, `POST`, `PATCH` en `DELETE` calls zijn. De `GET` call zal alle heroes ophalen, de `POST` call zal een hero aanmaken, de `PATCH` call zal een hero wijzigen en de `DELETE` call zal een hero verwijderen. Voor elke call is authenticatie nodig, dit wordt gedaan door een `Authorization` header toe te voegen aan de call.

## Authentication Guard

Momenteel is het mogelijk om alle pagina's te bezoeken, ook als de gebruiker niet ingelogd is. Dit is niet de gewenste situatie. Nu het de bedoeling is om de data van de heroes op te halen via de API, is het belangrijk dat de gebruiker ingelogd is.

Om zeker te zijn dat een gebruiker ingelogd is, zal er een `Authentication Guard` geïmplementeerd worden. Deze guard zal ervoor zorgen dat de gebruiker enkel pagina's kan bezoeken als deze ingelogd is. Indien de gebruiker niet ingelogd is, zal deze doorgestuurd worden naar de loginpagina.

```sh
mkdir src/guards && touch src/guards/authentication.guard.ts
```

We zullen de functie koppelen aan een route (iets later in deze tutorial). Vervolgens zal deze functie aangeroepen worden als de gebruiker naar de route navigeert. De functie zal een `boolean` teruggeven, `true` als de gebruiker toegang heeft tot de route en `false` als de gebruiker geen toegang heeft tot de route.

`authentication.guard.ts`

```ts
import type { RouteLocation } from "vue-router";

export const authenticationGuard = (
  _to: RouteLocation,
  _from: RouteLocation
) => {
  return true;
};
```

Merk op dat het formatteren de parameters `to` en `from` hernoemd naar `_to` en `_from`. Dit is omdat we deze parameters niet gebruiken in de functie. Er is een conventie om parameters die niet gebruikt worden te prefixen met een underscore `_`.

Momenteel geeft de functie altijd `true` terug. Indien we deze guard zouden koppelen aan een route, dan zou de gebruiker altijd toegang hebben tot de route (wat zonder guard ook al het geval is).

We zullen dus logica moeten toevoegen die bepaalt of de gebruiker toegang heeft tot de route of niet. We zullen de `AuthService` gebruiken om te bepalen of de gebruiker ingelogd is of niet.

`authentication.guard.ts`

```ts
import { useAuth } from "@/services/auth.service";
import type { RouteLocation } from "vue-router";

const { isAuthenticated } = useAuth();

export const authenticationGuard = (
  _to: RouteLocation,
  _from: RouteLocation
) => {
  if (isAuthenticated.value) {
    return true;
  } else {
    return false;
  }
};
```

Indien de gebruiker ingelogd is, zal de functie `true` teruggeven, indien de gebruiker niet ingelogd is, zal de functie `false` teruggeven.

Nu is er nog één ding dat we graag willen toevoegen. Indien de gebruiker niet ingelogd is, willen we dat de gebruiker doorgestuurd wordt naar de loginpagina. Dit kunnen we doen door gebruik te maken van de `Router`.

`authentication.guard.ts`

```ts
import { useAuth } from "@/services/auth.service";
import type { RouteLocation } from "vue-router";
import { useRouter } from "vue-router";

export const authenticationGuard = (
  _to: RouteLocation,
  _from: RouteLocation
) => {
  const { isAuthenticated } = useAuth();
  const router = useRouter();

  if (isAuthenticated.value) {
    return true;
  } else {
    router.push({ name: "login" });
    return false;
  }
};
```

De guard is nu klaar. We kunnen deze koppelen aan de routes die alleen bezocht mogen worden als de gebruiker ingelogd is.

`src/router/index.ts`

```ts
import { authenticationGuard } from "@/guards/authentication.guard";
import { createRouter, createWebHashHistory } from "vue-router";

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: "/",
      name: "MainLayout",
      component: () => import("@/layouts/MainLayout.vue"),
      children: [
        {
          path: "",
          name: "dashboard",
          beforeEnter: [authenticationGuard], // voeg de guard toe
          component: () => import("@/views/DashboardView.vue"),
        },
        {
          path: "heroes",
          name: "hero-list",
          beforeEnter: [authenticationGuard], // voeg de guard toe
          component: () => import("@/views/HeroListView.vue"),
        },
        {
          path: "heroes/add",
          name: "hero-add",
          beforeEnter: [authenticationGuard], // voeg de guard toe
          component: () => import("@/views/HeroAddView.vue"),
        },
        {
          path: "heroes/:id",
          name: "hero-details",
          beforeEnter: [authenticationGuard], // voeg de guard toe
          component: () => import("@/views/HeroDetailsView.vue"),
        },
        {
          path: "login",
          name: "login",
          component: () => import("@/views/LoginView.vue"),
        },
      ],
    },
  ],
});

export default router;
```

Test de applicatie. Indien je ingelogd bent, zal je de pagina's kunnen bezoeken. Indien je niet ingelogd bent, zal je bij het manueel bezoeken van de pagina's, doorgestuurd worden naar de loginpagina.

Alle wijziging: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/2be223d08847198cec5c685120cbc25ee756cc7f)

## Model backend Hero

Bij het bekijken van de API, zien we dat de `Hero` uit de database er als volgt uitziet:

```json
{
  "id": 61,
  "user_id": 2,
  "name": "John Duck",
  "created_at": "2023-11-29T15:28:07.000000Z",
  "updated_at": "2023-11-29T15:28:07.000000Z"
}
```

Dit wijkt af van de `Hero` die momenteel gebruikt wordt in de applicatie. De `Hero` die momenteel gebruikt wordt in de applicatie, ziet er als volgt uit:

```ts
export interface Hero {
  number: number;
  name: string;
}
```

We kunnen twee dingen doen, ofwel passen we de `Hero` aan in de applicatie, ofwel transformeren we de `Hero` die we ophalen via de API naar een `Hero` die gebruikt kan worden in de applicatie. We zullen de tweede optie gebruiken.

Voorzie een model voor de `Hero` die we ophalen via de API, om dit te onderscheiden van de `Hero` die al bestaat, zullen we deze `Hero` de naam `HeroBackend` geven.

`src/components/models.ts`

```ts
export interface Hero {
  number: number;
  name: string;
}

export interface HeroBackend {
  id: number;
  name: string;
}
```

Merk op dat we `user_id`, `created_at` en `updated_at` niet nodig hebben. Deze zullen we dus ook niet opnemen in het model (dit mag wel, maar is niet nodig op dit moment).

## Lijst van heroes ophalen

Momenteel is er een array met hard-coded heroes. Er is ook een functie die momenteel de lijst van heroes uit Local Storage ophaalt. Deze functie zal aangepast worden om de lijst van heroes op te halen via de API.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

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

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const loadHeroes = () => {
    const savedHeroes = localStorage.getItem("heroes");
    if (savedHeroes) {
      heroes.value = JSON.parse(savedHeroes);
    }
  };

  return {
    heroes,
    selectedHero,
    // hier nog code
    loadHeroes,
  };
};

export { useHeroes };
```

De `loadHeroes()`-functie zal aangepast worden om de lijst van heroes op te halen via de API.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

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

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const loadHeroes = () => {
    axios
      .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes")
      .then((res) => {
        console.log(res);
      });
  };

  return {
    heroes,
    selectedHero,
    // hier nog code
    loadHeroes,
  };
};

export { useHeroes };
```

De `loadHeroes()`-functie zal een HTTP call doen naar de API. De API zal een lijst van heroes teruggeven. Deze lijst van heroes zal in de console gelogd worden. Merk ook op dat we het type van het resultaat van de call meegeven aan de `get`-functie van `axios` (`axios.get<Array<Hero>>()`), hierdoor zal `axios` weten dat het resultaat van de call een array van `HeroBackend`-objecten is.

Herinner dat `loadHeroes` wordt aangeroepen in `App.vue`, zodra de applicatie opstart zal de lijst van heroes opgehaald worden. Bekijk de console in de browser en merk op dat er een `401 Unauthorized` error is. We zullen de token uit Local Storage moeten toevoegen aan de `Authorization` header van de call, zodat de API weet welke gebruiker de call doet.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const loadHeroes = () => {
    axios
      .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes", {
        headers: {
          Authorization: `Bearer ${localStorage.getItem("token")}`,
        },
      })
      .then((res) => {
        console.log(res);
      });
  };

  return {
    heroes,
    selectedHero,
    // hier nog code
    loadHeroes,
  };
};

export { useHeroes };
```

Merk op dat de `heroes`-ref nu een lege array is. Dit is omdat de lijst van heroes opgehaald zal worden via de API. De `loadHeroes()`-functie zal later aangepast worden om de lijst te vullen met de data die opgehaald wordt via de API. Ook is er een type toegevoegd aan de `heroes`-ref, dit is een array van `Hero`-objecten.

Nu wordt de token meegestuurd (indien er geen is, wordt `Bearer undefined` meegestuurd). Zoals we zien kan dit gedaan worden door een tweede parameter mee te geven aan de `get`-functie van `axios`. Deze tweede parameter is een object met een `headers` property. Deze `headers` property is een object met een `Authorization` property. De `Authorization` property is een string die begint met `Bearer ` gevolgd door de token. De reden dat er `Bearer ` voor de token moet staan, is omdat de API dit verwacht (dit is een veelgebruikte manier om een token mee te sturen met een request).

Test de applicatie. Indien je niet ingelogd bent, wordt er geen call gedaan naar de API. Log in, ververs na het inloggen de browser, nu wordt er een call gedaan naar de API. Bekijk de console in de browser en merk op dat er een array van heroes wordt teruggegeven. Indien er nog nooit heroes zijn toegevoegd aan de applicatie, zal de array leeg zijn.

Merk op dat hetgeen gelogd wordt in de console de response is van de call naar de API. Dit is een object met een aantal properties. De property die we nodig hebben is `data`. De `data` property is een array van `HeroBackend`-objecten.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/71139462588da3ac9a752bcfc386da651ce41c53)

## loadHeroes() refactoren

Momenteel wordt `loadHeroes()` aangeroepen in `App.vue`. Dit was geen probleem toen er nog met heroes uit Local Storage gewerkt werd. Maar nu er met een API waar authenticatie nodig is gewerkt wordt, willen we dat de pagina's zelf de data ophalen die nodig is op de pagina die ingeladen wordt.

`App.vue` voor wijziging

```html
<script setup lang="ts">
  import { onMounted } from "vue";
  import { RouterView } from "vue-router";
  import { useHeroes } from "./services/hero.service";

  const { loadHeroes } = useHeroes();

  onMounted(() => {
    loadHeroes();
  });
</script>

<template>
  <RouterView />
</template>
```

`App.vue` na wijziging

```html
<script setup lang="ts">
  import { RouterView } from "vue-router";
</script>

<template>
  <RouterView />
</template>
```

Er zijn twee pagina's die de lijst van heroes nodig hebben, `DashboardView.vue` en `HeroListView.vue`. We zullen de `loadHeroes()`-functie van de `HeroService` aanroepen in de `onMounted`-lifecycle hook van `DashboardView.vue` en `HeroListView.vue`.

`DashboardView.vue` voor wijziging

```html
<script setup lang="ts">
  import { useHeroes } from "@/services/hero.service";
  import { useRouter } from "vue-router";

  const { topHeroes } = useHeroes();
  const router = useRouter();
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

`DashboardView.vue` na wijziging

```html
<script setup lang="ts">
  import { useHeroes } from "@/services/hero.service";
  import { useRouter } from "vue-router";
  import { onMounted } from "vue";

  const { topHeroes, loadHeroes } = useHeroes();
  const router = useRouter();

  onMounted(() => {
    loadHeroes();
  });
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

`HeroListView.vue` voor wijziging

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { useHeroes } from "@/services/hero.service";
  import { useRouter } from "vue-router";

  const router = useRouter();

  const { heroes, selectedHero, deleteHero } = useHeroes();

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  const uppercase = (text: string) => text.toUpperCase();
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

`HeroListView.vue` na wijziging

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted } from "vue";
  import { useRouter } from "vue-router";

  const router = useRouter();

  const { heroes, selectedHero, deleteHero, loadHeroes } = useHeroes();

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  const uppercase = (text: string) => text.toUpperCase();

  onMounted(() => {
    loadHeroes();
  });
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Test de applicatie. Na inloggen kan er genavigeerd worden naar de `DashboardView` en de `HeroListView`. De pagina zelf is nu verantwoordelijk voor het ophalen van de data die nodig is op de pagina. Dit kan je zien door te kijken naar de console terwijl er genavigeerd wordt tussen `dashboard` en `hero-list` (klik op de knoppen). Er zal telkens een call gedaan worden naar de API om de lijst van heroes op te halen.

De eerder vermelde [CRUD /api/heroes](https://code-coaching.dev/blogs/crud-oefening) oefening kan gebruikt worden om heroes aan te maken indien er nog geen heroes aangemaakt zijn door jouw account.

Zelfs als er heroes zijn aangemaakt, zal de lijst van heroes niet getoond worden. Dit komt omdat we momenteel de data loggen naar de console in plaats van deze toe te kennen aan de `heroes` property van de `HeroService`.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const loadHeroes = () => {
    axios
      .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes", {
        headers: {
          Authorization: `Bearer ${localStorage.getItem("token")}`,
        },
      })
      .then((res) => {
        heroes.value = res.data; // Dit toont een TypeScript error
      });
  };

  return {
    heroes,
    selectedHero,
    // hier nog code
    loadHeroes,
  };
};

export { useHeroes };
```

Er is een probleem, de `data` die we terugkrijgen van de API is een array van `HeroBackend`-objecten. Maar de `heroes`-property van de `HeroService` verwacht een array van `Hero`-objecten. Dit is een probleem, want we kunnen objecten van het ene type (`{ id: number; name: string }`) niet zomaar toekennen aan een variabele van het andere type (`{ number: number; name: string }`). Dit is een veel voorkomende situatie. De data die we ophalen via een API is niet altijd in hetzelfde formaat als de data die we gebruiken in onze applicatie. We zullen de data die we ophalen via de API moeten transformeren naar het formaat dat we gebruiken in onze applicatie.

`hero.service.ts`

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const loadHeroes = () => {
    axios
      .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes", {
        headers: {
          Authorization: `Bearer ${localStorage.getItem("token")}`,
        },
      })
      .then((res) => {
        heroes.value = res.data.map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          return hero;
        });
      });
  };

  return {
    heroes,
    selectedHero,
    // hier nog code
    loadHeroes,
  };
};

export { useHeroes };
```

Herinner de uitleg over Promises. De `then()` wordt aangeroepen wanneer de call klaar is. De parameter dan de `then()` is een functie die aangeroepen wordt met de data die teruggegeven wordt door de call. In dit geval is de data een response met een property genaamd `data` waarin de data zit die we nodig hebben. Deze data is een array van `HeroBackend`-objecten. We hebben echter een array van `Hero`-objecten nodig, dus we transformeren de data.

De transformatie doen we door gebruik te maken van de `map()`-operator op de array. We gebruiken `satisfies` van TypeScript om aan te geven dat het object dat we aanmaken voldoet aan de `Hero`-interface.

Aangezien we buiten de service niet meer reageren op het voltooien van de call, is het niet nodig om de `axios.get` te returnen. Omdat we de `axios.get` niet returnen, is het ook niet nodig in de `.then()` de getransformeerde data terug te geven.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/97ab0bffdb5038a6cf1813c62b74aa6802e13428)

## Hero toevoegen

`hero.service.ts` - voor wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const addHero = (name: string) => {
    const maxNumber = Math.max(...heroes.value.map((h) => h.number));
    const newHero = { number: maxNumber + 1, name } satisfies Hero;
    heroes.value.push(newHero);
    saveHeroes();
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    addHero,
    // hier nog code
  };
};

export { useHeroes };
```

`hero.service.ts` - na wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const addHero = (name: string) => {
    return axios.post(
      "https://code-coaching.dev/api/heroes", // end point
      { name }, // body
      {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      } // opties - hier voegen we de token toe aan de Authorization header
    );
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    addHero,
    // hier nog code
  };
};

export { useHeroes };
```

Merk op dat we hier de Promise returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen een `.then()` gebruiken in de component.

`HeroAddView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { ref } from "vue";
  import { useRouter } from "vue-router";

  const router = useRouter();
  const { addHero } = useHeroes();

  const name = ref("");

  const saveHero = () => {
    addHero(name.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <div class="title">Add hero</div>

  <div>name: <input v-model="name" /></div>

  <div class="buttons">
    <StyledButton @click="router.go(-1)">Back</StyledButton>
    <StyledButton :type="'primary'" @click="saveHero()"> Save </StyledButton>
  </div>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .buttons {
    margin-top: 1rem;
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Start de applicatie, zorg dat je ingelogd bent en voeg twee heroes toe.

## Hero verwijderen

`hero.service.ts` - voor wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const deleteHero = (hero: Hero) => {
    heroes.value = heroes.value.filter((h) => h.number !== hero.number);
    if (selectedHero.value?.number === hero.number) {
      selectedHero.value = null;
    }
    saveHeroes();
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    deleteHero,
    // hier nog code
  };
};

export { useHeroes };
```

`hero.service.ts` - na wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const deleteHero = (hero: Hero) => {
    return axios
      .delete(`https://code-coaching.dev/api/heroes/${hero.number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .then(() => {
        selectedHero.value = null;
      });
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    deleteHero,
    // hier nog code
  };
};

export { useHeroes };
```

Merk op dat we hier de Promise returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen `.then()` gebruiken in de component. We gebruiken hier `.then()` om de `selectedHero.value` op `null` te zetten wanneer de call klaar is.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero, deleteHero } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = findHero(heroId);
  });

  const remove = () => {
    if (hero.value === null) return;
    deleteHero(hero.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <div class="buttons">
      <StyledButton @click="router.go(-1)">Back</StyledButton>
      <StyledButton
        :type="'primary'"
        @click="
          updateHero(hero);
          router.go(-1);
        "
      >
        Save
      </StyledButton>
      <StyledButton :type="'negative'" @click="remove()"> Delete </StyledButton>
    </div>
  </template>
  <template v-else>
    <div class="title">Hero not found!</div>
  </template>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .buttons {
    margin-top: 1rem;
    display: flex;
    gap: 0.5rem;
  }
</style>
```

De `remove()`-functie wordt aangeroepen in de template. De `deleteHero()`-functie van de `HeroService` returnt een Promise, we kunnen hierop `.then()` doen, zodat we weten wanneer de call klaar is. Wanneer de call klaar is, navigeren we terug naar de vorige pagina. De `remove()`-functie bevat ook een `early return`, dit is omdat we niet willen dat de functie verder uitgevoerd wordt wanneer de `hero`-ref `null` is.

`HeroListView.vue`

```html
<script setup lang="ts">
  import StyledButton from "@/components/StyledButton.vue";
  import type { Hero } from "@/components/models";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted } from "vue";
  import { useRouter } from "vue-router";

  const router = useRouter();

  const { heroes, selectedHero, deleteHero, loadHeroes } = useHeroes();

  const onClickHero = (hero: Hero) => {
    selectedHero.value = hero;
  };

  const uppercase = (text: string) => text.toUpperCase();

  onMounted(() => {
    loadHeroes();
  });

  const remove = () => {
    if (selectedHero.value === null) return;
    deleteHero(selectedHero.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
      class="hero"
      :class="{
        'hero--active': hero.number === selectedHero?.number
      }"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <StyledButton
    class="new-hero-button"
    :type="'primary'"
    @click="router.push({ name: 'hero-add' })"
  >
    New
  </StyledButton>

  <template v-if="selectedHero">
    <div class="title">{{ uppercase(selectedHero.name) }} is my hero</div>
    <div class="buttons">
      <StyledButton
        @click="router.push({ name: 'hero-details', params: { id: selectedHero?.number } })"
      >
        Details
      </StyledButton>
      <StyledButton :type="'negative'" @click="remove()">Delete</StyledButton>
    </div>
  </template>
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Wanneer er op `Delete` geklikt wordt, roepen we de `deleteHero()`-functie van de `HeroService` aan. De `deleteHero()`-functie van de `HeroService` returnt een Promise, we kunnen hierop `.then()` doen, zodat we weten wanneer de call klaar is. Wanneer de call klaar is, roepen we de `loadHeroes()`-functie van de `HeroService` aan. De `remove()`-functie bevat ook een `early return`, dit is omdat we niet willen dat de functie verder uitgevoerd wordt wanneer de `selectedHero`-ref `null` is.

Start de applicatie, zorg dat je ingelogd bent en verwijder een hero.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/6510fe1819a960adad9a2590b7c7d7d73775e755)

## Hero updaten

`hero.service.ts` - voor wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const updateHero = (hero: Hero) => {
    const index = heroes.value.findIndex((h) => h.number === hero.number);
    if (index !== -1) {
      heroes.value[index] = structuredClone(toRaw(hero));
    }
    saveHeroes();
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    updateHero,
    // hier nog code
  };
};

export { useHeroes };
```

`hero.service.ts` - na wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const updateHero = (hero: Hero) => {
    return axios
      .patch(
        `https://code-coaching.dev/api/heroes/${hero.number}`,
        {
          name: hero.name,
        },
        {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
      .then(() => {
        selectedHero.value = null;
      });
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    updateHero,
    // hier nog code
  };
};

export { useHeroes };
```

Herinner dat we de Promise returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen een `.then()` gebruiken in de component. We gebruiken hier `.then()` om de `selectedHero.value` op `null` te zetten wanneer de call klaar is.

De functie `saveHeroes` wordt nu niet meer gebruikt, deze kunnen we verwijderen.

`HeroDetailsView.vue`

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero, deleteHero } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = findHero(heroId);
  });

  const remove = () => {
    if (hero.value === null) return;
    deleteHero(hero.value).then(() => {
      router.go(-1);
    });
  };

  const save = () => {
    if (hero.value === null) return;
    updateHero(hero.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <template v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <div class="buttons">
      <StyledButton @click="router.go(-1)">Back</StyledButton>
      <StyledButton :type="'primary'" @click="save()"> Save </StyledButton>
      <StyledButton :type="'negative'" @click="remove()"> Delete </StyledButton>
    </div>
  </template>
  <template v-else>
    <div class="title">Hero not found!</div>
  </template>
</template>

<style scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .buttons {
    margin-top: 1rem;
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/42fd7cc63c9fdf4ac55c4d18d9f9a710afb4757e)

## findHero() refactoren

Momenteel werkt de detailspagina, maar enkel wanneer we er naartoe navigeren vanuit de lijst van heroes. Wanneer we de pagina rechtstreeks bezoeken, dan wordt er altijd "Hero not found" getoond. Dit komt omdat de `findHero()`-functie momenteel een hero zoekt in de array van heroes die we ophalen via de API. Wanneer we de pagina rechtstreeks bezoeken, is de array van heroes nog niet opgehaald, waardoor de `findHero()`-functie geen hero zal vinden.

Het gewenste gedrag is dat de `findHero()`-functie een API call uitstuurt om de hero op te halen, gebaseerd op de parameter die in de URL staat.

`hero.service.ts` - voor wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const findHero = (heroId: number) => {
    const matchingHero =
      heroes.value.find((hero) => hero.number === heroId) ?? null;
    return structuredClone(toRaw(matchingHero));
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    findHero,
    // hier nog code
  };
};

export { useHeroes };
```

`hero.service.ts` - na wijziging

```ts
import type { Hero } from "@/components/models";
import { computed, ref, toRaw, type Ref } from "vue";

const heroes: Ref<Array<Hero>> = ref([]); // type toegevoegd, lege array

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  // hier nog code

  const findHero = (heroId: number) => {
    return axios
      .get<HeroBackend>(`https://code-coaching.dev/api/heroes/${heroId}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .then(
        // we onderscheppen de data die terugkomt van de API via .then()
        (res) => {
          const heroBackend = res.data;
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          // de data wordt omgevormd van een HeroBackend object naar een Hero object
          // vanaf dit punt bevat de Promise een Hero object
          // als we later .then() doen, dan krijgen we een Hero object
          return hero;
        }
      );
  };

  // hier nog code

  return {
    heroes,
    selectedHero,
    // hier nog code
    findHero,
    // hier nog code
  };
};

export { useHeroes };
```

`HeroDetailsView.vue` - voor wijziging

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero, deleteHero } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  onMounted(() => {
    const heroId = Number(route.params.id);
    hero.value = findHero(heroId);
  });

  const remove = () => {
    if (hero.value === null) return;
    deleteHero(hero.value).then(() => {
      router.go(-1);
    });
  };

  const save = () => {
    if (hero.value === null) return;
    updateHero(hero.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

`HeroDetailsView.vue` - na wijziging

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero, deleteHero } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  /**
   * async/await om te wachten op de data
   */
  onMounted(async () => {
    const heroId = Number(route.params.id);
    hero.value = await findHero(heroId);
  });

  const remove = () => {
    if (hero.value === null) return;
    deleteHero(hero.value).then(() => {
      router.go(-1);
    });
  };

  const save = () => {
    if (hero.value === null) return;
    updateHero(hero.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Start de applicatie, zorg dat je ingelogd bent en navigeer naar de detailspagina van een hero. Dit is de flow die hiervoor ook al werkte. Kijk naar het nummer van een bestaande hero en navigeer naar deze hero door het nummer in de URL te wijzigen. Je zal zien dat de detailspagina nu ook werkt wanneer je de pagina rechtstreeks bezoekt.

Wijzig de url nu naar een nummer dat niet bestaat, bijvoorbeeld `abc69`. Je zal zien dat de vorige hero nog steeds getoond wordt. Dit komt omdat we momenteel enkel de use case afhandelen als de call naar de API succesvol is (we komen dan in de `.then()`-functie). We zullen ook de use case moeten afhandelen als de call naar de API niet succesvol is.

`HeroDetailsView.vue` - na wijziging met error handling

```html
<script setup lang="ts">
  import type { Hero } from "@/components/models";
  import StyledButton from "@/components/StyledButton.vue";
  import { useHeroes } from "@/services/hero.service";
  import { onMounted, ref, type Ref } from "vue";
  import { useRoute, useRouter } from "vue-router";

  const route = useRoute();
  const router = useRouter();

  const { findHero, updateHero, deleteHero } = useHeroes();
  const hero: Ref<Hero | null> = ref(null);

  onMounted(async () => {
    const heroId = Number(route.params.id);
    // try/catch om de error te onderscheppen
    try {
      hero.value = await findHero(heroId);
    } catch (e) {
      hero.value = null;
    }
  });

  const remove = () => {
    if (hero.value === null) return;
    deleteHero(hero.value).then(() => {
      router.go(-1);
    });
  };

  const save = () => {
    if (hero.value === null) return;
    updateHero(hero.value).then(() => {
      router.go(-1);
    });
  };
</script>

<template>
  <!-- Hier de bestaande html -->
</template>

<style scoped>
  /* Hier de bestaande css */
</style>
```

Herinner dat we de Promise returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen de `.then()`-functie in de component doen. We gebruiken hier `try/catch` om de error te onderscheppen. Wanneer er een error is, zetten we de `hero` op `null`.

Start de applicatie, bezoek een bestaande hero. Wijzig de URL naar een nummer dat niet bestaat (en herlaad de pagina). Je zal zien dat er "Hero not found" getoond wordt.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/98641a00726715fc8228ba1a678a5b3abaa3899e)

## API calls refactoren

Er zijn verschillende dingen die keer op keer terugkomen in elke API call. We kunnen dit refactoren naar een basis instantie van Axios en interceptors. Een interceptor (Nederlands: onderschepper) is een functie die uitgevoerd wordt voor elke API call, waarbij de request en response onderschept kan worden.

We zullen één instantie van Axios voorzien in de applicatie waarbij een `baseURL` wordt toegevoegd.
We zullen één interceptor toevoegen aan de Axios requests voor het toevoegen van de authorization token aan de header van de request.

### API service - Axios instantie

```sh
touch src/services/api.service.ts
```

`api.service.ts`

```ts
import axios from "axios";

const api = axios.create({
  baseURL: "https://code-coaching.dev/api",
});

const useApi = () => {
  return { api };
};

export { useApi };
```

Gebruik de `api`-instantie in de `AuthService`.

`auth.service.ts`

```ts
import { ref } from "vue";
import { useApi } from "./api.service"; // importeer de api service

const { api } = useApi(); // destructure de api (axios instantie met baseURL)

const isAuthenticated = ref(localStorage.getItem("token") !== null);

const useAuth = () => {
  const login = (email: string, password: string) => {
    // vervang "axios" door "api" en verwijder de baseURL
    return api
      .post<{ token: string }>("/token/login", {
        email,
        password,
      })
      .then((res) => {
        const token = res.data.token;
        localStorage.setItem("token", token);
        isAuthenticated.value = true;
        return res;
      });
  };

  const logout = () => {
    localStorage.removeItem("token");
    isAuthenticated.value = false;
  };

  return { login, logout, isAuthenticated };
};

export { useAuth };
```

Gebruik de `api`-instantie in de `HeroService`.

`hero.service.ts`

```ts
import type { Hero, HeroBackend } from "@/components/models";
import { computed, ref, type Ref } from "vue";
import { useApi } from "./api.service"; // importeer de api service

const { api } = useApi(); // destructure de api (axios instantie met baseURL)

const heroes: Ref<Array<Hero>> = ref([]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  const topHeroes = computed(() => {
    return heroes.value.slice(-4);
  });

  const findHero = (heroId: number) => {
    // vervang "axios" door "api" en verwijder de baseURL
    return api
      .get<HeroBackend>(`/heroes/${heroId}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .then((res) => {
        const heroBackend = res.data;
        const hero = {
          number: heroBackend.id,
          name: heroBackend.name,
        } satisfies Hero;
        return hero;
      });
  };

  const updateHero = (hero: Hero) => {
    // vervang "axios" door "api" en verwijder de baseURL
    return api
      .patch(
        `/heroes/${hero.number}`,
        {
          name: hero.name,
        },
        {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
      .then(() => {
        selectedHero.value = null;
      });
  };

  const deleteHero = (hero: Hero) => {
    // vervang "axios" door "api" en verwijder de baseURL
    return api
      .delete(`/heroes/${hero.number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .then(() => {
        selectedHero.value = null;
      });
  };

  const addHero = (name: string) => {
    // vervang "axios" door "api" en verwijder de baseURL
    return api.post(
      "/heroes",
      { name },
      {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      }
    );
  };

  const loadHeroes = () => {
    // vervang "axios" door "api" en verwijder de baseURL
    api
      .get<Array<HeroBackend>>("/heroes", {
        headers: {
          Authorization: `Bearer ${localStorage.getItem("token")}`,
        },
      })
      .then((res) => {
        heroes.value = res.data.map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          return hero;
        });
      });
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    findHero,
    updateHero,
    deleteHero,
    addHero,
    loadHeroes,
  };
};

export { useHeroes };
```

Test de applicatie uit, je zal zien dat alles nog steeds werkt. "https://code-coaching.dev/api" wordt nu automatisch toegevoegd aan elke request.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/fb72cefba88b696b8e392671fbfab02f5db7d3b1)

### Interceptor authorization

```sh
mkdir src/interceptors && touch src/interceptors/auth.interceptor.ts
```

`auth.interceptor.ts`

```ts
import type { InternalAxiosRequestConfig } from "axios";

export const authInterceptor = (req: InternalAxiosRequestConfig) => {
  console.log(req);
  return req;
};
```

Momenteel loggen we de request en geven we de request ongewijzigd terug. Om de interceptor te gebruiken, moeten we deze toevoegen aan de `Axios instantie`.

`api.service.ts`

```ts
import axios, { type InternalAxiosRequestConfig } from "axios";

const api = axios.create({
  baseURL: "https://code-coaching.dev/api",
});

/**
 * Merk op dat de anonieme functie exact hetzelfde doet als de authInterceptor functie
 */
api.interceptors.request.use((req: InternalAxiosRequestConfig) => {
  console.log(req);
  return req;
});

const useApi = () => {
  return { api };
};

export { useApi };
```

In plaats van de anonieme functie te gebruiken, kiezen we ervoor om de `authInterceptor`-functie te gebruiken.

`api.service.ts`

```ts
import { authInterceptor } from "@/interceptors/auth.interceptor";
import axios from "axios";

const api = axios.create({
  baseURL: "https://code-coaching.dev/api",
});

api.interceptors.request.use(authInterceptor);

const useApi = () => {
  return { api };
};

export { useApi };
```

Bij het uittesten van de applicatie zal je zien dat de request elke keer gelogd wordt wanneer er een HTTP call gebeurt.

We zullen nu de `Authorization` header toevoegen aan de request in de `authInterceptor`-functie.

```ts
import type { InternalAxiosRequestConfig } from "axios";

export const authInterceptor = (req: InternalAxiosRequestConfig) => {
  const authToken = localStorage.getItem("token");
  if (authToken) {
    req.headers.Authorization = `Bearer ${authToken}`;
  }
  return req;
};
```

We onderscheppen elke request. Indien er een token in de localStorage zit, dan voegen we de token toe aan de header van de request.

`hero.service.ts` - voor wijziging

```ts
import type { Hero, HeroBackend } from "@/components/models";
import { computed, ref, type Ref } from "vue";
import { useApi } from "./api.service";

const { api } = useApi();

const heroes: Ref<Array<Hero>> = ref([]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  const topHeroes = computed(() => {
    return heroes.value.slice(-4);
  });

  const findHero = (heroId: number) => {
    return api
      .get<HeroBackend>(`/heroes/${heroId}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .then((res) => {
        const heroBackend = res.data;
        const hero = {
          number: heroBackend.id,
          name: heroBackend.name,
        } satisfies Hero;

        return hero;
      });
  };

  const updateHero = (hero: Hero) => {
    return api
      .patch(
        `/heroes/${hero.number}`,
        {
          name: hero.name,
        },
        {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
      .then(() => {
        selectedHero.value = null;
      });
  };

  const deleteHero = (hero: Hero) => {
    return api
      .delete(`/heroes/${hero.number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .then(() => {
        selectedHero.value = null;
      });
  };

  const addHero = (name: string) => {
    return api.post(
      "/heroes",
      { name },
      {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      }
    );
  };

  const loadHeroes = () => {
    api
      .get<Array<HeroBackend>>("/heroes", {
        headers: {
          Authorization: `Bearer ${localStorage.getItem("token")}`,
        },
      })
      .then((res) => {
        heroes.value = res.data.map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          return hero;
        });
      });
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    findHero,
    updateHero,
    deleteHero,
    addHero,
    loadHeroes,
  };
};

export { useHeroes };
```

`hero.service.ts` - na wijziging

```ts
import type { Hero, HeroBackend } from "@/components/models";
import { computed, ref, type Ref } from "vue";
import { useApi } from "./api.service";

const { api } = useApi();

const heroes: Ref<Array<Hero>> = ref([]);

const selectedHero: Ref<null | Hero> = ref(null);

const useHeroes = () => {
  const topHeroes = computed(() => {
    return heroes.value.slice(-4);
  });

  const findHero = (heroId: number) => {
    // verwijder het object met de headers
    return api.get<HeroBackend>(`/heroes/${heroId}`).then((res) => {
      const heroBackend = res.data;
      const hero = {
        number: heroBackend.id,
        name: heroBackend.name,
      } satisfies Hero;

      return hero;
    });
  };

  const updateHero = (hero: Hero) => {
    // verwijder het object met de headers
    return api
      .patch(`/heroes/${hero.number}`, {
        name: hero.name,
      })
      .then(() => {
        selectedHero.value = null;
      });
  };

  const deleteHero = (hero: Hero) => {
    // verwijder het object met de headers
    return api.delete(`/heroes/${hero.number}`).then(() => {
      selectedHero.value = null;
    });
  };

  const addHero = (name: string) => {
    // verwijder het object met de headers
    return api.post("/heroes", { name });
  };

  const loadHeroes = () => {
    // verwijder het object met de headers
    api.get<Array<HeroBackend>>("/heroes").then((res) => {
      heroes.value = res.data.map((heroBackend) => {
        const hero = {
          number: heroBackend.id,
          name: heroBackend.name,
        } satisfies Hero;
        return hero;
      });
    });
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    findHero,
    updateHero,
    deleteHero,
    addHero,
    loadHeroes,
  };
};

export { useHeroes };
```

Start de applicatie en navigeer rond. Je zal zien dat de applicatie nog steeds werkt. De code is nu een stuk "cleaner".

Alle wijzigingen: [GitHub](https://github.com/code-coaching/vue-tour-of-heroes/commit/4d17f84cdffde265218fc62983ea539fa66b2c03)

## Conclusie

Er komt meer bij kijken wanneer de data beheerd wordt op een externe locatie. In deze tutorial is gekeken naar Axios om API calls te doen. Er is gekeken naar `axios.create` om een instantie met een `baseURL` aan te maken. Er is gekeken naar interceptors om de verzoeken naar de API te voorzien van een authorization token.

Er is gekeken naar Promises en hoe we de data kunnen bekijken (`.then()`) en transformeren (wijzigen van de data in de `.then()` en de gewijzigde data teruggeven). Er is gekeken hoe een error afgehandeld kan worden (`try/catch`). En er is gekeken hoe we kunnen weten wanneer een call klaar is (`.then()` in de component).

Services verzamelen alle functionaliteit rondom bepaalde data. Bijvoorbeeld `hero.service.ts` is verantwoordelijk voor het ophalen, toevoegen, wijzigen en verwijderen van heroes. In deze applicatie wordt de `state` (de data) ook bijgehouden in de service. In grotere applicaties kan het voorkomen dat er extra abstractielagen worden toegevoegd om de data te beheren.
