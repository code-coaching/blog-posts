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
---

This article starts from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The pages have been provided with elements, data and components. The data has been moved to a composable/service. The final situation is a Quasar project where the data is linked to an API and authentication is required.

The code can be obtained by downloading a `zip` of the `Source code`.

The situation that is used as a starting point is as follows:

![starting situation](/img/blog/quasar-toh-service-functions.gif)

Starting situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-services-composables)

The final situation being worked towards is as follows:

![end situation](/img/blog/quasar-toh-api-finished.gif)

End situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-api)

## API

An `API` (Application Programming Interface) is an interface that allows an application to communicate with another application. In this case, the front end application will communicate with the back end application.

The back-end application is provided by Code Coaching. Register a free account at [https://code-coaching.dev](https://code-coaching.dev/#/sign-up), this account will be used to log in and manage the heroes' data on the server.

## Axios

To make HTTP requests, the `fetch API` of the browser can be used. There are also alternative libraries such as `axios` to implement this functionality.

In this tutorial, `axios` will be chosen.

### Axios dependency

Install the `axios dependency` in the `package.json`:

```sh
npm install axios
```

### Quasar boot file

Within Quasar there is a concept of `boot files`, these are files that are executed before the application is started. Two things must happen to use a boot file:

Create a file in the `src/boot` directory, the name used in this tutorial is `axios.ts`.

`src/boot/axios.ts`

```ts
import axios, { AxiosInstance } from "axios";

const serverUrl = "https://api.code-coaching.dev";
const api = axios.create({ baseURL: serverUrl });

export { api };
```

Add the name of this boot file to the array `boot` in quasar.conf.js:

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

This is **not** the full code, this example is to make clear what needs to happen in the boot file.

### Using the Axios API

Add a new page called `Login.vue` to the `src/pages` directory.

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

Brief explanation of the above code:
When the `Login` button is clicked, the `login()` function is called. This function will make a `post` request to the API, to the `/authentication` endpoint.
The data sent as the body of the request is:

```json
{
  "strategy": "local",
  "email": "info+quasar@code-coaching.dev",
  "password": "SuperSecretPassword"
}
```

If the request is successful **then** the `console.log` function is called with the data from the response. `.then(console.log)` is a shorter syntax for `.then(response => console.log(response))`.

If the request fails, then the **catch** is executed. Here the error is logged to the console via `console.error`, this is the same as `console.log`, but will show the text in red. Also, a message will be shown to the user to let them know that the login failed.

Adding the page to the `src/router/routes.ts` file:

```ts
export const ROUTE_NAMES = {
  DASHBOARD: "Dashboard",
  HERO_LIST: "HeroList",
  HERO_DETAILS: "HeroDetails",
  HERO_ADD: "HeroAdd",
  LOGIN: "Login",
};
```

The `LOGIN` is new. Also add the route to the list of routes in `src/router/routes.ts`:

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

Change the hard-coded email address and password to your email address and password (**do not commit!**) - this is to perform a test to see if the API is properly linked.

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

Change `here@your-email.com` to the email address you used to register an account at [Code Coaching](https://code-coaching.dev/#/sign-up).

Change `HERE_THE_PASSWORD` to the password used to register the account.

(To test the **catch**, enter an incorrect password.)

The test:

- Start the application.
- Open the console (F12 - or right-click `Inspect`).
- Go to `/#/login` in the url and click on the `Login` button.

Inspect the console (or the network call response).

A very large object is returned, the important thing to understand from this is that an `accessToken` is returned.

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

Notice: Data has been removed to make the object clearer.

This `accessToken` is valid for 24 hours and can be used to send requests to the API with the logged in user. Since this token represents your personal user, it is therefore important to **not** share it with others.

This type of token is a **JWT** token. This stands for **J**SON **W**eb **T**oken, it is a standard that is widely used to provide authentication.

Since this token is needed to authenticate the user, it must be stored in the browser and then added to any request that requires authentication. This can be done by storing it in a `Cookie` or by adding it to `Local Storage`. This will be discussed later in this tutorial.

Add two way binding to the input fields so that the username and password are no longer hard-coded in the code.

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

Notice: In principle, it is not necessary to make the `user` object `reactive`. This is only necessary when the view would use this data, so that Vue knows that the data needs to be changed. However, there is no harm in making the `state` of the component `reactive`. Suppose `{{ user.email }}` is added in the view, then Vue will automatically update the value in the view.

Small change in `app.scss`, currently the styling is only applied to inputs of type `text`, but there are inputs without type and inputs with type `email` and `password` in this application, change the selector so it applies to all `input` elements.

Before change:

```scss
body,
input[type="text"],
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
```

After change:

```scss
body,
input,
button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/71730add0a6fc46a7d97eb0de0043313a7bcf690)

## Save JWT in Local Storage

There is now code to log in. On entering the correct data, we see a response in which an `accessToken` is returned. This `accessToken` is a `JSON Web Token (JWT)`, to be able to access it again at a later time, it needs to be stored somewhere.

In this tutorial, since only a string needs to be stored, we will use the Local Storage API which is present by default, it works with strings.

If more data is stored of different data types, for example numbers or booleans or dates... then it is interesting to use the [Quasar LocalStorage Plugin](https://quasar.dev/quasar-plugins/web-storage), this will automatically convert the stored string in Local Storage to the correct data type.

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

Where before it said `.then(console.log)`, which is the shortened syntax for `.then((result) => console.log(result))`, now it says `.then((result: { data: { accessToken: string }}) => {})`. This is because TypeScript is being used. Here it is indicated that `result` is a complex type. Namely, an object with a property called `data`. And Data is again an object with a property called `accessToken` of type `string`.

Next, the accessToken is assigned to a variable `accessToken`. Then it is checked whether this variable is `truthy` (i.e. not undefined or an empty string). If the variable has a value, it is added to Local Storage.

```ts
const accessToken = result.data.accessToken;
if (accessToken) localStorage.setItem("accessToken", accessToken);
```

Note: localStorage is not imported anywhere, this is the same as `window.localStorage`, the `window` object is always present in the browser without having to do any import, on this, among other things, localStorage exists as a property to use the Local Storage API.

Restart the application and log in again with the username and password used to register an account on Code Coaching. After logging in, view the contents of Local Storage.

Firefox
![Local Storage Firefox](/img/blog/quasar-toh-firefox-localstorage.png)

Chrome
![Local Storage Chrome](/img/blog/quasar-toh-chrome-localstorage.png)

## Authentication composable/service

To ensure that user authentication does not have to be implemented on a component-by-component basis, it will be implemented in a composable/service.

Create a new file in `src/services` called `auth.service.ts`.

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

The above code in more detail:

```ts
interface User {
  _id: string;
}
```

This is a TypeScript interface that describes a User object. Currently only `_id` is added to it, in a later step we will look at how to describe the full `model`. Eventually the interface will be moved to `src/components/models.ts`.

```ts
const isAuthenticated = computed(
  () => authenticatedUser.value._id !== undefined
);
```

It uses the `computed` function, taken from Vue `import { computed } from "vue";`. This function returns a computed property, this will automatically update the value of the computed property if the value of the property changes. Specifically, if `authenticatedUser.value._id` changes, the value of `isAuthenticated` will be automatically changed.

`tryToAuthenticate` retrieves the accessToken from Local Storage and attempts to use it to log the user in. If this succeeds, the user is assigned to the `authenticatedUser` variable (this will then subsequently update the `isAuthenticated` computed property). If this fails, it means that the token is no longer valid, then the `authenticatedUser` variable is assigned an empty object and the `accessToken` is removed from Local Storage. You will have to log in manually again.

`login` contains the logic that was first defined in the `Login.vue` page. With the addition that `authenticatedUser` is assigned a value if the login is successful and an `alert()` has also been added. The `console.log` remains temporarily, here we are going to see what the returned model from the server looks like.

`logout` removes the accessToken from Local Storage and sets the `authenticatedUser` variable to `{}` (this will update the `isAuthenticated` computed property). Note that a type casting is done `{} as User`, an alternative syntax is `<User>{}`. This is necessary because `{}` does not satisfy the `User` interface, but a user without \_id is seen as an unauthenticated user, so a type casting is needed.

Every variable and function that should be able to be used is returned as an object with named properties.

```ts
return {
  authenticatedUser: readonly(authenticatedUser),
  isAuthenticated: readonly(isAuthenticated),
  tryToAuthenticate,
  login,
  logout,
};
```

This allows us to call the functions and variables via destructuring. For example:

```ts
import { useAuth } from "src/services/auth.service";

const { login, logout, isAuthenticated } = useAuth();
```

The variables whose value we only want to read are returned as readonly properties. This ensures that the value cannot be changed outside the service.

```ts
return {
  authenticatedUser: readonly(authenticatedUser),
  isAuthenticated: readonly(isAuthenticated),
};
```

### Changes Login.vue

It is no longer the api that is directly addressed, but the `auth.service` that is used to log the user in.

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

A `name collision` has been found between the `login` function extracted from `useAuth()` and the existing `login` function within the component. To ensure that there is no conflict, the `login` function from `useAuth()` is renamed to `authenticate`.

```ts
const { login: authenticate } = useAuth();
```

This can be read as: `const authenticate = useAuth().login;`.
In other words, place the definition of the `login` function from `useAuth()` into a new variable called `authenticate`.
When `authenticate()` is called, it actually calls the `login` function from `useAuth()`.

Also note that the `.then` and `.catch` are handled in the auth service. It then uses `.finally` to reset the `user.email` and `user.password` after login. The callback function passed to `.finally()` is called when the Promise is successful (after the `.then`) or when an error occurs (after the `.catch`).

### Changes MainLayout.vue

A `header` is added to the `MainLayout.vue` page. This bar/header will always be visible on every page, hence the addition in the `MainLayout.vue` page.

In this, two buttons are shown, the `Login` and `Logout`.
It uses the `v-if` directive and the `v-else` directive. An alternative is to use a `v-if` directive twice, once with `v-if="!isAuthenticated"` and once with `v-if="isAuthenticated"`. The `isAuthenticated` is the computed property coming from the service, it will be `true` if the user is logged in and `false` if the user is not logged in.

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

In the `script` tag, the `lifecycle hook` called `onBeforeMount` is used. The code in the callback function that is assigned as a parameter of this hook is executed just before the component is mounted. So just before the component is mounted (is added to the DOM), the function `tryToAuthenticate` is called, this is also functionality provided in the auth service.

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

The styling of the header has been added.

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

### Testing the authentication

Start the application. If an `accessToken` is already present, an attempt is made to authenticate the user. If the token has expired, the token will be deleted.

If the token was valid, the user will be authenticated, if successful a `Logout` button will be seen in the upper right.

If the token was not valid, the user will not be logged in, if this is the case there will be a `Login` button in the upper right.

If you are logged in, click on `Logout`.

Click on `Login`, this will navigate the user to the login page.

Here, enter a valid Code Coaching login and click `Login` next to the entry fields.

If successful an alert will appear that it was successful, the button will automatically change from `Login` to `Logout`. This also means that wherever the auth service is used, the `isAuthenticated` computed property is available to determine whether a user is logged in or not.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d4a6dbc1da66786732ca59bcea0e97ae9a87490f)

## Login page styling

The `Login.vue` page currently looks a little weird. Add some styling to make it look better.

Make use of `StyledButton` instead of the normal `button` element. Place wrapper elements/container elements to improve the placement of the input fields and the button.

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

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/0b03ce0068d659f1b4a485a03c8116b3e3546146)

## User model

Currently, the `interface` of the `User` is in the auth service. This is also called the `model of the User`.

### User model definiëren

Since only the `_id` property is used, it is sufficient to have an interface with only this property. But many more properties exist on the User object returned from the server.

Log back into the application and look in the console for the `User` object that was logged out.

An example of the `User` object:

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

ASince only the `_id` is currently used, there is no need to add additional properties to the interface already. But if the application is going to be extended and the `email`, the `name`, the `permissions` etc. are also going to be used, then these can be added to the interface. Ideally, there is API documentation that shows what the possible properties are and what the possible values are that can be assigned to the properties. If this API documentation is not available, then these properties and values can be derived from the response.

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

Important as a front-end developer to know, things like `permissions` and `isVerified` are available. But **all** the data that is available in the front-end is available to the user in the browser and thus can be changed. It is okay to use this data in the front-end to conditionally show or not show certain routes or to show or not show certain non-important content. But suppose there is certain content that can only be seen by people who are verified and have the permission `vip`. Then this content will always have to be fetched via the back-end and the back-end will have to separately fetch the user and verify that this user has the correct rights.

An example, suppose there is a vip area, which can only be seen by people who have `vip` permission. Then it is possible to manipulate the user object in the front-end/browser to make the vip area visible to someone who does not have the permission in the database. This is not what a normal user will do, but this is something that a developer or someone with bad intentions can do. There is nothing you can do to change this. So what **is** important? Suppose the vip area has paid tutorials on display. Then it is important that these paid tutorials will only be fetched and shown, if the authenticated user also has `vip` in the database.

So on the back-end, the following will need to be done:

- the `accessToken` is read
- the user data from the database will be fetched
- the data from the database will be used to check whether `vip` is present in the `permissions` array

In a nutshell: **Never rely on the data present in the front-end!**

### Moving user model

Since the `User` model will most likely be used in multiple places, it is already moved to `src/components/models.ts`.

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

Note: `export` - so it can be used in other locations via `import`.

At the top of `auth.service.ts`

```ts
import { User } from "src/components/models";
```

Remove the `console.log` that is no longer needed from the `.then`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/ac8a284dedc66445a8c2a12596e94a7b9854074d)

## Connecting Hero service with the API

Until now, hard-coded data was used in the `Hero service`. But, with Code Coaching's account it is possible to manage the `Hero` data on your account, through an API.

### Explanation of how the API works

How the API works. Every API call to `/heroes` needs authentication.

If the `Authorization token` has expired, this will be the response:

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

If the `Authorization token` is not present, this will be the response:

```json
{
  "name": "NotAuthenticated",
  "message": "Not authenticated",
  "code": 401,
  "className": "not-authenticated",
  "errors": {}
}
```

Assuming the `Authorization token` is correct, this will be the response of subsequent API calls:

#### GET (read)

Retrieval of all Heroes.

`HTTP GET https://api.code-coaching.dev/heroes`

The response:

```json
{
  "total": 0,
  "limit": 10,
  "skip": 0,
  "data": []
}
```

This user currently has no heroes.

Retrieving a specific Hero.

After creating (POST) a hero, try making the API call below:

`HTTP GET https://api.code-coaching.dev/heroes/:id`
Where `:id` is the `_id` of the hero.
For the example created at the `POST` below, this is:
`HTTP GET https://api.code-coaching.dev/heroes/62882f786f3b143ad3a37558`

The response:

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

#### POST (create)

`HTTP POST https://api.code-coaching.dev/heroes`

The body that is sent along:

```json
{
  "name": "Superman",
  "id": 69
}
```

The response:

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

Creating a `Hero` with the same `name` and the same `id` is possible, but `_id` will be different, because of this every Hero, even the one with the same `name` and the same `id`, is still unique.

Note: `69` is sent as a number in the body, but the value returned is `"69"`, a string.

Note: the model of the back-end is different from the model in the front-end.

Current front-end model:

```ts
interface Hero {
  name: string;
  number: number;
}
```

Model of the back-end:

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

This may seem like a weird model due to `_id` and `id`, but `_id` is a unique identifier assigned by the back-end. `id` is a number chosen by the user while creating a hero through the front-end, this is what corresponds to `number` in the front-end model. The type of the property `number` is `number` and the type of `id` is `string`. This will need to be refactored in the front-end (later in this tutorial). `id` is **not unique**, `_id` **is unique**.

`belongsTo` is the `_id` of the user who created the hero.

`createdAt` is a timestamp in string format of the time the hero was created.
`updatedAt` is a timestamp in string format of the time the hero was last updated.

`__v` is a version number of MongoDB, the database technology used.

#### PATCH (update)

Often PUT is used to change data, but in doing so the entire object is overwritten.
With PATCH it is possible to change specific properties.

`HTTP PATCH https://api.code-coaching.dev/heroes/:id`

Where `:id` is the `_id` of the hero to be changed., for example assuming that the name of the hero created in the POST call is to be changed:
`HTTP PATCH https://api.code-coaching.dev/heroes/62882f786f3b143ad3a37558`

The body that is sent along:

```json
{
  "name": "Superwoman"
}
```

The response:

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

Note: The `name` has been changed to `Superwoman` and automatically `updatedAt` has also been changed, everything else is identical.

#### DELETE

`HTTP DELETE https://api.code-coaching.dev/heroes/:id`

Where `:id` is the `_id` of the hero to be changed., for example assuming that the name of the hero created in the POST call is to be changed:
`HTTP DELETE https://api.code-coaching.dev/heroes/62882f786f3b143ad3a37558`

The response:

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

### Linking API in hero.service.ts

Since it is now possible to `persist` (permanently store) the heroes, this is exactly what is going to happen!

The list of heroes that is now hard-coded will be replaced by a list of heroes that is fetched from a database via the API.

Changing a hero will change the hero in the database.

Delete a hero, will effectively remove the hero from the database.

Add a hero will add a new hero to the database.

#### Retrieve list of heroes

Currently, there is an array of hard-coded heroes.

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

A function will be added to the `hero.service.ts` to retrieve the list of heroes.

There are two options to work with Promises, one option is to work with `.then`.

This can be read as 'the code promises to retrieve the list of heroes, if the data is returned, **then** (`.then`) the code block of the callback function is executed'.

```ts
const getHeroes = () => {
  void api.get("/heroes").then((result: { data: Array<Hero> }) => {
    console.log("result", result);
    heroes.value = result.data;
  });
};
```

A second way is to work with `async/await`.

The function is given the `async` keyword and in the code block of the function, `await` can then be used.

This can be read as `I'll wait to execute the rest of the code until the data is returned'.

```ts
const getHeroes = async () => {
  const result = await api.get("/heroes");
  console.log("result", result);
  heroes.value = result.data as Array<Hero>;
};
```

Add this function to the return statement of the `useHeroes` function and use it in `MainLayout.vue`.

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

Start the application and log in, refresh, look at the console or at the network tab. The `getHeroes` function is called as soon as the application starts (`onBeforeMount` in `MainLayout.vue`). This will make an HTTP call to the server, but no result will be returned.

Result in the network tab of the `/heroes` call.

```json
{
  "name": "NotAuthenticated",
  "message": "Not authenticated",
  "code": 401,
  "className": "not-authenticated",
  "errors": {}
}
```

#### Add authorization header

The `getHeroes` function needs the `accessToken` from Local Storage to invoke the API with authentication.

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

`const accessToken = localStorage.getItem("accessToken") || "";` the syntax `|| ""` causes an empty string to be assigned if the `accessToken` does not exist.

Make sure you are logged in, refresh and look at the console or network tab again. Now data will be returned. The response will look different for everyone.

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

It can be inferred from the response that pagination is available. This is not discussed in this tutorial. But it is important to know that a maximum of 10 heroes are returned (``limit: 10`). If more than 10 heroes have been created in the application, delete a few heroes.

#### TypeScript axios

Since we are working with TypeScript and since it is desired to get auto-completion on the result, it is necessary to add the typings.

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

It is possible to add the type of the response to the api call.

```ts
api.get<Paged<Hero>>();
```

The type describes the type of the value that will be assigned to the `data` property of the axios response.

`Paged` is an interface added at the top of the `hero.service.ts` file.

```ts
interface Paged<T> {
  total: number;
  limit: number;
  skip: number;
  data: Array<T>;
}
```

Here, a `Generic` (`<T>`) is used.
Some examples to clarify:

```ts
const exampleString: Paged<string>;
```

This means that the `data` property will contain an array of strings.

```ts
const exampleHero: Paged<Hero>;
```

This means that the `data` property will contain an array of `Hero` objects.

The result of the response is an `AxiosResponse`, this in itself is also a generic, the typing below makes it clear what the actual type of the response is.

```ts
const result: AxiosResponse<Paged<Hero>>;
```

This means that the `data` property of the AxiosResponse will be of type `Paged`. `Paged<Hero>` means that the `data` property of the paged response will contain an array of `Hero` objects.

For example:

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

Note: `config`, `headers` and `request` are normally objects with properties, these are left empty to keep the example shorter.

This means, that the array with heroes can be obtained by `result.data.data`.
Assign the array to the `ref` variable `heroes`.

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

Confirm in the console that the array is printed. If no heroes have been created yet, the array will be empty.

This change has introduced some bugs. To show these bugs, the `addHero` function will be changed first.

#### Add Hero

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

A technique called `naive update` is used to ensure that the user can see the new hero immediately. If something goes wrong, causing the hero to not be added to the database, then this naive update will be reversed and the user will be notified that something has gone wrong via an alert.

Notice that a change has happened:

```ts
const maxNumber = Math.max(...heroes.value.map((h) => h.number));
```

```ts
let maxNumber = Math.max(...heroes.value.map((h) => h.number));
if (maxNumber === -Infinity) maxNumber = 0;
```

Previously, it was not possible to have no heroes. Now that this is possible, `Math.max()` will evaluate to `-Infinity`. This is handled by the rule:

```ts
if (maxNumber === -Infinity) maxNumber = 0;
```

Start the application, make sure you are logged in and add two heroes.

It seems to be working.

![Add Hero - working](/img/blog/quasar-toh-add-hero-working.png)

But there is another bug, refresh the browser.

![Add Hero - not working](/img/blog/quasar-toh-add-hero-not-working.png)

Both heroes appear to be selected and the number is not shown.

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

This is because the class `hero--active` is added when `hero.number === selectedHero?.number` evaluates to `true`. Because `.number` is not a property on the model returned from the back-end, `.number` will be `undefined`. Since a hero was never selected, `selectedHero?.number` will also be `undefined`.

To solve this, the model returned from the back-end will need to be transformed to the model used in the front-end.

Add an interface to `src/components/models.ts`:

```ts
export interface BackendHero {
  _id: string;
  id: string;
  name: string;
}
```

Use this interface instead of the interface `Hero` in the API calls.

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

The `.map()` iterates over the array of `BackendHero` objects and transforms each object into the `Hero` object used in the frontend.

This produces a TypeScript error.

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

When linking the back-end, it becomes clear that the front-end model does not match the reality. Refactor the front-end model.

```ts
export interface Hero {
  number: string;
  name: string;
}
```

When compiling the application, a whole bunch of other errors are now thrown up.

Change the type and initialization of the heroes ref.

```ts
const heroes: Ref<Array<Hero>> = ref([]);
```

Change the type of the parameter `findHero` and remove the type casting to number.

Before change:

```ts
const findHero = (id: number): Hero | undefined => {
  const matchingHero = heroes.value.find((h) => h.number === +id);
  if (matchingHero) return { ...matchingHero };
};
```

After change:

```ts
const findHero = (id: string): Hero | undefined => {
  const matchingHero = heroes.value.find((h) => h.number === id);
  if (matchingHero) return { ...matchingHero };
};
```

Add type casting in the code block of the `addHero` function.

Before change:

```ts
let maxNumber = Math.max(...heroes.value.map((h) => h.number));
if (maxNumber === -Infinity) maxNumber = 0;

const newHero = { number: maxNumber + 1, name };
```

After change:

```ts
let maxNumber = Math.max(...heroes.value.map((h) => +h.number));
if (maxNumber === -Infinity) maxNumber = 0;

const number = (maxNumber + 1).toString();
const newHero = { number, name };
```

#### Delete Hero

From the explanation of how the API works, it is known that `_id` must be used to remove a hero. Add this to the front-end model.

```ts
export interface Hero {
  _id?: string;
  number: string;
  name: string;
}
```

Properties with a `?` are optional properties. Since `_id` is not known when a naive update is done in `addHero`, this property still needs to be added when `addHero` is executed successfully.

Before change:

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

After change:

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

API linking in `deleteHero`.

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

Note that again a `naive update` is used. The hero is removed before the API call is made, the index is tracked. If the call fails, the hero will be put back in the same place and the user will be informed via an alert.

#### Updating Hero

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

Again, a `naive update` is used. The old hero is kept, if the API call fails, the old hero is restored and the user is informed via an alert.

Remove all `console.log` statements from the `hero.service.ts`.

One last minor change to `HeroList.vue`, change `'hero--active': hero.number === selectedHero.number,` to `'hero--active': hero._id === selectedHero?._id,`. Since it is possible to create a hero with the same `number` (not in this application, but suppose the API is called via Postman or via a terminal), the unique `_id` is now used to determine if a hero is selected.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/a42d394dd98865d0e1e2422a9b5202adc4d9b79e)

**BUG**: The bug is mentioned at the end of the tutorial and is also fixed there. In short: `api.delete` should be `api.patch`. And the second parameter should be `hero`. The commit with the bugfix: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92e3f6614601db5bd23bb60cdf3397aede6c9aff)

### Refactor API calls

With each call to the API, a `headers` object must be passed along. This object contains the `Authorization` header with the access token. This is currently implemented in every method that makes a call to the API.

Refactor this to a separate function at the top of the file.

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

Use this function in any method that makes a call to the API. Delete code that is handled by this function.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/9e1e5bde3b4ec94823677e80c25b4de261e048a2)

## UX improvements

### Login redirect

Because the entire application now depends on being logged in, it is a good idea to redirect the user to the login page if the application is visited and no user is logged in yet.

In `auth.service.ts`, refactor the `tryToAuthenticate` function:

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

If no user is logged in, or the token is not valid, the promise will do a `reject()`, this is caught in the `.catch()`.
If a user is logged in, the promise will do a `resolve()`, this is caught in the `.then()`.

When the user is logged in, the `getHeroes()` function is called to retrieve the list of heroes.

If the user is not logged in, the user is redirected to the login page.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/52b52c88734a774f73b196d9fcca5c7a54570111)

### Dashboard redirect

After logging in to the login page, the user should be redirected to the dashboard.

Refactor the `login` function in `auth.service.ts`:

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

Add the redirect in `Login.vue`:

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

The `router` and `ROUTE_NAMES` need to be imported.

Add a `watch` to `MainLayout.vue`:

```ts
watch(isAuthenticated, (newValue) => {
  if (newValue) {
    void getHeroes();
  } else {
    void router.push({ name: ROUTE_NAMES.LOGIN });
  }
});
```

The `watch` function will monitor (`watch`) the property `isAuthenticated`. When it changes the `callback` function will be executed, here the new value of the variable will be passed as a parameter.

When testing the application it can be noticed that after the redirect of the dashboard, the top heroes are not shown. To solve this, a `computed` property is needed.

In `hero.service.ts`:

Before change:

```ts
const topHeroes = heroes.value.slice(0, 4);
```

After change:

```ts
const topHeroes = computed(() => heroes.value.slice(0, 4));
```

`computed` is imported via `vue`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/64f213936c79cd89ce3fe6ad89569585f6900145)

### Auth guard

DBecause the application now depends on being logged in, it is possible to redirect the user to the login page if the user is not logged in and still requests a page where the user should be logged in.

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

The logic of `onBeforeMount` is moved to a `watch`. The `watch` keeps an eye on the property `route.path`. When it changes, the `callback` function is executed. If the user is not yet logged in, the `tryToAuthenticate()` function is called. Also note the `{ immediate: true }` parameter, this ensures that it is also executed immediately when the application is loaded, this replaces the `onBeforeMount` functionality.

Unlike the `watch` that monitors a ref variable (the `watch` function that monitors `isAuthenticated`), it is necessary to use `() => route.path` when nested properties need to be monitored.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/627f1c75393a3ac46bd2514e962d3c0f048e5254)

## Bugfix

In testing out all the functionality, there is a bug to note.

- Add a hero
- Click on the hero in the list
- Click on 'details
- Change the name
- Click on `save

Wat er verwacht wordt, is dat de hero wordt opgeslagen met de nieuwe naam.
Wat er effectief gebeurt, is dat de hero wordt verwijderd.

What is expected is that the hero is saved with the new name.
What effectively happens is that the hero is deleted.

In looking at the code responsible for saving the change to a hero, it is quickly apparent:

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

It says `api.delete()`, this is exactly what happens. What needs to happen is `api.patch()`. With `patch` it is also necessary to send the new object along.

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

The lesson to be learned from this is that it is extremely important to test every functionality after implementation. It is also important to understand that people make mistakes, bugs are part of development. In this case, the bug came up during `QA` (**Q**uality **A**ssurance).

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/92e3f6614601db5bd23bb60cdf3397aede6c9aff)

## Conclusion

There is more to it when the data is managed at a remote location. This tutorial teaches how to link an API to a Quasar application with Axios. Because the remote location also requires authentication, we looked at the possibility of a user logging in.

Composables/Services collect all functionality around certain data. For example, all the actions a user can perform on Hero data sticks in the `hero.service.ts` file. Because this functionality is in the `hero.service.ts` file, it is relatively easy to change the implementation. In this tutorial, the functions have been changed to functions that do the changes through the API. `naive updates` have also been introduced to change the data directly for the user.
