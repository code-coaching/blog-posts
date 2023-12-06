---
postUuid: ef9c4aff-2830-4df3-afba-069f29301040
title: Angular Tour of Heroes - API
slug: angular-toh-api
tags:
  - Angular
  - Tour of Heroes
categories:
  - Frontend
---

In dit artikel wordt vertrokken vanuit de situatie waar de layout opgezet is en de verschillende pagina's toegevoegd zijn aan de Tour of Heroes-applicatie. De pagina's zijn voorzien van elementen, data en componenten. De data is overgehuisd naar een service. De eindsituatie is een Angular-project waarin de data gekoppeld zit aan een API en waar authenticatie nodig is.

De code kan bekomen worden door een `zip` van de `Source code` te downloaden.

De situatie waar van vertrokken wordt ziet er als volgt uit:

![beginsituatie](/img/blog/quasar-toh-service-functions.gif)

Beginsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-services)

De eindsituatie waar naartoe gewerkt wordt ziet er als volgt uit:

![eindsituatie](/img/blog/quasar-toh-api-finished.gif)

Eindsituatie: [Source code](https://github.com/code-coaching/angular-tour-of-heroes/releases/tag/angular-toh-api)

## API

Een `API` (Application Programming Interface) is een interface waarmee een applicatie kan communiceren met een andere applicatie. In dit geval, zal de frontendapplicatie communiceren met de backendapplicatie.

De backendapplicatie wordt voorzien door Code Coaching. Registreer een gratis account op [https://code-coaching.dev](https://code-coaching.dev/registreren), deze account zal gebruikt worden om in te loggen en om de data van de heroes te beheren op de server.

## Fetch API (gebruiken we niet)

In andere frameworks wordt er vaak gebruikgemaakt van de `Fetch API` om data op te halen van een API. Deze API is standaard aanwezig in de browser. Vaak wordt er ook gebruikgemaakt van `axios`, een library met wat extra functionaliteit ten opzichte van de `Fetch API`.

Het is ook binnen Angular mogelijk om de `Fetch API` te gebruiken. Zie het voorbeeld hieronder.

Let op: dit voorbeeld is géén werkende code, het is enkel om een idee te geven van hoe de `Fetch API` gebruikt kan worden.

```ts
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  heroes: Array<Hero> = [];

  // hier nog code

  async loadHeroes() {
    const result = await fetch("http://localhost:8000/api/heroes").then((res) =>
      res.json()
    );
    if (result) {
      this.heroes = result;
    }
  }
}
```

Vervolgens kan de `loadHeroes()` functie aangeroepen worden om de data op te halen.

```ts
import { Component, OnInit } from "@angular/core";
import { CommonModule } from "@angular/common";
import { RouterOutlet } from "@angular/router";
import { HeroService } from "./services/hero.service";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent implements OnInit {
  constructor(private heroService: HeroService) {}

  async ngOnInit() {
    await this.heroService.loadHeroes();
  }
}
```

Toch wordt er in deze tutorial geen gebruikgemaakt van de `Fetch API` of `axios`. In plaats daarvan wordt er gebruikgemaakt van de `HttpClient` van Angular.

## HttpClient

De `HttpClient` is een service die standaard aanwezig is in Angular. Deze service kan gebruikt worden om verzoeken te sturen naar een API.

Om het voorbeeld van de `Fetch API` te vergelijken met de `HttpClient`, wordt de `loadHeroes()` functie herschreven naar de `HttpClient`-versie.

Let op: dit voorbeeld is géén werkende code, het is enkel om een idee te geven van hoe de `HttpClient` gebruikt kan worden.

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  heroes = [];

  constructor(private http: HttpClient) {}

  // hier nog code

  loadHeroes() {
    this.http.get("http://localhost:8000/api/heroes").subscribe((result) => {
      this.heroes = result;
    });
  }
}
```

## HttpClient gebruiken

Om de HttpClient te gebruiken, moet deze eerst toegevoegd worden aan de applicatie.

`src/app/app.config.ts`

```ts
import { provideHttpClient, withFetch } from "@angular/common/http"; // importeer provideHttpClient en withFetch
import { ApplicationConfig } from "@angular/core";
import { provideRouter, withHashLocation } from "@angular/router";
import { routes } from "./app.routes";

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withHashLocation()),
    provideHttpClient(withFetch()), // voeg provideHttpClient toe met withFetch
  ],
};
```

Dit zorgt ervoor dat de HttpClient beschikbaar is in de applicatie.

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

Genereer een nieuwe pagina om in te loggen.

```sh
ng g c pages/login --skip-tests
```

`src/pages/login/login.component.html`

```html
<button (click)="login()">Login</button>
```

```ts
import { CommonModule } from "@angular/common";
import { HttpClient } from "@angular/common/http";
import { Component } from "@angular/core";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  constructor(private http: HttpClient) {} // injecteren van de HttpClient service

  login() {
    this.http
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: "john@duck.com", // wijzig naar jouw e-mailadres
        password: "hier-jouw-wachtwoord", // wijzig naar jouw wachtwoord
      })
      .subscribe((data) => {
        console.log(data); // tijdelijke console.log om de data te bekijken
      });
  }
}
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

`this.http.post()` is een `Observable`. Een `Observable` is een object met een `.subscribe`-functie. De `Observable` wordt pas uitgevoerd wanneer `.subscribe()` wordt aangeroepen. Wanneer deze `subscribe()`-functie wordt aangeroepen, dan zal de data die teruggestuurd wordt van de API in de callback functie van `.subscribe()` terechtkomen. In dit geval kennen we de data toe aan de variabele `data`.

Dit end point zal een object terugsturen dat er als volgt uitziet:

```json
{
  "token": "400|915wnnNpHQuCcQlTDvCC50EX0lMMmjWjIin6QoGc0a225b8d"
}
```

Dit is ook het type dat we meegeven bij `this.http.post<{ token: string }>()`.

Belangrijk! Deze token stelt jouw ingelogde gebruiker voor. Net zoals met een wachtwoord is het belangrijk om deze token **niet** te delen met anderen.

Toevoegen van de pagina aan de `src/app/app.routes.ts` file:

```ts
import { Routes } from "@angular/router";
import { MainLayoutComponent } from "./layouts/main-layout/main-layout.component";

export const routes: Routes = [
  {
    path: "",
    component: MainLayoutComponent,
    children: [
      {
        path: "",
        loadComponent: () =>
          import("./pages/dashboard/dashboard.component").then(
            (c) => c.DashboardComponent
          ),
      },
      {
        path: "heroes",
        loadComponent: () =>
          import("./pages/hero-list/hero-list.component").then(
            (c) => c.HeroListComponent
          ),
      },
      {
        path: "heroes/add",
        loadComponent: () =>
          import("./pages/hero-add/hero-add.component").then(
            (c) => c.HeroAddComponent
          ),
      },
      {
        path: "heroes/:id",
        loadComponent: () =>
          import("./pages/hero-details/hero-details.component").then(
            (c) => c.HeroDetailsComponent
          ),
      },
      {
        path: "login",
        loadComponent: () =>
          import("./pages/login/login.component").then((c) => c.LoginComponent),
      },
    ],
  },
];
```

Ga naar `/login` en klik op de knop `Login`. Open de developer tools en bekijk de console. Hierin staat de data die teruggestuurd wordt van de API. Je zal een object zien met een `token` property en met een `user` property. De `user` property wordt momenteel niet gebruikt.

### Formulier voor inloggen

In plaats van het e-mailadres en het wachtwoord hard-coded in de code te zetten, kan er een formulier gemaakt worden waarin de gebruiker een e-mailadres en wachtwoord kan invullen.

`src/pages/login/login.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { HttpClient } from "@angular/common/http";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms"; // importeer FormsModule

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule, FormsModule], // voeg toe aan imports
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  email = ""; // property om te koppelen aan het input-veld voor e-mail
  password = ""; // property om te koppelen aan het input-veld voor wachtwoord

  constructor(private http: HttpClient) {}

  login() {
    this.http
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: this.email, // gebruik de property voor e-mail
        password: this.password, // gebruik de property voor wachtwoord
      })
      .subscribe((data) => {
        console.log(data);
      });
  }
}
```

`src/pages/login/login.component.html`

```html
<!-- Two way binding met [(ngModel)] -->
<input type="email" [(ngModel)]="email" />
<input type="password" [(ngModel)]="password" />
<button (click)="login()">Login</button>
```

Kleine wijziging in `styles.css`, momenteel wordt de styling enkel toegepast op inputs van type `text`, maar er zijn inputs zonder type en inputs met het type `email` en `password` in deze applicatie, wijzig de selector zodat het van toepassing is op alle `input`-elementen.

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

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/9dee85415082ae2904074cdc4470e89e2068ee9a)

## Token opslaan in Local Storage

Er is ondertussen code om in te loggen. Bij het invullen van de juiste gegevens zien we een respons waarin een `token` wordt teruggestuurd. Deze `token` stelt de ingelogde gebruiker voor (de gebruiker met het e-mailadres en het wachtwoord dat ingegeven werd), om deze op een later tijdstip terug aan te kunnen, moet deze ergen opgeslagen worden.

We zullen de token opslaan in `Local Storage`. Dit is een API die beschikbaar is in de browser. Deze API kan gebruikt worden om data op te slaan in de browser. Deze data blijft beschikbaar, ook als de pagina herladen wordt of als de browser afgesloten wordt.

`login.component.ts`

```js
import { CommonModule } from '@angular/common';
import { HttpClient } from '@angular/common/http';
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: './login.component.html',
  styleUrl: './login.component.css',
})
export class LoginComponent {
  email = '';
  password = '';

  constructor(private http: HttpClient) {}

  login() {
    this.http
      .post<{ token: string }>('https://code-coaching.dev/api/token/login', {
        email: this.email,
        password: this.password,
      })
      .subscribe((data) => {
        // verwijder de console.log,
        // sla de token op in Local Storage
        localStorage.setItem('token', data.token);
      });
  }
}
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

Om te zorgen dat de authenticatie van een gebruiker niet per component geïmplementeerd moet worden, zal deze in een service geïmplementeerd worden. Over het algemeen is het een goed idee om `HttpClient`-calls in een service te implementeren.

Genereer een nieuwe service.

```sh
ng g s services/auth --skip-tests
```

Verplaats de code om in te loggen naar de service.

`src/services/auth.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class AuthService {
  constructor(private http: HttpClient) {}

  login(email: string, password: string) {
    this.http
      .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
        email: email,
        password: password,
      })
      .subscribe((data) => {
        localStorage.setItem("token", data.token);
      });
  }
}
```

Merk op: de `login()` functie heeft nu twee parameters, `email` en `password`, zodat we deze kunnen meegeven vanuit de loginpagina.

`src/pages/login/login.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { AuthService } from "../../services/auth.service";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  email = "";
  password = "";

  // injecteer de AuthService (verwijder de HttpClient)
  constructor(private authService: AuthService) {}

  login() {
    // gebruik de AuthService om in te loggen
    this.authService.login(this.email, this.password);
  }
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/3b4f2c11fa49a1aa172f562905f937907daa1dba)

### Resetten van velden

Stel dat een gebruiker inlogt en we willen de velden resetten. Dan zouden we het volgende kunnen doen.

`src/pages/login/login.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { AuthService } from "../../services/auth.service";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  email = "";
  password = "";

  constructor(private authService: AuthService) {}

  login() {
    this.authService.login(this.email, this.password);
    this.email = ""; // reset de email property
    this.password = ""; // reset de password property
  }
}
```

Dit is een oplossing, maar in dit geval zal de gebruiker de velden zien resetten, ongeacht of het inloggen gelukt is of niet. Dit is niet de gewenste situatie.

De gewenste situatie is dat de velden enkel gereset worden als het inloggen gelukt is. We weten wanneer een login gelukt is als we in de callback-functie van `.subscribe()` terechtkomen.

Er is echter een probleem, momenteel wordt `.subscribe()` aangeroepen in de `AuthService`. We zouden graag de `.subscribe()`-functie aanroepen in de `LoginComponent`.

Om dit te doen, moeten we de `AuthService` aanpassen. De `login`-functie zal de `Observable` moeten teruggeven. Vervolgens kunnen we dan `.subscribe()` aanroepen in de `LoginComponent`.

`src/services/auth.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class AuthService {
  constructor(private http: HttpClient) {}

  login(email: string, password: string) {
    // Voeg return toe
    return this.http.post<{ token: string }>(
      "https://code-coaching.dev/api/token/login",
      {
        email: email,
        password: password,
      }
    );
    // Haal de .subscribe() weg uit de AuthService
    // .subscribe((data) => {
    //   localStorage.setItem('token', data.token);
    // });
  }
}
```

`src/pages/login/login.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { AuthService } from "../../services/auth.service";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  email = "";
  password = "";

  constructor(private authService: AuthService) {}

  login() {
    // subscribe in de LoginComponent
    this.authService.login(this.email, this.password).subscribe((data) => {
      // hier komen we als het inloggen gelukt is
      localStorage.setItem("token", data.token);
      this.email = "";
      this.password = "";
    });
  }
}
```

Herinner je dat een `Observable` een object is met een `.subscribe()`-functie. De `login()`-functie van de `AuthService` geeft een `Observable` terug. We kunnen dus `.subscribe()` aanroepen op de `login()`-functie van de `AuthService`.

We komen enkel in de callback-functie van `.subscribe()` terecht als het inloggen gelukt is. We kunnen dus de velden resetten als we in de callback-functie terechtkomen.

Wat we nu hebben werkt. Maar toch zouden we graag hebben dat de `AuthService` de token opslaat in de Local Storage. Er is echter een probleem, we zouden hiervoor een `.subscribe()` moeten doen in de `AutherService`, maar als we dit doen, dan kunnen we niet meer `.subscribe()` doen in de `LoginComponent`.

De oplossing is om een `pipe` (Nederlands: pijp) te gebruiken. Een `Observable` kan gezien worden als een `stream` (Nederlands: stroom) van data. Net zoals we een stroom van water kunnen bekijken met een `tap` (Nederlands: tapkraan), kunnen we een `stream` van data door een `pipe` laten gaan om vervolgens de data te bekijken via een `tap`. Vervolgens zouden we dus ook iets met deze data kunnen doen, zoals de data ergens opslaan.

Waar de analogie met de `tap`-kraan stopt, is dat bij een echte waterpijp, na het aftappen van het water, het water niet meer in de pijp zit. Bij onze `pipe` van data is dit wel het geval. We `tap`pen de data af, hierdoor kunnen we de data bekijken, en vervolgens gaat de data terug verder door de `pipe`.

`src/services/auth.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { tap } from "rxjs";

@Injectable({
  providedIn: "root",
})
export class AuthService {
  constructor(private http: HttpClient) {}

  login(email: string, password: string) {
    return (
      this.http
        .post<{ token: string }>("https://code-coaching.dev/api/token/login", {
          email: email,
          password: password,
        })
        // laat de data door een "pipe" (pijp) stromen
        .pipe(
          // zet een "tap" (tapkraan) erop om de data te bekijken
          tap((data) => {
            localStorage.setItem("token", data.token);
          })
          // hierna wordt de data terug in de "stream" van de "pipe" gestopt
        )
    );
  }
}
```

`src/pages/login/login.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { AuthService } from "../../services/auth.service";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  email = "";
  password = "";

  constructor(private authService: AuthService) {}

  login() {
    this.authService.login(this.email, this.password).subscribe((data) => {
      // verwijder de code om de token op te slaan in de Local Storage
      this.email = "";
      this.password = "";
    });
  }
}
```

Om in woorden uit te leggen wat er gebeurt:

- Er wordt op de `Login`-button geklikt.
- De `login()`-functie van de `LoginComponent` wordt aangeroepen.
- Er wordt een `.subscribe()` gedaan op de `login()`-functie van de `AuthService`. Hierdoor begint er data te stromen vanuit de `AuthService` naar de `LoginComponent`.
- We onderscheppen de data die stroomt vanuit de `AuthService` naar de `LoginComponent` door een `pipe` te gebruiken.
- We bekijken de data die stroomt vanuit de `AuthService` naar de `LoginComponent` door een `tap` te gebruiken.
- We slaan de token die zich in deze data bevindt op in de Local Storage.
- De data stroomt verder vanuit de `AuthService` naar de `LoginComponent`.
- We komen in de callback-functie van `.subscribe()` terecht in de `LoginComponent`.
- We resetten de velden.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/8317f47f2aad2bcfd8c1fe96e668ae1e36601c8e)

### Inloggen en uitloggen

We kunnen al inloggen, deze functionaliteit is geïmplementeerd in de `AuthService`. We zouden ook graag kunnen uitloggen. Hiervoor zullen we een nieuwe functie toevoegen aan de `AuthService`. Ook willen we weten of de gebruiker ingelogd is of niet. Hiervoor zullen we een afgeleide waarde toevoegen aan de `AuthService`.

`auth.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable, computed } from "@angular/core";
import { tap } from "rxjs";

@Injectable({
  providedIn: "root",
})
export class AuthService {
  constructor(private http: HttpClient) {}

  login(email: string, password: string) {
    return; // hier nog code
  }

  logout() {
    localStorage.removeItem("token");
  }

  get isAuthenticated() {
    return !!localStorage.getItem("token");
  }
}
```

De `logout()`-functie zal de token verwijderen uit de Local Storage. Hierdoor wordt de gebruiker uitgelogd.

De `isAuthenticated` property is een afgeleide waarde. Deze property zal `true` zijn als er een token aanwezig is in de Local Storage, anders zal deze property `false` zijn. Dit concept, een waarde afleiden van andere waarden, wordt ook wel eens `computed property` genoemd.

`main-layout.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { Router, RouterOutlet } from "@angular/router";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { AuthService } from "../../services/auth.service"; // importeer AuthService

@Component({
  selector: "app-main-layout",
  standalone: true,
  imports: [CommonModule, RouterOutlet, StyledButtonComponent],
  templateUrl: "./main-layout.component.html",
  styleUrl: "./main-layout.component.css",
})
export class MainLayoutComponent {
  constructor(
    public router: Router,
    public authService: AuthService // injecteer AuthService, public zodat deze beschikbaar is in de template
  ) {}
}
```

`main-layout.component.html`

```html
<div class="header">
  @if (authService.isAuthenticated) {
  <app-styled-button (onClick)="authService.logout()">
    Logout
  </app-styled-button>
  } @else {
  <app-styled-button (onClick)="router.navigate(['login'])">
    Login
  </app-styled-button>
  }
</div>
<div class="layout-container">
  <div class="title">Tour of Heroes</div>
  <div class="button-container">
    <app-styled-button (click)="router.navigate([''])">
      Dashboard
    </app-styled-button>
    <app-styled-button (click)="router.navigate(['heroes'])">
      Heroes
    </app-styled-button>
  </div>

  <router-outlet></router-outlet>
</div>
```

Merk op: Er wordt zowel gebruikgemaakt van `(onClick)` als `(click)`. Dit is omdat de `StyledButtonComponent` zowel een `@Output` property heeft met de naam `onClick` als een default `@Output` property met de naam `click`. Dit zullen we later refactoren.

Er wordt een `header` toegevoegd aan de "layout"-pagina, `main-layout.component.html`. Deze balk/header zal altijd zichtbaar zijn op elke pagina, vandaar de toevoeging in de `layout`-pagina.

Hierin worden twee knoppen getoond, `Login` en `Logout`.
Er wordt gebruikgemaakt van de `@if`-control flow en de `@else`-control flow. De `isAuthenticated` is de computed property afkomstig uit de service, deze zal `true` zijn als de gebruiker is ingelogd en `false` als de gebruiker niet is ingelogd.

`main-layout.component.css`

```css
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
```

Nu is te zien dat de balk niet de volledige breedte inneemt en ook dat deze niet bovenaan de pagina staat. Dit komt omdat er nog margin aanwezig is op de `body` van de pagina. Verwijder deze margin.

`src/styles.css`

```css
/* You can add global styles to this file, and also import other style files */

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

/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}

.title {
  font-size: 1.5rem;
  color: grey;
  font-weight: bold;
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/247ae3c24168a1864003b5242b86d0717998f2d4)

### De authenticatie uittesten

Start de applicatie op. Indien je ingelogd bent, zal er rechtsboven een knop `Logout` te zien zijn. Indien je niet ingelogd bent, zal er rechtsboven een knop `Login` te zien zijn.

Indien je ingelogd bent, klik op `Logout`.

Klik op `Login`, dit zal de gebruiker naar de loginpagina navigeren.

Vul hier een geldige login van Code Coaching in en klik op `Login` naast de invoervelden. De gebruiker zal ingelogd worden, dit is te zien aan de knop rechtsboven die veranderd is naar `Logout`.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/quasar-tour-of-heroes/commit/d4a6dbc1da66786732ca59bcea0e97ae9a87490f)

## Loginpagina stylen

De loginpagina ziet er momenteel een beetje raar uit. Voeg wat styling toe om het er beter uit te laten zien.

Maak gebruik van `app-styled-button` i.p.v. het normale `button`-element. Plaats wrapper elementen/container elementen om de plaatsing van de inputvelden en de knop te verbeteren.

`login.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component"; // importeer StyledButtonComponent
import { AuthService } from "../../services/auth.service";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [CommonModule, FormsModule, StyledButtonComponent], // voeg toe aan imports
  templateUrl: "./login.component.html",
  styleUrl: "./login.component.css",
})
export class LoginComponent {
  email = "";
  password = "";

  constructor(private authService: AuthService) {}

  login() {
    this.authService.login(this.email, this.password).subscribe(() => {
      this.email = "";
      this.password = "";
    });
  }
}
```

`login.component.html`

```html
<div class="login-container">
  <div class="login-fields">
    <input type="email" [(ngModel)]="email" />
    <input type="password" [(ngModel)]="password" />
  </div>
  <app-styled-button (click)="login()">Login</app-styled-button>
</div>
```

`login.component.css`

```css
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
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/8a02f57f431080c96e31fbfae1421fe2fb5f7846)

## Refactor StyledButtonComponent

Momenteel is er een custom `@Output` property `onClick` aanwezig in de `StyledButtonComponent`. Deze property is niet nodig, aangezien er al een default `@Output` property `click` aanwezig is op alle elementen (hiervoor krijgen we geen automatische code-completion in VSCode, maar deze is wél aanwezig).

Verwijder de custom `@Output` property `onClick`.

`styled-button.component.ts`

```ts
import { CommonModule } from "@angular/common";
import { Component, Input } from "@angular/core";

@Component({
  selector: "app-styled-button",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./styled-button.component.html",
  styleUrl: "./styled-button.component.css",
})
export class StyledButtonComponent {
  @Input() type: "default" | "primary" | "negative" = "default";
}
```

`styled-button.component.html`

Verwijder de `(onClick)` uit de template.

```html
<button
  [ngClass]="{
    primary: type === 'primary',
    negative: type === 'negative'
  }"
>
  <ng-content></ng-content>
</button>
```

Overal waar er gebruikgemaakt wordt van `StyledOveral waar er gebruikgemaakt wordt van `StyledOveral waar er gebruikgemaakt wordt van `StyledButtonComponent`, wijzig `(onClick)` naar `(click)`. Vanaf reageert de knop op een `click`-event dat door de component wordt gegenereerd. (Dit is enkel in `main-layout.component.html`.)

`main-layout.component.html`

```html
<div class="header">
  @if (authService.isAuthenticated) {
  <app-styled-button (click)="authService.logout()"> Logout </app-styled-button>
  } @else {
  <app-styled-button (click)="router.navigate(['login'])">
    Login
  </app-styled-button>
  }
</div>
<!-- hier nog code -->
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/ad48780398a8c92c1446829544f0bbdaf918d848)

## Hero service koppelen met API

Tot nu toe werd er hard-coded data gebruikt in de `Hero service`. Maar, met de account van Code Coaching is het mogelijk om de `Hero` data te beheren in een database via een API.

Er wordt vanuitgegaan dat de werking van het `/api/heroes` end point gekend is. Indien dit niet het geval is, bekijk dan deze korte oefening: [CRUD /api/heroes](https://code-coaching.dev/blogs/crud-oefening).

In de oefening wordt er gebruikgemaakt van Postman. En de token die de ingelogde gebruiker voorstelt, wordt aangemaakt via de profielpagina van Code Coaching. In deze tutorial wordt er gebruikgemaakt van de token die opgeslagen is in Local Storage, die bekomen wordt door in te loggen via de loginpagina.

In het kort: Er worden calls gedaan naar `https://code-coaching.dev/api/heroes`. Dit kunnen `GET`, `POST`, `PATCH` en `DELETE` calls zijn. De `GET` call zal alle heroes ophalen, de `POST` call zal een hero aanmaken, de `PATCH` call zal een hero wijzigen en de `DELETE` call zal een hero verwijderen. Voor elke call is authenticatie nodig, dit wordt gedaan door een `Authorization` header toe te voegen aan de call.

## Authentication Guard

Momenteel is het mogelijk om alle pagina's te bezoeken, ook als de gebruiker niet ingelogd is. Dit is niet de gewenste situatie. Nu het de bedoeling is om de data van de heroes op te halen via de API, is het belangrijk dat de gebruiker ingelogd is.

Om zeker te zijn dat een gebruiker ingelogd is, zal er een `Authentication Guard` geïmplementeerd worden. Deze guard zal ervoor zorgen dat de gebruiker enkel pagina's kan bezoeken als deze ingelogd is. Indien de gebruiker niet ingelogd is, zal deze doorgestuurd worden naar de loginpagina.

```sh
ng g guard guards/authentication --skip-tests
```

Dit zal een keuzemenu tonen. Klik gewoon op `Enter` om de default waarden te gebruiken.

```sh
? Which type of guard would you like to create? (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to proceed)
❯◉ CanActivate
 ◯ CanActivateChild
 ◯ CanDeactivate
 ◯ CanMatch
```

We zullen de functie koppelen aan een route (iets later in deze tutorial). Vervolgens zal deze functie aangeroepen worden als de gebruiker naar de route navigeert. De functie zal een `boolean` teruggeven, `true` als de gebruiker toegang heeft tot de route en `false` als de gebruiker geen toegang heeft tot de route.

`authentication.guard.ts`

```ts
import { CanActivateFn } from "@angular/router";

export const authenticationGuard: CanActivateFn = (route, state) => {
  return true;
};
```

Momenteel geeft de functie altijd `true` terug. Indien we deze guard zouden koppelen aan een route, dan zou de gebruiker altijd toegang hebben tot de route (wat zonder guard ook al het geval is).

We zullen dus logica moeten toevoegen die bepaalt of de gebruiker toegang heeft tot de route of niet. We zullen de `AuthService` gebruiken om te bepalen of de gebruiker ingelogd is of niet.

`authentication.guard.ts`

```ts
import { inject } from "@angular/core";
import { CanActivateFn } from "@angular/router";
import { AuthService } from "../services/auth.service";

export const authenticationGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService); // injecteer AuthService

  // controleer of de gebruiker ingelogd is
  if (authService.isAuthenticated) {
    // indien ingelogd, geef true terug, de gebruiker heeft toegang tot de route
    return true;
  } else {
    // indien niet ingelogd, geef false terug, de gebruiker heeft geen toegang tot de route
    return false;
  }
};
```

Tot nu toe zagen we dat `dependency injection` enkel gebruikt werd in de constructor van een class. Maar `dependency injection` kan ook gebruikt worden in een functie. Dit is wat er gebeurt in de bovenstaande code. De `AuthService` wordt geïnjecteerd in de functie `inject(AuthService)`;

Vervolgens gebruiken we de `isAuthenticated` property van de `AuthService` om te bepalen of de gebruiker ingelogd is of niet. Indien de gebruiker ingelogd is, geven we `true` terug, indien de gebruiker niet ingelogd is, geven we `false` terug. Met `true` geven we aan dat de gebruiker toegang heeft tot de route, met `false` geven we aan dat de gebruiker geen toegang heeft tot de route.

Nu is er nog één ding dat we graag willen toevoegen. Indien de gebruiker niet ingelogd is, willen we dat de gebruiker doorgestuurd wordt naar de loginpagina. Dit kunnen we doen door gebruik te maken van de `Router`.

`authentication.guard.ts`

```ts
import { inject } from "@angular/core";
import { CanActivateFn, Router } from "@angular/router";
import { AuthService } from "../services/auth.service";

export const authenticationGuard: CanActivateFn = (_route, _state) => {
  const authService = inject(AuthService);
  const router = inject(Router); // injecteer Router

  if (authService.isAuthenticated) {
    return true;
  } else {
    router.navigate(["login"]); // navigeer naar loginpagina
    return false;
  }
};
```

De guard is nu klaar. We kunnen deze koppelen aan de routes die alleen bezocht mogen worden als de gebruiker ingelogd is.

`app.routes.ts`

```ts
import { Routes } from "@angular/router";
import { MainLayoutComponent } from "./layouts/main-layout/main-layout.component";
import { authenticationGuard } from "./guards/authentication.guard"; // importeer de guard

export const routes: Routes = [
  {
    path: "",
    component: MainLayoutComponent,
    children: [
      {
        path: "",
        canActivate: [authenticationGuard], // voeg de guard toe
        loadComponent: () =>
          import("./pages/dashboard/dashboard.component").then(
            (c) => c.DashboardComponent
          ),
      },
      {
        path: "heroes",
        canActivate: [authenticationGuard], // voeg de guard toe
        loadComponent: () =>
          import("./pages/hero-list/hero-list.component").then(
            (c) => c.HeroListComponent
          ),
      },
      {
        path: "heroes/add",
        canActivate: [authenticationGuard], // voeg de guard toe
        loadComponent: () =>
          import("./pages/hero-add/hero-add.component").then(
            (c) => c.HeroAddComponent
          ),
      },
      {
        path: "heroes/:id",
        canActivate: [authenticationGuard], // voeg de guard toe
        loadComponent: () =>
          import("./pages/hero-details/hero-details.component").then(
            (c) => c.HeroDetailsComponent
          ),
      },
      {
        path: "login",
        loadComponent: () =>
          import("./pages/login/login.component").then((c) => c.LoginComponent),
      },
    ],
  },
];
```

Test de applicatie. Indien je ingelogd bent, zal je de pagina's kunnen bezoeken. Indien je niet ingelogd bent, zal je bij het manueel bezoeken van de pagina's, doorgestuurd worden naar de loginpagina.

Alle wijziging: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/7c5ca9b3835576d90f411a08681a6cb928e7eb8a)

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
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  // hier nog code

  heroes = [
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

  // hier nog code

  loadHeroes() {
    const heroes = localStorage.getItem("heroes");
    if (heroes) {
      this.heroes = JSON.parse(heroes);
    }
  }
}
```

De `loadHeroes()`-functie zal aangepast worden om de lijst van heroes op te halen via de API.

`hero.service.ts`

```ts
import { HttpClient } from "@angular/common/http"; // importeer HttpClient
import { Injectable } from "@angular/core";
import { Hero } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {} // injecteer HttpClient

  selectedHero: null | Hero = null;

  // start vanuit een lege array, verwijder de hard-coded heroes
  // voeg Array<Hero> toe als typing
  heroes: Array<Hero> = [];

  // hier nog code

  loadHeroes() {
    this.http
      .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes")
      .subscribe((data) => {
        console.log(data);
      });
  }
}
```

De `loadHeroes()`-functie zal een HTTP call doen naar de API. De API zal een lijst van heroes teruggeven. Deze lijst van heroes zal in de console gelogd worden. Merk ook op dat we het type van het resultaat van de call meegeven aan de `get`-functie van de `HttpClient` (`this.http.get<Array<Hero>>()`), hierdoor zal de `HttpClient` weten dat het resultaat van de call een array van `HeroBackend`-objecten is.

Herinner dat `loadHeroes` wordt aangeroepen in `app.component.ts`, zodra de applicatie opstart zal de lijst van heroes opgehaald worden. Bekijk de console in de browser en merk op dat er een `401 Unauthorized` error is. We zullen de token uit Local Storage moeten toevoegen aan de `Authorization` header van de call, zodat de API weet welke gebruiker de call doet.

`hero.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  // hier nog code

  loadHeroes() {
    const token = localStorage.getItem("token");
    if (token) {
      this.http
        .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes", {
          headers: {
            Authorization: "Bearer " + token,
          },
        })
        .subscribe((data) => {
          console.log(data);
        });
    }
  }
}
```

Nu wordt er eerst gekeken of er überhaupt een token aanwezig is. Indien deze niet aanwezig is, wordt de call niet uitgevoerd Indien deze aanwezig is, sturen we deze mee met de request. Zoals we zien kan dit gedaan worden door een tweede parameter mee te geven aan de `get`-functie van de `HttpClient`. Deze tweede parameter is een object met een `headers` property. Deze `headers` property is een object met een `Authorization` property. De `Authorization` property is een string die begint met `Bearer ` gevolgd door de token. De reden dat er `Bearer ` voor de token moet staan, is omdat de API dit verwacht (dit is een veelgebruikte manier om een token mee te sturen met een request).

Test de applicatie. Indien je niet ingelogd bent, wordt er geen call gedaan naar de API. Log in, ververs na het inloggen de browser, nu wordt er een call gedaan naar de API. Bekijk de console in de browser en merk op dat er een array van heroes wordt teruggegeven. Indien er nog nooit heroes zijn toegevoegd aan de applicatie, zal de array leeg zijn.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/63011bcd8676fd61582e34846d6e7b47460b507f)

## loadHeroes() refactoren

Momenteel wordt `loadHeroes()` aangeroepen in `app.component.ts`. Dit was geen probleem toen er nog met heroes uit Local Storage gewerkt werd. Maar nu er met een API waar authenticatie nodig is gewerkt wordt, willen we dat de pagina's zelf de data ophalen die nodig is op de pagina die ingeladen wordt.

`app.component.ts` voor wijziging

```ts
import { Component, OnInit } from "@angular/core";
import { CommonModule } from "@angular/common";
import { RouterOutlet } from "@angular/router";
import { HeroService } from "./services/hero.service";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent implements OnInit {
  constructor(private heroService: HeroService) {}

  ngOnInit(): void {
    this.heroService.loadHeroes();
  }
}
```

`app.component.ts` na wijziging

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { RouterOutlet } from "@angular/router";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent {}
```

Er zijn twee pagina's die de lijst van heroes nodig hebben, de `DashboardComponent` en de `HeroListComponent`. We zullen de `loadHeroes()`-functie van de `HeroService` aanroepen in de `ngOnInit`-lifecycle hook van `DashboardComponent` en de `HeroListComponent`.

`dashboard.component.ts` voor wijziging

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { HeroService } from "../../services/hero.service";
import { Router } from "@angular/router";

@Component({
  selector: "app-dashboard",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./dashboard.component.html",
  styleUrl: "./dashboard.component.css",
})
export class DashboardComponent {
  constructor(
    public heroService: HeroService,
    public router: Router
  ) {}
}
```

`dashboard.component.ts` na wijziging

```ts
import { CommonModule } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { Router } from "@angular/router";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-dashboard",
  standalone: true,
  imports: [CommonModule],
  templateUrl: "./dashboard.component.html",
  styleUrl: "./dashboard.component.css",
})
export class DashboardComponent implements OnInit {
  constructor(
    public heroService: HeroService,
    public router: Router
  ) {}

  ngOnInit(): void {
    this.heroService.loadHeroes();
  }
}
```

`hero-list.component.ts` voor wijziging

```ts
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";
import { Router } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent {
  constructor(
    public router: Router,
    public heroService: HeroService
  ) {}

  onClickHero(hero: Hero) {
    this.heroService.selectedHero = hero;
  }
}
```

`hero-list.component.ts` na wijziging

```ts
import { CommonModule } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { Router } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent implements OnInit {
  constructor(
    public router: Router,
    public heroService: HeroService
  ) {}

  ngOnInit(): void {
    this.heroService.loadHeroes();
  }

  onClickHero(hero: Hero) {
    this.heroService.selectedHero = hero;
  }
}
```

Test de applicatie. Na inloggen kan er genavigeerd worden naar de `DashboardComponent` en de `HeroListComponent`. De pagina zelf is nu verantwoordelijk voor het ophalen van de data die nodig is op de pagina. Dit kan je zien door te kijken naar de console terwijl er genavigeerd wordt tussen `dashboard` en `hero list` (klik op de knoppen). Er zal telkens een call gedaan worden naar de API om de lijst van heroes op te halen.

De eerder vermelde [CRUD /api/heroes](https://code-coaching.dev/blogs/crud-oefening) oefening kan gebruikt worden om heroes aan te maken indien er nog geen heroes aangemaakt zijn door jouw account.

Zelfs als er heroes zijn aangemaakt, zal de lijst van heroes niet getoond worden. Dit komt omdat we momenteel de data loggen naar de console in plaats van deze toe te kennen aan de `heroes` property van de `HeroService`.

`hero.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;
  heroes: Array<Hero> = [];

  // hier nog code

  loadHeroes() {
    const token = localStorage.getItem("token");
    if (token) {
      this.http
        .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes", {
          headers: {
            Authorization: "Bearer " + token,
          },
        })
        .subscribe((data) => {
          this.heroes = data; // dit toont een TypeScript error
        });
    }
  }
}
```

Er is een probleem, de `data` die we terugkrijgen van de API is een array van `HeroBackend`-objecten. Maar de `heroes`-property van de `HeroService` is een array van `Hero`-objecten. Dit is een probleem, want we kunnen objecten van het ene type (`{ id: number; name: string }`) niet zomaar toekennen aan een variabele van het andere type (`{ number: number; name: string }`). Dit is een veel voorkomende situatie. De data die we ophalen via een API is niet altijd in hetzelfde formaat als de data die we gebruiken in onze applicatie. We zullen de data die we ophalen via de API moeten transformeren naar het formaat dat we gebruiken in onze applicatie.

`hero.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;
  heroes: Array<Hero> = [];

  // hier nog code

  loadHeroes() {
    const token = localStorage.getItem("token");
    if (token) {
      this.http
        .get<Array<HeroBackend>>("https://code-coaching.dev/api/heroes", {
          headers: {
            Authorization: "Bearer " + token,
          },
        })
        .subscribe((data) => {
          this.heroes = data.map((heroBackend) => {
            const hero = {
              number: heroBackend.id,
              name: heroBackend.name,
            } satisfies Hero;
            return hero;
          });
        });
    }
  }
}
```

Herinner de uitleg over Observables (data stroomt naar de `subscribe()`-functie). In een eerder voorbeeld hebben we gekozen om de Observable te returnen, waardoor we in de service zelf met `.pipe(tap())` moesten werken. In `loadHeroes()` kiezen we ervoor om de `.subcribe()` in de service te doen. Aangezien we nergens anders nog moeten weten wanneer de data opgehaald is, maakt het niet uit dat we de `.subscribe()` in de service doen. Stel dat we ergens anders nog moeten weten wanneer de data opgehaald is, dan zouden we de Observable returnen en in de component waar we de data nodig hebben, zouden we de `.subscribe()` doen, waardoor er een `.pipe(tap())` nodig is in de service als we iets willen doen met de data in de service.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/416c010376bc6accc377b2fb0a82895da99dc9dc)

In principe is het na de refactoring niet meer nodig om te controleren of er een token aanwezig is voor de call. Zelfs als er geen token is, dan zal de call simpelweg een `401 Unauthorized` error teruggeven. Doordat `loadHeroes` nu enkel op pagina's gebruikt wordt die enkel bezocht kunnen worden als de gebruiker ingelogd is, is het niet meer nodig om te controleren of er een token aanwezig is. We gaan het voor deze tutorial wel laten staan.

## Hero toevoegen

`hero.service.ts` - voor wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  // hier nog code

  addHero(name: string) {
    const maxNumber = Math.max(...this.heroes.map((h) => h.number));
    const newHero = { number: maxNumber + 1, name } satisfies Hero;
    this.heroes.push(newHero);
    this.saveHeroes();
  }

  // hier nog code
}
```

`hero.service.ts` - na wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  // hier nog code

  addHero(name: string) {
    return this.http.post(
      "https://code-coaching.dev/api/heroes", // end point
      { name }, // body
      {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      } // opties - hier voegen we de token toe aan de Authorization header
    );
  }

  // hier nog code
}
```

Merk op dat we hier de Observable returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen de `.subscribe()`-functie in de component doen.

`hero-add.component.ts`

```ts
import { CommonModule, Location } from "@angular/common";
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-add",
  standalone: true,
  imports: [CommonModule, FormsModule, StyledButtonComponent],
  templateUrl: "./hero-add.component.html",
  styleUrl: "./hero-add.component.css",
})
export class HeroAddComponent {
  name: string = "";

  constructor(
    public location: Location,
    private heroService: HeroService
  ) {}

  saveHero(name: string) {
    // voeg de .subscribe() toe
    this.heroService.addHero(name).subscribe(() => {
      this.location.back(); // navigeer terug naar vorige pagina wanneer de call klaar is
    });
  }
}
```

Start de applicatie, zorg dat je ingelogd bent en voeg twee heroes toe.

## Hero verwijderen

`hero.service.ts` - voor wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;

  // hier nog code

  deleteHero(hero: Hero) {
    this.heroes = this.heroes.filter((h) => h.number !== hero.number);
    if (this.selectedHero?.number === hero.number) {
      this.selectedHero = null;
    }
    this.saveHeroes();
  }

  // hier nog code
}
```

`hero.service.ts` - na wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;

  // hier nog code

  deleteHero(hero: Hero) {
    return this.http
      .delete(`https://code-coaching.dev/api/heroes/${hero.number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .pipe(
        tap(() => {
          this.selectedHero = null;
          this.loadHeroes();
        })
      );
  }

  // hier nog code
}
```

Merk op dat we hier de Observable returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen de `.subscribe()`-functie in de component doen. We gebruiken hier `.pipe(tap())` om de `selectedHero` op `null` te zetten wanneer de call klaar is (herinner: de data stroomt naar de `subscribe()`-functie, via `.pipe(tap())` kunnen we iets doen wanneer de data richting de `subscribe()`-functie stroomt).

`hero-details.component.ts`

```ts
// hier nog code

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  // hier nog code

  constructor(
    private route: ActivatedRoute,
    public location: Location,
    private heroService: HeroService
  ) {}

  // hier nog code

  deleteHero(hero: Hero) {
    this.heroService.deleteHero(hero).subscribe(() => {
      this.location.back();
    });
  }
}
```

De `deleteHero()`-functie wordt aangeroepen in de template. Wijzig de implementatie van de `deleteHero()`-functie, zodat deze de `deleteHero()`-functie van de `HeroService` aanroept. De `deleteHero()`-functie van de `HeroService` returnt een Observable, we kunnen hierop `.subscribe()` doen, zodat we weten wanneer de call klaar is. Wanneer de call klaar is, navigeren we terug naar de vorige pagina.

`hero-list.component.ts`

```ts
// hier nog code

@Component({
  selector: "app-hero-list",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent],
  templateUrl: "./hero-list.component.html",
  styleUrl: "./hero-list.component.css",
})
export class HeroListComponent implements OnInit {
  constructor(
    public router: Router,
    public heroService: HeroService
  ) {}

  // hier nog code

  onDeleteHero() {
    if (this.heroService.selectedHero) {
      this.heroService
        .deleteHero(this.heroService.selectedHero)
        .subscribe(() => {
          this.heroService.loadHeroes();
        });
    }
  }
}
```

Voeg de `onDeleteHero()`-functie toe, deze zullen we koppelen in de template. Wanneer er op `Delete` geklikt wordt. Wanneer deze functie aangeroepen wordt, roepen we de `deleteHero()`-functie van de `HeroService` aan. De `deleteHero()`-functie van de `HeroService` returnt een Observable, we kunnen hierop `.subscribe()` doen, zodat we weten wanneer de call klaar is. Wanneer de call klaar is, roepen we de `loadHeroes()`-functie van de `HeroService` aan.

`hero-list.component.html`

```html
<!-- hier nog code -->

@if (heroService.selectedHero) {
<div class="title">
  {{ heroService.selectedHero.name | uppercase }} is my hero
</div>
<div class="buttons">
  <app-styled-button
    (click)="router.navigate(['heroes', heroService.selectedHero.number])"
  >
    Details
  </app-styled-button>
  <!-- Koppel onDeleteHero() functie aan het (click)-event -->
  <app-styled-button [type]="'negative'" (click)="onDeleteHero()">
    Delete
  </app-styled-button>
</div>
}
```

Start de applicatie, zorg dat je ingelogd bent en verwijder een hero.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/4e48c686f5929edba8a8f1055684f4024603b213)

## Hero updaten

`hero.service.ts` - voor wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;

  // hier nog code

  updateHero(hero: Hero) {
    const index = this.heroes.findIndex((h) => h.number === hero.number);
    if (index !== -1) {
      this.heroes[index] = structuredClone(hero);
    }
    this.saveHeroes();
  }

  // hier nog code
}
```

`hero.service.ts` - na wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;

  // hier nog code

  updateHero(hero: Hero) {
    return this.http
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
      .pipe(
        tap(() => {
          this.selectedHero = null;
        })
      );
  }

  // hier nog code
}
```

Herinner dat we de Observable returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen de `.subscribe()`-functie in de component doen. We gebruiken hier `.pipe(tap())` om de `selectedHero` op `null` te zetten wanneer de call klaar is (herinner: de data stroomt naar de `subscribe()`-functie, via `.pipe(tap())` kunnen we iets doen wanneer de data richting de `subscribe()`-functie stroomt).

De private functie `saveHeroes` wordt nu niet meer gebruikt, deze kunnen we verwijderen.

```ts
export class HeroDetailsComponent implements OnInit {
  // hier nog code

  // verwijder deze functie
  private saveHeroes() {
    localStorage.setItem("heroes", JSON.stringify(this.heroes));
  }

  // hier nog code
}
```

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/453daff6449dbdb4d3138108fdc05abcc72cf1ce)

## findHero() refactoren

Momenteel werkt de detailspagina, maar enkel wanneer we er naartoe navigeren vanuit de lijst van heroes. Wanneer we de pagina rechtstreeks bezoeken, dan wordt er altijd "Hero not found" getoond. Dit komt omdat de `findHero()`-functie momenteel een hero zoekt in de array van heroes die we ophalen via de API. Wanneer we de pagina rechtstreeks bezoeken, is de array van heroes nog niet opgehaald, waardoor de `findHero()`-functie geen hero zal vinden.

Het gewenste gedrag is dat de `findHero()`-functie een API call uitstuurt om de hero op te halen, gebaseerd op de parameter die in de URL staat.

`hero.service.ts` - voor wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  // hier nog code

  findHero(number: number): Hero | undefined {
    const matchingHero = this.heroes.find((h) => h.number === number);
    return structuredClone(matchingHero);
  }

  // hier nog code
}
```

`hero.service.ts` - na wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Observable, catchError, map, tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  // hier nog code

  findHero(number: number) {
    return this.http
      .get<HeroBackend>(`https://code-coaching.dev/api/heroes/${number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .pipe(
        // we onderscheppen de data die terugkomt van de API via .pipe()
        // we gebruiken de rxjs map-operator om de data te transformeren
        map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          // de data wordt omgevormd van een HeroBackend object naar een Hero object
          // vanaf dit punt bevat de stroom van data een Hero object
          // als we later .subscribe() doen, dan krijgen we een Hero object
          return hero;
        })
      );
  }

  // hier nog code
}
```

Herinner dat we de Observable returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen de `.subscribe()`-functie in de component doen. We gebruiken hier `.pipe(map())` om de data die terugkomt van de API te transformeren naar een `Hero`-object.

`hero-details.component.ts` - voor wijziging

```ts
import { CommonModule, Location } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute } from "@angular/router";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: undefined | Hero;

  constructor(
    private route: ActivatedRoute,
    public location: Location,
    private heroService: HeroService
  ) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        this.hero = this.heroService.findHero(id);
      }
    });
  }

  // hier nog code
}
```

`hero-details.component.ts` - na wijziging

```ts
import { CommonModule, Location } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute } from "@angular/router";
import { catchError, throwError } from "rxjs";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: undefined | Hero;

  constructor(
    private route: ActivatedRoute,
    public location: Location,
    private heroService: HeroService
  ) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        this.heroService.findHero(id).subscribe((hero) => {
          // wanneer de call klaar is, krijgen we een Hero object
          // ken dit toe aan de hero property
          this.hero = hero;
        });
      }
    });
  }

  // hier nog code
}
```

Start de applicatie, zorg dat je ingelogd bent en navigeer naar de detailspagina van een hero. Dit is de flow die hiervoor ook al werkte. Kijk naar het nummer van een bestaande hero en navigeer naar deze hero door het nummer in de URL te wijzigen. Je zal zien dat de detailspagina nu ook werkt wanneer je de pagina rechtstreeks bezoekt.

Wijzig de url nu naar een nummer dat niet bestaat, bijvoorbeeld `abc69`. Je zal zien dat de vorige hero nog steeds getoond wordt. Dit komt omdat we momenteel enkel de use case afhandelen als de call naar de API succesvol is (we komen dan in de `.subscribe()`-functie). We zullen ook de use case moeten afhandelen als de call naar de API niet succesvol is.

`hero-details.component.ts` - na wijziging met error handling

```ts
import { CommonModule, Location } from "@angular/common";
import { Component, OnInit } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { ActivatedRoute } from "@angular/router";
import { catchError, throwError } from "rxjs";
import { Hero } from "../../components/models";
import { StyledButtonComponent } from "../../components/styled-button/styled-button.component";
import { HeroService } from "../../services/hero.service";

@Component({
  selector: "app-hero-details",
  standalone: true,
  imports: [CommonModule, StyledButtonComponent, FormsModule],
  templateUrl: "./hero-details.component.html",
  styleUrl: "./hero-details.component.css",
})
export class HeroDetailsComponent implements OnInit {
  hero: null | Hero = null; // wijzig undefined naar null

  constructor(
    private route: ActivatedRoute,
    public location: Location,
    private heroService: HeroService
  ) {}

  ngOnInit() {
    this.route.params.subscribe((params) => {
      const id = +params["id"];
      if (id) {
        this.heroService
          .findHero(id)
          .pipe(
            // we onderscheppen de data
            // we gebruiken de rxjs catchError-operator om de error te onderscheppen
            catchError((err) => {
              this.hero = null; // we zetten de hero op null indien er een error is
            })
          )
          .subscribe((hero) => {
            this.hero = hero;
          });
      }
    });
  }

  // hier nog code
}
```

Herinner dat we de Observable returnen. Dit is omdat we in de component waar we de functie aanroepen, willen weten wanneer de call klaar is. We zullen de `.subscribe()`-functie in de component doen. We gebruiken hier `.pipe(catchError())` om de error te onderscheppen. Wanneer er een error is, zetten we de `hero` op `null`.

We opteren om de hero property te wijzigen van `undefined | Hero` naar `null | Hero`. Voorheen kozen we voor `undefined` omdat er een `.find()` gedaan werd op de array van heroes. Wanneer er geen hero gevonden werd, dan werd er `undefined` gereturned. Nu we zelf beslissen wat we toekennen aan de `hero` property, kunnen we kiezen voor `null` wanneer er geen hero gevonden werd. Een simpele regel om te volgen is dat een property `undefined` is wanneer er nooit een waarde aan toegekend is, wanneer we als developer willen aantonen dat een property geen waarde heeft, dan kiezen we voor `null`.

Start de applicatie, bezoek een bestaande hero. Wijzig de URL naar een nummer dat niet bestaat. Je zal zien dat er "Hero not found" getoond wordt.

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/888da1cd6a2bd6365fb83d6d8e46283ba2213afd)

## API calls refactoren

Er zijn verschillende dingen die keer op keer terugkomen in elke API call. We kunnen dit refactoren naar interceptors. Een interceptor (Nederlands: onderschepper) is een functie die uitgevoerd wordt voor elke API call, waarbij de request en response onderschept kan worden.

We zullen twee interceptors maken, één voor het toevoegen van de URL van de API, zodat we niet keer op keer `https://code-coaching.dev/api` moeten typen. En één voor het toevoegen van de authorization token aan de header van de request.

### Interceptor base-url

```sh
ng g interceptor interceptors/base-url --skip-tests
```

`base-url.interceptor.ts`

```ts
import { HttpInterceptorFn } from "@angular/common/http";

export const baseUrlInterceptor: HttpInterceptorFn = (req, next) => {
  console.log(req);
  return next(req);
};
```

Momenteel loggen we de request naar de console en geven we de request door (`return next(req);`). Om deze interceptor te gebruiken, moeten we deze toevoegen aan de configuratie.

`app.config.ts`

```ts
import {
  provideHttpClient,
  withFetch,
  withInterceptors, // importeer withInterceptors
} from "@angular/common/http";
import { ApplicationConfig } from "@angular/core";
import { provideRouter, withHashLocation } from "@angular/router";
import { routes } from "./app.routes";
import { baseUrlInterceptor } from "./interceptors/base-url.interceptor"; // importeer de baseUrlInterceptor

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withHashLocation()),
    // voeg de baseUrlInterceptor toe aan de withInterceptors array
    provideHttpClient(withFetch(), withInterceptors([baseUrlInterceptor])),
  ],
};
```

Start de applicatie en navigeer rond. Je zal zien dat elke request naar de API nu gelogd wordt naar de console. Hier zien we dat de request een object is met verschillende properties. Één van de properties is `url`. We kunnen de `url` property wijzigen in de interceptor. Dit zullen we doen in de `baseUrlInterceptor`.

`base-url.interceptor.ts`

```ts
import { HttpInterceptorFn } from "@angular/common/http";

export const baseUrlInterceptor: HttpInterceptorFn = (req, next) => {
  req = req.clone({
    url: `https://code-coaching.dev/api${req.url}`,
  });
  return next(req);
};
```

Doordat elke request nu onderschept wordt en bij elke request wordt `https://code-coaching.dev/api` toegevoegd voor de URL, kunnen we dit deel van de URL verwijderen in de `HeroService` en de `AuthService`.

`auth.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { tap } from "rxjs";

@Injectable({
  providedIn: "root",
})
export class AuthService {
  constructor(private http: HttpClient) {}

  login(email: string, password: string) {
    return this.http
      .post<{ token: string }>("/token/login", {
        // verwijder https://code-coaching.dev/api
        email: email,
        password: password,
      })
      .pipe(
        tap((data) => {
          localStorage.setItem("token", data.token);
        })
      );
  }

  logout() {
    localStorage.removeItem("token");
  }

  get isAuthenticated() {
    return !!localStorage.getItem("token");
  }
}
```

`hero.service.ts`

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { map, tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;
  heroes: Array<Hero> = [];

  topHeroes() {
    return this.heroes.slice(-4);
  }

  findHero(number: number) {
    return this.http
      .get<HeroBackend>(`/heroes/${number}`, {
        // verwijder https://code-coaching.dev/api
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .pipe(
        map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          return hero;
        })
      );
  }

  updateHero(hero: Hero) {
    return this.http
      .patch(
        `/heroes/${hero.number}`, // verwijder https://code-coaching.dev/api
        {
          name: hero.name,
        },
        {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
      .pipe(
        tap(() => {
          this.selectedHero = null;
        })
      );
  }

  deleteHero(hero: Hero) {
    return this.http
      .delete(`/heroes/${hero.number}`, {
        // verwijder https://code-coaching.dev/api
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .pipe(
        tap(() => {
          this.selectedHero = null;
        })
      );
  }

  addHero(name: string) {
    return this.http.post(
      "/heroes",
      { name },
      {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      }
    );
  }

  loadHeroes() {
    const token = localStorage.getItem("token");
    if (token) {
      this.http
        .get<Array<HeroBackend>>("/heroes", {
          // verwijder https://code-coaching.dev/api
          headers: {
            Authorization: "Bearer " + token,
          },
        })
        .subscribe((data) => {
          this.heroes = data.map((heroBackend) => {
            const hero = {
              number: heroBackend.id,
              name: heroBackend.name,
            } satisfies Hero;
            return hero;
          });
        });
    }
  }
}
```

### Interceptor authorization

```sh
ng g interceptor interceptors/auth --skip-tests
```

`auth.interceptor.ts`

```ts
import { HttpInterceptorFn } from "@angular/common/http";

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authToken = localStorage.getItem("token");
  if (authToken) {
    const authReq = req.clone({
      headers: req.headers.set("Authorization", `Bearer ${authToken}`),
    });

    return next(authReq);
  }
  return next(req);
};
```

We onderscheppen elke request. Indien er een token in de localStorage zit, dan voegen we de token toe aan de header van de request.

`hero.service.ts` - voor wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { map, tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;
  heroes: Array<Hero> = [];

  topHeroes() {
    return this.heroes.slice(-4);
  }

  findHero(number: number) {
    return this.http
      .get<HeroBackend>(`/heroes/${number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .pipe(
        map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          return hero;
        })
      );
  }

  updateHero(hero: Hero) {
    return this.http
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
      .pipe(
        tap(() => {
          this.selectedHero = null;
        })
      );
  }

  deleteHero(hero: Hero) {
    return this.http
      .delete(`/heroes/${hero.number}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
      .pipe(
        tap(() => {
          this.selectedHero = null;
        })
      );
  }

  addHero(name: string) {
    return this.http.post(
      "/heroes",
      { name },
      {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      }
    );
  }

  loadHeroes() {
    const token = localStorage.getItem("token");
    if (token) {
      this.http
        .get<Array<HeroBackend>>("/heroes", {
          headers: {
            Authorization: "Bearer " + token,
          },
        })
        .subscribe((data) => {
          this.heroes = data.map((heroBackend) => {
            const hero = {
              number: heroBackend.id,
              name: heroBackend.name,
            } satisfies Hero;
            return hero;
          });
        });
    }
  }
}
```

`hero.service.ts` - na wijziging

```ts
import { HttpClient } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { map, tap } from "rxjs";
import { Hero, HeroBackend } from "../components/models";

@Injectable({
  providedIn: "root",
})
export class HeroService {
  constructor(private http: HttpClient) {}

  selectedHero: null | Hero = null;
  heroes: Array<Hero> = [];

  topHeroes() {
    return this.heroes.slice(-4);
  }

  findHero(number: number) {
    // verwijder het object met de headers
    return this.http.get<HeroBackend>(`/heroes/${number}`).pipe(
      map((heroBackend) => {
        const hero = {
          number: heroBackend.id,
          name: heroBackend.name,
        } satisfies Hero;
        return hero;
      })
    );
  }

  updateHero(hero: Hero) {
    // verwijder het object met de headers
    return this.http
      .patch(`/heroes/${hero.number}`, {
        name: hero.name,
      })
      .pipe(
        tap(() => {
          this.selectedHero = null;
        })
      );
  }

  deleteHero(hero: Hero) {
    // verwijder het object met de headers
    return this.http.delete(`/heroes/${hero.number}`).pipe(
      tap(() => {
        this.selectedHero = null;
      })
    );
  }

  addHero(name: string) {
    // verwijder het object met de headers
    return this.http.post("/heroes", { name });
  }

  loadHeroes() {
    const token = localStorage.getItem("token");
    if (token) {
      // verwijder het object met de headers
      this.http.get<Array<HeroBackend>>("/heroes").subscribe((data) => {
        this.heroes = data.map((heroBackend) => {
          const hero = {
            number: heroBackend.id,
            name: heroBackend.name,
          } satisfies Hero;
          return hero;
        });
      });
    }
  }
}
```

Start de applicatie en navigeer rond. Je zal zien dat de applicatie nog steeds werkt. De code is nu een stuk "cleaner".

Alle wijzigingen: [GitHub](https://github.com/code-coaching/angular-tour-of-heroes/commit/a1d475fcfb17bc98f0b5205f95d00809b4d8c698)

## Conclusie

Er komt meer bij kijken wanneer de data beheerd wordt op een externe locatie. In deze tutorial is gekeken naar de ingebouwde Angular HttpClient om API calls te doen. Er is gekeken naar interceptors om de verzoeken naar de API te onderscheppen om de URL en de authorization token toe te voegen aan de request.

Er is gekeken naar Observables en hoe we de stroom van data kunnen bekijken (`.pipe(tap())`) en transformeren (`.pipe(map())`). Er is gekeken hoe een error afgehandeld kan worden (`.pipe(catchError())`). En er is gekeken hoe we kunnen weten wanneer een call klaar is (`.subscribe()`).

Services verzamelen alle functionaliteit rondom bepaalde data. Bijvoorbeeld `hero.service.ts` is verantwoordelijk voor het ophalen, toevoegen, wijzigen en verwijderen van heroes. In deze applicatie wordt de `state` (de data) ook bijgehouden in de service. In grotere applicaties kan het voorkomen dat er extra abstractielagen worden toegevoegd om de data te beheren.
