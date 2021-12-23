---
title: Logica
slug: logica
tags:
  - Leren Programmeren
categories:
  - Frontend
---

## Bits en bytes

Om beter te begrijpen hoe computers werken, wordt er gekeken naar de simpelste versie van een computer: een Turing-machine. Een Turing-machine, ondanks de naam, is geen machine. Het is een model om te omschrijven wat een computer doet.

Turing verwijst naar Alan Turing, een computerwetenschapper die realiseerde dat alle rekenproblemen voorgesteld kunnen worden in een digitale taal: 0 en 1. Een Turing-machine kan gemaakt worden uit alles wat in twee toestanden geplaatst kan worden: 'aan' en 'uit', 'ja' en 'nee ...

### Bit

Een bit kan een 0 of een 1 zijn. Dit kan gevisualiseerd worden met een schakelaar. Staat de schakelaar omhoog, dan is het een 1. Staat de schakelaar omlaag, dan is het een 0.

Een bit kan bijvoorbeeld gebruikt worden om te bepalen of een lamp aan- of uitstaat.

| bit | lamp |
| :-: | :--: |
|  0  | uit  |
|  1  | aan  |

### Tweebitssysteem

Een systeem bestaande uit 2 bits, kan vier verschillende toestanden voorstellen. Denk hierbij aan een RGB-ledlampje. De ledlamp kan uitstaan of de kleuren rood, groen en blauw weergeven wanneer het aanstaat.

| bit 1 | bit 2 |  led  |
| :---: | :---: | :---: |
|   0   |   0   |  uit  |
|   0   |   1   | rood  |
|   1   |   0   | groen |
|   1   |   1   | blauw |

### Byte

Een byte bestaat uit 8 bits. Dit geeft 256 mogelijke combinaties, wat zou zorgen voor een tabel met 256 rijen. Om een karakter weer te geven uit de [ASCII-tabel](https://www.ascii-code.com/), wordt één byte gebruikt.

| bit 1 | bit 2 | bit 3 | bit 4 | bit 5 | bit 6 | bit 7 | bit 8 | karakter |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :------: |
|  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |   ...    |
|   0   |   1   |   0   |   0   |   0   |   0   |   0   |   1   |    A     |
|   0   |   1   |   0   |   0   |   0   |   0   |   1   |   0   |    B     |
|  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |   ...    |
|   0   |   1   |   1   |   0   |   0   |   0   |   0   |   1   |    a     |
|   0   |   1   |   1   |   0   |   0   |   0   |   1   |   0   |    b     |
|  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |   ...    |

Een kilobyte (KB) is 1024 bytes. Één kilogram is 1000 gram. Één kilocalorie is 1000 calorieën. Waarom is een kilobyte dan 1024 bytes in plaats van 1000 bytes? Dit komt omdat de capaciteit van het geheugen altijd gebaseerd is op machten van 2. 1024 is het dichtstbijzijnde getal bij 1000 wanneer machten van 2 gebruikt worden. 2<sup>10</sup> = 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 = 1024.

Een megabyte (MB) is 1024 kilobytes. Ofwel 1 048 576 bytes. Dit zijn dus 1 048 576 tabellen van 8 kolommen en 256 rijen, waarbij elke cel op 0 of 1 geplaatst kan worden om unieke toestanden te bekomen.

Een gigabyte (GB) is 1024 megabytes. Dit zijn 1 073 741 824 bytes. Een computer met 8 GB RAM (werkgeheugen) heeft dus meer dan een miljard tabellen van elk 8 kolommen en 256 rijen waarbij elke cel op 0 of 1 geplaatst kan worden om unieke toestanden te bekomen.

Een terabyte (TB) is 1024 gigabyte.

Een petabyte (PB) is ...

## Algoritmes

Een algoritme is een reeks van stappen om een bepaald doel te bereiken.

Een voorbeeld van een algoritme is een recept om een gerecht te maken. Wanneer alle ingrediënten aanwezig zijn en de stappen gevolgd worden, zal altijd hetzelfde gerecht de uitkomst zijn.

De ingrediënten en het recept zijn de invoer. Het proces is het volgen van de stappen in het recept. De uitvoer is het gerecht.

Bij een computer is een voorbeeld van een algoritme, een programma dat twee getallen bij elkaar optelt.

De getallen 3 en 7 en het programma zijn de invoer. Het proces is het volgen van de stappen in het programma (3 en 7 worden bij elkaar opgeteld). De uitvoer is het getal 10.

## Formele logica

Bij formele logica wordt gekeken of een redenering, bestaande uit beweringen en een conclusie, geldig of ongeldig is. Er wordt van uitgegaan dat de beweringen waar zijn.

Een bewering is een zin die waar of onwaar kan zijn.

- "Alle mensen zijn zoogdieren", is een bewering die feitelijk waar is.
- "Alle mensen zijn vissen", is een bewering die feitelijk onwaar is.

Let op: Bij de formele logica wordt ervan uitgegaan dat alle beweringen waar zijn, ook al zijn ze feitelijk onwaar. Dus ook 'alle mensen zijn vissen' wordt als waar gezien.

### Voorbeelden

- Bewering 1: Alle mensen zijn zoogdieren.
- Bewering 2: Ik ben een mens.
- Conclusie: Ik ben een zoogdier.

Dit is een geldige redenering.

- Bewering 1: Alle mensen zijn vissen.
- Bewering 2: Ik ben een mens.
- Conclusie: Ik ben een vis.

Dit is een geldige redenering.
De conclusie is waar, gebaseerd op **beweringen waarvan uitgegaan wordt dat ze waar zijn**.

- Bewering 1: Alle wagens die na 1991 gemaakt zijn, hebben een gordel.
- Bewering 2: Mijn wagen heeft een gordel.
- Conclusie: Mijn wagen is gemaakt na 1991.

Dit is een ongeldige redenering.
Bewering 1 sluit niet uit dat er voor 1991 ook al wagens gemaakt zijn met gordels. Het geeft alleen maar bevestiging dat alle wagens die na 1991 gemaakt zijn, een gordel hebben.

- Bewering 1: Alle wagens die na 1991 gemaakt zijn, hebben een gordel.
- Bewering 2: Mijn wagen heeft geen gordel.
- Conclusie: Mijn wagen is gemaakt voor 1991.

Dit is een geldige redenering.
Bewering 1 sluit uit dat er na 1991 nog wagens gemaakt zijn die geen gordel hebben.

- Bewering 1: Alle wagens die na 1991 gemaakt zijn, hebben een gordel.
- Bewering 2: Alle wagens die voor 1991 gemaakt zijn, hebben geen gordel.
- Bewering 3: Mijn wagen heeft een gordel.
- Conclusie: Mijn wagen is gemaakt na 1991.

Dit is een geldige redenering.
Bewering 2 sluit uit dat er voor 1991 gordels aanwezig waren in wagens.

- Bewering 1: Alle fietsen zijn blauw.
- Bewering 2: Alle motors zijn groen.
- Conclusie: Mijn wagen is rood.

Dit is een ongeldige redenering. Bewering 1 en 2 sluiten niet uit dat de wagen een andere kleur kan hebben dan rood.

## Deductieve en inductieve logica

Deductieve beweringen tonen onomstotelijk aan dat de conclusie waar is, als alle beweringen waar zijn.
Inductieve beweringen tonen aan dat een conclusie waarschijnlijk, maar niet met zekerheid, waar is.

### Deductieve voorbeelden

- Bewering 1: Alle mensen zijn zoogdieren.
- Bewering 2: Alle zoogdieren zijn dieren.
- Conclusie: Alle mensen zijn dieren.
  <br/>
  <br/>
- Bewering 1: Het eten vanavond zal wok of frieten zijn.
- Bewering 2: Het eten vanavond zijn geen frieten.
- Conclusie: Het eten vanavond is wok.
  <br/>
  <br/>
- Bewering 1: Als er water door het dak komt, komt er water door het plafond.
- Bewering 2: Als er water door het plafond komt, wordt de vloer nat.
- Conclusie: Wanneer er water door het dak komt, wordt de vloer nat.

### Inductieve voorbeelden

- Bewering 1: Het is in de geschiedenis nooit voorgekomen dat er een leerling niet geslaagd is.
- Conclusie: Het is hoogstwaarschijnlijk, maar niet zeker, dat alle leerlingen dit jaar zullen slagen.
  <br/>
  <br/>
- Bewering 1: Elke ochtend van de afgelopen tien dagen kraait de haan van de buren.
- Conclusie: Waarschijnlijk kraait de haan morgenvroeg.

#### Sterke inductieve beweringen

Opnieuw wordt er vanuit gegaan dat de beweringen waar zijn. Een sterke inductieve bewering zorgt voor een conclusie die hoogstwaarschijnlijk waar is.

- Bewering 1: Alle mensen die tot nu toe geboren zijn, zijn geboren met elf tenen.
- Conclusie: Hoogstwaarschijnlijk gaat de volgende mens die geboren wordt, ook elf tenen hebben.

Dit is een sterke inductieve bewering.

#### Zwakke inductieve beweringen

- Bewering 1: Een doos bevat honderd appels.
- Bewering 2: Drie willekeurig geselecteerde appels zijn rijp.
- Conclusie: Alle appels in de doos zijn waarschijnlijk rijp.

Dit is een zwakke inductieve bewering. Het kan een sterke inductieve bewering worden door het aantal geïnspecteerde appels te vergroten.

- Bewering 1: Een doos bevat honderd appels.
- Bewering 2: Tachtig willekeurig geselecteerde appels zijn rijp.
- Conclusie: Alle appels in de doos zijn waarschijnlijk rijp.

Dit is een sterke inductieve bewering.

Bij bovenstaande voorbeelden is het duidelijk of het een zwakke of sterke inductieve bewering is. Er zijn gevallen waarbij het minder duidelijk is, bijvoorbeeld wanneer er vijftig appels geïnspecteerd worden, maakt dat de conclusie meer of minder waarschijnlijk?

## Categoriale logica - Venn-diagram

Bij deze vorm van logica worden categorieën gevormd. Hierbij wordt gebruikgemaakt van de woorden 'alle', 'sommige' en 'geen'.

- Alle mensen zijn zoogdieren.

- Sommige mensen worden honderd jaar oud.

- Geen enkele vis is een zoogdier.

Elke categorie kan voorgesteld worden door een cirkel. Een Venn-diagram is een groep van cirkels, waarbij elke cirkel een categorie is en de overlappende delen van cirkels de relatie tussen categorieën weergeeft.

Een Venn-diagram met twee categorieën.

![Venn-diagram-2](/img/blog/venn-diagram-2.png)

- De roze categorie zijn alle honden.
- De gele categorie zijn alle huisdieren.
- Het overlappende rode gedeelte zijn alle honden die ook een huisdier zijn.

Een Venn-diagram met drie categorieën.

![Venn-diagram-3](/img/blog/venn-diagram-3.png)

- De roze categorie zijn alle honden.
- De gele categorie zijn alle huisdieren.
- De lichtblauwe cirkel zijn alle dieren met een zwarte vacht.
- Het overlappende rode gedeelte zijn alle honden die ook een huisdier zijn.
- Het overlappende blauwe gedeelte zijn alle honden met een zwarte vacht.
- Het overlappende groene gedeelte zijn alle huisdieren met een zwarte vacht.
- Het overlappende zwarte gedeelte zijn alle honden die ook een huisdier zijn en een zwarte vacht hebben.

Combinaties zijn ook mogelijk:

- Het roze met het rode gecombineerd stellen alle honden voor die ook een huisdier zijn, maar geen zwarte vacht hebben.
- Het roze met het rode en het blauwe gecombineerd bevat geen honden die zowel een huisdier zijn als een zwarte vacht hebben.
