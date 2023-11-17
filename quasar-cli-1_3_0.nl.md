---
postUuid: 42162bb1-7ed1-4c91-b4a3-855c5b4753be
title: Quasar CLI 1.3.0 - een nieuw Quasar-project aanmaken
slug: quasar-cli-1-3-0-een-nieuw-quasar-project-aanmaken
tags:
  - Quasar
  - Vue
categories:
  - Frontend
---

## Wat is Quasar

Quasar is een framework gebaseerd op VueJS waarmee het mogelijk is om applicaties te bouwen voor verschillende platformen met één code base. Het is dus mogelijk om met webtechnologie code te schrijven die werkt op alle platformen:

- Webbrowsers
- Desktop: Linux / MacOS / Windows
- Smartphones: Android / iOS

## Quasar CLI

Wanneer er verwezen wordt naar `terminal` in onderstaande tekst en er wordt met Windows gewerkt, dan moet het uitgevoerd worden in `Git Bash`. Indien CTRL+V niet werkt om gekopieerde inhoud te plakken in de terminal, probeer SHIFT+INSERT.

Quasar voorziet een `CLI` (**C**ommand **L**ine **I**nterface). Dit is software die werkt in een terminal. Om de Quasar CLI te kunnen gebruiken, moet deze op het systeem geïnstalleerd worden.

Opmerking: Dit commando is ook het commando dat gebruikt wordt om de Quasar CLI te updaten naar de laatste versie.

```sh
npm install -g @quasar/cli
```

Bovenstaand commando zal de Quasar CLI globaal installeren. Hierdoor is het op heel het systeem beschikbaar.

Bevestig dat het installeren gelukt is.

```sh
quasar -v
```

Indien dit een nummer uitprint (bv. `1.3.0`), is het installeren gelukt.

## Een nieuw Quasar project aanmaken

In dit blog wordt er vanuit de `home directory`. Hier kan heen genavigeerd worden via onderstaand commando.

```sh
cd ~
```

Maak een nieuwe map aan genaamd `mijn-eerste-quasar-project`.

```sh
mkdir mijn-eerste-quasar-project
```

Navigeer in deze map.

```sh
cd mijn-eerste-quasar-project
```

In deze map gaat er gebruikgemaakt worden van de Quasar CLI om een nieuw Quasar-project aan te maken.

Belangrijk voor Windows-gebruikers: Er is een bug in `git bash` waardoor onderstaand commando niet werkt. Gebruik een gewone `Command Prompt` (**niet** PowerShell) om dit commando uit te voeren.

```sh
npm init quasar
```

De eerste keer dat dit commando uitgevoerd wordt, zal er gevraagd worden om extra software te installeren. Klik op `Enter`.

```sh
Need to install the following packages:
  create-quasar
Ok to proceed? (y)
```

Dit zal in de terminal enkele vragen stellen.

Klik drie keer op `Enter` om door te gaan.

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

Gebruik de pijltoetsen om TypeScript te kiezen. Klik op `Enter`.

```sh
? Pick script type: › - Use arrow-keys. Return to submit.
    Javascript
❯   Typescript
```

Indien het project **niet** voor productie dient, kan gebruikgemaakt worden van de snellere variant die gebruiktmaakt van `Vite`. Indien deze uit `BETA stage` is en ook aangeduid staat als `stable`, dan is het ook oké om deze optie te kiezen indien het wél voor productie is.

```sh
? Pick Quasar App CLI variant: › - Use arrow-keys. Return to submit.
    Quasar App CLI with Webpack (stable)
❯   Quasar App CLI with Vite (BETA stage)
```

Kies alle standaard keuzes bij de volgende vragen.

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

## Projectstructuur van een Quasar-project

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

Van al deze bestanden en mappen, zijn er veel configuratiebestanden voor technologieën die gebruikt worden door Quasar. Hier hoeft niks aan gewijzigd te worden. Deze kunnen eens bekeken worden, maar zelfs dit is niet nodig.

Alle configuratiebestanden:

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

Wat er nog overblijft:

```sh
node_modules/
public/
src/
index.html
package-lock.json
package.json
README.md
```

Hiervan behoren twee bestanden en een map toe tot `npm`/`node`. Namelijk:

```sh
node_modules/
package-lock.json
package.json
```

`package.json` bevat de informatie van dit `Node`-project. Hierin staan bijvoorbeeld de `dependencies` en `devDependencies`, dit omschrijft de software die nodig is om het project te doen werken.

Deze `dependencies` en `devDependencies` zijn geïnstalleerd toen er gekozen erd voor `Yes, use NPM`. Indien er gekozen was voor `No, I will handle that myself`, dan zou er nu manueel een `npm install` uitgevoerd moeten worden.

Het `package-lock.json`-bestand wordt automatisch gegenereerd na het uitvoeren van een `npm install`. Hierin moeten geen manuele aanpassingen gebeuren.

De map `node_modules/` wordt toegevoegd door `npm install`, hierin zit alle benodigde software uit `dependencies` en `devDependencies`.

Wat er nog overblijft:

```sh
public/
src/
index.html
README.md
```

De `README.md` is een bestand dat teruggevonden wordt in bijna alle projecten. De extensie `.md` geeft aan dat het een `Markdown`-bestand is. Hierin staat meestal de informatie die nodig is om met het project te werken. Bijvoorbeeld in de `README.md` van dit project is terug te vinden dat `npm install` uitgevoerd moet worden om alle `dependencies` (afhankelijke software) te installeren.

`public/` bevat alle bestanden die overgenomen moeten worden wanneer het project online gezet moet worden, hierin is bijvoorbeeld het logo terug te vinden en er kunnen afbeeldingen toegevoegd worden.

`src/` staat voor `source` (Nederlands: bron), hierin is de broncode van het Quasar-project terug te vinden. Dit is de map waarin alle zelfgeschreven code toegevoegd zal worden.

`index.html` is het bestand waarin de Vue-applicatie toegevoegd zal worden.

De inhoud van `index.html` is:

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

Hier zien we een rare syntax `<%= productName %>`, dit is de syntax van een `templating language` (Nederlands: sjabloontaal). Dit kan vervangen worden door gewone hard coded informatie. Maar het mag ook gewoon zo blijven staan.

Het Quasar Framework zal verschillende dingen toevoegen aan dit bestand achter de schermen. Een van deze dingen is `<div id="q-app"></div>` dat toegevoegd zal worden in de body. Hierin zal de Vue-applicatie worden geplaatst.

### src

De `src`-map wordt verder besproken.

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

Alle bestanden met de extensie `.d.ts` zijn er omdat er gekozen is voor `TypeScript`, dit bevat informatie over de types die gebruikt worden.

De map `assets` kan afbeeldingen en andere bestanden bevatten die gebruikt worden in het project.

De map `boot` is Quasar-specifiek, hierin komen dependencies die gekoppeld moeten worden voordat Vue opgestart is.

In de mappen `components`, `layouts` en `pages` zitten allemaal `.vue`-bestanden. Elk `.vue`-bestand is een `component`.

De map `css` bevat globale styling.

De map `router` bevat bestanden gerelateerd aan `vue-router`. Dit zorgt ervoor dat er verschillende pagina's ingeladen kunnen worden op verschillend `routes`. Bijvoorbeeld `https://code-coaching.dev/` voor de hoofdpagina en `https://code-coaching.dev/blogs` voor de blogpagina.

Dan blijft er nog het `App.vue`-bestand over. Dit is het hoofdcomponent van de applicatie, waarin `Vue router` gekoppeld zit om alle andere componenten in te laden.

## Quasar-project opstarten

Zoals in de `README.md` terug te vinden is, kunnen alle `dependencies` (de afhankelijke software van dit project) geïnstalleerd worden via:

```sh
npm install
```

En kan het project opgestart worden via:

```sh
quasar dev
```

Dit zal het project opstarten. De browser zal automatisch geopend worden. Standaard op `http://localhost:8080`.

Het project werkt zonder problemen. Echter wordt in `src/pages/Index.vue` iets rood onderstreept. Dit is geen probleem, het werkt. Indien de rode lijn storend is, kan dit opgelost worden door een `relatief pad` te gebruiken:

```ts
// import { Todo, Meta } from 'components/models';
import { Todo, Meta } from "../components/models";
```
