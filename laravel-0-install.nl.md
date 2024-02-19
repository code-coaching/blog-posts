---
postUuid: 5ecb182e-e820-43ad-a1e4-ccb1048a7108
title: Laravel - Installatie
slug: laravel-installatie
tags:
  - PHP
  - Laravel
categories:
  - Backend
---

## Laravel

Laravel is een PHP framework voor het bouwen van webapplicaties. Het is een `MVC`-framework, wat staat voor `Model-View-Controller`. Dit is een manier om de code van een applicatie te structureren. Het `Model` is de laag die de data bevat, de `View` is de laag die de gebruiker te zien krijgt, en de `Controller` is de laag die het `Model` en de `View` met elkaar verbindt.

Om Laravel te kunnen gebruiken heb je PHP en Composer nodig.

De [Laravel documentatie](https://laravel.com) is zeer goed en bevat veel informatie. Doordat dit veel informatie bevat, kan het soms overweldigend zijn om te starten met Laravel. In deze cursus zal er één specifiek pad gevolgd worden om Laravel aan te leren. Waar mogelijk zal er verwezen worden naar de documentatie.

## Requirements

### PHP

Indien je nog geen PHP hebt geïnstalleerd, kan je dit doen door XAMPP te installeren. De uitleg hiervoor kan je terugvinden bij de post [PHP Installatie](/blogs/php-installatie).

### Composer

Composer is een package manager voor PHP. Voor de mensen die al gekend zijn met JavaScript, dit is te vergelijken met NPM. Het wordt gebruikt om Laravel te installeren, maar ook om andere packages te installeren.

Mensen met een Mac kunnen Composer installeren met Homebrew:

```sh
brew install composer
```

Mensen met Windows kunnen Composer installeren via de installer [Compose-Setup.exe](https://getcomposer.org/doc/00-intro.md#installation-windows).

## Laravel installeren

Laravel kan geïnstalleerd worden met Composer. Dit kan door het volgende commando uit te voeren:

```sh
composer create-project laravel/laravel project-name
```

Waarbij `project-name` vervangen wordt door de naam van je project.

## Laravel opstarten

Om de applicatie te starten navigeer je naar de map van het project:

```sh
cd project-name
```

En vervolgens kan je de applicatie starten met:

```sh
php artisan serve
```
