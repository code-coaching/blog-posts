---
  title: Quasar CLI - een nieuw Quasar-project aanmaken
  keywords: ["Quasar", "VueJS"]
  modules: ["Jaar 2"]
---

Wanneer er verwezen wordt naar `terminal` in onderstaande tekst en er wordt met Windows gewerkt, dan moet het uitgevoerd worden in `Git Bash`. Indien CTRL+V niet werkt om gekopieerde inhoud te plakken in de terminal, probeer SHIFT+INSERT.

## Wat is Quasar

Quasar is een framework gebaseerd op VueJS waarmee het mogelijk is om applicaties te bouwen voor verschillende platformen met één code base. Het is dus mogelijk om met webtechnologie code te schrijven die werkt op alle platformen:

- Webbrowsers
- Desktop: Linux / MacOS / Windows
- Smartphones: Android / iOS

## Quasar CLI

Quasar voorziet een `CLI` (**C**ommand **L**ine **I**nterface). Dit is software die werkt in een terminal. Om de Quasar CLI te kunnen gebruiken, moet deze op het systeem geïnstalleerd worden.

```sh
npm install -g @quasar/cli
```

Bovenstaand commando zal de Quasar CLI globaal installeren. Hierdoor is het op heel het systeem beschikbaar.

Bevestig dat het installeren gelukt is.

```sh
quasar -v
```

Indien dit een nummer uitprint (bv. `1.2.1`), is het installeren gelukt.

## Een nieuw Quasar project aanmaken

We werken in dit blog vanuit de `home directory`. Hier kan heen genavigeerd worden via onderstaand commando.

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
quasar create .
```

Opmerking: Het punt betekent `in deze map`. `quasar create .` kan gelezen worden als `maak een nieuw Quasar project in de huidige map`.

Dit zal in de terminal enkele vragen stellen.

Klik vijf keer op `Enter`.

```sh
? Generate project in current directory? Yes
? Project name (internal usage for dev) mijn-eerste-quasar-project
? Project product name (must start with letter if building mobile apps) Quasar A
pp
? Project description A Quasar Framework app
? Author Bart Duisters <bartduisters@bartduisters.com>
```

Kies voor `Sass with SCSS syntax` door op `Enter` te klikken.

```sh
? Pick your CSS preprocessor: (Use arrow keys)
❯ Sass with SCSS syntax
  Sass with indented syntax
  None (the others will still be available)
```

Gebruik de pijltoetsen om naar `TypeScript` te gaan en klik op `spatie` om dit te selecteren. Dit resulteert in zowel `ESLint` als `TypeScript` met een ingekleurd bolletje. Klik op `Enter`.

```sh
? Check the features needed for your project:
 ◉ ESLint (recommended)
❯◉ TypeScript
 ◯ Vuex
 ◯ Axios
 ◯ Vue-i18n
```

Kies voor `Composition API` door op `Enter` te klikken.

```sh
? Pick a component style: (Use arrow keys)
❯ Composition API (recommended) (https://github.com/vuejs/composition-api)
  Class-based (https://github.com/vuejs/vue-class-component & https://github.com/kaorun343/vue-
property-decorator)
  Options API
```

Kies voor `Prettier` door op `Enter te klikken.

```sh
? Pick an ESLint preset: (Use arrow keys)
❯ Prettier (https://github.com/prettier/prettier)
  Standard (https://github.com/standard/standard)
  Airbnb (https://github.com/airbnb/javascript)
```

Gebruik de pijltoetsen om te navigeren naar `Yes, use NPM` en klik op `Enter`.

```sh
? Continue to install project dependencies after the project has been created? (recommended)
  Yes, use Yarn (recommended)
❯ Yes, use NPM
  No, I will handle that myself
```

Quasar zal nu beginnen met een project aan te maken. De volledige output in de terminal is:

```sh
  ___
 / _ \ _   _  __ _ ___  __ _ _ __
| | | | | | |/ _` / __|/ _` | '__|
| |_| | |_| | (_| \__ \ (_| | |
 \__\_\\__,_|\__,_|___/\__,_|_|



? Generate project in current directory? Yes
? Project name (internal usage for dev) mijn-eerste-quasar-project
? Project product name (must start with letter if building mobile apps) Quasar App
? Project description A Quasar Framework app
? Author Bart Duisters <bartduisters@bartduisters.com>
? Pick your CSS preprocessor: SCSS
? Check the features needed for your project: ESLint (recommended), TypeScript
? Pick a component style: Composition
? Pick an ESLint preset: Prettier
? Continue to install project dependencies after the project has been created? (recommended) NPM
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
.postcssrc.js
.prettierrc
babel.config.js
package-lock.json
package.json
quasar.conf.js
README.md
tsconfig.json
```

Van al deze bestanden en mappen, zijn er veel configuratiebestanden voor technologieën die gebruikt worden door Quasar. Hier hoeft niks aan gewijzigd te worden. Deze kunnen eens bekeken worden, maar zelfs dit is niet nodig.

Alle configuratiebestanden:

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

Wat er nog overblijft:

```sh
node_modules/
public/
src/
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
README.md
```

De `README.md` is een bestand dat teruggevonden wordt in bijna alle projecten. De extensie `.md` geeft aan dat het een `Markdown`-bestand is. Hierin staat meestal de informatie die nodig is om met het project te werken. Bijvoorbeeld in de `README.md` van dit project is terug te vinden dat `npm install` uitgevoerd moet worden om alle `dependencies` (afhankelijke software) te installeren.

`public/` bevat alle bestanden die overgenomen moeten worden wanneer het project online gezet moet worden, hierin is bijvoorbeeld het logo terug te vinden en er kunnen afbeeldingen toegevoegd worden.

`src/` staat voor `source` (Nederlands: bron), hierin is de broncode van het Quasar-project terug te vinden. Dit is de map waarin alle zelfgeschreven code toegevoegd zal worden.

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
index.template.html
quasar.d.ts
shims-vue.d.ts
```

Alle bestanden met de extensie `.d.ts` zijn er omdat er gekozen is voor `TypeScript`, dit bevat informatie over de types die gebruikt worden.

De map `assets` kan afbeeldingen en andere bestanden bevatten die gebruikt worden in het project.

De map `boot` is Quasar-specifiek, hierin komen dependencies die gekoppeld moeten worden voordat Vue opgestart is.

In de mappen `components`, `layouts` en `pages` zitten allemaal `.vue`-bestanden. Elk `.vue`-bestand is een `component`.

De map `css` bevat globale styling.

De map `router` bevat bestanden gerelateerd aan `vue-router`. Dit zorgt ervoor dat er verschillende pagina's ingeladen kunnen worden op verschillend `routes`. Bijvoorbeeld `https://code-coaching.dev/` voor de hoofdpagina en `https://code-coaching.dev/contact` voor de contactpagina.

Dan blijft er nog het `App.vue`-bestand over en het `index.template.html`-bestand. Het `App.vue`-bestand is de hoofdcomponent.

De inhoud van `index.template.html` is:

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= productName %></title>

    <meta charset="utf-8">
    <meta name="description" content="<%= productDescription %>">
    <meta name="format-detection" content="telephone=no">
    <meta name="msapplication-tap-highlight" content="no">
    <meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width<% if (ctx.mode.cordova || ctx.mode.capacitor) { %>, viewport-fit=cover<% } %>">

    <link rel="icon" type="image/png" sizes="128x128" href="icons/favicon-128x128.png">
    <link rel="icon" type="image/png" sizes="96x96" href="icons/favicon-96x96.png">
    <link rel="icon" type="image/png" sizes="32x32" href="icons/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="icons/favicon-16x16.png">
    <link rel="icon" type="image/ico" href="favicon.ico">
  </head>
  <body>
    <!-- DO NOT touch the following DIV -->
    <div id="q-app"></div>
  </body>
</html>
```

Hier zien we een rare syntax `<%= productName %>`, dit is de syntax van een `templating language` (Nederlands: sjabloontaal). Dit kan vervangen worden door gewone hard coded informatie. Maar het mag ook gewoon zo blijven staan. Het belangrijkste in dit bestand is het element met de id `q-app`.

```html
<!-- DO NOT touch the following DIV -->
<div id="q-app"></div>
```

Dit is het element waar VueJS in zal werken. Vandaar ook de commentaar erbij `<!-- DO NOT touch the following DIV -->` (Raak onderstaande div NIET aan).

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