---
postUuid: efa86eb4-7174-4578-a563-f324ad9c9d1b
title: Quasar layouts - Quasar-pages - Vue Router
slug: quasar-layouts-quasar-pages-vue-router
tags:
  - Quasar
  - Vue
  - Vue Router
categories:
  - Frontend
---

Start a new Quasar project to follow along step by step.

## What is Vue Router

Vue router is a `library` that allows you to define a `router` in a Vue application. A `router` is a solution to provide multiple routes in an `SPA (Single Page Application)`.

As an example, `/` could be the route to show the main page and `/admin` could be the route to show the admin page.

In Quasar the Vue Router is configures in `src/router/index.ts`.

src/router/index.ts (the automatically generated commentary is deleted)

```ts
import { route } from "quasar/wrappers";
import {
  createMemoryHistory,
  createRouter,
  createWebHashHistory,
  createWebHistory,
} from "vue-router";
import routes from "./routes";

export default route(function () {
  const createHistory = process.env.SERVER
    ? createMemoryHistory
    : process.env.VUE_ROUTER_MODE === "history"
    ? createWebHistory
    : createWebHashHistory;

  const Router = createRouter({
    scrollBehavior: () => ({ left: 0, top: 0 }),
    routes,

    history: createHistory(
      process.env.MODE === "ssr" ? void 0 : process.env.VUE_ROUTER_BASE
    ),
  });

  return Router;
});
```

No changes are necessary in this file. The important thing to understand is that this is the location where the router is defined.

```ts
// ...
const Router = createRouter({
  scrollBehavior: () => ({ left: 0, top: 0 }),
  routes,

  history: createHistory(
    process.env.MODE === "ssr" ? void 0 : process.env.VUE_ROUTER_BASE
  ),
});
// ...
```

The Router is created and the `routes` are the routes imported through `src/router/routes.ts`.

src/router/router.ts

```ts
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [{ path: "", component: () => import("pages/Index.vue") }],
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

There are two routes defined. The bottom route has the `path` `/:catchAll(.*)*` which is a wildcard route, this will load the component `pages/Error404.vue` if the route is not defined.

To test this, start the project:

```sh
quasar dev
```

Go to `http://localhost:8080/#/example`.

A page is shown, this is loaded through `src/pages/Error404.vue`. This is because `/example` is not a known route, which results in a match on the wildcard route `/:catchAll(.*)*`. This will load `src/pages/Error404.vue`.

src/pages/Error404.vue

```html
<template>
  <div
    class="fullscreen bg-blue text-white text-center q-pa-md flex flex-center"
  >
    <div>
      <div style="font-size: 30vh">404</div>

      <div class="text-h2" style="opacity: 0.4">Oops. Nothing here...</div>

      <q-btn
        class="q-mt-xl"
        color="white"
        text-color="blue"
        unelevated
        to="/"
        label="Go Home"
        no-caps
      />
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "Error404",
  });
</script>
```

Delete everything in the template tag and change to some simple HTML. In the `<template>` tag, all normal HTML can be applied.

src/pages/Error404.vue after changing the `<template>` tag.

```html
<template>
  <h1>404</h1>
  <div>Page not found!</div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "Error404",
  });
</script>
```

Visit `http://localhost:8080/#/example` again.

Now, add a `style` tag and use some CSS knowledge to style the elements.

src/pages/Error404.vue after adding the `<style>` tag.

```html
<template>
  <div id="page404">
    <div id="content-wrapper">
      <h1>404</h1>
      <div id="sub-text">Page not found!</div>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "Error404",
  });
</script>

<style lang="scss" scoped>
  #page404 {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;

    background-color: deeppink;
    color: white;
  }

  #content-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #sub-text {
    font-size: 3em;
  }
</style>
```

Look at the changes at [GitHub](https://github.com/code-coaching/blog-posts-examples/compare/53e9e05..981d873?diff=split) (the HTML content is Dutch, all the rest is similar).

The result looks a lot like the initial content. But this time without using helper classes (e.g. `fullscreen`, `q-pa-md`) of Quasar and without using custom components (e.g. `<q-btn />`) of Quasar.

Quasar has a lot of custom components, this is not looked at in this article.
Quasar has a lot of helper classes, this is not looked at in this article.

It is not obligatory to use Quasar components, it is possible to use plain HTML in the `<template>` tag and CSS in the `<style>` tag.

Only the `<script>` tag is left to discuss, this is not looked at in this article. This is the place where JavaScript/TypeScript knowledge can be applied.

## Pages

All `.vue` files are components. Pages as well. Add a new file to the directory `src/pages` named `First.vue`.

Add a `<template>` tag and a `<script>` tag in the file. Since TypeScript is used, add the `lang` attribute with the value `ts` to the `<script>` tag.

src/pages/First.vue

```html
<template>
  <h1>The page First.vue works!</h1>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      console.log("First.vue: This is shown in the console!");
    },
  });
</script>
```

In the `<script>` tag a new component is defined with the function `defineComponent()`. This function accepts a configuration object as parameter. In the configuration object, the `setup()` function is defined. This function is called when the component is created.

When the component is shown in the browser, the `setup()` function is called. In this example something will be printed in the console. And then, the content of the `<template>` tag is shown.

### Load a page through the router

Add a new route in the `src/router/routes.ts` file.

src/router/router.ts

```ts
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [{ path: "", component: () => import("pages/Index.vue") }],
  },

  {
    path: "/first",
    component: () => import("pages/First.vue"),
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Visit `http://localhost:8080/#/first` in the browser. The content of the `<template>` tag is shown. Take a look at the console of the browser, here `First.vue: This is shown in the console!` is printed.

Create a second page named `Second.vue` with identical content. Change `First.vue` to `Second.vue` in both the template and the console.log. Make sure this page is loaded through the router on `/second`. Visit both `/#/first` and `/#/second` in the browser and look at how the content of the page changes.

Look at the changes at [GitHub](https://github.com/code-coaching/blog-posts-examples/compare/981d873..ddf3a38?diff=split) (the HTML content is Dutch, all the rest is similar).

## Layouts

Imagine there is a fixed header and a fixed footer that must be shown on both pages. Then a solution is to add them on both pages. But imagine something must be changed in the footer, then this must be done on both places. There is a way that this is not necessary, but instead, it needs to be done only once. This is achieved by using a `<router-view />`.

In Quasar, this is done in a component in the directory `src/layouts`. Add a new file to `src/layouts`, called `PrimaryLayout.vue`.

src/layouts/PrimaryLayout.vue

```html
<template>
  <header>Code Coaching</header>

  <router-view />

  <footer>
    <a href="mailto:info@code-coaching.dev">info@code-coaching.dev</a>
  </footer>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      console.log("PrimaryLayout setup is being executed!");
    },
  });
</script>

<style lang="scss" scoped>
  header {
    background-color: $primary;
    position: sticky;
    top: 0;
    padding: 1em;
    font-size: 1.5em;
    color: white;
  }

  footer {
    background-color: $secondary;
    position: fixed;
    bottom: 0;
    padding: 1em;
    width: 100%;
  }

  footer a {
    text-decoration: none;
    color: white;
  }
</style>
```

Note: Some of the SCSS variables defined by Quasar in `src/css/quasar.variables.scss` are used. By using these variables instead of hard-coded color codes, it is easier to change the look of the whole website by changing the colors in one location.

When this component is loaded, the `setup()` function is executed and `PrimaryLayout setup is being executed!` is printed in the console. The content of the `<template>` tag is shown in the browser. The `<route-view />` tag is the location where other components are loaded. This becomes clear with the changes in `src/router/routes.ts`.

### Loading layouts through router

src/router/routes.ts

```ts
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/PrimaryLayout.vue"),
    children: [
      {
        path: "first",
        component: () => import("pages/First.vue"),
      },
      {
        path: "second",
        component: () => import("pages/Second.vue"),
      },
    ],
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];

export default routes;
```

Notice there are currently two routes, `/` and `"/:catchAll(.*)*"`. The first route loads the `PrimaryLayout` component. It also defines `children`. In this example there are two routes defines:

1. `"first"`, which loads the `First.vue` component.
2. `"second"`, which loads the `Second.vue` component.

The way this can be read is that visiting `/first`, will load the component on `/` (`PrimaryLayout`) and then the component on `first` is loaded (`First.vue`).

In the `<template>` tag of the `PrimaryLayout.vue` the follwing can be found:

```html
<template>
  <header>Code Coaching</header>

  <router-view />

  <footer>
    <a href="mailto:info@code-coaching.dev">info@code-coaching.dev</a>
  </footer>
</template>
```

By visiting `http://localhost:8080/#/first`, the `PrimaryLayout.vue` is loaded (because of the match on `"/"` in `src/router/routes.ts`). Next the `children` are looked at for a match, here is a match on `"first"`. The component `First.vue` is loaded in the `<router-view>` tag.

Same goes for visiting `http://localhost:8080/#/second`. `PrimaryLayout.vue` is loaded, then `Second.vue` is loaded in the `<router-view>` tag.

The result is that the header and the footer are always shown and the other component is shown on the location of the `<router-view />` tag.

Multiple layouts can be handy, for example to have a unique layout for normal visitors and a unique layout for admins of the website.

Take a look at the changes at [GitHub](https://github.com/code-coaching/blog-posts-examples/compare/ddf3a38..e08f465?diff=split).

## Conclusion

Vue router provides functionality to provide multiple routes in a Vue application. With the `<router-view />` tag it is possible to define the location where child components are loaded. This is possible by adding `children` in `src/router/routes.ts` to an existing route.

Quasar provides a project structure to place components that represent a layout in a directory called `src/layouts`. It also provides a directory called `src/pages` to place components that represent a page. This is a good way to keep the code clean and easy to maintain.

Code can be found at [GitHub](https://github.com/code-coaching/blog-posts-examples/tree/quasar-vue-router)!
