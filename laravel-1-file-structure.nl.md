---
postUuid: 079ba08a-fd55-4db8-8be7-123a8dd27040
title: Laravel - Bestandsstructuur
slug: laravel-bestandsstructuur
tags:
  - PHP
  - Laravel
categories:
  - Backend
---

## Laravel bestandsstructuur

Laravel maakt gebruik van een vaste bestandsstructuur. Dit is een overzicht van de mappen en enkele bestanden. Dit dient enkel als eerste kennismaking met de bestandsstructuur (het is oké om in deze fase nog niks zelf uit te voeren), later wordt er in meer detail ingegaan op de verschillende mappen en bestanden.

```
├── app
│   ├── Console
│   ├── Exceptions
│   ├── Http
│   │   ├── Controllers
│   │   ├── Middleware
│   ├── Models
│   ├── Providers
│   └── ...
├── bootstrap
├── config
├── database
├── public
├── resources
│   ├── css
│   ├── js
│   └── views
├── routes
├── storage
├── tests
├── vendor
├── .env
├── .env.example
├── .gitignore
├── artisan
├── composer.json
├── composer.lock
├── package.json
├── phpunit.xml
├── .vite.config.js
└── ...
```

Het is oké dat niet alle bestanden of de inhoud van de bestanden volledig duidelijk zijn. Dit wordt duidelijker naar mate je meer ervaring opdoet met Laravel.

## Model View Controller

Laravel is een MVC-framework, wat staat voor Model-View-Controller. Dit is een manier om de code van een applicatie te structureren. Het Model is de laag die de data bevat, de View is de laag die de gebruiker te zien krijgt, en de Controller is de laag die het Model en de View met elkaar verbindt.

### Model

Laravel houdt de Models bij in de map `app/Models`. Een Model is een PHP-class die de data van een bepaalde tabel in de database bevat. Een Model is een object-georiënteerde representatie van een tabel in de database. Een Model kan bijvoorbeeld een `User` zijn, die de data van de `users`-tabel bevat.

Bekijk `app/Models/User.php` als voorbeeld.

### View

Laravel gebruikt de `Blade`-templating engine om Views te genereren. Een View is een HTML-bestand dat de gebruiker te zien krijgt. Een View kan bijvoorbeeld een `home`-pagina zijn, of een `login`-pagina.

Views zijn terug te vinden in de map `resources/views`. Een View is een bestand met de extensie `.blade.php`. Met PHP is het al mogelijk om PHP-code te schrijven in een HTML-bestand, maar met de `Blade Templating Engine` is het mogelijk om nog meer te doen. Zo is het bijvoorbeeld mogelijk om conditionele en iteratieve statements te gebruiken, om de inhoud van een variabele te tonen, om componenten te gebruiken, enzovoort.

Bekijk `resources/views/welcome.blade.php` als voorbeeld.

Hier vallen enkele dingen op:

- Er wordt gebruikgemaakt van Tailwind als CSS-framework.
- Er staan `@if` statements in de HTML-code.
- Er staan `{{ }}` in de HTML-code om de waarde van een variabele te tonen.

### Controller

Een Controller is een PHP-class die een Model en een View met elkaar verbindt. Bijvoorbeeld, stel dat er een endpoint `/students` is, dan kan er een `StudentController` zijn die de data via `Student`-Model ophaalt (data uit de database) en deze data doorgeeft aan de `student`-View (de HTML-pagina die de gebruiker te zien krijgt).

Dit wordt later bekeken wanneer er custom controllers worden aangemaakt.

## Routes

Laravel gebruikt routes om te bepalen welke code er moet uitgevoerd worden wanneer een bepaalde URL bezocht wordt. Een route is een PHP-bestand dat zich bevindt in de map `app/routes`. Er zijn twee soorten routes: web routes en api routes. We beginnen met routes in `web.php`, hierin worden de routes gedefinieerd die de gebruiker kan bezoeken in de browser of de routes waarnaar data gestuurd kan worden om op te slaan in de database. Een route in `web.php` zal dus altijd een View teruggeven.

Later wordt er gekeken naar de routes in `api.php`, deze routes worden gebruikt om data op te halen of te versturen naar de applicatie via een API. Een route in `api.php` zal dus altijd (JSON) data teruggeven.

Bekijk `routes/web.php` als voorbeeld.

```php
Route::get('/', function () {
    return view('welcome');
});
```

Hierin staat dus: wanneer de gebruiker de URL `/` bezoekt, geef dan de View `welcome` terug. Dit verwijst naar `welcome.blade.php` in de map `resources/views`. Het rechtstreeks teruggeven van een View is mogelijk, maar later wordt er gekeken naar het gebruik van een Controller om de View terug te geven.

### Middleware

Middelware is een manier om code uit te voeren voor of na het uitvoeren van een route.

Bestaande middleware is terug te vinden in de map `app/Http/Middleware/`.

Bekijk `app/Http/Middleware/Authenticate.php` als voorbeeld.

De `handle`-functie wordt uitgevoerd wanneer een route wordt uitgevoerd. In dit geval wordt er gecontroleerd of de gebruiker ingelogd is. Wanneer de gebruiker ingelogd is, wordt de gebruiker doorgestuurd naar de home-pagina. Wanneer de gebruiker niet ingelogd is, wordt de gebruiker naar de opgevraagde pagina doorgestuurd.

## Migraties

Een migratie is een PHP-bestand dat zich bevindt in de map `database/migrations`. Een migratie wordt gebruikt om de structuur van de database te definiëren. Een migratie bevat de code om een tabel aan te maken, om een kolom toe te voegen aan een tabel, om een kolom te verwijderen van een tabel, enzovoort.

Bekijk `database/migrations/2014_10_12_000000_create_users_table.php` als voorbeeld.

Wanneer we `php artisan migrate` uitvoeren, zal Laravel alle nog niet uitgevoerde migraties uitvoeren. Dit betekent dat de code in de `up()`-methode wordt uitgevoerd. Wanneer we `php artisan migrate:rollback` uitvoeren, zal Laravel de laatst uitgevoerde migratie ongedaan maken. Dit betekent dat de code in de `down()`-methode wordt uitgevoerd.

Momenteel kan je nog geen migraties uitvoeren, omdat er nog geen database is. In een latere stap wordt er gekeken naar het opzetten van een database.

## Conclusie

Dit is een overzicht van enkele belangrijke mappen en bestanden in Laravel. Één van de doelen van deze cursus is om je vertrouwd te maken met het toevoegen van nieuwe functionaliteit aan een Laravel-applicatie. Dit betekent dat je niet alle bestanden en mappen van buiten moet kennen, maar dat je wel moet weten waar je nieuwe code moet toevoegen: MVC - Model in `app/Models`, View in `resources/views`, Controller in `app/Http/Controllers`. Routes in `routes/web.php` (of `routes/api.php` indien een RESTful API gemaakt wordt). Migraties in `database/migrations`.

Meer informatie over de bestandsstructuur is terug te vinden in de [documentatie](https://laravel.com/docs/10.x/structure).
