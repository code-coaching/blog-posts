---
postUuid: d998d0d1-1011-4068-8283-8ed246772c47
title: PHP - OOP
slug: php-oop
tags:
  - PHP
categories:
  - Backend
  - OOP
---

Verwachte voorkennis: PHP Basis

Object Oriented Programming (Nederlands: Objectgeoriënteerd Programmeren) is een programmeerparadigma dat in veel moderne programmeertalen terug te vinden is. Het is een manier om variabelen en functies te groeperen.

We werken met XAMPP, maak in de map `htdocs` een nieuwe map aan, genaamd `php-oop`. Start de Apache-server via XAMPP en navigeer naar `http://localhost/php-basis`.

## Niet OOP

Maak een nieuw bestand aan in de map `php-oop`, genaamd `01_not_oop.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-oop/01_not_oop.php`. Er zal momenteel nog niks te zien zijn.

Om te begrijpen waarom OOP handig is, gaan we kijken naar een voorbeeld zonder OOP.

```php
<?php
$first_name = 'John';
$last_name = 'Duck';
$age = 32;

echo 'My name is ' . $first_name . ' ' . $last_name . ' and I am ' . $age . ' years old.';
echo '<br>';
```

Dit is een voorbeeld van code waarbij er gegevens over een persoon worden opgeslagen in variabelen. Dit werkt, maar wat als we nu meerdere personen willen opslaan? Dan moeten we voor elke persoon nieuwe variabelen aanmaken. `first_name_1`, `last_name_1`, `age_1`, `first_name_2`, `last_name_2`, `age_2`, etc. Dit is niet zo handig.

Een mogelijke oplossing om dit te voorkomen is om `associative arrays` te gebruiken (arrays waarbij je zelf de indexen kan bepalen).

```php
<?php
$person = [
  'first_name' => 'John',
  'last_name' => 'Duck',
  'age' => 32
];

echo 'My name is ' . $person['first_name'] . ' ' . $person['last_name'] . ' and I am ' . $person['age'] . ' years old.';
echo '<br>';
```

Als we nu meerdere personen willen opslaan, kunnen we dit doen door meerdere arrays aan te maken.

```php
<?php
$person_1 = [
  'first_name' => 'John',
  'last_name' => 'Duck',
  'age' => 32
];

$person_2 = [
  'first_name' => 'Jane',
  'last_name' => 'Duck',
  'age' => 28
];

echo 'My name is ' . $person_1['first_name'] . ' ' . $person_1['last_name'] . ' and I am ' . $person_1['age'] . ' years old.';
echo '<br>';
echo 'My name is ' . $person_2['first_name'] . ' ' . $person_2['last_name'] . ' and I am ' . $person_2['age'] . ' years old.';
echo '<br>';
```

Dit werkt. Om nu te voorkomen dat we voor elke persoon steeds dezelfde code moeten herhalen om de gegevens te tonen, kunnen we een functie maken.

```php
<?php
$person_1 = [
  'first_name' => 'John',
  'last_name' => 'Duck',
  'age' => 32
];

$person_2 = [
  'first_name' => 'Jane',
  'last_name' => 'Duck',
  'age' => 28
];

function get_details($person)
{
  return 'My name is ' . $person['first_name'] . ' ' . $person['last_name'] . ' and I am ' . $person['age'] . ' years old.' . '<br>';
}

echo get_details($person_1);
echo get_details($person_2);
```

Dit werkt. Maar wat als we nu ook een property willen hebben die de volledige naam van de persoon bevat? Dan moeten we dit manueel toevoegen aan elke array. Vervolgens kunnen we `full_name` gebruiken in de functie.

```php
<?php
$person_1 = [
  'first_name' => 'John',
  'last_name' => 'Duck',
  'age' => 32,
];
$person_1['full_name'] = $person_1['first_name'] . ' ' . $person_1['last_name'];

$person_2 = [
  'first_name' => 'Jane',
  'last_name' => 'Duck',
  'age' => 28,
];
$person_2['full_name'] = $person_2['first_name'] . ' ' . $person_2['last_name'];

function get_details($person)
{
  return 'My name is ' . $person['full_name'] . ' and I am ' . $person['age'] . ' years old.' . '<br>';
}

echo get_details($person_1);
echo get_details($person_2);
```

Zoals we zien, dit werkt, maar het wordt steeds onoverzichtelijker.

## OOP

Maak een nieuw bestand aan in de map `php-oop`, genaamd `02_oop_basics.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-oop/02_oop_basics.php`. Er zal momenteel nog niks te zien zijn.

We gaan het eerste concept van OOP bekijken: `class`. Een class is een "template"/"blauwdruk" voor een object. Een object is te vergelijken met een associative array.

### Class

Een class kan aangemaakt worden met het `class`-keyword. De naam van de class wordt met een hoofdletter geschreven.

```php
<?php
class Person
{
}
```

Om een object aan te maken van een class, gebruiken we het `new`-keyword.

```php
<?php
class Person
{
}

$person_1 = new Person();

var_dump($person_1);
```

`$person_1` is nu een object van de class `Person`, dit noemen we een `instance` van de class.

Bij het bekijken van de browser zien we:

```
object(Person)#1 (0) { }
```

We zien dat het gaat om een `object` van de class `Person`, object(Person).
We zien dat het over eerste instance gaat, #1.
We zien dat er geen properties zijn, (0).
We zien dat er geen waarden gekend zijn, { }.

## Properties en access modifiers

We kunnen properties toevoegen aan een object door ze te definiëren in de class. Properties kan je zien als variabelen die bij een object horen.

```php
<?php
class Person
{
  private $first_name;
  private $last_name;
  public $age;
}

$person_1 = new Person();
$person_1->age = 32;

var_dump($person_1);
```

We zien dat er drie properties gedefinieerd zijn, `$first_name`, `$last_name` en `$age`. `$first_name` en `$last_name` zijn `private`, dit wil zeggen dat ze enkel toegankelijk zijn binnen de class. `$age` is `public`, dit wil zeggen dat deze overal toegankelijk is.

Met andere woorden, we kunnen `$age` aanpassen buiten het codeblok van de class, maar `$first_name` en `$last_name` niet.
`$person_1->age = 32;` is dus toegestaan, maar `$person_1->first_name = 'John';` niet.

Probeer `$person_1->first_name = 'John';` toe te voegen en bekijk de foutmelding die getoond wordt (zowel in de IDE als in de browser).

`private` en `public` worden `access modifiers` genoemd. Er zijn er meer dan deze twee, maar voorlopig gaan we verder met deze twee.

Wanneer we de `var_dump` bekijken, zien we:

```
object(Person)#1 (3) { ["first_name":"Person":private]=> NULL ["last_name":"Person":private]=> NULL ["age"]=> int(32) }
```

We zien dat het gaat om een `object` van de class `Person`, object(Person).
We zien dat het over eerste instance gaat, #1.
We zien dat er 3 properties zijn, (3).
We zien dat er waarden gekend zijn, `{ ["first_name":"Person":private]=> NULL ["last_name":"Person":private]=> NULL ["age"]=> int(32) }`.

Bij de waarden zien we:
`["first_name":"Person":private]=> NULL`, dit wil zeggen dat `$first_name` `private` is en dat er geen waarde gekend is (`NULL`).
`["last_name":"Person":private]=> NULL`, dit wil zeggen dat `$last_name` `private` is en dat er geen waarde gekend is (`NULL`).
`["age"]=> int(32)`, dit wil zeggen dat `$age` `public` is en dat de waarde `32` is.

## Constructor

Een `constructor` is een functie die automatisch wordt uitgevoerd wanneer een instance van een class wordt aangemaakt. Een constructor wordt gedefinieerd met de naam `__construct`.

Om het nut van een constructor te begrijpen, zullen we eerst een voorbeeld maken met allemaal public properties.

```php
<?php
class Person
{
  public $first_name;
  public $last_name;
  public $age;
}

$person_1 = new Person();
$person_1->first_name = 'John';
$person_1->last_name = 'Duck';
$person_1->age = 32;

$person_2 = new Person();
$person_2->first_name = 'Jane';
$person_2->last_name = 'Duck';
$person_2->age = 28;

var_dump($person_1);
echo '<br>';
var_dump($person_2);
echo '<br>';
```

We zien dat we nu voor elke instance van de class `Person` de properties moeten instellen. Dit is niet zo handig. We kunnen dit oplossen met een constructor.

```php
<?php
class Person
{
  public $first_name;
  public $last_name;
  public $age;

  function __construct($first_name, $last_name, $age)
  {
    $this->first_name = $first_name;
    $this->last_name = $last_name;
    $this->age = $age;
  }
}

$person_1 = new Person('John', 'Duck', 32);
$person_2 = new Person('Jane', 'Duck', 28);

var_dump($person_1);
echo '<br>';
var_dump($person_2);
echo '<br>';
```

Een functie in een class wordt een `method` genoemd. De constructor is dus een method.
Een method is standaard `public`, dit hoeft dus niet expliciet vermeld te worden voor de naam van de method.

## Methods

Een method is een functie die bij een object hoort. Een method kan enkel uitgevoerd worden op een instance van een class.

```php
<?php
class Person
{
  public $first_name;
  public $last_name;
  public $age;

  function __construct($first_name, $last_name, $age)
  {
    $this->first_name = $first_name;
    $this->last_name = $last_name;
    $this->age = $age;
  }

  private function full_name() {
    return $this->first_name . ' ' . $this->last_name;
  }

  function get_details() {
    return 'My name is ' . $this->full_name() . ' and I am ' . $this->age . ' years old.';
  }
}

$person_1 = new Person('John', 'Duck', 32);
$person_2 = new Person('Jane', 'Duck', 28);

echo $person_1->get_details();
// echo $person_1->full_name(); Dit werkt niet - private method
echo '<br>';
echo $person_2->get_details();
echo '<br>';
```

We zien dat er twee methods zijn toegevoegd, `full_name` en `get_details`. `full_name` is `private`, dit wil zeggen dat deze enkel toegankelijk is binnen de class. `get_details` is `public`, dit wil zeggen dat deze overal toegankelijk is.

## Inheritance

Maak een nieuw bestand aan in de map `php-oop`, genaamd `03_oop_inheritance.php`. Dit bestand kan getoond worden in de browser door te navigeren naar `http://localhost/php-oop/03_oop_inheritance.php`. Er zal momenteel nog niks te zien zijn.

Inheritance (Nederlands: overerving) is een concept waarbij een class de eigenschappen van een andere class kan overerven. De class die properties van een andere class overerft, wordt een `child class` genoemd. De class waarvan de properties overgeërfd worden, wordt een `parent class` genoemd.

Dit klinkt allemaal nogal abstract, dus we gaan dit bekijken aan de hand van een voorbeeld.

Wanneer we denken aan voertuigen op de weg, dan kunnen we een aantal eigenschappen bedenken die alle voertuigen gemeenschappelijk hebben. Bijvoorbeeld: kleur, aantal wielen, merk, type, etc.

Een auto (Car) heeft ook nog een aantal eigenschappen die een fiets (Bicycle) niet heeft. Bijvoorbeeld: aantal deuren, type brandstof, etc.

We kunnen een class maken die alle gemeenschappelijke properties bevat, deze class noemen we `Vehicle` (Nederlands: Voertuig).

```php
<?php
class Vehicle
{
  private $color;
  private $number_of_wheels;
  private $brand;
  private $type;

  function __construct($brand, $type, $color, $number_of_wheels)
  {
    $this->brand = $brand;
    $this->type = $type;
    $this->color = $color;
    $this->number_of_wheels = $number_of_wheels;
  }

  function info()
  {
    return 'This ' . $this->color . ' ' . $this->brand . ' ' . $this->type . ' has ' . $this->number_of_wheels . ' wheels.';
  }
}

$car_1 = new Vehicle('Tesla', 'Model S', 'blue', 4);
$car_2 = new Vehicle('Tesla', 'Model 3', 'gray', 4);
$car_3 = new Vehicle('Ferrari', 'F50', 'red', 4);
$bicycle_1 = new Vehicle('Gazelle', 'CityZen', 'black', 2);
$bicycle_2 = new Vehicle('Gazelle', 'Orange C8', 'orange', 2);

echo $car_1->info();
echo '<br>';
echo $car_2->info();
echo '<br>';
echo $car_3->info();
echo '<br>';
echo $bicycle_1->info();
echo '<br>';
echo $bicycle_2->info();
echo '<br>';
```

Dit zal het volgende tonen in de browser:

```
This blue Tesla Model S has 4 wheels.
This gray Tesla Model 3 has 4 wheels.
This red Ferrari F50 has 4 wheels.
This black Gazelle CityZen has 2 wheels.
This orange Gazelle Orange C8 has 2 wheels.
```

### Extends en Method Overriding

We zien dat we nu een class hebben die de gemeenschappelijke properties bevat. We kunnen nu een class maken voor een auto (Car) en een class voor een fiets (Bicycle). Deze classes zullen de properties van de class `Vehicle` overerven, dit werkt via het `extends`-keyword.

```php
<?php
class Vehicle
{
  private $color;
  private $number_of_wheels;
  private $brand;
  private $type;

  function __construct($brand, $type, $color, $number_of_wheels)
  {
    $this->brand = $brand;
    $this->type = $type;
    $this->color = $color;
    $this->number_of_wheels = $number_of_wheels;
  }

  function info()
  {
    return 'This ' . $this->color . ' ' . $this->brand . ' ' . $this->type . ' has ' . $this->number_of_wheels . ' wheels.';
  }
}

class Car extends Vehicle
{
  private $number_of_doors;

  // Constructor van Car
  function __construct($brand, $type, $color, $number_of_wheels, $number_of_doors)
  {
    // Roep de constructor van de parent class (Vehicle) aan en geef de nodige parameters mee
    parent::__construct($brand, $type, $color, $number_of_wheels);
    // Specifieke property van Car
    $this->number_of_doors = $number_of_doors;
  }

  // "Method Overriding", wanneer ->info() wordt aangeroepen op een instance van Car, zal deze method worden uitgevoerd in plaats van de method in Vehicle
  function info()
  {
    // parent::info() roept de method info() aan in de parent class (Vehicle)
    return parent::info() . ' It has ' . $this->number_of_doors . ' doors.';
  }
}


$car_1 = new Car('Tesla', 'Model S', 'blue', 4, 4);
$car_2 = new Car('Tesla', 'Model 3', 'gray', 4, 4);
$car_3 = new Car('Ferrari', 'F50', 'red', 4, 2);
$bicycle_1 = new Vehicle('Gazelle', 'CityZen', 'black', 2);
$bicycle_2 = new Vehicle('Gazelle', 'Orange C8', 'orange', 2);

echo $car_1->info();
echo '<br>';
echo $car_2->info();
echo '<br>';
echo $car_3->info();
echo '<br>';
echo $bicycle_1->info();
echo '<br>';
echo $bicycle_2->info();
echo '<br>';
```

Dit zal het volgende tonen in de browser:

```
This blue Tesla Model S has 4 wheels. It has 4 doors.
This gray Tesla Model 3 has 4 wheels. It has 4 doors.
This red Ferrari F50 has 4 wheels. It has 2 doors.
This black Gazelle CityZen has 2 wheels.
This orange Gazelle Orange C8 has 2 wheels.
```

### Default values

Het is mogelijk om een `default value` (Nederlands: standaardwaarde) toe te kennen aan een parameter van een functie. Aangezien een constructor een functie is, is dit ook mogelijk bij een constructor.

```php
<?php
// hier nog code

class Car extends Vehicle
{
  private $number_of_doors;

  // Merk op: $number_of_doors en $number_of_wheels zijn gewisseld van plaats
  function __construct($brand, $type, $color, $number_of_doors = 4, $number_of_wheels = 4)
  {
    parent::__construct($brand, $type, $color, $number_of_wheels);
    $this->number_of_doors = $number_of_doors;
  }

  function info()
  {
    return parent::info() . ' It has ' . $this->number_of_doors . ' doors.';
  }
}

$car_1 = new Car('Tesla', 'Model S', 'blue');
$car_2 = new Car('Tesla', 'Model 3', 'gray');
$car_3 = new Car('Ferrari', 'F50', 'red', 2);

echo $car_1->info();
echo '<br>';
echo $car_2->info();
echo '<br>';
echo $car_3->info();
echo '<br>';
```

De reden voor het wisselen van de parameters is omdat `$number_of_doors` vaker zal afwijken van de standaardwaarde dan `$number_of_wheels`.

Doordat er nu standaardwaarden gekend zijn voor `$number_of_doors` en `$number_of_wheels`, kunnen we de constructor van `Car` aanroepen zonder deze parameters, indien we willen dat de standaardwaarden gebruikt worden.

```php
$car_1 = new Car('Tesla', 'Model S', 'blue');
$car_2 = new Car('Tesla', 'Model 3', 'gray');
$car_3 = new Car('Ferrari', 'F50', 'red', 2);
```

Bij de eerste twee wagens wordt de standaardwaarde gebruikt voor `$number_of_doors` en `$number_of_wheels`, bij de derde wagen wordt de standaardwaarde gebruikt voor `$number_of_wheels` en wordt de waarde `2` gebruikt voor `$number_of_doors`.

We voorzien ook een class voor een fiets (Bicycle).

```php
// hier nog code
class Bicycle extends Vehicle
{
  function __construct($brand, $type, $color, $number_of_wheels = 2)
  {
    parent::__construct($brand, $type, $color, $number_of_wheels);
  }
}

$car_1 = new Car('Tesla', 'Model S', 'blue', 4, 4);
$car_2 = new Car('Tesla', 'Model 3', 'gray', 4, 4);
$car_3 = new Car('Ferrari', 'F50', 'red', 4, 2);
$bicycle_1 = new Bicycle('Gazelle', 'CityZen', 'black');
$bicycle_2 = new Bicycle('Gazelle', 'Orange C8', 'orange');
// hier nog code
```

Merk op dat er geen extra properties zijn. Merk op dat `info()` niet overschreven wordt, dit is niet nodig omdat er geen extra informatie moet worden toegevoegd. Deze class dient momenteel enkel om een specifiek type voertuig aan te maken, met een standaardwaarde voor het aantal wielen. Er kan later nog extra functionaliteit worden toegevoegd.

## Conclusie

Object Oriented Programming is een manier om variabelen en functies te groeperen. Dit kan handig zijn om code overzichtelijker te maken en om code te hergebruiken. Gemeenschappelijke functionaliteit kan in een parent class worden geplaatst en child classes kunnen deze functionaliteit overerven.

Momenteel hebben we enkel de basis gezien van OOP. Er zijn nog veel meer concepten die bekeken kunnen worden, zoals: interfaces, abstract classes, static properties en methods, etc. Met deze basis kan al veel gedaan worden.