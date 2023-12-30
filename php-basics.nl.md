---
postUuid: a0f549d5-b353-4161-baff-c04329659420
title: PHP - Basis
slug: php-basis
tags:
  - PHP
categories:
  - Backend
---

PHP is een programmeertaal die vooral gebruikt wordt voor het maken van dynamische webpagina's. PHP staat voor `PHP: Hypertext Preprocessor`, een afkorting waarbij de afkorting zelf een onderdeel is van de volledige naam (een recursief acroniem).

PHP is een geïnterpreteerde taal, wat betekent dat de code niet eerst gecompileerd hoeft te worden voordat het uitgevoerd kan worden. De code wordt door de PHP-interpreter regel voor regel uitgevoerd. Dit in tegenstelling tot gecompileerde talen zoals C, C++ en Java, waarbij de code eerst gecompileerd moet worden voordat het uitgevoerd kan worden.

PHP kan samen met HTML gebruikt worden in `.php`-bestanden. De PHP-code wordt dan tussen `<?php` en `?>` gezet. De PHP-code wordt door de PHP-interpreter uitgevoerd en de HTML-code wordt naar de browser gestuurd. De browser ziet dus alleen de HTML-code en niet de PHP-code.

Deze module is gebaseerd op de [PHP Crash Course](https://www.youtube.com/watch?v=BUCiSSyIGGU) van Traversy Media. Indien automatisch herladen gewenst is, kan je kijken naar de gelinkte video bij het hoofdstuk 'VS Code Setup & Auto Reload'. Het is ook mogelijk om na elke wijziging in de code, de pagina manueel te herladen.

We werken met XAMPP, maak in de map `htdocs` een nieuwe map aan, genaamd `php-basis`. Start de Apache-server via XAMPP en navigeer naar `http://localhost/php-basis`.

Bij het gebruik van VSCode is het handig om de [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client) extensie te installeren.

## Output

Maak een nieuw bestand aan in de map `php-basis`, genaamd `01_output.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/01_output.php`. Er zal momenteel nog niks te zien zijn.

Bezoek na elke wijziging in de code de pagina opnieuw en ververs de pagina om de wijzigingen te zien.

Om met PHP te werken in een `.php`-bestand, moet er een PHP-tag geopend worden. Dit gebeurt met `<?php` en gesloten worden met `?>`.

`01_output.php`

```php
<?php
// hier komt de PHP-code
?> // dit is optioneel
```

De sluittag is optioneel, dit kan weggelaten worden.

```php
<?php
// hier komt de PHP-code
```

### echo

Om iets te tonen in de browser, kan er gebruikgemaakt worden van `echo`.

```php
<?php
echo('John Duck'); // echo is een 'language construct', geen functie
echo 'John Duck'; // 'language construct' kan ook zonder haakjes
```

`echo` is geen functie, het is een `language construct`, dit kan met en zonder haakjes aangeroepen worden. Het is een conventie om `echo` zonder haakjes te gebruiken.

`echo` kan ook meerdere parameters meegegeven krijgen.

`'<br>'` is een HTML-tag, deze zorgt ervoor dat er een nieuwe regel getoond wordt in de browser. Dit wordt toegevoegd voor de leesbaarheid van de output in de browser.

```php
<?php
/**
 * echo
 * Print waarden naar de browser, één of meerdere waarden
 */
echo ("John Duck met haakjes");
echo '<br>'; // Print een nieuwe regel (html tag)

echo "John Duck zonder haakjes";
echo '<br>';

echo "Toon meerdere regels", "<br>"; // echo kan meerdere parameters hebben

echo "Regel 1", "<br>", "Regel 2", "<br>", "Regel 3", "<br>";

echo 1, 'twee', 3, 'vier', "<br>"; // echo kan getallen en string tonen
```

### print

`print` is een andere `language construct` om iets te tonen in de browser.

```php
<?php
/**
 * print
 * Print één waarde naar de browser
 */
print "John Duck met print";
echo '<br>';
```

Er wordt voor `echo` gekozen omdat `echo` meer parameters kan hebben dan `print`.

### print_r

```php
<?php
/**
 * print_r
 * Print een array naar de browser
 */
print_r("John Duck met print_r <br>");
print_r(['John', 'Duck', 32]); // Dit toont in de browser: Array([0] => John [1] => Duck [2] => 32)
echo '<br>';
```

Indien je dit met `echo` zou proberen, dan wordt er `Array` getoond in de browser - niet de inhoud van de array.

### var_dump

```php
<?php
/**
 * var_dump
 * Print een waarde naar de browser, met extra informatie
 */
var_dump("John Duck met var_dump"); // string(22) "John Duck met var_dump"
echo '<br>';
var_dump(['John', 'Duck', 32]); // array(3) { [0]=> string(4) "John" [1]=> string(4) "Duck" [2]=> int(32) }
echo '<br>';
```

### comments

```php
<?php
// Single line comment
/*
 * Multi line comment
 */
```

### html

Er kan ook HTML-code toegevoegd worden in een `.php`-bestand.

```php
<?php
/**
 * HTML
 */
?>
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <h1>Dit is de H1 tag</h1>
</body>

</html>
```

Er kan ook PHP-code toegevoegd worden tussen de HTML-code.

```php
<?php
/**
 * HTML
 */
?>
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <h1>Dit is de H1 tag</h1>

  <!-- PHP kan tussen de HTML toegevoegd worden -->
  <h2><?php echo 'Dit wordt toegevoegd via PHP'; ?></h2>

  <!-- Een alternatieve syntax waarmee php en echo weggelaten kunnen worden -->
  <h2><?= 'Dit wordt toegevoegd via PHP'; ?></h2>
</body>

</html>
```

## Variabelen

Maak een nieuw bestand aan in de map `php-basis`, genaamd `02_variables.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/02_variables.php`. Er zal momenteel nog niks te zien zijn.

### Data types

PHP is een dynamisch getypeerde taal, wat betekent dat de data types niet expliciet aangegeven moeten worden. De data types worden automatisch bepaald.

Het is wel nuttig om de data types te kennen, zodat je weet wat je kan verwachten.

- String - Tekst, een reeks van karakters
- Integer - Een geheel getal
- Float - Een decimaal getal, getal met een komma
- Boolean - Een waarde die `true` of `false` kan zijn
- Array - Een lijst van waarden
- Object - Een object/instantie van een class
- NULL - Een variabele zonder waarde
- Resource - Een verwijzing naar een externe bron

## Variabelen

Er zijn enkele regels waaraan een variabele moet voldoen:

- Een variabele moet beginnen met een `$`-teken
- Na de `$`-teken mag er geen cijfer staan, maar er moet wel een letter of een `_` staan
- Een variabele is hoofdlettergevoelig, `$name` is niet hetzelfde als `$Name` of `$NAME`

`=` is de toekenningsoperator, hiermee wordt een waarde toegekend aan een variabele.

```php
<?php
$name = 'John Duck'; // string (tekst) - kan met enkele of dubbele quotes
var_dump($name);
echo '<br>';

$age = 32; // integer (geheel getal)
var_dump($age);
echo '<br>';

/**
 * $hasKids is geldig als variabelenaam, maar $has_kids is de conventie binnen PHP
 */
$has_kids = false; // boolean (true of false)
var_dump($has_kids);
echo '<br>';

$height = 1.75; // float (komma getal)
var_dump($height); // float(1.75)
echo '<br>';
```

### String concatenation

```php
<?php
/**
 * String concatenation
 */
$name = 'John Duck';
$age = 32;
echo 'Mijn naam is ' . $name . ' en ik ben ' . $age . ' jaar oud.';
echo '<br>';
```

### String interpolation

```php
<?php
/**
 * String interpolation
 */
$name = 'John Duck';
$age = 32;
echo "Mijn naam is $name en ik ben $age jaar oud."; // variabelen worden geëvalueerd
echo "Mijn naam is {$name} en ik ben {$age} jaar oud."; // variabelen worden geëvalueerd en staan expliciet tussen {}
echo '<br>';
```

### Berekeningen

```php
<?php
echo 5 + 5; // 10 - som
echo '<br>';
echo '5' + 5; // 10 - som (in PHP wordt '5' geconverteerd naar 5, in JavaScript is string + number een string -> '55')
echo '<br>';
echo 10 - 5; // 5 - verschil
echo '<br>';
echo 10 * 5; // 50 - product
echo '<br>';
echo 10 / 5; // 2 - quotient
echo '<br>';
echo 10 % 2; // 0 - modulo (restdeling)
echo '<br>';
echo 11 % 2; // 1 - modulo (restdeling)
echo '<br>';
echo 10 ** 2; // 100 - exponent
echo '<br>';
```

### Constanten

```php
/**
 * Constanten
 */
define('PI', 3.141592653); // defineer een constante
echo PI; // 3.141592653
echo '<br>';
```

## Arrays

Maak een nieuw bestand aan in de map `php-basis`, genaamd `03_arrays.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/03_arrays.php`. Er zal momenteel nog niks te zien zijn.

Een array is een lijst van waarden. Een array kan meerdere waarden bevatten van verschillende data types.

### Simpele array

```php
<?php
// Array van getallen
$numbers = [1, 2, 3, 4, 5];
$numbersAlternative = array(1, 2, 3, 4, 5);

// Array van strings
$names = ['Kwik', 'Kwek', 'Kwak'];
$namesAlternative = array('Kwik', 'Kwek', 'Kwak');

// Array van mixed types
$things = ['Kwik', 2, true, 3.141592653];
$thingsAlternative = array('Kwik', 2, true, 3.141592653);
```

Een array kan aangemaakt worden met `[]` of met `array()`.
Een array kan meerdere data types bevatten.

### Printen van een array

```php
<?php
$numbers = [1, 2, 3, 4, 5];
print_r($numbers); // Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )
echo '<br>';
var_dump($numbers); // array(5) { [0]=> int(1) [1]=> int(2) [2]=> int(3) [3]=> int(4) [4]=> int(5) }
echo '<br>';
```

### Waarde uit een array halen

```php
<?php
$numbers = [1, 2, 3, 4, 5];
$names = ['Kwik', 'Kwek', 'Kwak'];
$things = ['Kwik', 2, true, 3.141592653];

// Eerste element
var_dump($numbers[0]); // int(1)
echo '<br>';
var_dump($names[0]); // string(4) "Kwik"
echo '<br>';

// Derde element
var_dump($numbers[2]); // int(3)
echo '<br>';
var_dump($names[2]); // string(4) "Kwak"
echo '<br>';
var_dump($things[2]); // bool(true)
echo '<br>';
```

### Associative array

Een associative array is een array waarbij de index een zelf te bepalen string is. Dit is handig om een array te maken met key-value pairs (de index (key) en de waarde (value) vormen samen een paar).

```php
<?php
$names = ['Kwik', 'Kwek', 'Kwak']; // numerieke indexen vanaf 0
var_dump($names[1]); // string(4) "Kwek"
echo '<br>';

$names = [
  0 => 'Kwik',
  1 => 'Kwek',
  2 => 'Kwak'
]; // numerieke indexen vanaf 0 (hetzelfde als hierboven)
var_dump($names[1]); // string(4) "Kwek"
echo '<br>';

$names = [
  10 => 'Kwik',
  20 => 'Kwek',
  30 => 'Kwak'
]; // zelf gekozen numerieke indexen
var_dump($names[20]); // string(4) "Kwek"
echo '<br>';

$names = [
  'first' => 'Kwik',
  'second' => 'Kwek',
  'third' => 'Kwak'
]; // associatieve indexen (key-value pairs)
var_dump($names['second']); // string(4) "Kwek"
echo '<br>';

$colors = [
  'red' => '#ff0000',
  'green' => '#00ff00',
  'blue' => '#0000ff'
]; // associatieve indexen (key-value pairs)
var_dump($colors['green']); // string(7) "#00ff00"
echo '<br>';
```

### Multidimensionele array

Een multidimensionele array is een array waarbij de waarde van een index een array is. Dit kan oneindig diep gaan. Dit is te vergelijken met een JSON-object.

Stel dat we deze JSON hebben:

```json
{
  "name": "John Duck",
  "age": 32,
  "nephews": [
    {
      "name": "Kwik",
      "age": 16,
      "hobbies": ["Gamen", "Voetbal"]
    },
    {
      "name": "Kwek",
      "age": 16,
      "hobbies": ["Gamen"]
    },
    {
      "name": "Kwak",
      "age": 16,
      "hobbies": []
    }
  ]
}
```

Dit kan in PHP als volgt geschreven worden:

```php
<?php
$data = [
  "name" => "John Duck",
  "age" => 32,
  "nephews" => [
    [
      "name" => "Kwik",
      "age" => 16,
      "hobbies" => ["Gamen", "Voetbal"]
    ],
    [
      "name" => "Kwek",
      "age" => 16,
      "hobbies" => ["Gamen"]
    ],
    [
      "name" => "Kwak",
      "age" => 16,
      "hobbies" => []
    ]
  ]
];
```

Of met de `array()`-syntax:

```php
$data = array(
  "name" => "John Duck",
  "age" => 32,
  "nephews" => array(
    array(
      "name" => "Kwik",
      "age" => 16,
      "hobbies" => array("Gamen", "Voetbal")
    ),
    array(
      "name" => "Kwek",
      "age" => 16,
      "hobbies" => array("Gamen")
    ),
    array(
      "name" => "Kwak",
      "age" => 16,
      "hobbies" => array()
    )
  )
);
```

Stel dat we de eerste hobby van Kwik willen tonen, dan kan dit als volgt:

```php
<?php
$data = [
  "name" => "John Duck",
  "age" => 32,
  "nephews" => [
    [
      "name" => "Kwik",
      "age" => 16,
      "hobbies" => ["Gamen", "Voetbal"]
    ],
    [
      "name" => "Kwek",
      "age" => 16,
      "hobbies" => ["Gamen"]
    ],
    [
      "name" => "Kwak",
      "age" => 16,
      "hobbies" => []
    ]
  ]
];

// $data is een associative array, dus we kunnen de index 'nephews' gebruiken om de array met neefjes op te halen
// $data['nephews'] is een numerieke array, dus we kunnen de index 0 gebruiken om het eerste neefje op te halen
// $data['nephews'][0] is een associative array, dus we kunnen de index 'hobbies' gebruiken om de array met hobbies op te halen
// $data['nephews'][0]['hobbies'] is een numerieke array, dus we kunnen de index 0 gebruiken om de eerste hobby op te halen
var_dump($data['nephews'][0]['hobbies'][0]); // string(5) "Gamen"
echo '<br>';
```

### JSON naar PHP

Stel dat we deze JSON hebben:

```json
{
  "name": "John Duck",
  "age": 32,
  "nephews": [
    {
      "name": "Kwik",
      "age": 16,
      "hobbies": ["Gamen", "Voetbal"]
    },
    {
      "name": "Kwek",
      "age": 16,
      "hobbies": ["Gamen"]
    },
    {
      "name": "Kwak",
      "age": 16,
      "hobbies": []
    }
  ]
}
```

Stel dat we de JSON hebben opgehaald en deze in stringformaat hebben. Dan kunnen we deze string omzetten naar een PHP-array met `json_decode`.

```php
<?php
$jsonData = '{"name":"John Duck","age":32,"nephews":[{"name":"Kwik","age":16,"hobbies":["Gamen","Voetbal"]},{"name":"Kwek","age":16,"hobbies":["Gamen"]},{"name":"Kwak","age":16,"hobbies":[]}]}';

$parsedData = json_decode($jsonData, true); // true zorgt ervoor dat de data als associative array wordt teruggegeven
var_dump($parsedData);
echo '<br>';
```

Dit is een veelgebruikte manier om data die opgehaald wordt van een API om te zetten naar een PHP-array om er vervolgens mee te werken.

## Conditionele statements

Maak een nieuw bestand aan in de map `php-basis`, genaamd `04_conditionals.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/04_conditionals.php`. Er zal momenteel nog niks te zien zijn.

Er zijn verschillende operators om te vergelijken:

- `==` - gelijk aan
- `===` - gelijk aan en van hetzelfde type
- `!=` - niet gelijk aan
- `!==` - niet gelijk aan of niet van hetzelfde type
- `>` - groter dan
- `<` - kleiner dan
- `>=` - groter dan of gelijk aan
- `<=` - kleiner dan of gelijk aan

### If statement

```php
<?php

if (true) {
  echo 'Dit wordt getoond in de browser.';
  echo '<br>';
}

if (false) {
  echo 'Dit wordt niet getoond in de browser.';
  echo '<br>';
}
```

Dit heeft weinig nut als we de waarde van de conditie hardcoden. We kunnen een variabele gebruiken om de conditie te bepalen.

```php
<?php

$age = 32;

if ($age >= 18) {
  echo 'Je bent oud genoeg om alcohol te drinken.';
  echo '<br>';
}

if ($age < 18) {
  echo 'Je bent te jong om alcohol te drinken.';
  echo '<br>';
}
```

Wanneer de waarde van de variabele `$age` verandert, dan zal de output ook veranderen. Wijzig de waarde van de variabele `$age` naar `16` en herlaad de pagina.

Beeld je nu in dat de waarde van de variabele `$age` niet hardcoded is, maar dat dit ingevuld wordt door een gebruiker. Afhankelijk van de input van de gebruiker, zal de output veranderen.

Momenteel staan er twee `if` statements onder elkaar. Dit kan ook in één `if-else` statement.

```php
<?php

$age = 32;

if ($age >= 18) {
  echo 'Je bent oud genoeg om alcohol te drinken.';
  echo '<br>';
} else {
  echo 'Je bent te jong om alcohol te drinken.';
  echo '<br>';
}
```

Wanneer de leeftijd niet groter of gelijk is aan 18, dan zal de `else`-statement uitgevoerd worden. In andere woorden, wanneer de leeftijd kleiner is dan 18, dan zal de `else`-statement uitgevoerd worden.

Een alternatieve syntax voor een `if-else`-statement is de ternary operator. Dit is een verkorte versie van een `if-else`-statement. Bij simpelere statements kan dit handig zijn omdat het minder code is en toch leesbaar blijft.

```php
<?php

$age = 32;

echo $age >= 100 ? 'Je bent een eeuweling.' : 'Je bent geen eeuweling.';
echo '<br>';
```

Het is ook mogelijk om meerdere `if`-statements te combineren met `else if`. Dit kan handig zijn wanneer er meerdere condities zijn, bijvoorbeeld bij het bepalen van het moment van de dag.

```php
<?php

$hour = date('H'); // H = 24-uurs formaat van een uur (00 tot en met 23)

if ($hour < 12) {
  echo 'Goeiemorgen!';
} elseif ($hour < 17) {
  echo 'Goeiemiddag!';
} else {
  echo 'Goeienavond!';
}

echo '<br>';
```

Afhankelijk van het moment van de dag, zal er een andere begroeting getoond worden.

In een applicatie komt het vaak voor dat je bepaalde informatie aan de gebruiker wilt tonen indien er geen data is. Bijvoorbeeld wanneer een blog nog geen reacties heeft, dan wil je de gebruiker laten weten dat er nog geen reacties zijn.

```php
<?php

$reactions = [];

if (empty($reactions)) {
  echo 'Er zijn nog geen reacties geplaatst.';
} else {
  echo 'Er zijn al ' . count($reactions) . ' reacties geplaatst.';
}

echo '<br>';
```

`empty()` evalueert naar `true` wanneer de variabele leeg is. In dit geval is de variabele `$reactions` een array zonder waarden, dus zal `empty()` `true` evalueren.

```php
<?php

$reactions = ['Echt cool!', 'Vet strak!', 'Andere hippe woorden'];

if (empty($reactions)) {
  echo 'Er zijn nog geen reacties geplaatst.';
} else {
  echo 'Er zijn al ' . count($reactions) . ' reacties geplaatst.';
}

echo '<br>';
```

`empty()` evalueert naar `false` wanneer de variabele niet leeg is. In dit geval is de variabele `$reactions` een array met waarden, dus zal `empty()` `false` evalueren.

`count()` telt het aantal waarden in een array.

### Switch statement

```php
<?php

$color = 'red';

switch ($color) {
  case 'red':
    echo 'De kleur is rood.';
    break;
  case 'green':
    echo 'De kleur is groen.';
    break;
  case 'blue':
    echo 'De kleur is blauw.';
    break;
  default:
    echo 'De kleur is onbekend.';
    break;
}

echo '<br>';
```

Wijzig de `$color` variabele naar `green` en herlaad de pagina.
Wijzig de `$color` variabele naar `blue` en herlaad de pagina.
Wijzig de `$color` variabele naar `yellow` en herlaad de pagina.

Het is ook mogelijk om meerdere cases te combineren.

```php
<?php

$color = 'red';

switch ($color) {
  case 'red':
  case 'green':
  case 'blue':
    echo 'De kleur is rood, groen of blauw.';
    break;
  default:
    echo 'De kleur is onbekend.';
    break;
}

echo '<br>';
```

Wijzig de `$color` variabele naar `green` en herlaad de pagina.
Wijzig de `$color` variabele naar `blue` en herlaad de pagina.
Wijzig de `$color` variabele naar `yellow` en herlaad de pagina.

## Iteratieve statements

Maak een nieuw bestand aan in de map `php-basis`, genaamd `05_loops.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/05_loops.php`. Er zal momenteel nog niks te zien zijn.

### for

```php
<?php

/**
 * For loop
 */

for ($i = 0; $i < 5; $i++) {
  echo $i;
  echo '<br>';
}

echo '<br>';

/**
 * While loop
 */

$i = 0;

while ($i < 5) {
  echo $i;
  echo '<br>';
  $i++;
}

echo '<br>';

/**
 * Do while loop
 */

$i = 6;

do {
  echo $i;
  echo '<br>';
  $i++;
} while ($i < 5);
```

Merk op dat de `do-while` loop minstens één keer uitgevoerd wordt, ook al is de conditie `false`.

## Functies

Maak een nieuw bestand aan in de map `php-basis`, genaamd `06_functions.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/06_functions.php`. Er zal momenteel nog niks te zien zijn.

Functies zijn een manier om code te hergebruiken. Functies kunnen parameters meekrijgen en kunnen een waarde teruggeven.

### Functie zonder parameters

```php
<?php

function register()
{
  echo 'Registratie gelukt!';
}

register(); // Functie aanroepen - het codeblok van de functie wordt uitgevoerd
echo '<br>';
```

### Functie met parameters

```php
<?php

function registerUser($username)
{
  // $username is een lokale variabele binnen de scope van de functie
  echo "Registratie gelukt! Welkom, $username.";
}

registerUser('John'); // Registratie gelukt! Welkom, John.
echo '<br>';
registerUser('Janine'); // Registratie gelukt! Welkom, Janine.
echo '<br>';
echo var_dump($username); // NULL - $username bestaat niet in de globale scope
echo '<br>';
```

### Functie met return statement

```php
$existingUsers = ['John', 'Janine'];

function registerNewUser($username)
{
  global $existingUsers; // global keyword om toegang te krijgen tot de globale variabele $existingUsers
  if (in_array($username, $existingUsers)) {
    return 'Gebruikersnaam bestaat al.';
  } else {
    $existingUsers[] = $username; // Voeg de nieuwe gebruiker toe aan de array met bestaande gebruikers
    return 'Registratie gelukt! Welkom, ' . $username . '.';
  }
}

echo registerNewUser('John'); // Gebruikersnaam bestaat al.
echo '<br>';
echo registerNewUser('Janine'); // Gebruikersnaam bestaat al.
echo '<br>';
echo registerNewUser('Laika'); // Registratie gelukt! Welkom, Laika.
echo '<br>';
```

`global` is een keyword om toegang te krijgen tot een globale variabele binnen de scope van een functie. In dit geval is de globale variabele `$existingUsers` nodig om te controleren of de gebruikersnaam al bestaat.

`in_array()` controleert of een waarde voorkomt in een array.

Merk ook op dat de waarde teruggegeven wordt met `return`. Als we het resultaat van de functie willen tonen in de browser, dan moeten we de functie aanroepen in een `echo` statement.

### Functie met default parameter

```php
<?php

$existingUsers = ['John', 'Janine'];

function addUser($username, $existingMessage = 'Gebruikersnaam bestaat al.')
{
  global $existingUsers;
  if (in_array($username, $existingUsers)) {
    return $existingMessage;
  } else {
    $users[] = $username;
    return 'Registratie gelukt! Welkom, ' . $username . '.';
  }
}

echo addUser('John'); // Gebruikersnaam bestaat al.
echo '<br>';
echo addUser('Janine', "Mee joh, dees is al bezet!"); // Mee joh, dees is al bezet!
echo '<br>';
echo addUser('Kwik'); // Gebruikersnaam bestaat al.
echo '<br>';
```

De `$existingMessage` parameter heeft een default waarde van `'Gebruikersnaam bestaat al.'`. Wanneer er geen waarde meegegeven wordt voor deze parameter, dan zal de default waarde gebruikt worden.

### Alternatieve syntax

```php
<?php

/**
 * Anonieme functie toekennen aan een variabele
 */
$newUser = function ($username) {
  return 'Registratie gelukt! Welkom, ' . $username . '.';
};

echo $newUser('John'); // Registratie gelukt! Welkom, John.
echo '<br>';

/**
 * Anonieme arrow functie toekennen aan een variabele
 */
$addNewUser = fn ($username) => 'Registratie gelukt! Welkom, ' . $username . '.';

echo $addNewUser('John'); // Registratie gelukt! Welkom, John.
echo '<br>';
```

## Array functies

```php
<?php
/**
 * Er zijn veel functies beschikbaar om met arrays te werken
 * https://www.php.net/manual/en/ref.array.php
 *
 * We gaan er een aantal bekijken
 */

$fruits = ['apple', 'banana', 'orange'];

/**
 * count()
 * Geeft het aantal elementen in een array terug
 */
var_dump(count($fruits)); // int(3)
echo '<br>';

/**
 * in_array($element, $array)
 * Controleert of een element in een array voorkomt
 */
var_dump(in_array('apple', $fruits)); // bool(true)
echo '<br>';
var_dump(in_array('potato', $fruits)); // bool(false)
echo '<br>';

/**
 * Elementen aan een array toevoegen
 */
$fruits[] = 'pear'; // Voegt 'pear' toe aan de array $fruits
array_push($fruits, 'pineapple'); // Voegt 'pineapple' toe aan de array $fruits
array_unshift($fruits, 'strawberry'); // Voegt 'strawberry' toe aan het begin van de array $fruits

print_r($fruits); // Array ( [0] => strawberry [1] => apple [2] => banana [3] => orange [4] => pear [5] => pineapple )
echo '<br>';

/**
 * Elementen verwijderen uit een array
 */
array_pop($fruits); // Verwijdert het laatste element uit de array $fruits
array_shift($fruits); // Verwijdert het eerste element uit de array $fruits

print_r($fruits); // Array ( [0] => apple [1] => banana [2] => orange [3] => pear )
echo '<br>';

unset($fruits[1]); // Verwijdert het element met index 1 uit de array $fruits
// Merk op dat de indexen niet opnieuw worden geordend, 0, 2 en 3 blijven behouden - 1 wordt verwijderd
print_r($fruits); // Array ( [0] => apple [2] => orange [3] => pear )
echo '<br>';

$chunked_array = array_chunk($fruits, 2); // De array $fruits wordt opgedeeld in arrays van 2 elementen
print_r($chunked_array); // Array ( [0] => Array ( [0] => apple [1] => orange ) [1] => Array ( [0] => pear ) )
echo '<br>';

/**
 * Arrays samenvoegen
 */

$first_numbers = [1, 2, 3];
$second_numbers = [4, 5, 6];
$combined_numbers = array_merge($first_numbers, $second_numbers);
print_r($combined_numbers); // Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 [5] => 6 )
echo '<br>';

$alternative_combined_numbers = [...$first_numbers, ...$second_numbers]; // Spread operator
print_r($alternative_combined_numbers); // Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 [5] => 6 )
echo '<br>';

/**
 * Arrays key-value pairs maken
 */
$color_names = ['red', 'green', 'blue'];
$color_hex_codes = ['#ff0000', '#00ff00', '#0000ff'];
$colors = array_combine($color_names, $color_hex_codes);
print_r($colors); // Array ( [red] => #ff0000 [green] => #00ff00 [blue] => #0000ff )
echo '<br>';

/**
 * Arrays sorteren
 */
$numbers = [5, 3, 1, 4, 2];
sort($numbers); // Sorteert de array $numbers van laag naar hoog - wijzigt de originele array
print_r($numbers); // Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )
echo '<br>';

/**
 * Wijzig elk element van een array
 */
$numbersDoubled = array_map(function ($number) {
  return $number * 2;
}, $numbers);

$numbersDoubledArrow = array_map(fn ($number) => $number * 2, $numbers);

print_r($numbersDoubled); // Array ( [0] => 2 [1] => 4 [2] => 6 [3] => 8 [4] => 10 )
echo '<br>';
print_r($numbersDoubledArrow); // Array ( [0] => 2 [1] => 4 [2] => 6 [3] => 8 [4] => 10 )
echo '<br>';

/**
 * Filter een array
 */
$lessThan10 = array_filter($numbersDoubled, function ($number) {
  return $number < 10;
});

$lessThan10Arrow = array_filter($numbersDoubled, fn ($number) => $number < 10);

print_r($lessThan10); // Array ( [0] => 2 [1] => 4 [2] => 6 [3] => 8 )
echo '<br>';
print_r($lessThan10Arrow); // Array ( [0] => 2 [1] => 4 [2] => 6 [3] => 8 )
echo '<br>';
```

Merk op dat soms een array als eerste parameter wordt meegegeven en soms als laatste parameter, dit is afhankelijk van de functie.

Merk ook op dat sommige functies de originele array wijzigen en sommige functies een nieuwe array teruggeven.

## String functies

```php
<?php
/**
 * Er zijn veel functies beschikbaar om met strings te werken
 * https://www.php.net/manual/en/ref.strings.php
 *
 * We gaan er een aantal bekijken
 */

$string = 'Hello World';

// lengte van een string
echo strlen($string); // 11
echo '<br>';

// vind de eerste match voor een positie van een substring in een string
echo strpos($string, 'World'); // 6
echo '<br>';
echo strpos($string, 'o'); // 4
echo '<br>';

// vind de laatste match voor een positie van een substring in een string
echo strrpos($string, 'o'); // 7
echo '<br>';

// draai een string om
echo strrev($string); // dlroW olleH
echo '<br>';

// converteer alle karakters naar kleine letters
echo strtolower($string); // hello world
echo '<br>';

// converteer alle karakters naar hoofdletters
echo strtoupper($string); // HELLO WORLD

// converteer elke eerste letter van een woord naar een hoofdletter
echo ucwords('hello world'); // Hello World
echo '<br>';

// vervang een deel van een string door een andere string
echo str_replace('World', 'Everyone', $string); // Hello Everyone

// geef een deel van een string terug
echo substr($string, 6, 3); // Wor
echo '<br>';
echo substr($string, 6); // World
echo '<br>';
echo substr($string, -5); // World
echo '<br>';

// kijk of een string begint met een bepaalde substring
var_dump(str_starts_with($string, 'Hello')); // bool(true)
echo '<br>';
var_dump(str_starts_with($string, 'hello')); // bool(false)
echo '<br>';

// kijk of een string eindigt met een bepaalde substring
var_dump(str_ends_with($string, 'World')); // bool(true)
echo '<br>';
var_dump(str_ends_with($string, 'world')); // bool(false)
echo '<br>';

// html entities
$htmlString = '<h1>Hello World</h1>';
// &lt;h1&gt;Hello World&lt;/h1&gt; - de html tags worden geconverteerd naar html entities
// dit zorgt ervoor dat de browser de html tags niet zal interpreteren
// het zal de html tags tonen als tekst in de browser: <h1>Hello World</h1>
echo htmlentities($htmlString);
echo '<br>';

// string formatting
$person = 'John';
$age = 32;

// %s is een placeholder voor een string ($person)
// %d is een placeholder voor een integer ($age)
printf('Hello %s, you are %d years old.', $person, $age); // Hello John, you are 32 years old.
echo '<br>';
```

## Super globals

Maak een nieuw bestand aan in de map `php-basis`, genaamd `07_superglobals.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/07_superglobals.php`. Er zal momenteel nog niks te zien zijn.

Super globals zijn variabelen die altijd beschikbaar zijn in elke scope. Dit zijn variabelen die automatisch aangemaakt worden door PHP.

- `$GLOBALS` - verwijst naar alle globale variabelen
- `$_SERVER` - verwijst naar informatie over de server en de uitvoeromgeving
- `$_GET` - verwijst naar alle variabelen die via de URL worden meegegeven
- `$_POST` - verwijst naar alle variabelen die via een formulier worden meegegeven
- `$_FILES` - verwijst naar alle bestanden die via een formulier worden meegegeven
- `$_COOKIE` - verwijst naar alle cookies
- `$_SESSION` - verwijst naar alle sessievariabelen
- `$_REQUEST` - verwijst naar alle variabelen die via de URL of via een formulier worden meegegeven
- `$_ENV` - verwijst naar alle omgevingsvariabelen

```php
<?php

var_dump($GLOBALS); // Super global - verwijst naar alle globale variabelen, dit bevat bijvoorbeeld _COOKIE indien er cookies zijn ingesteld
echo '<br>';
```

```php
<?php

var_dump($_REQUEST); // bezoek http://localhost/php-basis/09_superglobals.php?name=John&age=32
// array(2) { ["name"]=> string(4) "John" ["age"]=> string(2) "32" }
echo '<br>';
```

```php
<?php
$title = 'Super Globals';
?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><?php echo $title ?></title>
</head>

<body>
  <ul>
    <li>Host: <?php echo $_SERVER['HTTP_HOST']; ?></li>
    <li>Document Root: <?php echo $_SERVER['DOCUMENT_ROOT']; ?></li>
    <li>System Root: <?php echo $_SERVER['SystemRoot']; ?></li>
    <li>Server Name: <?php echo $_SERVER['SERVER_NAME']; ?></li>
    <li>Server Port: <?php echo $_SERVER['SERVER_PORT']; ?></li>
    <li>Current File Dir: <?php echo $_SERVER['PHP_SELF']; ?></li>
    <li>Request URI: <?php echo $_SERVER['REQUEST_URI']; ?></li>
    <li>Server Software: <?php echo $_SERVER['SERVER_SOFTWARE']; ?></li>
    <li>Client Info: <?php echo $_SERVER['HTTP_USER_AGENT']; ?></li>
    <li>Remote Address: <?php echo $_SERVER['REMOTE_ADDR']; ?></li>
    <li>Remote Port: <?php echo $_SERVER['REMOTE_PORT']; ?></li>
  </ul>
</body>

</html>
```

## \_GET en \_POST

Maak een nieuw bestand aan in de map `php-basis`, genaamd `10_get_post.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/10_get_post.php`. Er zal momenteel nog niks te zien zijn.

### \_GET

We kunnen gegevens doorgeven van de client naar de server met behulp van `query parameters` in de URL.

```php
<?php

if ($_GET['username']) {
  echo 'Dit is de username die binnenkomt via de query parameter: ' . $_GET['username'];
  echo '<br>';
}
```

Bezoek manueel de URL `http://localhost/php-basis/10_get_post.php?username=John` en je zal zien dat de username getoond wordt in de browser.

Bezoek opnieuw manueel de URL `http://localhost/php-basis/10_get_post.php?username=Janine` en je zal zien dat de username getoond wordt in de browser.

Bezoek de url zonder query parameter `http://localhost/php-basis/10_get_post.php` en je zal niks uitgeprint zien.

We kunnen ook via een HTML-pagina linken naar een PHP-pagina met een query parameter.

```php
<?php

if ($_GET['username']) {
  echo 'Dit is de username die binnenkomt via de query parameter: ' . $_GET['username'];
  echo '<br>';
}

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <a href="<?php echo $_SERVER['PHP_SELF'] ?>?username=John">Query Parameter</a>
</body>

</html>
```

Hier zien we exact hetzelfde als voorheen, maar nu wordt de link gegenereerd met PHP.
`$_SERVER['PHP_SELF']` verwijst naar de huidige pagina.

Wanneer we op de link klikken, zal er genavigeerd worden naar `http://localhost/php-basis/10_get_post.php?username=John` en zal de username getoond worden in de browser.

De if statement verplaatsen we naar de HTML-pagina.

```php
<?php

$username = $_GET['username'];

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php if ($username) : ?>
    <h1>Hallo <?php echo $username ?></h1>
    <p>De username komt uit de url.</p>
  <?php else : ?>
    <a href="<?php echo $_SERVER['PHP_SELF'] ?>?username=John">Query Parameter</a>
  <?php endif; ?>
</body>

</html>
```

Nu wordt de data uit de query parameter in een variabele gestoken. In de HTML gebruiken we een PHP if-statement om te controleren of de variabele een waarde heeft. Indien de variabele een waarde heeft, dan tonen we een welkomstbericht. Indien de variabele geen waarde heeft, dan tonen we een link om de query parameter mee te geven.

### \_POST

We kunnen data doorgeven van de client naar de server door middel van een formulier.

```php
<?php

$usernamePost = $_POST['username'];

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php if ($usernamePost) : ?>
    <h1>Hallo <?php echo $usernamePost ?></h1>
    <p>De username komt uit het formulier.</p>
  <?php else : ?>
    <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
      <div>
        <label>Username: </label>
        <input type="text" name="username">
      </div>
      <br>
      <input type="submit" name="submit" value="Submit">
    </form>
  <?php endif; ?>
</body>

</html>
```

Merk op dat de actie verwijst naar de huidige pagina en dat de methode `POST` is.
Merk op dat de input een `name` attribuut heeft. Dit is de naam van de key die terug te vinden zal zijn in de `$_POST` super global.

Wanneer we de pagina bezoeken, dan zien we het formulier. Wanneer we het formulier invullen en op de submit knop klikken, dan zal de data verstuurd worden naar de server. De data zal beschikbaar zijn in de `$_POST` super global.

Als de username ingevuld is, dan tonen we een welkomstbericht. Als de username niet ingevuld is, dan tonen we het formulier.

### \_REQUEST

We kunnen data doorgeven van de client naar de server door middel van een formulier of door middel van een query parameter in de URL, beide methodes kunnen opgevangen worden met de `$_REQUEST` super global.

```php
<?php

$username = $_GET['username'];
$usernamePost = $_POST['username'];

$usernameRequest = $_REQUEST['username'];
?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php if ($usernameRequest) : ?>
    <h2>REQUEST: <?php echo $usernameRequest ?></h2>
    <p>Deze variabele komt ofwel uit de url ofwel uit het formulier.</p>
  <?php else : ?>
    GET <br>
    <a href="<?php echo $_SERVER['PHP_SELF'] ?>?username=John">Query Parameter</a>
    <br>
    <br>
    POST
    <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
      <div>
        <label>Username: </label>
        <input type="text" name="username">
      </div>
      <br>
      <input type="submit" name="submit" value="Submit">
    </form>
  <?php endif; ?>
</body>

</html>
```

### Combinatie

```php
<?php

$username = $_GET['username'];
$usernamePost = $_POST['username'];

$usernameRequest = $_REQUEST['username'];
?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php if ($usernameRequest) : ?>
    <h2>REQUEST: <?php echo $usernameRequest ?></h2>
    <p>Deze variabele komt ofwel uit de url ofwel uit het formulier.</p>
  <?php endif; ?>

  <?php if ($username) : ?>
    <h2>GET: <?php echo $username ?></h2>
    <p>De username komt uit de url.</p>
  <?php else : ?>
    <a href="<?php echo $_SERVER['PHP_SELF'] ?>?username=John">Query Parameter</a>
  <?php endif; ?>

  <?php if ($usernamePost) : ?>
    <h2>POST: <?php echo $usernamePost ?></h2>
    <p>De username komt uit het formulier.</p>
  <?php else : ?>
    <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
      <div>
        <label>Username: </label>
        <input type="text" name="username">
      </div>
      <br>
      <input type="submit" name="submit" value="Submit">
    </form>
  <?php endif; ?>

</body>

</html>
```

## Sanitization

Maak een nieuw bestand aan in de map `php-basis`, genaamd `11_sanitizing_inputs`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/11_sanitizing_inputs.php`. Er zal momenteel nog niks te zien zijn.

```php
<?php

$username = $_REQUEST['username'];

echo $username; // de input van de gebruiker wordt getoond én uitgevoerd

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>

  <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
    <div>
      <label>Username: </label>
      <input type="text" name="username">
    </div>
    <br>
    <input type="submit" name="submit" value="Submit">
  </form>

</body>

</html>
```

We verwachten dat de gebruiker een string zal invullen in het invoerveld. Maar stel dat de gebruiker dit niet doet, in plaats van een string, wordt er JavaScript meegegeven in een script tag.

![XSS](/img/blog/php/php-xss.png)

Probleem! De JavaScript code wordt toegevoegd aan de pagina en deze wordt uitgevoerd in de browser. Dit is een voorbeeld van een XSS (Cross-Site Scripting) aanval.

Stel dat de input onderdeel is van een query die uitgevoerd wordt op een database, dan kan de gebruiker de query manipuleren en zo de database aanpassen. Dit is een voorbeeld van een SQL-injectie aanval.

Om dit te voorkomen, moeten we de input van de gebruiker sanitizen. Dit wil zeggen dat we de input van de gebruiker gaan 'zuiveren'.

Dit kunnen we doen met de functie `htmlspecialchars()`.

```php
<?php

$username = htmlspecialchars($_REQUEST['username']);

echo $username;

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>

  <form action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']); ?>" method="POST">
    <div>
      <label>Username: </label>
      <input type="text" name="username">
    </div>
    <br>
    <input type="submit" name="submit" value="Submit">
  </form>

</body>

</html>
```

Merk op dat we dit zowel doen bij het ophalen van de data uit de `$_REQUEST` super global als bij het tonen van de `$_SERVER['PHP_SELF']` super global.

Wanneer er nu JavaScript wordt meegegeven in de input, dan zal dit niet uitgevoerd worden in de browser. In plaats daarvan zal de JavaScript code getoond worden als tekst.

![XSS fix](/img/blog/php/php-xss-fix.png)

Voor de input is er ook nog een andere functie beschikbaar, namelijk `filter_input()`.

```php
<?php

$username = filter_input(INPUT_POST, 'username', FILTER_SANITIZE_FULL_SPECIAL_CHARS);
$email = filter_input(INPUT_POST, 'email', FILTER_SANITIZE_EMAIL);

echo $username;
echo '<br>';
echo $email;
echo '<br>';

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>

  <form action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']); ?>" method="POST">
    <div>
      <label>Username: </label>
      <input type="text" name="username">
    </div>
    <div>
      <label>Email: </label>
      <input type="email" name="email">
    </div>
    <br>
    <input type="submit" name="submit" value="Submit">
  </form>

</body>

</html>
```

## Cookies

Maak een nieuw bestand aan in de map `php-basis`, genaamd `12_cookies.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/12_cookies.php`. Er zal momenteel nog niks te zien zijn.

Cookies zijn een manier om data op te slaan in de browser van de gebruiker. Deze data kan dan op een later moment terug opgehaald worden wanneer de gebruiker de website opnieuw bezoekt.

Doordat cookies opgeslagen worden in de browser van de gebruiker, mogen we geen gevoelige data opslaan in cookies.

```php
<?php

setcookie('username', 'John', time() + 10);
// time() is het huidige tijdstip in seconden sinds 1 januari 1970
// time() + 10 is dus 10 seconden vanaf nu
// De cookie vervalt dus na 10 seconden
// In de praktijk wordt de cookie langer bewaard, bijvoorbeeld 30 dagen

if (isset($_COOKIE['username'])) {
  echo $_COOKIE['username'];
} else {
  echo 'Cookie not set.';
}
```

Bezoek de pagina en je zal zien dat `Cookie not set.` getoond wordt. Ververs de pagina en je zal zien dat `John` getoond wordt. Wacht 10 seconden en ververs de pagina en je zal zien dat `Cookie not set.` getoond wordt.

```php
<?php

setcookie('username', 'John', time() + 10);

$username = $_COOKIE['username'];

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>

  <?php if ($username) : ?>
    <h2>Welkom terug <?php echo $username ?></h2>
    <p>De username komt uit een cookie.</p>
  <?php else : ?>
    <h2>Welkom gast</h2>
    <p>Er is geen (geldige) cookie aanwezig.</p>
  <?php endif; ?>

</body>

</html>
```

Als de cookie niet aanwezig is (of als de cookie aanwezig is, maar vervallen), dan tonen we een welkomstbericht voor een gast. Als de cookie wel aanwezig is, dan tonen we een welkomstbericht voor de gebruiker.

Beeld je in dat in de praktijk een token wordt opgeslagen in een cookie die de ingelogde gebruiker identificeert, dit wordt gedaan nadat de gebruiker inloggegevens heeft ingevuld en doorgestuurd. Beeld je ook in dat de token een vervaldatum heeft van 30 dagen. Wanneer de gebruiker de website bezoekt, dan wordt de token opgehaald uit de cookie en wordt de gebruiker ingelogd. Wanneer de gebruiker de website bezoekt na 30 dagen, dan is de token vervallen en moet de gebruiker opnieuw inloggen.

## Sessions

Maak een nieuw bestand aan in de map `php-basis`, genaamd `13_sessions.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/13_sessions.php`. Er zal momenteel nog niks te zien zijn.

Maak een nieuw bestand aan in de map `php-basis`, genaamd `13_sessions_dashboard.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/13_sessions_dashboard.php`. Dit is de pagina die getoond zal worden nadat de gebruiker ingelogd is.

Maak een nieuw bestand aan in de map `php-basis`, genaamd `13_sessions_logout.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/13_sessions_logout.php`. Dit is de pagina die bezocht zal worden om de sessie te vernietigen.

Sessions zijn een manier om data op te slaan op de server. Deze data kan op een later moment terug opgehaald worden wanneer de gebruiker die de data heeft opgeslagen, de website opnieuw bezoekt.

Doordat sessions opgeslagen worden op de server, kunnen we gevoelige data opslaan in sessions.

`13_sessions.php`

```php
<?php

session_start(); // nodig op elke pagina waar je de sessie wilt gebruiken

if (isset($_POST['submit'])) {
  $username = filter_input(INPUT_POST, 'username', FILTER_SANITIZE_FULL_SPECIAL_CHARS);
  $password = $_POST['password'];

  if ($username == 'John' && $password == '1234') {
    session_start();
    $_SESSION['username'] = $username;
    header('Location: /php-basis/13_sessions_dashboard.php'); // redirect naar dashboard
  } else {
    echo 'Invalid credentials.';
  }
}

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>

  <form action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']); ?>" method="POST">
    <div>
      <label>Username: </label>
      <input type="text" name="username">
    </div>
    <div>
      <label>Password: </label>
      <input type="password" name="password">
    </div>
    <br>
    <input type="submit" name="submit" value="Submit">

  </form>
</body>

</html>
```

Merk op dat in een echte situatie de gebruiker niet ingelogd wordt met een username en wachtwoord die in de code staan. Dit is enkel om aan te tonen hoe sessions werken. In een echte situatie zou de gebruiker ingelogd worden met een username en wachtwoord die opgeslagen zijn in een database.

Wanneer de gebruiker inloggegevens invult en op de submitknop klikt, dan worden de gegevens gecontroleerd. Indien de gegevens correct zijn, dan wordt de gebruiker doorgestuurd naar de dashboard pagina. Indien de gegevens niet correct zijn, dan wordt er een foutmelding getoond op de huidige pagina.

`13_sessions_dashboard.php`

```php
<?php

session_start(); // nodig op elke pagina waar je de sessie wilt gebruiken

$username = $_SESSION['username'];

if ($username) {
  echo 'Welcome, ' . $username . '.';
} else {
  echo 'You are not logged in.';
}
```

Wanneer de gebruiker de dashboard pagina bezoekt, dan wordt er gecontroleerd of de gebruiker ingelogd is. Indien de gebruiker ingelogd is, dan wordt er een welkomstbericht getoond. Indien de gebruiker niet ingelogd is, dan wordt er een foutmelding getoond.

Om aan te tonen dat sessies per browser werken, open je een tweede browser (stel dat je Firefox open hebt staan, open dan Chrome). Bezoek `http://localhost/php-basis/13_sessions_dashboard.php` in de tweede browser. Je zal zien dat je niet ingelogd bent. Ververs dezelfde pagina in de eerste browser en je zal zien dat je wel ingelogd bent.

We kunnen het dashboard uitbreiden met HTML.

`13_sessions_dashboard.php`

```php
<?php

session_start(); // nodig op elke pagina waar je de sessie wilt gebruiken

$username = $_SESSION['username'];

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php if ($username) : ?>
    <a href="/php-basis/13_sessions_logout.php">Logout</a>
    <h2>Welcome, <?php echo $username ?>!</h2>
  <?php else : ?>
    <p>You are not logged in.</p>
    <a href="/php-basis/13_sessions.php">Login</a>
  <?php endif; ?>
</body>

</html>
```

Wanneer de gebruiker ingelogd is, dan tonen we een link om uit te loggen en een welkomstbericht. Wanneer de gebruiker niet ingelogd is, dan tonen we een foutmelding en een link om in te loggen.

`13_sessions_logout.php`

```php
<?php
session_start(); // nodig op elke pagina waar je de sessie wilt gebruiken

session_destroy(); // vernietig de sessie
header('Location: /php-crash/13_sessions.php'); // redirect naar 13_sessions.php
```

Wanneer de gebruiker op de link klikt om uit te loggen, dan wordt de gebruiker doorgestuurd naar de logoutpagina. De sessie wordt vernietigd en de gebruiker wordt doorgestuurd naar de loginpagina.

## File handling

Maak een nieuw bestand aan in de map `php-basis`, genaamd `14_file_handling.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/14_file_handling.php`. Er zal momenteel nog niks te zien zijn.

Maak een nieuwe map aan in de map `php-basis`, genaamd `extras`. Maak in deze map een nieuw bestand aan genaamd `friends.json`.

In dit bestand plaatsen we een JSON object met vrienden.

`friends.json`

```json
{
  "friends": [
    {
      "firstName": "John",
      "lastName": "Duck",
      "age": 32
    },
    {
      "firstName": "Kwik",
      "lastName": "Duck",
      "age": 16
    },
    {
      "firstName": "Kwek",
      "lastName": "Duck",
      "age": 16
    },
    {
      "firstName": "Kwak",
      "lastName": "Duck",
      "age": 16
    }
  ]
}
```

PHP heeft ingebouwde functionaliteit om te werken met bestanden. We kunnen bestanden openen, lezen, schrijven, bewerken en sluiten.

```php
<?php

/**
 * PHP heeft ingebouwde functies om bestanden te openen, lezen, schrijven en sluiten.
 */

 $file = 'extras/friends.json';

 if (file_exists($file)) {
   // open het bestand
   $handle = fopen($file, 'r');

   // lees het bestand
   $content = fread($handle, filesize($file));

   // sluit het bestand
   fclose($handle);

   // toon de inhoud van het bestand
   var_dump($content);
 } else {
   echo 'Het bestand bestaat niet.';
 }
```

De inhoud die gelezen wordt is een string met daarin de JSON data. We kunnen deze string omzetten naar een PHP array met behulp van de functie `json_decode()`.

```php
<?php

/**
 * PHP heeft ingebouwde functies om bestanden te openen, lezen, schrijven en sluiten.
 */

$file = 'extras/friends.json';

if (file_exists($file)) {
  // open het bestand
  $handle = fopen($file, 'r');

  // lees het bestand
  $content = fread($handle, filesize($file));

  // sluit het bestand
  fclose($handle);

  $data = json_decode($content, true);
  var_dump($data);
} else {
  echo 'Het bestand bestaat niet.';
}
```

Nu de data een associative array is, kunnen we deze data gebruiken in de HTML.

```php
<?php

/**
 * PHP heeft ingebouwde functies om bestanden te openen, lezen, schrijven en sluiten.
 */

$file = 'extras/friends.json';

if (file_exists($file)) {
  // open het bestand
  $handle = fopen($file, 'r');

  // lees het bestand
  $contents = fread($handle, filesize($file));

  // sluit het bestand
  fclose($handle);

  // zet de JSON string om naar een PHP array
  $data = json_decode($contents, true);
} else {
  echo 'Het bestand bestaat niet.';
}

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php foreach ($data['friends'] as $friend) : ?>
    <p>Voornaam: <?php echo $friend['firstName']; ?></p>
    <p>Achternaam: <?php echo $friend['lastName']; ?></p>
    <p>Leeftijd: <?php echo $friend['age']; ?></p>
    <br>
  <?php endforeach; ?>
</body>

</html>
```

Verwijder de `friends.json` file en bezoek de pagina. Je zal zien dat er een foutmelding getoond wordt. We zullen code toevoegen om een nieuw bestand aan te maken indien het nog niet bestaat.

```php
<?php

$file = 'extras/friends.json';

if (file_exists($file)) {
  // open het bestand
  $handle = fopen($file, 'r');

  // lees het bestand
  $contents = fread($handle, filesize($file));

  // sluit het bestand
  fclose($handle);

  // zet de JSON string om naar een PHP array
  $data = json_decode($contents, true);
} else {
  // De data die we willen opslaan
  $data = [
    'friends' => [
      [
        'firstName' => 'Donald',
        'lastName' => 'Duck',
        'age' => 92
      ],
    ]
  ];

  // zet de PHP array om naar een JSON string
  $contents = json_encode($data);

  // open het bestand
  $handle = fopen($file, 'w');

  // schrijf de JSON string naar het bestand
  fwrite($handle, $contents);

  // sluit het bestand
  fclose($handle);
}

?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <?php foreach ($data['friends'] as $friend) : ?>
    <p>Voornaam: <?php echo $friend['firstName']; ?></p>
    <p>Achternaam: <?php echo $friend['lastName']; ?></p>
    <p>Leeftijd: <?php echo $friend['age']; ?></p>
    <br>
  <?php endforeach; ?>
</body>

</html>
```

Indien het bestand niet wordt aangemaakt, dan kan het zijn de de map `extras` niet de juiste rechten heeft om bestanden aan te maken. Dit kan je oplossen door de rechten van de map aan te passen.

In de map `php-basis` open je een terminal en voer je volgende commando uit:

```sh
chmod -R 777 extras
```

## File upload

Maak een nieuw bestand aan in de map `php-basis`, genaamd `15_file_upload.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/15_file_upload.php`. Er zal momenteel nog niks te zien zijn.

Maak een nieuwe map aan in de map `php-basis`, genaamd `uploads`. Deze map zal gebruikt worden om bestanden in op te slaan.

```php
<?php

$allowed_extensions = array('png', 'jpg', 'jpeg', 'gif');

// Controleer of er een formulier gesubmit is
if (isset($_POST['submit'])) {
  // Controleer of er een bestand is geüpload
  if (!empty($_FILES['upload']['name'])) {
    $file_name = $_FILES['upload']['name'];
    $file_size = $_FILES['upload']['size'];
    $file_tmp = $_FILES['upload']['tmp_name'];
    // Stel dat de afbeelding 'image.png' heet
    $target_dir = "uploads/$file_name"; // 'uploads/image.png'
    $file_ext = explode('.', $file_name); // ['image', 'png']
    $file_ext = strtolower(end($file_ext)); // 'png'

    // Valideer of het bestand de juiste extensie heeft
    if (in_array($file_ext, $allowed_extensions)) {
      // Valideer of het bestand niet te groot is
      if ($file_size <= 1000000) { // 1000000 bytes = 1MB
        // Upload het bestand
        // Het bestand wordt verplaatst naar de map 'uploads'
        move_uploaded_file($file_tmp, $target_dir);

        // Success message
        $success = true;
      } else {
        $message = 'Bestand is te groot!';
      }
    } else {
      $message = 'Ongeldig bestandstype!';
    }
  } else {
    $message = 'Kies een bestand om te uploaden';
  }
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>File Upload</title>
</head>

<body>
  <?php if ($message) : ?>
    <p style="color: red;"><?php echo $message; ?></p>
  <?php endif; ?>

  <form action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']); ?>" method="post" enctype="multipart/form-data">
    <p>Selecteer een afbeelding om te uploaden</p>
    <input type="file" name="upload"><br><br>
    <input type="submit" value="Submit" name="submit"><br>
  </form>

  <?php if ($success) : ?>
    <p style="color: green;">Afbeelding is geüpload!</p>
    <img src="<?php echo $target_dir; ?>" alt="Uploaded image">
  <?php endif; ?>

  <?php if ($too_large) : ?>
    <p style="color: red;">Afbeelding is te groot!</p>
  <?php endif; ?>


</body>

</html>
```

Merk op dat `encrypte="multipart/form-data"` toegevoegd is aan het formulier. Dit is nodig om bestanden te kunnen uploaden.

Op Firefox krijg je na het uploaden van een bestand een popup - klik hier op 'Cancel'. Dit is niet het geval in Chrome. In een echte situatie zou het verwerken (uploaden) van het bestand gebeuren in een aparte file, bijvoorbeeld `upload.php` - momenteel zit het uploadproces in dezelfde file als de HTML, wat voor de popup zorgt.

## Exceptions

Maak een nieuw bestand aan in de map `php-basis`, genaamd `16_exceptions.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-basis/16_exceptions.php`. Er zal momenteel nog niks te zien zijn.

```php
<?php

function inverse($x)
{
  if (!$x) {
    throw new Exception('Division by zero.');
  }
  return 1 / $x;
}

echo inverse(0); // Dit gooit een exception, het script stopt met uitvoeren
```

try/catch

```php
<?php

function inverse($x)
{
  if (!$x) {
    throw new Exception('Division by zero.');
  }
  return 1 / $x;
}

try {
  echo inverse(5) . ' ';
  echo '<br>';
  echo inverse(0) . ' '; // Dit gooit een exception
  echo '<br>';
} catch (Exception $e) {
  // Dit vangt de exception op en toont de foutmelding in de browser
  // Het script stopt niet met uitvoeren
  echo 'Caught exception: ',  $e->getMessage(), ' ';
  echo '<br>';
}

echo '<br>';
```

try/catch/finally

```php
<?php

function inverse($x)
{
  if (!$x) {
    throw new Exception('Division by zero.');
  }
  return 1 / $x;
}

try {
  echo inverse(5) . ' ';
  echo '<br>';
} catch (Exception $e) {
  // We komen niet in dit catch block terecht
  echo 'Caught exception: ',  $e->getMessage(), ' ';
  echo '<br>';
} finally {
  // We komen wel in dit finally block terecht
  echo 'First finally ';
  echo '<br>';
}

echo '<br>';

try {
  echo inverse(0) . ' ';
  echo '<br>';
} catch (Exception $e) {
  // We komen wel in dit catch block terecht
  echo 'Caught exception: ',  $e->getMessage(), ' ';
  echo '<br>';
} finally {
  // We komen wel in dit finally block terecht
  echo "Second finally ";
  echo '<br>';
}

echo '<br>';
```

## Conclusie

Dit was een korte introductie tot PHP. Er is nog meer te leren, zoals OOP (Object Oriented Programming) in PHP, MVC (Model View Controller) in PHP, PHP frameworks zoals Laravel ... maar dit is een goed begin om mee te zijn met de basis van PHP.
