---
postUuid: 42162bb1-7ed1-4c91-b4a3-855c5b4753be
title: Quasar CLI 1.3.0 - creating a new Quasar project
slug: quasar-cli-1-3-0-a-new-quasar-project
tags:
  - Quasar
  - Vue
categories:
  - Frontend
---

## What is Quasar

Quasar is a framework based on VueJS that makes it possible to build applications for several platforms with one code base. It is possible to write code with web technologies that works on all platforms:

- Web browsers
- Desktop: Linux / MacOS / Windows
- Smartphones: Android / iOS

## Quasar CLI

When there is a reference to `terminal` in the text below and Windows is used as an operating system, it means that the command needs to be executed in `Git Bash`. If CTRL+V does not work to paste the copied content in the terminal, try SHIFT+INSERT.

Quasar provides a `CLI` (**C**ommand **L**ine **I**nterface). This is software that works in a terminal. To be able to use it, it needs to be installed on the system.

```sh
npm install -g @quasar/cli
```

The command above will install the Quasar CLI globally on the system. This will make it available everywhere on the system.

Confirm the installation was successful:

```sh
quasar -v
```

In case it prints a number (e.g. `1.3.0`), it means the installation was successful.

## Een nieuw Quasar project aanmaken

In this blog post the working directory is the `home directory`. To navigate to the home directory, the following command can be used.

```sh
cd ~
```

Create a new directory named `my-first-quasar-project`.

```sh
mkdir my-first-quasar-project
```

Navigate into the created directory.

```sh
cd  mijn-eerste-quasar-project
```

In this directory the Quasar CLI will be used to create a new Quasar project.

Import for Windows users: There is a bug in `git bash` that prevents the command to be executed. To fix this, use the normal `Command Prompt` (**not** PowerShell) to execute the command.

```sh
npm init quasar
```

The first time this command is run, you will be asked to install additional software. Click `Enter`.

```sh
Need to install the following packages:
  create-quasar
Ok to proceed? (y)
```

This will ask some questions in the terminal.

Click `Enter` three times to continue.

```sh
 .d88888b.
d88P" "Y88b
888     888
888     888 888  888  8888b.  .d8888b   8888b.  888d888
888     888 888  888     "88b 88K          "88b 888P"
888 Y8b 888 888  888 .d888888 "Y8888b. .d888888 888
Y88b.Y8b88P Y88b 888 888  888      X88 888  888 888
 "Y888888"   "Y88888 "Y888888  88888P' "Y888888 888
       Y8b

✔ What would you like to build? › App with Quasar CLI, let's go!
✔ Project folder: … quasar-project
✔ Pick Quasar version: › Quasar v2 (Vue 3 | latest and greatest)
```

Use the arrow keys to select TypeScript. Click `Enter`.

```sh
? Pick script type: › - Use arrow-keys. Return to submit.
    Javascript
❯   Typescript
```

If the project is **not** for production, the faster variant using `Vite` can be used. If it is out of `BETA stage` and also marked as `stable`, then it is also okay to choose this option if it is for production.

```sh
? Pick Quasar App CLI variant: › - Use arrow-keys. Return to submit.
    Quasar App CLI with Webpack (stable)
❯   Quasar App CLI with Vite (BETA stage)
```

Choose all default choices in the following questions.

```sh
 .d88888b.
d88P" "Y88b
888     888
888     888 888  888  8888b.  .d8888b   8888b.  888d888
888     888 888  888     "88b 88K          "88b 888P"
888 Y8b 888 888  888 .d888888 "Y8888b. .d888888 888
Y88b.Y8b88P Y88b 888 888  888      X88 888  888 888
 "Y888888"   "Y88888 "Y888888  88888P' "Y888888 888
       Y8b

✔ What would you like to build? › App with Quasar CLI, let's go!
✔ Project folder: … quasar-project
✔ Pick Quasar version: › Quasar v2 (Vue 3 | latest and greatest)
✔ Pick script type: › Typescript
✔ Pick Quasar App CLI variant: › Quasar App CLI with Vite (BETA stage)
✔ Package name: … quasar-project
✔ Project product name: (must start with letter if building mobile apps) … Quasar App
✔ Project description: … A Quasar Project
✔ Author: … Bart Duisters <bartduisters@bartduisters.com>
✔ Pick a Vue component style: › Composition API
✔ Pick your CSS preprocessor: › Sass with SCSS syntax
✔ Check the features needed for your project: › ESLint
✔ Pick an ESLint preset: › Prettier
```

## Project structure of a Quasar project

```sh
.vscode/
node_modules/
public/
src/
.editorconfig
.eslintignore
.eslintrc.js
.gitignore
.prettierrc
index.html
package-lock.json
package.json
postcssrc.config.js
quasar.config.js
README.md
tsconfig.json
tsconfig.node.json
```

of all these files and directories, there are quite some configuration files for technologies used by Quasar. Nothing has to be changed about these files. These can be looked at, but it is not necessary.

All configuration files:

```sh
.editorconfig
.eslintignore
.eslintrc.js
.gitignore
.prettierrc
postcssrc.config.js
quasar.config.js
tsconfig.json
tsconfig.node.json
```

What is left:

```sh
node_modules/
public/
src/
index.html
package-lock.json
package.json
README.md
```

Two of these files and one directory belong to `npm`/`node`:

```sh
node_modules/
package-lock.json
package.json
```

`package.json` contains the information of this `Node` project. This contains things like the `dependencies` and `devDependencies`, this describes the software that is necessary to run the project.

These `dependencies` and `devDependencies` are installed when `Yes, use NPM` was selected. In case the choice was `No, I will handle that myself`, then a manual `npm install` would be necessary.

The `package-lock.json` file is automatically generated when executing `npm install`. No manual changes are necessary.

The directory `node_modules/` is added by `npm install`. It contains all the dependencies that were installed.

What is left:

```sh
public/
src/
index.html
README.md
```

The `README.md` file is a file that can be found in most projects. The extension `.md` is used for Markdown files. This usually contains information to work with the project. As an example, in the `README.md` of this project shows that `npm install` should be executed to run the project.

`public/` contains files that should be copied over to the eventual build directory. This contains the logo and images could be added.

`src/` stands for `source`, this contains the source code of the Quasar project. This is the directory that will contain all self-written code.

`index.html` is het bestand waarin de Vue-applicatie toegevoegd zal worden.

De content of `index.html` is:

```html
<!doctype html>
<html>
  <head>
    <title><%= productName %></title>

    <meta charset="utf-8" />
    <meta name="description" content="<%= productDescription %>" />
    <meta name="format-detection" content="telephone=no" />
    <meta name="msapplication-tap-highlight" content="no" />
    <meta
      name="viewport"
      content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width<% if (ctx.mode.cordova || ctx.mode.capacitor) { %>, viewport-fit=cover<% } %>"
    />

    <link
      rel="icon"
      type="image/png"
      sizes="128x128"
      href="icons/favicon-128x128.png"
    />
    <link
      rel="icon"
      type="image/png"
      sizes="96x96"
      href="icons/favicon-96x96.png"
    />
    <link
      rel="icon"
      type="image/png"
      sizes="32x32"
      href="icons/favicon-32x32.png"
    />
    <link
      rel="icon"
      type="image/png"
      sizes="16x16"
      href="icons/favicon-16x16.png"
    />
    <link rel="icon" type="image/ico" href="favicon.ico" />
  </head>
  <body>
    <!-- quasar:entry-point -->
  </body>
</html>
```

This shows some odd syntax `<%= productName %>`, this is the syntax of a `templating language`. This can be replace by normal hard coded information. But it can also stay the way it is.

The Quasar Framework will add several things to this file behind the scenes. One of these things being added is `<div id="q-app"></div>`, which is added to the body. This will host the Vue application.

### src

The `src` directory will be further discussed.

```sh
assets/
boot/
components/
css/
layouts/
pages/
router/
App.vue
env.d.ts
quasar.d.ts
shims-vue.d.ts
```

All files with the exentension `.d.ts` are TypeScript definitions. These files are used to define the types of the code.

The `assets` can contain images and other files to be used in the project.

The `boot` directory is a Quasar specific directory. This will contain dependencies that should be linked before vue is loaded.

The directories `components`, `layouts` and `pages` all contain `.vue` files. Every `.vue` file is a `component`.

The directory `css` contains global styling.

The directory `router` contains files related to `vue-router`. This makes it such that different pages can be loaded on different `routes`. E.g. `https://code-coaching.dev/` will load the home page and `https://code-coaching.dev/blogs` will load the blog page.

That leaves the `App.vue` file. This is the main component of the application, where `Vue router` is linked to load all the other components.

## Running a Quasar project

As can be seen in the `README.md` file, the project can be run by executing `npm install`.

```sh
npm install
```

The project can then be run in development mode by executing `quasar dev`.

```sh
quasar dev
```

The browser will automatically open and the project will be loaded. By default this is on `http://localhost:8080`.

The project works without any problems. However, in `src/pages/Index.vue` something is underlined in red. This is not a problem, it works. If the red line is disturbing, it can be solved by using a `relative path`:

```ts
// import { Todo, Meta } from 'components/models';
import { Todo, Meta } from "../components/models";
```
