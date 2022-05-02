---
postUuid: 32e8808f-e109-4b0f-ab7a-afe0fb3422db
title: Quasar Tour of Heroes - Composables & Services
slug: quasar-toh-composables-and-services
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

This article starts from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The pages are provided with elements, data and components. The starting situation is an existing Quasar project with a layout component and working pages. The final situation is a Quasar project in which the data has been moved to services.

The code can be obtained by downloading a zip of the source code.

The situation that is used as a starting point is as follows:

![starting situation](/img/blog/quasar-toh-components.gif)

Starting situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-components)

The final situation being worked towards is as follows:

![end situation](/img/blog/quasar-toh-service-functions.gif)

End Situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-services-composables)

## Composables & Services

A composable is a collection of reusable functionality. This is also sometimes called a service.

Currently, there is a "problem" with the `hero` data. The list of `hero` objects is defined multiple times in different components.

Both the `HeroDetails` page and the `HeroList` page define the list of `hero` objects:

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

This is a "problem". When a new hero is added, deleted or changed it would need to be changed on all pages.

## Hero Service

Create a service to manage the `hero` data. Then use the service at the location where the list of `hero` objects is used.

When creating the service, it is important to understand whether a variable is scoped or global. To demonstrate this, a smaller list is used.

Scoped within the function:

```ts
import { ref } from "vue";

const useHeroes = () => {
  const heroes = ref([
    { number: 11, name: "Mr. Nice" },
    { number: 12, name: "Narco" },
  ]);

  return {
    heroes,
  };
};

export { useHeroes };
```

In the case of a variable in the scope of the function, a new list of `hero` objects will be created **everywhere**.

Global scope:

```ts
import { ref } from "vue";

const heroes = ref([
  { number: 11, name: "Mr. Nice" },
  { number: 12, name: "Narco" },
]);

const useHeroes = () => {
  return {
    heroes,
  };
};

export { useHeroes };
```

In the case of a variable in the global scope of the file, **the same list** of `hero` objects will be used everywhere.

Since the list of `hero` objects must point to the same list everywhere, the code will use a `ref` variable in the global scope.

Create a new folder in the `src` folder named `services`. In this folder provide a service/composable named `hero.service.ts`.

The file defines the `hero` data:

```ts
import { ref } from "vue";

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

const useHeroes = () => {
  return {
    heroes,
  };
};

export { useHeroes };
```

Then import the service in the places where a hard-coded list of `hero` objects is currently used.

Replace the hard-coded data in both `HeroList.vue` and `HeroDetails.vue` with the data from the service.

Delete the hard-coded list:

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

Add the import of the service:

```ts
import { useHeroes } from "src/services/hero.service";
```

Use the heroes from the service:

```ts
const { heroes } = useHeroes();
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/f4ff9c923fb9964540e686e0de23e6773705fb63)

## Selected Hero

The `selectedHero` is currently only used in the `HeroList` page. So it is not necessarily necessary to move it to the service. However, it is useful to move all data involving heroes to the service, this way work can always be done via the service when working with `hero` data.

Move the `selectedHero` to the service:

```ts
import { Hero } from "src/components/models";
import { ref, Ref } from "vue";

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

const selectedHero: Ref<Hero> = ref() as Ref<Hero>;

const useHeroes = () => {
  return {
    heroes,
    selectedHero,
  };
};

export { useHeroes };
```

Here it is important to decide whether the variable is global or scoped. If the variable is scoped in the function, there will be a unique `selectedHero` per component where the service is used. If the variable is global, the same `selectedHero` will be used everywhere.

So with the choice made above, there will be one selectedHero for the entire project. Everywhere where `useHeroes()` is used, from which the `selectedHero` is then extracted, it will have the same value.

In the `HeroList.vue`, the `selectedHero` is imported from the service:

```ts
const { heroes, selectedHero } = useHeroes();
```

Confirm that selecting a hero in the list still works on the `HeroList` page.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/9e03cce4f20713f838f8c09cd35eadcbad7fb4eb)

## Top Heroes

Currently, the top heroes are displayed in the `Dashboard` page. This is currently just hard-coded data in the template of the `Dashboard` page. But, these are the names of `hero` objects that are listed in the service.

To ensure that the service can be used, a `script` tag must be added. This is where the service is imported.

```html
<template>
  <div class="title">Top Heroes</div>

  <div class="top-heroes">
    <div v-for="(hero, index) in topHeroes" :key="index" class="top-hero">
      {{ hero.name }}
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent, computed } from "vue";
  import { useHeroes } from "src/services/hero.service";

  export default defineComponent({
    setup() {
      const { heroes } = useHeroes();

      const topHeroes = heroes.value.slice(0, 4);

      return {
        topHeroes,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
    display: flex;
    justify-content: center;
    margin-bottom: 1rem;
  }

  .top-heroes {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 2rem;
  }

  .top-hero {
    display: flex;
    justify-content: center;
    align-items: center;

    background-color: #5f7d8c;
    color: white;

    height: 8rem;
    width: 10rem;
    font-weight: 600;
    font-size: 1.1rem;

    &:hover {
      background-color: #eeeeee;
      color: #5f7d8c;
      cursor: pointer;
    }
  }
</style>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/8e735ac022ca8e24321d0cfddf619ebb6062c059)

### Moving topHeroes to the service

It was discussed that it would be convenient to move all data around heroes to the service. The `topHeroes`-property is currently added in the `Dashboard` page. It will be moved to the service:

```ts
import { Hero } from "src/components/models";
import { ref, Ref } from "vue";

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

const selectedHero: Ref<Hero> = ref() as Ref<Hero>;

const useHeroes = () => {
  const topHeroes = heroes.value.slice(0, 4);

  return {
    heroes,
    selectedHero,
    topHeroes,
  };
};

export { useHeroes };
```

Note that the `topHeroes` variable is in the scope of the function. A simple rule to follow is to place `state` in the global scope and place derived values in the scope of the function. Also, functions that change the `state` in the global scope can be placed in the scope of the function.

The `script` section of the `Dashboard` page is modified:

```ts
import { defineComponent } from "vue";
import { useHeroes } from "src/services/hero.service";

export default defineComponent({
  setup() {
    const { topHeroes } = useHeroes();

    return {
      topHeroes,
    };
  },
});
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/828571746cf61ff4884828d46ebec2311a90d0f7)

## Click through from dashboard to detail

Add a function that navigates to the detail page when a hero is clicked.

In the setup of the `Dashboard` page, the function `navigateToHero` is added:

```ts
const router = useRouter();

const navigateToHero = (hero: Hero) => {
  void router.push({
    name: ROUTE_NAMES.HERO_DETAILS,
    params: {
      id: hero.number,
    },
  });
};
```

Execute the function when a hero is clicked:

```html
<div
  v-for="(hero, index) in topHeroes"
  :key="index"
  class="top-hero"
  @click="navigateToHero(hero)"
>
  {{ hero.name }}
</div>
```

Now when a hero is clicked, navigation to the `HeroDetails` page will take place. When the `back` button is clicked on the `HeroDetails` page, it will navigate to the `HeroList` page. This is the desired behavior when navigating from the `HeroList` page to the `HeroDetails` page. This is **not** the desired behavior when navigating from the `Dashboard` page to the `HeroDetails` page.

Change the functionality of the `HeroDetails` page:

`HeroDetails.vue`:

```ts
const moveBack = () => void router.go(-1);
```

Again, try to navigate from both the `HeroList` and `Dashboard` pages to the `HeroDetails` page and then click the `back` button.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/1502f1e7377ae9d9413c63a4f1d431e5012738ec)

## Modify hero

The desired functionality is that there is a `save` button that saves the changes to the hero on the details page.

One of the possibilities is that the global `heroes` variable is modified in the `HeroDetails` page. Instead of doing this, the functionality is going to be implemented directly in the service.

The idea is that the global `ref` variables are used to read the values. Changing these global `ref` variables is done through functions in the service.

`hero.service.ts`:

```ts
const useHeroes = () => {
  const topHeroes = heroes.value.slice(0, 4);

  const editHero = (hero: Hero) => {
    heroes.value = heroes.value.map((h) =>
      h.number === hero.number ? hero : h
    );
  };

  return {
    heroes,
    selectedHero,
    topHeroes,
    editHero,
  };
};
```

Link the function in the `HeroDetails` page.

```html
<template>
  <div v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input v-model="hero.name" /></div>

    <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
    <StyledButton class="save-button" @click="saveHero()">Save</StyledButton>
  </div>

  <div v-else class="title">Hero not found!</div>
</template>
```

The second `StyledButton` is new.

```js
export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const route = useRoute();
    const router = useRouter();
    const hero: Ref<Hero> = ref() as Ref<Hero>;
    const { heroes, editHero } = useHeroes();

    onBeforeMount(() => {
      const { id } = route.params;
      if (id) {
        const matchingHero = heroes.value.find((h) => h.number === +id);
        if (matchingHero) hero.value = matchingHero;
      }
    });

    const moveBack = () => void router.go(-1);
    const saveHero = () => editHero(hero.value);

    return {
      hero,
      moveBack,
      saveHero,
    };
  },
});
```

### Removing the two-way binding

Removing the two-way binding in the `HeroDetails` page. Currently, the hero is removed from the list. Because JavaScript works with `by reference` with objects and arrays, the `hero`-ref refers to the same object as the object that is retrieved from the global `heroes` list.

To ensure that this does not happen, the `hero`-ref must be changed to a new object. This can be done by using the `spread` operator.

Before the change:

```ts
onBeforeMount(() => {
  const { id } = route.params;
  if (id) {
    const matchingHero = heroes.value.find((h) => h.number === +id);
    if (matchingHero) hero.value = matchingHero;
  }
});
```

After the change:

```ts
onBeforeMount(() => {
  const { id } = route.params;
  if (id) {
    const matchingHero = heroes.value.find((h) => h.number === +id);
    if (matchingHero) hero.value = { ...matchingHero };
  }
});
```

Note: the `spread` operator "copies" the object, because of this the result is a new object, there is no longer a reference to the original object. **But** this only works one level deep. If the object contains a property with an object or an array. In that case also that property should be copied via the `spread` operator.

### Moving findHero to the service

The `Hero` service is responsible for all functionality of the `Hero` data. The functionality to search for a hero is moved to the service.

In the service:

```ts
const findHero = (id: number): Hero | undefined => {
  const matchingHero = heroes.value.find((h) => h.number === +id);
  if (matchingHero) return { ...matchingHero };
};
```

```ts
const { editHero, findHero } = useHeroes();

onBeforeMount(() => {
  const { id } = route.params;
  if (id) {
    const matchingHero = findHero(+id);
    if (matchingHero) hero.value = matchingHero;
  }
});
```

When testing out the application, the hero can no longer be changed by simply changing the name and then clicking `back`. There **must** be a click on `save`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/ac3a44bc8545ea14a361ec41e60fb779e673683f)

### Nicer styling of the buttons

Place the buttons in a wrapper div. Use the wrapper div to position the buttons.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/6122975233efc7f642db7b7a85da5d7e8f7b169a)

The Quasar SCSS variables `css/quasar.variables.scss`:

```scss
$primary: #1976d2;
$secondary: #26a69a;
$accent: #9c27b0;

$dark: #1d1d1d;

$positive: #21ba45;
$negative: #c10015;
$info: #31ccec;
$warning: #f2c037;
```

Are available as CSS variables, this can be seen in the `inspect` mode of the browser:

```css
:root {
  --q-primary: #1976d2;
  --q-secondary: #26a69a;
  --q-accent: #9c27b0;
  --q-positive: #21ba45;
  --q-negative: #c10015;
  --q-info: #31ccec;
  --q-warning: #f2c037;
  --q-dark: #1d1d1d;
  --q-dark-page: #121212;
}
```

These CSS variables can be used in the `style` tag. The `--q-primary` variable is the color used to indicate main actions. Provide a way to indicate that a `StyledButton` should be styled as a button that uses `--q-primary`.

Add a prop to the `StyledButton` component:

```html
<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    props: {
      primary: {
        type: Boolean,
        default: false,
      },
    },
  });
</script>
```

Use this prop to dynamically add a class if the prop `primary` is true.

```html
<template>
  <button :class="{ primary: primary }">
    <slot />
  </button>
</template>
```

In the `style` tag, overwrite the styling if the class `primary` is present.

```scss
.primary {
  background-color: var(--q-primary);
  color: white;

  &:hover {
    background-color: darken($primary, 10%);
    color: white;
  }
}
```

Note that both the CSS variable `var(--q-primary)` and the SCSS variable `$primary` can be used.

Two things are important to know:

- If other SCSS functions, such as `darken` are used, then the SCSS variant **must** be used.
- If SCSS functions are used, then the color is not changeable by modifying the CSS variable in the browser. This is only important if themability is provided that does not require compilation.

The `primary StyledButton` can now be used. This can be done by using the `primary` prop:

```html
<StyledButton :primary="true">Save</StyledButton>
```

Or the alternative, shorter syntax of using a boolean prop:

```html
<StyledButton primary>Save</StyledButton>
```

This is the preferred method for this tutorial.

```html
<StyledButton primary class="save-button" @click="saveHero()">
  Save
</StyledButton>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/b82ba86c6bdd3f8aa8b423f141d78e80c7444330)

## Delete Hero

Provide functionality to remove a hero in the service. Link this functionality in the `HeroList` component and in the `HeroDetail` component.

```ts
const deleteHero = (hero: Hero) => {
  heroes.value = heroes.value.filter((h) => h.number !== hero.number);
};
```

Provide a button to delete a hero. This button must be able to be styled as a `StyledButton` with `negative` styling.

Implement the `negative` styling in the `StyledButton` component.

```html
<template>
  <button
    :class="{
      primary: primary,
      negative: negative,
    }"
  >
    <slot />
  </button>
</template>
```

```ts
export default defineComponent({
  props: {
    primary: {
      type: Boolean,
      default: false,
    },
    negative: {
      type: Boolean,
      default: false,
    },
  },
});
```

```scss
.negative {
  background-color: var(--q-negative);
  color: white;

  &:hover {
    background-color: darken($negative, 5%);
    color: white;
  }
}
```

Use this button in the `HeroList` component.
Add a `StyledButton` to the `HeroList` component with the `negative` prop.

```html
<StyledButton negative @click="onDeleteClick()">Delete</StyledButton>
```

Also add this button in the `HeroDetail` component.

```html
<div class="buttons">
  <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
  <StyledButton primary class="save-button" @click="saveHero()">
    Save
  </StyledButton>
  <StyledButton negative @click="removeHero()"> Delete </StyledButton>
</div>
```

While adding this button, we suddenly notice that the classes `back-button` and `save-button` are not used. These will be removed.

```html
<div class="buttons">
  <StyledButton @click="moveBack()">Back</StyledButton>
  <StyledButton primary @click="saveHero()">Save</StyledButton>
  <StyledButton negative @click="removeHero()">Delete</StyledButton>
</div>
```

When viewing the `HeroList` component, it can be seen that the buttons are against each other. This problem is already solved in the `HeroDetail` component. To avoid duplication of code, a separate component is created to solve this problem once. Then this component is used in the locations where there should be multiple buttons next to each other.

`src/components/ButtonGroup.vue`

```html
<template>
  <div class="buttons">
    <slot></slot>
  </div>
</template>

<style lang="scss" scoped>
  .buttons {
    display: flex;
    gap: 0.5rem;
  }
</style>
```

Then use this new component at both locations.

`HeroDetails.vue`

```html
<ButtonGroup class="button-group">
  <StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
  <StyledButton primary @click="saveHero()">Save</StyledButton>
  <StyledButton negative @click="removeHero()">Delete</StyledButton>
</ButtonGroup>
```

Note that a class `button-group` has been added to apply the `margin-top: 1rem;`.

`HeroList.vue`

```html
<ButtonGroup>
  <StyledButton @click="onDetailsClick()">Details</StyledButton>
  <StyledButton negative @click="onDeleteClick()">Delete</StyledButton>
</ButtonGroup>
```

In either case, remember to import the component and add it to the `components`-property.

Link the `deleteHero` function of the service to the `delete` buttons. The implementation is slightly different in both cases.

`HeroDetail.vue`

```ts
const moveBack = () => void router.go(-1);
const saveHero = () => editHero(hero.value);
const removeHero = () => {
  deleteHero(hero.value);
  moveBack();
};
```

When a hero is deleted, it cannot be displayed again. You will then be navigated back to the page you were navigating from. This can be the `Dashboard` page or the `HeroList` page.

Add the same logic to saving a hero.

```ts
const moveBack = () => void router.go(-1);
const saveHero = () => {
  editHero(hero.value);
  moveBack();
};
const removeHero = () => {
  deleteHero(hero.value);
  moveBack();
};
```

`HeroList.vue`

```ts
const onDeleteClick = () => {
  deleteHero(selectedHero.value);
  selectedHero.value = {} as Hero;
};
```

A casting is done from an empty object to a `Hero`. This is how from now on it is shown that the `selectedHero` is not a `Hero`. This also requires refactoring in the service.

The current implementation turns a `ref` variable that has the value `undefined` into a `Hero`. The problem with this is that `undefined` can never be reassigned and so logic is going to be written around this principle. A better way is to initialize the `selectedHero` variable with an empty object. An empty object can always be cast to a `Hero` and then the logic that is written is always usable.

Before the change:

```ts
const selectedHero: Ref<Hero> = ref() as Ref<Hero>;
```

After the change:

```ts
const selectedHero = ref({} as Hero);
```

The explicit typing has been removed. TypeScript implicitly gives the typing `Ref<Hero>` to the variable. This can be seen by hovering over the variable in VSCode.

Now when the application is tested, there will be an error in the `HeroList` component. This is because an empty object, is seen as a `truthy` value. So the `v-if="selectedhero"` evaluates to `true` and the elements are displayed. In these elements, `selectedHero.name` is used. Since this property is used in the function `upperCase(selectedHero.name)`, there will be an error.

Change the `v-if` condition to `selectedHero?.name`.

```html
<div v-if="selectedHero?.name">
  <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
  <ButtonGroup>
    <StyledButton @click="onDetailsClick()">Details</StyledButton>
    <StyledButton negative @click="onDeleteClick()">Delete</StyledButton>
  </ButtonGroup>
</div>
```

This is an abbreviated syntax for `v-if="selectedHero && selectedHero.name"`.

Instead of changing the `selectedHero` variable directly, a function will be provided in the service to change this variable.

`src/services/hero.service.ts`

```ts
const resetSelectedHero = () => (selectedHero.value = {} as Hero);
```

Use this function to reset the `selectedHero` variable in the `HeroList` component.

```ts
const onDeleteClick = () => {
  deleteHero(selectedHero.value);
  resetSelectedHero();
};
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/8fc9fe745568f0dc5b1b8ea0c351e3913142b9f8)

## Add hero

To add a new hero, several things need to be done:

- A route must be provided to display the page.
- A function must be provided to add the hero to the list in the service.
- A page must be created where the data can be entered.
- A button must be added to the `HeroList` page to navigate to the new page.

### Route

It is not necessary to put the route before the wildcard route `/heroes/:id`. This is a preference, first define all explicit routes and then the wildcard route.

```ts
const routes: RouteRecordRaw[] = [
  {
    path: "/",
    component: () => import("layouts/MainLayout.vue"),
    children: [
      {
        name: ROUTE_NAMES.DASHBOARD,
        path: "",
        component: () => import("pages/Dashboard.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_LIST,
        path: "/heroes",
        component: () => import("pages/HeroList.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_ADD,
        path: "/heroes/add",
        component: () => import("pages/HeroAdd.vue"),
      },
      {
        name: ROUTE_NAMES.HERO_DETAILS,
        path: "/heroes/:id",
        component: () => import("pages/HeroDetails.vue"),
      },
    ],
  },

  {
    path: "/:catchAll(.*)*",
    component: () => import("pages/Error404.vue"),
  },
];
```

### Service

Note that only the hero name is passed when creating a new hero. The `number`-property is not passed, but it is determined based on the already existing heroes.

```ts
const addHero = (name: string) => {
  const maxNumber = Math.max(...heroes.value.map((h) => h.number));
  const newHero = { number: maxNumber + 1, name };
  heroes.value.push(newHero);
};
```

### HeroAdd.vue

After adding the page, it is possible to manually navigate to `/heroes/add`.

```html
<template>
  <div class="title">Add hero</div>

  <div>name: <input v-model="name" /></div>

  <ButtonGroup class="button-group">
    <StyledButton @click="moveBack()">Back</StyledButton>
    <StyledButton primary @click="saveHero()">Save</StyledButton>
  </ButtonGroup>
</template>

<script lang="ts">
  import { defineComponent, ref } from "vue";
  import StyledButton from "components/StyledButton.vue";
  import ButtonGroup from "components/ButtonGroup.vue";
  import { useRouter } from "vue-router";
  import { useHeroes } from "src/services/hero.service";

  export default defineComponent({
    components: {
      StyledButton,
      ButtonGroup,
    },
    setup() {
      const router = useRouter();
      const name = ref("");
      const { addHero } = useHeroes();

      const moveBack = () => void router.go(-1);
      const saveHero = () => {
        addHero(name.value);
        moveBack();
      };

      return {
        name,
        moveBack,
        saveHero,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  .button-group {
    margin-top: 1rem;
  }
</style>
```

### Add button HeroList.vue

```html
<StyledButton class="new-hero-button" primary @click="onNewClick()">
  New Hero
</StyledButton>
```

```ts
const onNewClick = () => void router.push({ name: ROUTE_NAMES.HERO_ADD });
```

```scss
.new-hero-button {
  margin-top: 1rem;
}
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/e35ae4b4fe9c056afe9bbdb07637f690a7c33d2a)

## readonly

In the service, add the `readonly` function provided by Vue to the global state `heroes` and `selectedHero`.

```ts
import { readonly, ref } from "vue";
```

```ts
return {
  heroes: readonly(heroes),
  selectedHero: readonly(selectedHero),
  topHeroes,
  editHero,
  findHero,
  deleteHero,
  resetSelectedHero,
  addHero,
};
```

After this change, there is an error in the console when the application is started:

```ts
ERROR in src/pages/HeroList.vue:52:20
TS2540: Cannot assign to 'value' because it is a read-only property.
    50 |
    51 |     const onClickHero = (hero: Hero) => {
  > 52 |       selectedHero.value = hero;
       |                    ^^^^^
    53 |     };
    54 |
    55 |     const onDetailsClick = () => {
```

In the `HeroList.vue` page, another value is directly assigned to the `selectedHero` ref.

To prevent this, a function is added `setSelectedHero`. This function will then change the `selectedHero`-ref in the service.

```ts
const setSelectedHero = (hero: Hero) => (selectedHero.value = hero);
```

Use this function in the `HeroList.vue` page.

Before the change:

```ts
const { heroes, selectedHero, deleteHero, resetSelectedHero } = useHeroes();

const onClickHero = (hero: Hero) => {
  selectedHero.value = hero;
};
```

After the change:

```ts
const { heroes, selectedHero, deleteHero, resetSelectedHero, setSelectedHero } =
  useHeroes();

const onClickHero = (hero: Hero) => setSelectedHero(hero);
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/3c48e4c30f2c9cc67d595974e3f95109807000da)

## Conclusion

A service/composable is used to manage data. This data can be used as `global state` in the application. To ensure that `global state` cannot be changed directly, it is exposed via a `readonly` function.

Because all logic around `heroes` is now in a separate service/composable, it will be easier in the future to use a real API for example, the implementation can be changed in one place.
