---
postUuid: b01db0b2-ec19-4650-8ac8-cbdab35ad7e3
title: Quasar CLI - creating a new Quasar project
slug: quasar-cli-creating-a-new-quasar-project
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

In case it prints a number (e.g. `1.2.1`), it means the installation was successful.

Note: If the number is `1.3.0` or higher, take a look at the blog post `Quasar CLI 1.3.0 - creating a new Quasar project`.

## Creating a new Quasar project

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
quasar create .
```

Note: The dot `.` means `in this directory`. `quasar create .` can be read as `create a new Quasar project in the current directory`.

Note: In case the command has the output seen below, then take a look at `Quasar CLI 1.3.0 - creating a new Quasar project`.

Output of the command if Quasar CLI is v1.3.0 or higher.
If this is not the output, then the version is lower than 1.3.0 and then the current blog post can be continued. However, it is recommended to use the new version.

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

 This command is now reserved only for scaffolding
 with CUSTOM starter kits or legacy v0.x projects.

 For scaffolding an official Quasar project please use this instead:

   yarn create quasar
   # or
   npm init quasar
```

This will show a couple of questions in the terminal.

Press five times `Enter` to continue.

```sh
? Generate project in current directory? Yes
? Project name (internal usage for dev) my-first-quasar-project
? Project product name (must start with letter if building mobile apps) Quasar A
pp
? Project description A Quasar Framework app
? Author Bart Duisters <bartduisters@bartduisters.com>
```

Pick `Sass with SCSS syntax` by hitting `Enter`.

```sh
? Pick your CSS preprocessor: (Use arrow keys)
❯ Sass with SCSS syntax
  Sass with indented syntax
  None (the others will still be available)
```

Use the arrow keys to navigate to `TypeScript` and hit `space` to select this option. This results in both `ESlint` and `TypeScript` being enabled. Hit `Enter`.

```sh
? Check the features needed for your project:
 ◉ ESLint (recommended)
❯◉ TypeScript
 ◯ Vuex
 ◯ Axios
 ◯ Vue-i18n
```

Opt for `Composition API` by hitting `Enter`.

```sh
? Pick a component style: (Use arrow keys)
❯ Composition API (recommended) (https://github.com/vuejs/composition-api)
  Class-based (https://github.com/vuejs/vue-class-component & https://github.com/kaorun343/vue-
property-decorator)
  Options API
```

Opt for `Prettier` by hitting `Enter`.

```sh
? Pick an ESLint preset: (Use arrow keys)
❯ Prettier (https://github.com/prettier/prettier)
  Standard (https://github.com/standard/standard)
  Airbnb (https://github.com/airbnb/javascript)
```

Use the arrow keys to navigate to `Yes, use NPM` and hit `Enter`.

```sh
? Continue to install project dependencies after the project has been created? (recommended)
  Yes, use Yarn (recommended)
❯ Yes, use NPM
  No, I will handle that myself
```

Quasar will start creating a project. The following is the output:

```sh
  ___
 / _ \ _   _  __ _ ___  __ _ _ __
| | | | | | |/ _` / __|/ _` | '__|
| |_| | |_| | (_| \__ \ (_| | |
 \__\_\\__,_|\__,_|___/\__,_|_|



? Generate project in current directory? Yes
? Project name (internal usage for dev) my-first-quasar-project
? Project product name (must start with letter if building mobile apps) Quasar App
? Project description A Quasar Framework app
? Author Bart Duisters <bartduisters@bartduisters.com>
? Pick your CSS preprocessor: SCSS
? Check the features needed for your project: ESLint (recommended), TypeScript
? Pick a component style: Composition
? Pick an ESLint preset: Prettier
? Continue to install project dependencies after the project has been created? (recommended) NPM
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
.postcssrc.js
.prettierrc
babel.config.js
package-lock.json
package.json
quasar.conf.js
README.md
tsconfig.json
```

of all these files and directories, there are quite some configuration files for technologies used by Quasar. Nothing has to be changed about these files. These can be looked at, but it is not necessary.

All configuration files:

```sh
.editorconfig
.eslintignore
.eslintrc.js
.gitignore
.postcssrc.js
.prettierrc
babel.config.js
quasar.conf.js
tsconfig.json
```

What is left:

```sh
node_modules/
public/
src/
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
README.md
```

The `README.md` file is a file that can be found in most projects. The extension `.md` is used for Markdown files. This usually contains information to work with the project. As an example, in the `README.md` of this project shows that `npm install` should be executed to run the project.

`public/` contains files that should be copied over to the eventual build directory. This contains the logo and images could be added.

`src/` stands for `source`, this contains the source code of the Quasar project. This is the directory that will contain all self-written code.

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
index.template.html
quasar.d.ts
shims-vue.d.ts
```

All files with the exentension `.d.ts` are TypeScript definitions. These files are used to define the types of the code.

The `assets` can contain images and other files to be used in the project.

The `boot` directory is a Quasar specific directory. This will contain dependencies that should be linked before vue is loaded.

The directories `components`, `layouts` and `pages` all contain `.vue` files. Every `.vue` file is a `component`.

The directory `css` contains global styling.

The directory `router` contains files related to `vue-router`. This makes it such that different pages can be loaded on different `routes`. E.g. `https://code-coaching.dev/` will load the home page and `https://code-coaching.dev/blogs` will load the blog page.

All that is left, is the `App.vue` file. and the `index.template.html` file. The `App.vue` file is the main component.

The content of `index.template.html` is:

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
    <!-- DO NOT touch the following DIV -->
    <div id="q-app"></div>
  </body>
</html>
```

This shows some odd syntax `<%= productName %>`, this is the syntax of a `templating language`. This can be replace by normal hard coded information. But it can also stay the way it is. The most important part of this file is the element with id `q-app`.

```html
<!-- DO NOT touch the following DIV -->
<div id="q-app"></div>
```

This is the element on which VueJS will do its magic. This explains the comment `<!-- DO NOT touch the following DIV -->`.

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
