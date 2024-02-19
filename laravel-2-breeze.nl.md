---
postUuid: 383944fe-eff9-4d83-8190-6d05561536b4
title: Laravel - Breeze
slug: laravel-breeze
tags:
  - PHP
  - Laravel
  - Breeze
categories:
  - Backend
---

## Laravel

## Laravel installeren

Laravel installeren

```sh
composer create-project laravel/laravel project-name
```

Verander `project-name` naar de naam van je project.

Nu kan je de applicatie starten met `php artisan serve`. Je kan de applicatie bekijken op `http://localhost:8000`.

Tip: Doe een `git init` in de projectmap om een Git-repository aan te maken. Commit de huidige code met `git add .` en `git commit -m "Initial commit"`. Dan is het heel duidelijk wat de volgende stap zal toevoegen aan de codebase.

## Laravel Breeze

Laravel Breeze installeren - dit is een `starter kit` voor Laravel, het bevat authentificatie en pagina's voor het registreren, inloggen, bevestigen van e-maildressen, wachtwoord vergeten, enz.

```sh
composer require laravel/breeze --dev
```

Gebruik vervolgens het volgende om de Breeze-installatie uit te voeren:

```sh
php artisan breeze:install
```

Er zijn verschillende opties beschikbaar, er wordt gekozen voor `Blade with Alpine`.

```
 ┌ Which Breeze stack would you like to install? ───────────────┐
 │ › ● Blade with Alpine                                        │
 │   ○ Livewire (Volt Class API) with Alpine                    │
 │   ○ Livewire (Volt Functional API) with Alpine               │
 │   ○ React with Inertia                                       │
 │   ○ Vue with Inertia                                         │
 │   ○ API only                                                 │
 └──────────────────────────────────────────────────────────────┘
```

```
 ┌ Which Breeze stack would you like to install? ───────────────┐
 │ Blade with Alpine                                            │
 └──────────────────────────────────────────────────────────────┘

 ┌ Would you like dark mode support? ───────────────────────────┐
 │ Yes                                                          │
 └──────────────────────────────────────────────────────────────┘

 ┌ Which testing framework do you prefer? ──────────────────────┐
 │ PHPUnit                                                      │
 └──────────────────────────────────────────────────────────────┘
```

Merk op dat er heel wat Controllers zijn bijgekomen. Ook krijgen we heel wat Views cadeau. De routes zijn ook allemaal al gekoppeld - dit is wat Breeze voorziet: een starter kit om snel te starten met een applicatie die reeds authentificatie bevat.

## Database

Voor deze oefening wordt er gekozen om met `SQLite` te werken als database. Dit is een eenvoudige database die in een bestand wordt opgeslagen. Dit is handig voor ontwikkeling, maar iets minder geschikt voor productie - daar zou bijvoorbeeld gekozen kunnen worden voor `MySQL` of `PostgreSQL`.

In de `.env` file kennen we `sqlite` toe aan de `DB_CONNECTION` variabele.
In de `.env` file kennen we `/Users/johnduck/Projects/project-name/database/my-database.sqlite` toe aan de `DB_DATABASE` variabele, dit is het **absolute pad** naar het bestand. Op Windows zou dit bijvoorbeeld `C:\Users\johnduck\Projects\project-name\database\my-database.sqlite` kunnen zijn.

```sh
DB_CONNECTION=sqlite
DB_DATABASE=/Users/johnduck/Projects/project-name/database/my-database.sqlite
```

Denk eraan om het absolute pad (DB_DATABASE) aan te passen naar het pad op jouw computer.

Tip: Gebruik `pwd` in de root van je project om het absolute pad te bekomen van het project, plaats hierachter dan `/database/my-database.sqlite`.

Voer de migratie uit, indien er gevraagd wordt of de database aangemaakt mag worden, antwoord dan met `yes`:

```sh
php artisan migrate

  WARN  The SQLite database does not exist: medflix_local.

┌ Would you like to create it? ────────────────────────────────┐
│ Yes                                                          │
└──────────────────────────────────────────────────────────────┘
```

Het bestand `my-database.sqlite` zal aanwezig zijn in de `database` folder.

Indien je gebruik maakt van `Visual Studio Code` kan je de [`SQLite`-extensie](https://marketplace.visualstudio.com/items?itemName=alexcvzz.vscode-sqlite) installeren om de database te bekijken.

## Mail

Voor deze oefening wordt er gekozen om met `log` te werken. Dat wilt zeggen dat de e-mails niet echt verstuurd worden, maar dat ze in een logbestand worden opgeslagen. Dit is handig tijdens development. In de praktijk zou er gekozen kunnen worden voor `smtp` met de `SMTP`-gegevens van een e-mailprovider, bijvoorbeeld je eigen Gmail-account.

In de `.env` file kennen we `log` toe aan de `MAIL_MAILER` variabele.

```sh
MAIL_MAILER=log
```

## User model - MustVerifyEmail

Indien het niet nodig is om e-mailadressen te verifiëren, kan deze stap overgeslagen worden.

In het `app/Models/User.php`-bestand wordt de `MustVerifyEmail`-interface toegevoegd aan de `User`-class.

`app/Models/User.php`

```php
<?php

namespace App\Models;

use Laravel\Sanctum\HasApiTokens;
use Illuminate\Notifications\Notifiable;
use Illuminate\Contracts\Auth\MustVerifyEmail; // Voeg deze regel toe
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;

// voeg toe: implements MustVerifyEmail
class User extends Authenticatable implements MustVerifyEmail
{
    use HasApiTokens, HasFactory, Notifiable;

    // hier nog code
}
```

## Uittesten

- Start de applicatie met `php artisan serve` en bekijk de applicatie op `http://localhost:8000`.
- Bekijk het logbestand `storage/logs/laravel.log`, indien hier al iets instaat, verwijder dan de inhoud van het bestand.
- Klik op de webpagina op `Register` en registreer een gebruiker.
- Bekijk via de `SQLite`-extensie of de gebruiker is toegevoegd aan de database.

| id  | name      | email         | email_verified_at |
| --- | --------- | ------------- | ----------------- |
| 1   | John Duck | john@duck.com | NULL              |

Merk op dat `email_verified_at` `NULL` is, dit wilt zeggen dat het e-mailadres nog niet bevestigd is, de gebruiker is nog niet geverifieerd.

- Bekijk het logbestand `storage/logs/laravel.log` en zoek naar de registratie-e-mail, klik op de verificatielink in de e-mail.
- Bekijk via de `SQLite`-extensie of de `email_verified_at` nu een waarde heeft (het kan zijn dat je opnieuw op 'play' moet drukken in de `SQLite`-extensie om de query opnieuw uit te voeren).
  | id | name | email | email_verified_at |
  | --- | --------- | ------------- | ------------------- |
  | 1 | John Duck | john@duck.com | 2024-01-08 21:09:29 |

Merk op dat `email_verified_at` nu een waarde heeft, dit wilt zeggen dat het e-mailadres bevestigd is, de gebruiker is geverifieerd. Deze informatie kan gebruikt worden om bepaalde pagina's enkel toegankelijk te maken voor geverifieerde gebruikers.

## Hot Module Reloading

Wanneer we enkel `php artisan serve` uitvoeren, zal de applicatie opgestart worden. Maar, na een wijziging in de code, zal de applicatie niet automatisch herladen worden, we moeten manueel de browser herladen.

Hot Module Reloading is een techniek die ervoor zorgt dat de browser automatisch herlaadt wanneer er een wijziging is in de code. Dit is handig tijdens development. Binnen Laravel kan dit geactiveerd worden door in één terminal `npm run dev` uit te voeren en in een andere terminal `php artisan serve`. `npm run dev` zal de code opnieuw verwerken na elke wijziging, `php artisan serve` zal de PHP server beschikbaar maken op `http://localhost:8000`.

Test dit uit door `npm run dev` en `php artisan serve` uit te voeren in twee verschillende terminals. Bekijk de applicatie op `http://localhost:8000`. Maak een wijziging in een `blade`-bestand, bijvoorbeeld `resources/views/auth/login.blade.php`. Bekijk de wijziging in de browser, de wijziging zou automatisch zichtbaar moeten zijn.

Let op: `welcome.blade.php` wordt **niet** automatisch herladen. Indien je wilt dat hier ook Hot Module Reloading op werkt, voeg je `@vite(['resources/css/app.css', 'resources/js/app.js'])` toe in de `head`-tag:

```html
<!doctype html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <title>Laravel</title>

    <!-- hier nog elementen -->

    @vite(['resources/css/app.css', 'resources/js/app.js'])
  </head>
  <body>
    <!-- hier de inhoud van body -->
  </body>
</html>
```

De reden dat alle andere pagina's wel automatisch herladen worden, is omdat deze pagina's gebruik maken van `<x-app-layout></x-app-layout>` of `<x-guest-layout></x-guest-layout>`, wanneer je deze componenten bekijken (`resources/views/layouts/app.blade.php` en `resources/views/layouts/guest.blade.php`), zie je dat deze componenten ook deze regel bevatten: `@vite(['resources/css/app.css', 'resources/js/app.js'])`. Dit is wat ervoor zorgt dat de pagina's automatisch herladen worden. Na het toevoegen van deze regel, kan heel de `style`-tag verwijderd worden waarin Tailwind hardcoded staat.

## Formatting

Gebruik je `Visual Studio Code`? Dan kan je [Headwind](https://marketplace.visualstudio.com/items?itemName=heybourn.headwind) installeren. Dit zorgt voor formatting in de `blade`-bestanden én het sorteert de Tailwind classes op een vaste manier.

## Conclusie

Met Laravel en Laravel Breeze is het eenvoudig om een applicatie te maken met authentificatie en e-mailverificatie. De applicatie kan eenvoudig getest worden met `SQLite` en de e-mails kunnen eenvoudig bekeken worden in het logbestand.

Meer informatie over de verschillende [Laravel Starter Kits](https://laravel.com/docs/10.x/starter-kits) - kan teruggevonden worden in de documentatie. Hiervan hebben we specifiek `Laravel Breeze + Blade` gebruikt.