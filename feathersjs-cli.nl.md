---
  title: FeathersJS CLI - een nieuw FeathersJS-project aanmaken
  keywords: ["FeathersJS", "TypeScript"]
  modules: ["Backend"]
---

Vereisten om dit blog optimaal te volgen:

- [basiskennis JavaScript](blog/nl/javascript-basis/), de kennis bouwt verder op JavaScript
- [basiskennis TypeScript](blog/nl/van-javascript-naar-typescript/), er wordt gebruikgemaakt van TypeScript
- basiskennis [NodeJS](https://nodejs.org/en/), kunnen uitvoeren van `npm`-commando's

Wanneer er verwezen wordt naar `terminal` in onderstaande tekst en er wordt met Windows gewerkt, dan moet het uitgevoerd worden in `Git Bash`. Indien CTRL+V niet werkt om gekopieerde inhoud te plakken in de terminal, probeer SHIFT+INSERT.

## Voorbereiding

Tijdens de installatie zal er gevraagd worden naar een url die wijst naar een database. De makkelijkste manier om dit te doen is om een gratis account te maken op [MongoDB Atlas](https://www.mongodb.com).

Zorg dat er een een database aangemaakt is (gratis optie is voldoende).

![database](/img/blog/mongodb-database.png)

Klik vervolgens op `Connect`. En kies voor `Connect to application`.

![database-popup](/img/blog/mongodb-connect-your-application.png)

De connectiestring die hier getoond wordt, is de connectiestring die tijdens het aanmaken van het project nodig is.

![database-connection-string](/img/blog/mongodb-connection-string.png)

## Wat is FeathersJS

FeathersJS is een framework om real-time-applicaties te bouwen en om een REST API op te zetten met JavaScript of TypeScript. FeathersJS kan werken met verschillende databases (in dit blog wordt gebruikgemaakt van MongoDB). Het kan gebruikt worden met alle frontendtechnologieën (VueJS, Angular, React, Android, iOS).

In de rest van het artikel zal `Feathers` gebruikt worden om naar `FeathersJS` te verwijzen.

## Feathers

Feathers voorziet een `CLI` (**C**ommand **L**ine **I**nterface). Dit is software die werkt in een terminal. Om de Feathers CLI te kunnen gebruiken, moet deze op het systeem geïnstalleerd worden.

```sh
npm install @feathersjs/cli -g
```

Bovenstaand commando zal de Feathers CLI globaal installeren. Hierdoor is het op heel het systeem beschikbaar.

Bevestig dat het installeren gelukt is.

```sh
feathers --version
```

Indien dit een nummer uitprint (bv. `4.5.0`), is het installeren gelukt.

## Een nieuw Feathers project aanmaken

We werken in dit blog vanuit de `home directory`. Hier kan heen genavigeerd worden via onderstaand commando.

```sh
cd ~
```

Maak een nieuwe map aan genaamd `mijn-eerste-feathers-project`.

```sh
mkdir mijn-eerste-feathers-project
```

Navigeer in deze map.

```sh
cd mijn-eerste-feathers-project
```

In deze map gaat er gebruikgemaakt worden van de Feathers CLI om een nieuw Feathers-project aan te maken.

```sh
feathers generate app
```

Opmerking: `feathers generate app` kan gelezen worden als `maak een nieuw Feathers project in de huidige map`.

Dit zal in de terminal enkele vragen stellen.

Gebruik de pijltoetsen om naar `TypeScript` te gaan en klik op `spatie` om dit te selecteren. Klik op `Enter` om de keuze te bevestigen.

```sh
? Do you want to use JavaScript or TypeScript?
  JavaScript
❯ TypeScript
```

Klik drie keer op enter.

```sh
? Project name premium-backend
? Description
? What folder should the source files live in? src
```

Klik opnieuw op enter om `npm` te bevestigen.

```sh
? Which package manager are you using (has to be installed globally)? (Use arrow key
s)
❯ npm
  Yarn
```

Gebruik de pijltoetsen om te navigeren. Gebruik `spatie` om een keuze aan of uit te zetten. Zorg dat enkel `REST` een ingekleurd bolletje heeft. Klik op `Enter` om de keuze te bevestigen.

```sh
? What type of API are you making?
 ◉ REST
❯◯ Realtime via Socket.io
 ◯ Realtime via Primus
```

Kies voor `Jest` als testing framework.

```sh
? Which testing framework do you prefer?
  Mocha + assert
❯ Jest
```

Klik op `Enter` om de standaardkeuze te bevestigen.

```sh
? This app uses authentication (Y/n)
```

Klik op `Enter` om de standaardkeuze te bevestigen.

```sh
? What authentication strategies do you want to use? (See API docs for all 180+ supp
orted oAuth providers)
❯◉ Username + Password (Local)
 ◯ Auth0
 ◯ Google
 ◯ Facebook
 ◯ Twitter
 ◯ GitHub
```

Klik op `Enter` om de standaardkeuze te bevestigen.

```sh
? What is the name of the user (entity) service? (users)
```

Gebruik de pijltoetsen om te navigeren naar `Mongoose` en klik op `Enter`.

```sh
? What kind of service is it?
  In Memory
  NeDB
  MongoDB
❯ Mongoose
  Sequelize
  KnexJS
  Objection
(Move up and down to reveal more choices)
```

Klik op Enter om de default `conncection string` te gebruiken. Deze zal later uitgewisseld worden.

```sh
? What is the database connection string? mongodb://localhost:27017/premium_backend
```

## Projectstructuur van een Feathers-project

```sh
config/
node_modules/
public/
src/
test/
.editorconfig
.eslintrc.json
.gitignore
jest.config.js
package-lock.json
package.json
README.md
tsconfig.json
```

Van al deze bestanden en mappen, zijn er veel configuratiebestanden voor technologieën die gebruikt worden door Feathers. Hier hoeft niks aan gewijzigd te worden. Deze kunnen eens bekeken worden, maar zelfs dit is niet nodig.

Alle configuratiebestanden:

```sh
.editorconfig
.eslintrc.json
.gitignore
jest.config.js
tsconfig.json
```

Wat er nog overblijft:

```sh
config/
node_modules/
public/
src/
test/
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

Deze `dependencies` en `devDependencies` zijn geïnstalleerd toen er gekozen erd voor `npm`. Indien dit project via git binnengehaald zou zijn, dan zou er nu manueel een `npm install` uitgevoerd moeten worden om alle `dependencies` te installeren.

Het `package-lock.json`-bestand wordt automatisch gegenereerd na het uitvoeren van een `npm install`. Hierin moeten geen manuele aanpassingen gebeuren.

De map `node_modules/` wordt toegevoegd door `npm install`, hierin zit alle benodigde software uit `dependencies` en `devDependencies`.

Wat er nog overblijft:

```sh
config/
public/
src/
test/
README.md
```

De `README.md` is een bestand dat teruggevonden wordt in bijna alle projecten. De extensie `.md` geeft aan dat het een `Markdown`-bestand is. Hierin staat meestal de informatie die nodig is om met het project te werken. Bijvoorbeeld in de `README.md` van dit project is terug te vinden dat `npm install` uitgevoerd moet worden om alle `dependencies` (afhankelijke software) te installeren.

`public/` bevat alle bestanden die getoond worden wanneer het project online gezet wordt. Hierin zou bijvoorbeeld een frontend applicatie geplaatst kunnen worden.

`config/` bevat `JSON`-bestanden. Hierin steekt omgevingspecifieke configuratie. Zo wordt `default.json` gebruikt tijdens development en `production.json` wanneer de applicatie gebouwd wordt.

```sh
default.json
production.json
test.json
```

`test/` bevat code die de geschreven code aftest, er is tijdens de keuzes gekozen voor `Jest`, hier zijn de bestanden terug te vinden waar gebruikgemaakt wordt van `Jest`.

`src/` staat voor `source` (Nederlands: bron), hierin is de broncode van het Feathers-project terug te vinden. Dit is de map waarin alle zelfgeschreven code toegevoegd zal worden.

### src

De `src`-map wordt verder besproken.

```sh
middleware/
models/
services/
app.hooks.ts
app.ts
authentication.ts
channels.ts
declarations.d.ts
index.ts
logger.ts
mongoose.ts
```

Alle bestanden met de extensie `.d.ts` zijn er omdat er gekozen is voor `TypeScript`, dit bevat informatie over de types die gebruikt worden.

Alle bestanden met de extensie `.hooks.ts` zijn bestanden die `hooks` bevatten. Dit is een concept binnen Feathers om bepaalde code uit te voeren voor of na bepaalde acties of wanneer er een error voor komt.

Het bestand `app.ts` bevat alle code om de Feathers-applicatie te configureren. Hierin is bijvoorbeeld de koppeling terug te vinden tussen alle `middleware`, de `authentication` en alle `services`. Voor de mensen die gekend zijn met `express` zal dit een bekend concept zijn.

Het bestand `authentication.ts` bevat de logica omtrent de gekozen strategieën. In dit project is er enkel gekozen voor registratie via e-mail en wachtwoord.

Het bestand `channels.ts` bevat logica omtrent de real-time-functionaliteit. Dit zal niet gebruikt worden indien Feathers wordt gebruikt om een REST API op te zetten.

Het bestand `index.ts` bevat de logica om de applicatie op te starten.

Het bestand `logger.ts` bevat logica omtrent de configuratie voor het loggen van data.

Het bestand `mongoose.ts` bevat de logica omtrent `Mongoose`, de technologie die tijdens de configuratie gekozen is om met een `MongoDB`-database te werken.

Dan blijven de mappen nog over.

```sh
middleware/
models/
services/
```

De term `middleware` betekent `software die ergens **middenin** draait`. In de map `middleware` zal code gestoken worden die uitgevoerd dient te worden om bijvoorbeeld data om te vormen voor er verder gegaan wordt in het hele proces.

De map `models` bevat de modellen van de data, in dit geval gekoppeld aan de modellen van de database.

De map `services` bevat de logica omtrent de verschillend `end points`. Aangezien er gekozen is voor authenticatie met een service genaamd `user`, is deze al aanwezig. Elke server bestaat uit een `class`, `hooks` (logica die op specifieke momenten gedurende de afhandeling van een verzoek uitgevoerd moet worden) en een `service` (bestand waarin het `end point` geconfigureerd wordt).

## Feathers-project opstarten

### Extra typings installeren

Tijdens de installatie gaat er soms iets mis met het installeren van de `devDependencies`. Om zeker te zijn dat alles geïnstalleerd is, kan onderstaand commando uitgevoerd worden.

```sh
npm install --save-dev @types/serve-favicon @types/compression @types/cors @types/jest @types/axios ts-node-dev shx typescript
```

Dit zal `npm install` uitvoeren, met `--save-dev`, de dependencies zullen dus toegevoegd worden aan de property `devDependencies` in `package.json`. Wat zal er geïnstalleerd worden? `@types/serve-favicon` en `@types/compression` en ...

Goed om te weten, niet alle software komt met TypeScript types. Er is een plaats op `npm`, genaamd `@types` waar andere developers `.d.ts` bestanden kunnen uploaden voor software waarvoor er geen typings zijn.

### Tijdelijk probleem

Indien `npm run compile` een error toont:

```sh
npm run compile

> premium-backend@0.0.0 compile
> shx rm -rf lib/ && tsc

node_modules/feathers-mongoose/types/index.d.ts:25:35 - error TS2344: Type 'T & Document<any, any, any>' does not satisfy the constraint 'Document<any, any, any>'.
  The types of 'replaceOne(...).$where(...).and' are incompatible between these types.
    Type '(array: FilterQuery<T & Document<any, any, any>>[]) => Query<(T & Document<any, any, any>)[], T & Document<any, any, any>, {}, T & Document<...>>' is not assignable to type '(array: FilterQuery<Document<any, any, any>>[]) => Query<Document<any, any, any>[], Document<any, any, any>, {}, Document<any, any, any>>'.
      Types of parameters 'array' and 'array' are incompatible.
        Type 'FilterQuery<Document<any, any, any>>[]' is not assignable to type 'FilterQuery<T & Document<any, any, any>>[]'.
          Type 'FilterQuery<Document<any, any, any>>' is not assignable to type 'FilterQuery<T & Document<any, any, any>>'.
            Type 'FilterQuery<Document<any, any, any>>' is not assignable to type '{ [P in keyof (T & Document<any, any, any>)]?: Condition<[Extract<(T & Document<any, any, any>)[P], ObjectId>] extends [never] ? (T & Document<any, any, any>)[P] : string | (T & Document<...>)[P]> | undefined; }'.

25   options: MongooseServiceOptions<T & Document>;
                                     ~~~~~~~~~~~~


Found 1 error.
```

Dan kan dit opgelost worden door [node_modules/feathrs-mongoose/types/index.d.ts](https://github.com/feathersjs-ecosystem/feathers-mongoose/pull/413/files) te updaten met de wijziging. Klik op de link om de wijziging te zien.

### MongoDB koppelen

Er is gekozen voor MongoDB, tijdens het configureren van het project is de standaard configuratiestring gebruikt. Dit wijst niet naar de online database die eerder is opgezet.

Bij het opstarten van het project via `npm run start` of `npm run dev` zal een error getoond worden dat het niet lukt om te connecteren.

Om te zorgen dat dit wijst naar de online database, moet `config/default.json` gewijzigd worden.

```diff
...
       "passwordField": "password"
     }
   },
-  "mongodb": "mongodb://localhost:27017/premium_backend"
+  "mongodb": "mongodb+srv://GEBRUIKERSNAAM:WACHTWOORD@barrybot-dev-mri2b.mongodb.net/DATABASENAAM?authSource=admin&replicaSet=barrybot-dev-shard-0&readPreference=primary&appname=MongoDB%20Compass&retryWrites=true&ssl=true"
 }
```

**Opmerking**: voeg dit nog niet toe aan Git, lees het stukje op het einde over `.env`-bestanden.

#### Gebruikersnaam

Vervang `GEBRUIKERSNAAM` met de naam van de gebruiker, terug te vinden in de connectiestring.

![database-connection-string](/img/blog/mongodb-connection-string.png)

In bovenstaande afbeelding is de gebruikersnaam `barrybot_dev_user`.

#### Wachtwoord

Vervang `WACHTWOORD` door het wachtwoord van de gebruiker. Dit is het wachtwoord van de gebruiker die terug te vinden is onder `database access`.

![database-connection-string](/img/blog/mongodb-database-access.png)

#### Databasenaam

Vervang `DATABASENAAM` door de naam van de database die gebruikt zal worden. Deze hoeft niet op voorhand aangemaakt te worden. Indien de database niet bestaat, dan zal deze aangemaakt worden bij het maken van de eerste connectie.

### Effectief opstarten van de applicatie

Zoals in de `README.md` terug te vinden is, kunnen alle `dependencies` (de afhankelijke software van dit project) geïnstalleerd worden via:

```sh
npm install
```

En kan het project opgestart worden via:

```sh
npm run start
```

Dit zal het project opstarten.

In `package.json` staan nog meer scripts die gebruikt kunnen worden. Zo is er een `dev` script gedefinieerd, dit kan gebruikt worden tijdens het ontwikkelen van de applicatie.

```sh
npm run dev
```

Bevestig dat de applicatie werkt door de url te bezoeken die uitgeprint wordt in de terminal.

```sh
info: Feathers application started on http://localhost:3030
```

In dit geval [http://localhost:3030](http://localhost:3030).

In de online MongoDB kan de aangemaakte database/collectie nu teruggevonden worden door op `Browse Collections` te klikken.

![mongodb-browse-collections](/img/blog/mongodb-browse-collections.png)

De aangemaakte end point `users` kan bezocht worden op `/users`. [http://localhost:3030/users](http://localhost:3030/users). Dit zal een `Unauthorized` tonen.

Het project is nu aangemaakt. Extra functionaliteit en gebruikers toevoegen wordt in een volgende blogpost aangehaald.

## .env

Er is momenteel een probleem. De bestanden in de map `config/` mogen mee in Git gestoken worden. Maar het bestand `config/default.json` bevat nu geheime informatie.

```sh
"mongodb": "mongodb+srv://GEBRUIKERSNAAM:WACHTWOORD@barrybot-dev-mri2b.mongodb.net/DATABASENAAM?authSource=admin&replicaSet=barrybot-dev-shard-0&readPreference=primary&appname=MongoDB%20Compass&retryWrites=true&ssl=true"
```

Namelijk het `WACHTWOORD`. Geheime informatie mag nooit gecommit worden naar Git. Om dit op te lossen kan er gebruikgemaakt worden van `.env`-bestanden.

Installeer `dotenv` en de stylings voor dit pakket.

```sh
npm install dotenv
```

Om te zorgen dat de `.env`-bestanden ingeladen worden. Moet er iets toegevoegd worden aan `app.ts`.

```diff
...
 import configuration from "@feathersjs/configuration";
 import express from "@feathersjs/express";

+import dotenv from "dotenv";
+dotenv.config();
+
 import { Application } from "./declarations";
 import logger from "./logger";
 import middleware from "./middleware";
...
```

Maak nu een bestaand aan genaamd `.env` in de root van het project.

```sh
touch .env
```

Zorg ervoor dat dit nieuwe bestand niet aan Git toegevoegd wordt door het toe te voegen aan het bestand `.gitignore`.

```diff
+.env
+
 # Logs
 logs
 *.log
...
```

Vervang de connectiestring in `config/default.json` door een key die toegevoegd zal worden in het `.env`-bestand.

```diff
       "passwordField": "password"
     }
   },
-  "mongodb": "mongodb+srv://GEBRUIKERSNAAM:WACHTWOORD@barrybot-dev-mri2b.mongodb.net/DATABASENAAM?authSource=admin&replicaSet=barrybot-dev-shard-0&readPreference=primary&appname=MongoDB%20Compass&retryWrites=true&ssl=true"
+  "mongodb": "MONGO_DB"
 }
```

Voeg de connectiestring toe aan het `.env`-bestand.

```sh
MONGO_DB="mongodb+srv://GEBRUIKERSNAAM:WACHTWOORD@barrybot-dev-mri2b.mongodb.net/DATABASENAAM?authSource=admin&replicaSet=barrybot-dev-shard-0&readPreference=primary&appname=MongoDB%20Compass&retryWrites=true&ssl=true"
```

Start nu het project opnieuw op om te bevestigen dat het `.env`-bestand gebruikt wordt.

```sh
npm run dev
```

## Code

De volledige code kan van dit blog kan [hier](https://github.com/bartduisters/feathers-ts-template/releases/tag/first-blog) gedownload worden.

Opmerking: Bij bovenstaande link is er tijdens de configuratie niet voor gekozen om de `real-time`-optie uit te zetten. Dit heeft als gevolg dat bijvoorbeeld `@feathersjs/socketio` voorkomt in `package.json` en dat deze dependency ingekoppeld wordt in `app.ts`.
