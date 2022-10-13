---
postUuid: 90bdd1f8-10ba-4d53-96cb-3623332a3abc
title: Databases
slug: databases
tags:
  - Databases
categories:
  - Backend
---

Databases zijn georganiseerde verzamelingen van data. Databasebeheersystemen, zijn systemen om met de data in een database te werken. Paradigma's/modellen beschrijven hoe data in een database wordt bijgehouden.

## NoSQL databases

NoSQL is een term voor alle databasemodellen die verschillen van het relationele model. Een voorbeeld van een NoSQL-database is een documentdatabase.

Een persoon met twee wagens zou op onderstaande manier opgeslagen kunnen worden.

```text
{
    "id": "0",
    "voornaam": "John",
    "achternaam": "Duck",
    "leeftijd": 29,
    "geslacht": "man",
    "wagens": [
        {
            "id": "0",
            "merk": "Opel",
            "type": "Manta 400"
        },
        {
            "id": "1",
            "merk": "Ferrari",
            "type": "F50"
        }
    ]

}
```

Voorbeelden van documentdatabases zijn MongoDB, RethinkDB ...

## Relationele databases

Bij relationele databases wordt gewerkt met tabellen om data te bewaren, waarbij de tabellen een bepaalde relatie tot elkaar hebben. Als voorbeeld kan een persoon voorgesteld worden door een rij in een tabel genaamd `personen`. Een wagen kan voorgesteld worden door een rij in een tabel genaamd `wagens`. Om aan te tonen dat een persoon de eigenaar is van een wagen, kan de wagen een kolom hebben genaamd `Eigenaar` waarin verwezen wordt naar een persoon.

Voorbeelden van relationele databases zijn MySQL, MariaDB, PostgreSQL ...

Personen

| id  | voornaam | achternaam | leeftijd | geslacht |
| :-: | :------- | :--------- | :------- | :------- |
|  0  | John     | Duck       | 29       | man      |
| ... | ...      | ...        | ...      | ...      |

Wagens

| id  | merk    | type      | eigenaar |
| :-: | :------ | :-------- | :------- |
|  0  | Opel    | Manta 400 | 0        |
|  1  | Ferrari | F50       | 0        |
| ... | ...     | ...       | ...      |

Uit de tabellen kunnen we afleiden dat één persoon meerdere wagens kan bezitten. De kolom `eigenaar` bevat de `id` van de eigenaar uit de tabel `personen`.

Een rij in een tabel, wordt een record genoemd.

### SQL

SQL (Structured Query Language) is een taal gemaakt om data op te vragen. Een voorbeeld om alle voor- en achternamen op te vragen van alle personen die achttien of ouder zijn: `SELECT voornaam, achternaam FROM personen WHERE leeftijd >= 18`.

### Datanormalisatie

Om te weten welke tabellen er gemaakt moeten worden, wordt datanormalisatie toegepast. Normalisatie zorgt ervoor dat data in de simpelste vorm mogelijk wordt opgeslagen, zonder dat er data dubbel opgeslagen wordt. In dit blogbericht wordt normalisatie niet stap per stap uitgelegd, het is belangrijk om te weten dat dit proces voorafgaat aan het bepalen van welke tabellen er gemaakt moeten worden.

In een bedrijf wordt dit meestal gedaan door een software-architect of een backendontwikkelaar.

### ERD

Een ERD (Entity Relationship Diagram) is een diagram met alle relaties tussen verschillende entiteiten/tabellen. Als frontendontwikkelaar is het belangrijk dat een ERD correct geïnterpreteerd kan worden. Het maken van een ERD wordt niet behandeld in dit blogbericht, het begrijpen van een ERD is wel belangrijk.

## ERD

Een ERD (Entity Relationship Diagram) geeft de verschillende relaties aan tussen verschillende entiteiten/tabellen. Het weergeven van de relaties kan met behulp van verschillende technieken. In dit blogbericht wordt gebruik gemaakt van de kraaienpootnotatie.

Nul of één

![nul of één](/img/blog/zero-or-one.png)

Exact één

![exact één](/img/blog/exactly-one.png)

Veel

![veel](/img/blog/many.png)

Één of veel

![een of veel](/img/blog/one-or-many.png)

Nul, één of veel

![nul, een of veel](/img/blog/zero-one-or-many.png)

### Opbouw van een ERD

![opbouw ERD](/img/blog/erd-creation.nl.png)

Elke tabel uit een database wordt in een ERD voorgesteld aan de hand van een tabel met één kolom. De koptekst (het donkergrijze gedeelte in de afbeelding) van de tabel (in het ERD) is de verzamelnaam voor hetgeen dat opgeslagen wordt in de tabel (in de database). Elke kolom uit de tabel van de database, krijgt een rij in de tabel van het ERD.

Elke rij in de tabel van een database, noemt een record. Elke rij in de tabel van een ERD bevat de kolomnaam van een record uit de tabel van een database.

#### Voorbeeld

Personen

| id  | voornaam | achternaam | leeftijd | geslacht |
| :-: | :------- | :--------- | :------- | :------- |
|  0  | John     | Duck       | 29       | man      |
| ... | ...      | ...        | ...      | ...      |
| 100 | Sylvia   | Duisters   | 28       | vrouw    |
| ... | ...      | ...        | ...      | ...      |

In deze tabel worden personen opgeslagen. Elke persoon heeft een "id" waarmee hij/zij uniek geïdentificeerd kan worden, een voornaam, een achternaam, een leeftijd en een geslacht.

Dit wordt in een ERD weergegeven als:

![personentabel](/img/blog/relation-example.nl.png)

<br/>

### Één-op-één-relatie

Een voorbeeld met exact één:

<br/>

![exact één voorbeeld](/img/blog/exactly-one-example.nl.png)

Het ERD kan gelezen worden als:

- een persoon heeft exact één wagen
- een wagen heeft exact één persoon als eigenaar

Uit het ERD kan ook afgeleid worden dat de relatie bijgehouden wordt in de tabel met wagens in een veld genaamd eigenaar.

Personen

| id  | voornaam | achternaam | leeftijd | geslacht |
| :-: | :------- | :--------- | :------- | :------- |
|  0  | John     | Duck       | 29       | man      |
| ... | ...      | ...        | ...      | ...      |
| 100 | Sylvia   | Duisters   | 28       | vrouw    |
| ... | ...      | ...        | ...      | ...      |

Wagens

| id  | merk    | type      | eigenaar |
| :-: | :------ | :-------- | :------- |
|  0  | Opel    | Manta 400 | 0        |
|  1  | Ferrari | F50       | 100      |
| ... | ...     | ...       | ...      |

<br/>

Een voorbeeld met nul of één:

<br/>

![nul of één voorbeeld](/img/blog/zero-or-one-example.nl.png)

Het ERD kan gelezen worden als:

- een persoon heeft nul wagens of één wagen
- een wagen heeft exact één persoon als eigenaar

Uit het ERD kan ook afgeleid worden dat de relatie bijgehouden wordt in de tabel met wagens in een veld genaamd eigenaar. In het geval dat een persoon geen wagens heeft, zal er geen rij terug te vinden zijn in de tabel met wagens voor deze persoon.

Personen

| id  | voornaam | achternaam | leeftijd | geslacht |
| :-: | :------- | :--------- | :------- | :------- |
|  0  | John     | Duck       | 29       | man      |
|  1  | Harry    | Potter     | 8        | man      |
| ... | ...      | ...        | ...      | ...      |
| 100 | Sylvia   | Duisters   | 28       | vrouw    |
| ... | ...      | ...        | ...      | ...      |

Wagens

| id  | merk    | type      | eigenaar |
| :-: | :------ | :-------- | :------- |
|  0  | Opel    | Manta 400 | 0        |
|  1  | Ferrari | F50       | 100      |
| ... | ...     | ...       | ...      |

<br/>

### Één-op-veel-relatie

Een voorbeeld:

![één op veel voorbeeld](/img/blog/one-to-many-example.nl.png)

Het ERD kan gelezen worden als:

- een persoon heeft nul wagens, één wagen of veel wagens
- een wagen heeft exact één persoon als eigenaar

Uit het ERD kan ook afgeleid worden dat de relatie bijgehouden wordt in de tabel met wagens in een veld genaamd eigenaar. In het geval dat een persoon geen wagens heeft, zal er geen rij terug te vinden zijn in de tabel met wagens voor deze persoon. In het geval dat een persoon exact één wagen heeft, zal er exact één rij terug te vinden zijn in de tabel met wagens voor deze persoon. In het geval dat een persoon meerdere wagens heeft, zullen er meerdere rijen terug te vinden zijn in de tabel met wagens voor deze persoon.

Personen

| id  | voornaam | achternaam | leeftijd | geslacht |
| :-: | :------- | :--------- | :------- | :------- |
|  0  | John     | Duck       | 29       | man      |
|  1  | Harry    | Potter     | 8        | man      |
| ... | ...      | ...        | ...      | ...      |
| 100 | Sylvia   | Duisters   | 28       | vrouw    |
| ... | ...      | ...        | ...      | ...      |

Wagens

| id  | merk    | type      | eigenaar |
| :-: | :------ | :-------- | :------- |
|  0  | Opel    | Manta 400 | 0        |
|  1  | Ferrari | F50       | 100      |
|  2  | Opel    | Corsa     | 100      |
| ... | ...     | ...       | ...      |

<br/>

### Veel-op-veel-relatie

In het geval dat een persoon meerdere wagens kan hebben en dat een wagen ook kan toebehoren tot meerdere eigenaren, is er een extra tabel nodig om deze relatie mogelijk te maken. Een tussentabel.

Een voorbeeld:

![veel op veel voorbeeld](/img/blog/many-to-many-example.nl.png)

Het ERD kan gelezen worden als:

- een persoon heeft één of meerdere wagens
- een wagen heeft één persoon of meerdere personen als eigenaar

Uit het ERD kan ook afgeleid worden dat de relatie bijgehouden wordt in de tabel genaamd `Eigenaren_Wagens`.

Als voorbeeld nemen we de personen uit voorgaande voorbeelden. John heeft een Opel Manta 400. Harry heeft geen wagen. Sylvia heeft een Ferrari F50 en een Opel Corsa. Als bijkomend scenario hebben John en Sylvia samen een Tesla Model S gekocht.

Personen

| id  | voornaam | achternaam | leeftijd | geslacht |
| :-: | :------- | :--------- | :------- | :------- |
|  0  | John     | Duck       | 29       | man      |
|  1  | Harry    | Potter     | 8        | man      |
| ... | ...      | ...        | ...      | ...      |
| 100 | Sylvia   | Duisters   | 28       | vrouw    |
| ... | ...      | ...        | ...      | ...      |

Wagens

| id  | merk    | type      |
| :-: | :------ | :-------- |
|  0  | Opel    | Manta 400 |
|  1  | Ferrari | F50       |
|  2  | Opel    | Corsa     |
| ... | ...     | ...       |
| 45  | Tesla   | Model S   |
| ... | ...     | ...       |

Eigenaren_Wagens

| id  | eigenaar | wagen |
| :-: | :------- | :---- |
|  0  | 0        | 0     |
|  1  | 100      | 1     |
|  2  | 100      | 2     |
|  3  | 0        | 45    |
|  4  | 100      | 45    |
| ... | ...      | ...   |

<br/>

## SQL - Structured Query Language

SQL is een standaard manier om met data van relationele databases te werken.

Dit is het best aan te leren door middel van het toepassen van de queries.
Hiervoor wordt gebruik gemaakt van [w3schools](https://www.w3schools.com).

Dit hoofdstuk wordt gedaan door middel van zelfstudie (in de les).
En het maken van de oefeningen (in de les).

Bekijk de [theorie](https://www.w3schools.com/sql)
vanaf `SQL Syntax` tot en met `SQL Group By` (in het zijmenu).

Ter voorbereiding van de test, doe de [quiz](https://www.w3schools.com/sql/sql_quiz.asp)
en de [oefeningen](https://www.w3schools.com/sql/sql_exercises.asp).
Het onderdeel `SQL Database` van de oefeningen is geen onderdeel van de te kennen
leerstof.

## API

API staat voor Application Programming Interface, dit is een manier om applicaties
met elkaar te laten communiceren. Het meest voorkomende type API bij het
ontwikkelen van webapplicaties is een RESTful API.
Een ander type dat meer en meer voorkomt tegenwoordig, is een GraphQL API.

### Protocols

Gebruikte termen:

- `client`: Een toestel (computer, smartphone, tablet ...) dat gebruikt wordt door een gebruiker.
- `server`: Een toestel dat altijd aanstaat waarop bestanden staan die via een protocol (bv. HTTP) opgehaald kunnen worden.
- `request`: Een verzoek dat gedaan wordt vanuit de client naar de server.
- `response`: Een antwoord dat gestuurd wordt vanuit de server naar de client.

#### HTTP/HTTPS

HTTP staat voor *H*yper*t*ext *T*ransfer *P*rotocol. De `S` in HTTP*S* staat voor *S*ecure.

Het verschil tussen HTTP en HTTPS is dat wanneer een wachtwoord verstuurd wordt via HTTP, dit in plain text is. Indien deze communicatie onderschept wordt, kan het wachtwoord gewoon gelezen worden door de tussenpersoon. Bij HTTPS wordt het wachtwoord geëncrypteerd/versleuteld verstuurd. Indien deze communicatie onderschept wordt, ziet de tussenpersoon het versleutelde wachtwoord.

HTTP(S) is een request/response protocol. Er wordt een request (verzoek) uitgestuurd vanuit een client naar een server. De server verwerkt dit verzoek en stuurd een response (antwoord) terug.

HTTP(S) is unidirectioneel, dit betekent dat de communicatie maar langs één kant loopt. Een client zal altijd een request uitsturen, een server zal altijd antwoorden.

##### POST/GET/PUT/DELETE & CRUD

Er kunnen verschillende soorten requests uitgestuurd worden naar de server.
Een POST geeft aan dat er een item aangemaakt (Create) moet worden.
Een GET geeft aan dat er een item gelezen (Read) moet worden.
Een PUT geeft aan dat er een item gewijzigd (Update) moet worden.
Een DELETE geeft aan dat er een item verwijderd (Delete) moet worden.

Een term die vaker terug gaat komen is `CRUD`. Deze term betekent dat er *C*reate, *R*ead, *U*pdate en *D*elete uitgevoerd kan worden.

#### WS/WSS

WS staat voor *W*eb*S*ocket.
De `S` in WS*S* staat voor *S*ecure.

Net als bij HTTPS, zorgt WSS ervoor dat de communicatie versleuteld is.

WS(S) is bidirectioneel, dit betekent dat zowel de client berichten kan sturen
naar de server en dat de server berichten kan sturen naar de client.

Een voorbeeld waarvoor dit gebruikt kan worden is Instant Messaging.

### RESTful

REST staat voor **RE**presentational **S**tate **T**ransfer. Dit is een architectuur die gevolgd kan worden om een API op te bouwen.

Stel dat er een domein (bv. https://ditisnietecht.com) is. Op de server waar dit domein naar wijst, staat een API om alle cursisten op te vragen.

Dan zou dat in een RESTful API als volgt zijn:

```
HTTP GET https://ditisnietecht.com/api/cursisten
```

HTTP geeft aan dat het over het HTTP-protocol gaat, GET geeft het type van de request aan. De domeinnaam wijst naar de server en `/api/cursisten` is een `end point` van de API. In dit geval zal een GET request naar `/api/cursisten` een response geven waarin alle cursisten zitten.

Achterliggend zal de server de request afhandelen. Één van de mogelijke opties is dat de server een `SELECT * FROM cursisten` doet en het resultaat van deze query terugstuurd.

Een RESTful API werkt met POST/GET/PUT/DELETE requests om CRUD-operaties uit te voeren.

### GraphQL

GraphQL is een andere manier om met een server te communiceren. GraphQL werkt
enkel met POST requests. In de inhoud die meegestuurd wordt met de POST requests
wordt aangegeven wat de request wilt dat de server doet.

### JSON

Tussen een browser en een server kan enkel gecommuniceerd worden via tekst. Om
te zorgen dat er toch complexe data gestuurd kan worden, wordt er gebruik gemaakt
van JSON.

JSON staat voor *J*ava*S*cript *O*bject *N*otation.

Een object in JavaScript kan complexe data voorstellen. Bijvoorbeeld een persoon.
Een persoon heeft een voornaam, achternaam en leeftijd.

```js
const persoon = { voornaam: "Bart", achternaam: "Duisters", leeftijd: 29 };
```

Een JSON-object is bijna gelijkend aan een JavaScript-object, maar alle
properties staan tussen dubbele quotes.

```json
{
  "voornaam": "Bart",
  "achternaam": "Duisters",
  "leeftijd": 29
}
```

Tegenwoordig wordt meestal JSON gebruikt om data tussen client en server uit
te wisselen.
