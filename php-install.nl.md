---
postUuid: 2fe073fd-6bcd-4657-9d53-ddde2a1d31e8
title: PHP - Installatie
slug: php-installatie
tags:
  - PHP
  - XAMPP
categories:
  - Backend
---

Om PHP te gebruiken heb je verschillende dingen nodig. Je hebt een webserver nodig, een database en PHP zelf. Je kan dit allemaal apart installeren, maar je kan ook een softwarepakket gebruiken die dit allemaal voor je regelt. Een van de meest gebruikte softwarepakketten is XAMPP.

## Installatie

[Download XAMPP](https://www.apachefriends.org/download.html). Kies de versie die bij jouw besturingssysteem past. Kies voor de laatste versie van PHP.

## XAMPP starten

Start XAMPP op (bij Mac kan het zijn dat dit staat onder 'other', onder de naam 'manager-osx').

Start de Apache service, dit is de webserver die nodig is om PHP te kunnen gebruiken.

### Mac

![Mac XAMPP welcome](/img/blog/php/xampp-mac-welcome.png)

![Mac XAMPP servers)](/img/blog/php/xampp-mac-servers.png)

### Windows

![Windows XAMPP](/img/blog/php/xampp-windows.png)

## Testen

Om te testen of het werkt kan je in een webbrowser (Chrome, Firefox, Safari, Edge, ...) naar `http://localhost` gaan. Je zou dan een welkomstpagina moeten zien.

![XAMPP welcome](/img/blog/php/xampp-welcome.png)

## Werkmap

De werkmap van XAMPP is `htdocs`. Deze map kan je vinden in de map waar je XAMPP hebt geïnstalleerd. In deze map kunnen we onze website plaatsen (html/css/js én php).

### Mac

Klik op `Open Application Folder`, terug te vinden in de `Welcome`-tab.

![Mac XAMPP application folder](/img/blog/php/xampp-mac-path.png)

Dit is standaard `/Applications/XAMPP/`. Op de knop klikken opent de locatie `/Applications/XAMPP/xamppfiles/`.

Hierin vind je de map `htdocs`. Dit is de werkmap.

### Windows

De werkmap is standaard `C:\xampp\`.

Indien dit op een andere locatie staat kan dit teruggevonden worden in de logs van XAMPP.

![Windows XAMPP application folder](/img/blog/php/xampp-windows-path.png)

Hierin vind je de map `htdocs`. Dit is de werkmap.

## Nieuw HTML project aanmaken

Maak een nieuwe map aan in `htdocs`. De mapnaam is de naam van het project. Bijvoorbeeld `php-test`.

In deze map kan je een nieuw bestand aanmaken. Bijvoorbeeld `index.html`.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>This works!</h1>
  </body>
</html>
```

Als je nu in een webbrowser naar `http://localhost/php-test` gaat, zou je de inhoud van `index.html` moeten zien.

Voor de oplettende lezer, bij het bezoeken van `http://localhost` werd er een welkomstpagina getoond, waar komt deze vandaan? Als we naar de url kijken, zien we dat er een redirect naar `http://localhost/dashboard/` gebeurd is. Dit is de website die teruggevonden kan worden in de map `dashboard` in de `htdocs`-map.

## HTML wijzigen naar PHP

Wijzig de bestandsnaam van `index.html` naar `index.php`.

```php
<?php
  $name = "John Duck";
?>

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>This works!</h1>
    <p><?php echo 'This works!' ?></p>
    <p>My name is <?php echo $name; ?></p>
  </body>
</html>
```

Als je nu in een webbrowser naar `http://localhost/php-test` gaat, zou je de inhoud van `index.php` moeten zien.

Bij het bekijken van de broncode van de pagina in de browser, zien we dat de PHP-code niet zichtbaar is. Dit komt omdat de PHP-code op de server wordt uitgevoerd en het resultaat wordt naar de browser gestuurd, dit is de HTML-code die we zien.

![PHP code in browser](/img/blog/php/xampp-example.png)

## Conclusie

Je hebt nu een werkende PHP-omgeving. XAMPP is een tool die je kan gebruiken om lokaal een webserver te draaien die PHP ondersteunt.

We hebben gezien dat we een HTML-bestand kunnen wijzigen naar een PHP-bestand door de bestandsnaam te wijzigen. We kunnen dan PHP-code toevoegen aan het bestand. Deze code wordt op de server uitgevoerd en het resultaat wordt naar de browser gestuurd.
