---
postUuid: 5d65c5d7-736e-4505-a28d-65cd6f60341b
title: Quasar Tour of Heroes - Components
slug: quasar-toh-components
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

This article starts from the situation where the layout is set up and the different pages are added to the Tour of Heroes application. The initial situation is an existing Quasar project with a layout component and placeholder pages. The final situation is a Quasar project in which the pages use components.

The code can be obtained by downloading a zip of the source code.

The situation that is used as a starting point is as follows:

![starting situation](/img/blog/quasar-toh-styling.gif)

Starting situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-layouts-and-pages)

The final situation being worked towards is as follows:

![end situation](/img/blog/quasar-toh-components.gif)

End situation: [Source code](https://github.com/code-coaching/quasar-tour-of-heroes/releases/tag/quasar-toh-components)

## Components

Everything within Vue is a component. Since Quasar is a Vue framework, all components are also Vue components. In the application, you can already see a component for the layout, called `MainLayout.vue`. There are also already components for the different pages.

The different pages are:

- Error404.vue - 404 page, by default present in a new Quasar project.
- Dashboard.vue - this is a page where the `top heroes` will be shown.
- HeroList.vue - this is a page where the list of `heroes` will be shown.
- HeroDetail.vue - this is a page where the details of a `hero` will be shown.

A layout is a component and contains HTML elements and other components that are present on all pages.

A page is a component and contains HTML elements and other components that are present on a specific page.

During this article, new components will be added to the application.

## Style page: Dashboard

![dashboard](/img/blog/quasar-toh-dashboard.png)

Change the content of `src/pages/Dashboard.vue`:

```html
<template>
  <div>Top Heroes</div>

  <div>Narco</div>
  <div>Bombasto</div>
  <div>Celeritas</div>
  <div>Magneta</div>
</template>
```

![Quasar dashboard elements](/img/blog/quasar-toh-dashboard-elements.png)

The elements are present, but the styling does not match.

Add a `style` tag to the Dashboard page:

```html
<template>
  <div class="title">Top Heroes</div>

  <div class="top-heroes">
    <div class="top-hero">Narco</div>
    <div class="top-hero">Bombasto</div>
    <div class="top-hero">Celeritas</div>
    <div class="top-hero">Magneta</div>
  </div>
</template>

<style lang="scss" scoped>
  .title {
    font-size: 1.5rem;
    color: grey;
    font-weight: bold;

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

Note that a `container element` has been added around the `hero` elements.

![Quasar Tour of Heroes Dashboard Styling](/img/blog/quasar-toh-dashboard-styling.png)

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/e4c7e2534940826a43dd00b5aee2373659fcb8c7..cb681e49138851e87ffa8e39c7fa9c6955c0f1a3)

## Style page: HeroList

![HeroList](/img/blog/quasar-toh-hero-list.png)

Change the content of `src/pages/HeroList.vue`:

```html
<template>
  <div>My Heroes</div>

  <div>11 Mr. Nice</div>
  <div>12 Narco</div>
  <div>13 Bombasto</div>
  <div>14 Celeritas</div>
  <div>15 Magneta</div>
  <div>16 RubberMan</div>
  <div>17 Dynama</div>
  <div>18 Dr IQ</div>
  <div>19 Magma</div>
  <div>20 Tornado</div>
</template>
```

![Quasar hero list elements](/img/blog/quasar-toh-hero-list-elements.png)

Style the elements, add additional elements as needed to achieve the desired result.

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div class="hero">
      <span class="hero-number">11</span>
      <span class="hero-name">Mr. Nice</span>
    </div>
    <div class="hero">
      <span class="hero-number">12</span> <span class="hero-name">Narco</span>
    </div>
    <div class="hero">
      <span class="hero-number">13</span>
      <span class="hero-name">Bombasto</span>
    </div>
    <div class="hero">
      <span class="hero-number">14</span>
      <span class="hero-name">Celeritas</span>
    </div>
    <div class="hero">
      <span class="hero-number">15</span> <span class="hero-name">Magneta</span>
    </div>
    <div class="hero">
      <span class="hero-number">16</span>
      <span class="hero-name">RubberMan</span>
    </div>
    <div class="hero">
      <span class="hero-number">17</span> <span class="hero-name">Dynama</span>
    </div>
    <div class="hero">
      <span class="hero-number">18</span> <span class="hero-name">Dr IQ</span>
    </div>
    <div class="hero">
      <span class="hero-number">19</span> <span class="hero-name">Magma</span>
    </div>
    <div class="hero">
      <span class="hero-number">20</span> <span class="hero-name">Tornado</span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  .title {
    font-size: 1.5rem;
    color: grey;
    font-weight: bold;

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
    cursor: pointer;
    color: #8d8d8d;
    border-radius: 0.5rem;

    &:hover {
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
</style>
```

![Quasar Tour of Heroes styled hero list](/img/blog/quasar-toh-hero-list-styled.gif)

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/cb681e4..ee30bd8)

## Extending page: HeroList

In order for a hero to be selected, a `click handler` must be added to each hero. Since with the current code, there are ten separate elements representing a hero, ten `click handlers` would be needed.

To avoid this, a `v-for` loop in the `template` section, combined with a list of heroes, can be used.

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div class="hero" v-for="(hero, index) in heroes" :key="index">
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      const heroes = [
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
      ];

      return {
        heroes,
      };
    },
  });
</script>

<style lang="scss" scoped>
  /* No changes! */
</style>
```

It is important to add the `v-for` loop in the template. This is done on the element that should be repeated. In order for `change detection` to work optimally, a `key` attribute must be used. This ensures that each element is uniquely identified.

A `<script lang="ts"></script>` has been added. `lang="ts"` means that TypeScript is used. The `defineComponent` function is used. This function allows you to define a component. The `setup` function is the function that is called when the component is created. The `setup` function defines the `heroes` list, which includes an object with the properties `number` and `name`. Each object represents a hero.

Variables used in the `template` section must be in the `return` statement of the `setup` function. Only the variables listed here are available in the template.

### Selecting a hero

![Click hero in list](/img/blog/quasar-toh-click-hero-in-list.png)

In order for a hero to be selected, a few things are needed:

- a `click handler` on each hero element, this is a function that is called when a hero is clicked
- a `selectedHero` variable that will contain the selected hero

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      class="hero"
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div>{{ selectedHero }}</div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    setup() {
      let selectedHero;

      const heroes = [
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
      ];

      function onClickHero(hero: { number: number; name: string }) {
        selectedHero = hero;
      }

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>

<style lang="scss" scoped>
  /* No changes! */
</style>
```

Remember: `selectedHero` is a variable defined with the keyword `let`. This is necessary since the variable will be assigned a different value at a later time.

The `click handler` is added by means of the `@click` directive. This directive is used to call a function when an element is clicked. The function that is called is defined in the `setup` function.

The function that is called is defined as `onClickHero`, with a hero as argument. This function is called when a hero is clicked.

The function modifies the `selectedHero` variable. This variable is used in the template to show the selected hero `{{ selectedHero }}`.

BUT when testing out this code, the `selectedHero` variable will not be changed in the template. This is because the `selectedHero` variable must be a `ref` variable. A `ref` variable is a variable that contains a reference to an element. This reference is used by Vue to know that the value of the variable has changed and therefore that this variable should be renewed in the template.

### Using `ref` variables

A brief explanation regarding `ref` variables. Vue provides a function called `ref`, this must be imported `import { ref } from "vue";`. This function accepts any value as an argument and will return an object with the value assigned to the property called `value`.

```ts
import { ref } from "vue";

let someVariable; // value is undefined
someVariable = "John Duck"; // value is "John Duck"

const someRef = ref(); // value is { value: undefined }
const someOtherRef = ref("John Duck"); // value is { value: "John Duck" }
someOtherRef.value = "John - The - Duck"; // value is { value: "John - The - Duck" }
```

Some differences to note, a `ref` variable can always be assigned to a variable created with keyword `const`. This is because the `ref()` function returns an object, then a property of this object (called `value`) is always used to assign a new value. So the variable itself is not assigned a new value, since it is still the same object.

Because the object being changed is being watched by Vue, Vue knows that this value has changed and can then send `change detected` events to the template.

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";

  export default defineComponent({
    setup() {
      // let selectedHero = undefined;
      const selectedHero = ref();

      const heroes = [
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
      ];

      function onClickHero(hero: { number: number; name: string }) {
        selectedHero.value = hero;
      }

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>
```

Notice that `selectedHero` has become a `ref` variable. This is an object in which the value is kept in the property `value`. So to change the value, the `.value` property must be used. This can be seen in the `onClickHero` function.

Try this code again and notice that the `selectedHero` variable is now changed in the template after selecting a hero.

Why should the `heroes` array not be changed to a `ref` variable? This is not necessary, because the list of heroes is not changed. If the list of heroes did change, for example by adding a new hero by clicking a button, then this list does need to be changed.

The `heroes` array **should not** be changed to a `ref` variable, but it **may** be changed to a `ref` variable. A simple rule to follow is _make every variable used in the template, a `ref` variable_. This will ensure that every variable used in the template is `reactive`. Vue will always display an up-to-date value for each `reactive variable` in the template.

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      function onClickHero(hero: { number: number; name: string }) {
        selectedHero.value = hero;
      }

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>
```

Note that nothing changes in the `template`. Vue automatically knows that a variable is a `ref` variable and will automatically use the `.value` property to display the value of the variable in the template.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/ee30bd8..be91f2b)

![Ref-variable in HeroList](/img/blog/quasar-toh-selected-hero-ref.png)

A small refactoring of the code to add an `interface` and change the definition of the `onClickHero` function to an anonymous arrow function.

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
      };
    },
  });
</script>
```

The function has changed from a named function.

```ts
function onClickHero(hero: { number: number; name: string }) {
  selectedHero.value = hero;
}
```

To an anonymous arrow function assigned to a variable called `onClickHero`.

```ts
const onClickHero = (hero: { number: number; name: string }) => {
  selectedHero.value = hero;
};
```

This does not change the functionality of the function. It is simply a preference in terms of syntax.

The interface is added to represent the complex type `Hero`. This interface is not necessary, but it is a good way to make the code more readable.

For adding the Hero interface:

```ts
const onClickHero = (hero: { number: number; name: string }) => {
  selectedHero.value = hero;
};
```

After adding the Hero interface:

```ts
interface Hero {
  number: number;
  name: string;
}

const onClickHero = (hero: Hero) => {
  selectedHero.value = hero;
};
```

The advantage of this TypeScript interface, is that wherever a Hero object is used, the interface can be used to define the type.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/be91f2b..37b8e4a)

### Styling of selectedHero

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
    <div
      class="hero"
      v-for="(hero, index) in heroes"
      :key="index"
      @click="onClickHero(hero)"
    >
      <span class="hero-number">{{ hero.number }}</span>
      <span class="hero-name">{{ hero.name }}</span>
    </div>
  </div>

  <div v-if="selectedHero">
    <div class="title">{{ selectedHero.name }} is my hero</div>
    <button>Details</button>
  </div>
</template>
```

Note that the `v-if` directive is used. This ensures that the element and all child elements are only shown if the variable `selectedHero` is `truthy`. Or in other words, if the variable has a value, the element is shown. Without the `v-if` directive, the application would show an error, since the `.name` property of the variable `selectedHero` does not exist, since `selectedHero` is `undefined` at that point.

The elements are there, the styling is not yet correct.

![elements selected hero](/img/blog/quasar-toh-selected-hero-elements.png)

Now that the selected hero is known, the styling of the selected hero must be changed. The name of the selected hero must be shown in all capital letters. The button must have the same styling as the existing buttons.

#### Styling of active hero

Adding a dynamic class:

```html
<div
  :class="{
    'hero--active': hero.number === selectedHero?.number,
  }"
></div>
```

Note that a `v-bind` directive is used to allow variables to be used in assigning the class `v-bind:class` or the short syntax `:class`. This can be read as "the class `hero--active` is added to this element if the statement is `truthy`.

The statement `hero.number === selectedHero?.number` evaluates to `true` if the the number of the hero being iterated over is equal to the number of the selected hero.

Note the `?.` operator. This operator ensures that `.number` is read only if `selectedHero` has a value. If `selectedHero` is `undefined`, `.number` is not read and the statement `hero.number === selectedHero?.number` evaluates to `false`.

The complete template:

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
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
  </div>

  <div v-if="selectedHero">
    <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
    <button>Details</button>
  </div>
</template>
```

The modified SCSS class `hero--active`:

```scss
.hero {
  display: flex;
  max-width: 10rem;
  background-color: darken(#eeeeee, 10%);
  cursor: pointer;
  color: #8d8d8d;
  border-radius: 0.5rem;

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
```

```scss
.hero {
  &:hover {
  }

  &--active {
  }
}
```

Is similar to the following CSS:

```css
.hero {
}

.hero:hover {
}

.hero--active {
}
```

Now when a hero is selected, the class `hero--active` is added to the element.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/37b8e4a..bb8d8ce)

![Styling of selected hero](/img/blog/quasar-toh-selected-hero-styling.png)

#### Name of hero in capital letters

One way to display the name in uppercase is to place the name in an element and then use css to capitalize the name. However, this tutorial chooses to change the name using a JavaScript function.

```html
<template>
  <div class="title">My Heroes</div>

  <div class="hero-list">
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
  </div>

  <div v-if="selectedHero">
    <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
    <button>Details</button>
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref } from "vue";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      const upperCase = (str: string) => str.toUpperCase();

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
      };
    },
  });
</script>
```

The function `upperCase` is created in the `setup` function. The function is added to the `return` statement so that it is available in the template. The function is added to `{{ selectedHero.name }}` so that the name is displayed in uppercase: `{{ upperCase(selectedHero.name) }}`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/bb8d8ce..6b1f836)

Since `upperCase` is a function that returns any string in uppercase, it is ideal to put the function in a separate location so that the function can be imported in different places.

This tutorial chooses to place the function in a file called `utils.ts` in the `components` folder.

`components/utils.ts`:

```ts
const upperCase = (str: string) => str.toUpperCase();

export { upperCase };
```

Importing the `upperCase` function into the `HeroList` component:

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";
  import { upperCase } from "components/utils";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
      };
    },
  });
</script>
```

Note that the `upperCase` function is now imported and is no longer defined in the `setup` function. Also note that the `upperCase` function is still placed in the `return` statement of the `setup` function so that it is available in the template.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/6b1f836..6b8760c)

#### Adding the details button

Since the button should be styled the same as the already existing buttons in `MainLayout.vue`, the styling can be copied there and then pasted into the `style` tag of `HeroList.vue`.

```scss
button {
  background-color: #eeeeee;
  border-radius: 0.25rem;
  font-weight: 500;
  border: none;
  padding: 0.25rem 0.5rem;
  color: #567868;

  &:hover {
    background-color: darken(#eeeeee, 10%);
    color: #0096e8;
    cursor: pointer;
  }
}
```

This is a quick fix, but it creates a problem. What if the `background-color` should no longer be `#eeeeee`, but `deeppink`? Then this must be changed in both the `HeroList.vue` and the `MainLayout.vue`.

To avoid this, a separate component can be created in which the button as well as the styling is placed.

`components/StyledButton.vue`

```html
<template>
  <button>Details</button>
</template>

<style lang="scss" scoped>
  button {
    background-color: #eeeeee;
    border-radius: 0.25rem;
    font-weight: 500;
    border: none;
    padding: 0.25rem 0.5rem;
    color: #567868;

    &:hover {
      background-color: darken(#eeeeee, 10%);
      color: #0096e8;
      cursor: pointer;
    }
  }
</style>
```

The custom component can be imported into the `HeroList.vue`:

```html
<script lang="ts">
  import { defineComponent, ref } from "vue";
  import { upperCase } from "components/utils";
  import StyledButton from "components/StyledButton.vue"; // 1.

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton, // 2.
    },
    setup() {
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
      };
    },
  });
</script>
```

Using the custom component:

```html
<div v-if="selectedHero">
  <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>

  <!-- Option 1 -->
  <styled-button></styled-button>

  <!-- Option 2 -->
  <styled-button />

  <!-- Option 3 -->
  <StyledButton></StyledButton>

  <!-- Option 4 -->
  <StyledButton />
</div>
```

The custom component can be used in several ways.

As `kebab-case` syntax, with start and end tags:

- `<styled-button></styled-button>`.

As `kebab-case` syntax, self-closing tag:

- `<styled-button />`

As `PascalCase`-syntax, with beginning and ending tag:

- `<StyledButton></StyledButton>`.

As `PascalCase` syntax, self-closing tag:

- `<StyledButton />`

Within this tutorial, `PascalCase` syntax is chosen. If possible the self-closing tag, if there is content between the start and end tags, then `PascalCase` syntax with start and end tags is used.

```html
<div v-if="selectedHero">
  <div class="title">{{ upperCase(selectedHero.name) }} is my hero</div>
  <StyledButton />
</div>
```

The problem with the current `StyledButton`, is that it is hardcoded to contain the text `Details`. But in the `MainLayout.vue` component, it should display `Dashboard` and `Heroes`.

A normal `button` element in html displays the text between a start and end tag. To obtain this in the custom component, a `slot` tag can be used.

`components/StyledButton.vue`

```html
<template>
  <button>
    <slot></slot>
  </button>
</template>
```

Or with a self closing slot tag:

```html
<template>
  <button>
    <slot />
  </button>
</template>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/6b8760c..c33f42b)

It is now possible to also use the button in the `MainLayout.vue` component. The styling of the `button` can be taken from the `style` tag of the `MainLayout.vue` component.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/c33f42b..d323783)

When the `Details` button is clicked, navigation to the route `/heroes/:id` should occur. Where :id is the `number` property of the selected hero.

`pages/HeroList.vue`

template

```html
<StyledButton @click="onDetailsClick()">Details</StyledButton>
```

script-tag

```html
<script lang="ts">
  import { defineComponent, ref, Ref } from "vue";
  import { upperCase } from "components/utils";
  import { useRouter } from "vue-router";
  import StyledButton from "components/StyledButton.vue";
  import { ROUTE_NAMES } from "src/router/routes";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const router = useRouter();
      const selectedHero = ref();

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      const onDetailsClick = () => {
        void router.push({
          name: ROUTE_NAMES.HERO_DETAILS,
          params: {
            id: selectedHero.value.number,
          },
        });
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
        onDetailsClick,
      };
    },
  });
</script>
```

In JavaScript, this code would work, but because it uses TypeScript, some errors are shown:

```sh
Compiled with problems:X

ERROR in src/pages/HeroList.vue:66:11

@typescript-eslint/no-unsafe-assignment: Unsafe assignment of an `any` value.
    64 |         name: ROUTE_NAMES.HERO_DETAILS,
    65 |         params: {
  > 66 |           id: selectedHero.value.number,
       |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    67 |         },
    68 |       });
    69 |     };


ERROR in src/pages/HeroList.vue:66:15

@typescript-eslint/no-unsafe-member-access: Unsafe member access .number on an `any` value.
    64 |         name: ROUTE_NAMES.HERO_DETAILS,
    65 |         params: {
  > 66 |           id: selectedHero.value.number,
       |               ^^^^^^^^^^^^^^^^^^^^^^^^^
    67 |         },
    68 |       });
    69 |     };
```

The problem is that `selectedHero` is a `ref` variable. This is of type `any` by default.

```ts
import { ref } from "vue";

const selectedHero = ref();
```

Is equal to:

```ts
import { ref, Ref } from "vue";

const selectedHero: Ref<any> = ref();
```

Since it is known what will be in the `ref` variable, namely a hero, the interface can be used to indicate that the `ref` variable will contain a `Hero` object.

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero> = ref();
```

Nice, solved. Right? No! There is a new compilation error. The error indicates that the `ref` variable can only contain a `Hero` object, but it is also assigned `undefined`.

```sh
Compiled with problems:

ERROR in src/pages/HeroList.vue:43:11

TS2322: Type 'Ref<Hero | undefined>' is not assignable to type 'Ref<Hero>'.
  Type 'Hero | undefined' is not assignable to type 'Hero'.
    Type 'undefined' is not assignable to type 'Hero'.
    41 |   setup() {
    42 |     const router = useRouter();
  > 43 |     const selectedHero: Ref<Hero> = ref();
       |           ^^^^^^^^^^^^
    44 |
    45 |     const heroes = ref([
    46 |       { number: 11, name: 'Mr. Nice' },
```

There are two ways to fix this error.

The first option is to add `undefined` as a possible type of this `ref` variable.

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero | undefined> = ref();
```

The disadvantage of this approach is that a `?` sign must always be used when addressing a property of the object in the `.value` property, since it may be `undefined`.

```ts
console.log(selectedHero.value?.number);
console.log(selectedHero.value?.name);
```

The second option is to define the `ref` variable as `Ref<Hero>` and `type cast` the ref itself to `Ref<Hero>`.

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero> = <Ref<Hero>>ref();
```

Or the alternative `type cast`-syntax:

```ts
import { ref, Ref } from "vue";

interface Hero {
  number: number;
  name: string;
}

const selectedHero: Ref<Hero> = ref() as Ref<Hero>;
```

Now a property of the object in the `.value`-property can be called without the `?` sign.

```ts
console.log(selectedHero.value.number);
console.log(selectedHero.value.name);
```

To ensure that it is always clear what the exact type is of the value in the `ref` variable. Is it smart to always assign a type to the `ref` variable.

```html
<script lang="ts">
  import { defineComponent, ref, Ref } from "vue";
  import { upperCase } from "components/utils";
  import { useRouter } from "vue-router";
  import StyledButton from "components/StyledButton.vue";
  import { ROUTE_NAMES } from "src/router/routes";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const router = useRouter();
      const selectedHero: Ref<Hero> = ref() as Ref<Hero>;

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

      const onClickHero = (hero: Hero) => {
        selectedHero.value = hero;
      };

      const onDetailsClick = () => {
        void router.push({
          name: ROUTE_NAMES.HERO_DETAILS,
          params: {
            id: selectedHero.value.number,
          },
        });
      };

      return {
        heroes,
        selectedHero,
        onClickHero,
        upperCase,
        onDetailsClick,
      };
    },
  });
</script>
```

When testing the code, click on the hero `Magneta` and then click on the `Details` button. Verify that the browser url has been changed to `/heroes/15`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/d323783..92e2080)

## Style page: HeroDetails

First, all the elements needed to show the details of a hero are added.

```html
<template>
  <div v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref, Ref } from "vue";
  import StyledButton from "components/StyledButton.vue";

  interface Hero {
    number: number;
    name: string;
  }

  export default defineComponent({
    components: {
      StyledButton,
    },
    setup() {
      const hero: Ref<Hero> = ref() as Ref<Hero>;

      return {
        hero,
      };
    },
  });
</script>

<style lang="scss" scoped>
  .title {
    font-size: 1.5rem;
    color: grey;
    font-weight: bold;

    margin-top: 1rem;
    margin-bottom: 1rem;
  }
</style>
```

Now when a hero is selected, or when manually navigating to `/heroes/:id` (e.g. `/heroes/15`), the `HeroDetails` page is shown. But nothing is shown in the browser. To style the elements, a hero can be temporarily hard-coded.

```ts
const hero: Ref<Hero> = ref({ number: 15, name: "Magneta" }) as Ref<Hero>;
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/92e2080..0a1e9d0)

![Hero Details - setup](/img/blog/quasar-toh-hero-details-initial.png)

Before proceeding with styling the page, look at what is duplicated on this page relative to the `HeroList` page. Duplicated things are candidates to be set apart so that it is reusable.

### Refactor Hero Interface

The `Hero` interface was copied and pasted from the `HeroList` page.

```ts
interface Hero {
  number: number;
  name: string;
}
```

Suppose an additional property is added to the `Hero` interface, it would have to be changed in both the `HeroList` page and the `HeroDetails` page. This may not seem like a big deal, but consider what needs to be done if this interface is used in ten different components.

This is solved by placing the interface in `src/components/models.ts`. From this file the interface is exported and then the interface is imported into all the files where this interface is used.

`src/components/models.ts`:

```ts
export interface Hero {
  number: number;
  name: string;
}
```

Change the existing `Hero` interface in the two places where it is used in the components, to an import that references the interface in `src/components/models.ts`.

```ts
import { Hero } from "src/components/models";
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/0a1e9d0..b341f6a)

### Refactor .title class

The class `.title` is also used several times.

`src/layouts/MainLayout.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
}
```

`src/pages/Dashboard.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}
```

`src/pages/HeroDetails.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

`src/pages/HeroList.vue`

```scss
.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;

  margin-top: 1rem;
  margin-bottom: 1rem;
}
```

The common properties of this class can be moved to the global stylesheet `src/css/app.scss`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/b341f6a..9029358)

### Heroes - onBeforeMount - url parameter useRoute

To make sure that the correct hero can be displayed, we will look at the url parameter called `id`. Then the hero will be filtered from the list of heroes, which in this article will be copied from the `HeroList` page, in a later article the list of heroes will be moved to a `service` so that only one unique list of heroes exists.

First, the `heroes` list is copied and pasted from the `HeroList` page.

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

To get to the route parameter, the `useRoute` composable must be used. This is a function that returns a route object, available through `vue-router`.

```ts
import { useRoute } from "vue-router";

export default defineComponent({
  setup() {
    const route = useRoute();

    console.log(route.params.id); // Visiting /heroes/15, this will log 15
  },
});
```

Next, the `onBeforeMount` hook is used, which is a `lifecycle hook`. This hook is called after Vue has finished setting up the reactive data, but before any DOM elements have been created.

```ts
import { defineComponent, onBeforeMount, ref, Ref } from "vue";
import StyledButton from "components/StyledButton.vue";
import { Hero } from "components/models";
import { useRoute } from "vue-router";

export default defineComponent({
  components: {
    StyledButton,
  },
  setup() {
    const route = useRoute();
    const hero: Ref<Hero> = ref() as Ref<Hero>;

    onBeforeMount(() => {
      const { id } = route.params;
      if (id) {
        const matchingHero = heroes.value.find((h) => h.number === +id);
        if (matchingHero) hero.value = matchingHero;
      }
    });

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

    return {
      hero,
    };
  },
});
```

Note that `const hero = ref() as Ref<Hero>` no longer contains the hard-coded hero. It is now assigned based on the `id` parameter. Test the code in two ways. The first way is to go to the list of heroes via the dashboard, click on a hero there and then click on the `Details` button. The second way is to manually visit the route, for example `/heroes/15`. Also try to manually enter an id of a hero that does not appear in the list, for example `/heroes/69`.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/9029358..3eef809)

### Hero not found

To provide the user with better feedback when entering a hero that does not exist, a `Hero not found!` message will be displayed if the `hero`-ref is empty. This is done by using the `v-else` directive. An alternative is to use the `v-if` directive with the `!` operator.

```html
<div v-if="hero">Hero exists!</div>
<div v-else class="title">Hero not found!</div>
```

Alternative with v-if:

```html
<div v-if="!hero" class="title">Hero not found!</div>
```

Applied in the code:

```html
<template>
  <div v-if="hero">
    <div class="title">{{ hero.name }} details!</div>

    <div>id: {{ hero.number }}</div>
    <div>name: <input :value="hero.name" /></div>

    <StyledButton>Back</StyledButton>
  </div>

  <div v-else class="title">Hero not found!</div>
</template>
```

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/3eef809..5deb200)

### Style and wire the back button

To place the button correctly, a class is added to the button. To ensure that the button navigates back to the `HeroList` page when clicked, a `click handler` must be attached.

```html
<StyledButton class="back-button" @click="moveBack()">Back</StyledButton>
```

```scss
.back-button {
  margin-top: 1rem;
}
```

```ts
const moveBack = () => void router.push({ name: ROUTE_NAMES.HERO_LIST });
```

Remember to put the `moveBack` function in the `return` statement so that it is available in the `template` tag.

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/5deb200..d01060c)

### Two way binding - v-model

Currently, `v-bind:value`, or the shortened syntax `:value` is used to assign the value to the input. This is `one way binding`, the input is assigned a value, but if the value is changed, it is not propagated to the `hero` ref. To obtain `two way binding`, use `v-model`.

`v-model` assigns the value of the `hero` ref to the input, but will also update the value of the `hero` ref when the input value is changed.

One way binding `:value`

```html
<div>name: <input :value="hero.name" /></div>
```

![One way binding](/img/blog/quasar-toh-one-way-binding.gif)

Two way binding `v-model`

```html
<div>name: <input v-model="hero.name" /></div>
```

![Two way binding](/img/blog/quasar-toh-two-way-binding.gif)

All changes: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/compare/d01060c..caa059b)

## Conclusion

A page can be broken down into reusable components.

It is okay if components are not created up front, but added when it is noticed that there are identical blocks of code throughout the code base.

It is important to identify potential components. The more experience, the better identifying components will go.
