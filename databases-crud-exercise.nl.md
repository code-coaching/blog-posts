---
postUuid: ab7789a4-bc14-4135-b6b8-b0e9b3795648
title: CRUD oefening
slug: crud-oefening
tags:
  - Databases
categories:
  - Backend
---

CRUD staat voor *C*reate, *R*ead, *U*pdate en *D*elete. Dit zijn de vier basisoperaties die uitgevoerd kunnen worden op data in een database.

## Personal Access Token

Om deze oefening te kunnen maken heb je een personal access token nodig. Deze kun je genereren via jouw [profiel](https://code-coaching.dev/profiel). Na het genereren krijg je deze één keer te zien, bijv. `69|U4Kpv9yZkl4Y92J9onqAoSe7EIyKzhKyWEnv0OIt96cef6b1`. Ben je deze kwijt? Dan kun je een nieuwe genereren.

Het is belangrijk dat je deze token privé houdt. De token geeft je toegang tot jouw account en kan gebruikt worden om allerlei acties uit te voeren. Indien je Git gebruikt, zorg er dan voor dat je de token **NIET** commit.

## Heroes

In deze oefening ga je data van superhelden (heroes) aanmaken (Create), ophalen (Read), aanpassen (Update) en verwijderen (Delete). De data wordt achterliggend opgeslagen in een database.

De screenshots zijn van het programma [Postman](https://www.postman.com/). Dit is een handige tool om HTTP requests mee te maken en te testen.

## Authorization

Om de HTTP-verzoeken te kunnen sturen, zal je jouw token moeten meesturen met elk verzoek.

![Authorization](/img/blog/crud-heroes/authorization.png)

Dit kan gedaan worden door:

1. Geef de HTTP-methode aan (GET, POST, PUT/PATCH, DELETE) en de URL `https://code-coaching.dev/api/heroes`
2. Klik op de tab `Authorization`
3. Kies bij `Type` voor `Bearer Token`
4. Vul bij `Token` jouw token in (bijv. `69|U4Kpv9yZkl4Y92J9onqAoSe7EIyKzhKyWEnv0OIt96cef6b1`)

## GET

```sh
GET https://api.code-coaching.dev/api/heroes
```

Dit endpoint geeft een array/lijst van alle heroes terug. Indien er nog geen heroes aangemaakt zijn via jouw gebruiker (token), dan zie je een lege array.

![GET heroes](/img/blog/crud-heroes/get-heroes.png)

## POST

```sh
POST https://api.code-coaching.dev/api/heroes
{
  "name": "John Duck"
}
```

Dit endpoint maakt een nieuwe hero aan. De naam van de hero wordt meegegeven in de body van de request. De response bevat de aangemaakte hero.

![POST heroes](/img/blog/crud-heroes/post-heroes.png)

1. Geef de HTTP-methode aan `POST` en de URL `https://api.code-coaching.dev/api/heroes`
2. Klik op de tab `Body`
3. Kies voor `raw`
4. Kies voor `JSON`
5. Vul de body in met het JSON object:

```json
{
  "name": "John Duck"
}
```

6. Klik op `Send`
7. De response bevat de aangemaakte hero

Merk op dat de hero een `id` heeft gekregen. Deze wordt automatisch gegenereerd door de database. Ook bevat de hero `timestamps` (`created_at` en `updated_at`). Deze worden automatisch gegenereerd door de database. De `user_id` is het id van de gebruiker die de hero heeft aangemaakt.

## GET :id

Doe opnieuw een GET op `/heroes` en je ziet de aangemaakte hero in de array staan.

```sh
GET https://api.code-coaching.dev/api/heroes
```

![GET heroes](/img/blog/crud-heroes/get-heroes-result.png)

Merk op dat dit een array teruggeeft met alle heroes. Op dit moment is er maar één hero aangemaakt, dus de array bevat maar één element.

Doe een GET op een specifieke hero door het id van de hero mee te geven in de URL.

```sh
GET https://api.code-coaching.dev/api/heroes/:id
```

Vervang `:id` door het id van de hero die je wilt ophalen.

![GET hero](/img/blog/crud-heroes/get-heroes-id.png)

Merk op dat dit één object teruggeeft, niet een array. Dit is omdat je een specifieke hero hebt opgevraagd.

## PUT/PATCH

`PUT` wordt gebruikt om een volledig object te vervangen. `PATCH` wordt gebruikt om een deel van een object te vervangen.

De Code Coaching API heeft enkel een `PATCH` endpoint.

```sh
PATCH https://api.code-coaching.dev/api/heroes/:id
{
  "name": "John Doe"
}
```

Vervang `:id` door de id van de hero die je wilt aanpassen. De wijziging wordt meegegeven in de body van de request. De response bevat de aangepaste hero.

![PATCH hero](/img/blog/crud-heroes/patch-heroes-id.png)

## DELETE

```sh
DELETE https://api.code-coaching.dev/api/heroes/:id
```

Vervang `:id` door de id van de hero die je wilt verwijderen. De response bevat het object dat verwijderd is.

![DELETE hero](/img/blog/crud-heroes/delete-heroes-id.png)

Vraag opnieuw de hero op met het id dat je verwijderd hebt. Je krijgt een `404 Not Found` terug.

![GET hero 404](/img/blog/crud-heroes/get-heroes-unknown-id.png)

## Oefening

Maak een lijst van heroes aan. Gebruik de bovenstaande endpoints om de heroes aan te maken, op te halen, aan te passen en te verwijderen.

- Maak minstens 5 heroes aan
- Geef elke hero een andere naam
- Pas de naam van één hero aan
- Verwijder één hero
- Haal de lijst van heroes op
