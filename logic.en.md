---
postUuid: 5db2c650-7474-4bc6-8cad-839ad24fcd23
title: Logic
slug: logic
tags:
  - Learn To Program
  - Logic
categories:
  - Frontend
---

## Bits and bytes

To better understand how computers work, we need to look at the simplest form of a computer: a Turing machine. A Turing machine, unlike the name suggests, is not a machine. It is a model to describe how a computer works.

Turing refers to Alan Turing, a computer scientist that realized that all computational problems can be visualized by a digital language: 0 and 1. A Turing machine can be made out of everything that can have two states: 'on' and 'off', 'yes' and 'no' ...

### Bit

A bit can be 0 or 1. This can be visualized by a light switch. When the switch is on, the value is 1. When the switch is off, the value is 0.

A bit can for example be used to represent the state of a lamp. When the lamp is on, the bit is 1. When the lamp is off, the bit is 0.

| bit | lamp |
| :-: | :--: |
|  0  | off  |
|  1  |  on  |

### Two bit system

A system existing of two bits, can have four different states. Think about a RGB led. The led can be off, red, green or blue.

| bit 1 | bit 2 |  led  |
| :---: | :---: | :---: |
|   0   |   0   |  off  |
|   0   |   1   |  red  |
|   1   |   0   | green |
|   1   |   1   | blue  |

### Byte

A byte exists of 8 bits. This gives 256 possible states, which results in a table of 256 rows. To visualize a character from the [ASCII table](https://en.wikipedia.org/wiki/ASCII_table), one byte is used.

| bit 1 | bit 2 | bit 3 | bit 4 | bit 5 | bit 6 | bit 7 | bit 8 | karakter |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :------: |
|  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |   ...    |
|   0   |   1   |   0   |   0   |   0   |   0   |   0   |   1   |    A     |
|   0   |   1   |   0   |   0   |   0   |   0   |   1   |   0   |    B     |
|  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |   ...    |
|   0   |   1   |   1   |   0   |   0   |   0   |   0   |   1   |    a     |
|   0   |   1   |   1   |   0   |   0   |   0   |   1   |   0   |    b     |
|  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |  ...  |   ...    |

One kilobyte (KB) is 1024 bytes. One kilogram is a 1000 grams. One kilcalorie is a 1000 calories. Why on earth is one kilobyte 1024 bytes? The reason is that the capacity of memory is always based on powers of 2.
1024 is the closest number to 1000 that is a power of 2. 2<sup>10</sup> = 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 \* 2 = 1024

One megabyte (MB) is 1024 kilobytes. Or 1 048 576 bytes. This is equal 1 048 576 tables of 8 columns and 256 rows, of which each cell can be 0 or 1 to get unique states.

A gigabyte (GB) is 1024 megabytes. Or 1 073 741 824 bytes. A computer with 8 GB of RAM (Random Access Memory) has more than a billion tables that exist of 8 columns and 256 rows of which each cell can be 0 or 1 to get unique states.

A terabyte (TB) is 1024 gigabyte.

A petabyte (PB) is ...

## Algorithms

An algorithm is a sequence of steps that can be used to solve a problem.

An example of an algorithm is a recipe to follow to make a dish. When all ingredients are present and the steps are followed, the same dish will be the result.

The ingredients and the recipe are the input. The process is following the steps of the recipe. The output is the dish.

In a computer an example of an algorithm is a program that returns the sum of two numbers.

The numbers 3 and 7 and the program are the input. The process is following the steps in the program (3 and 7 are added). The output is 10.

## Formal logic

Formal logic looks at whether a reasoning, consisting of assertions and a conclusion, is valid or invalid. It assumes that the statements are true.

An assertion is a statement that can be true or false.

- "All humans are mammals," is an assertion that is factually true.
- "All humans are fish," is a statement that is factually false.

Note: Formal logic assumes that all statements are true even if they are factually false. So also "all people are fish" is considered true.

### Examples

- Assertion 1: All humans are mammals.
- Assertion 2: I am a human being.
- Conclusion: I am a mammal.

This reasoning is valid.

- Assertion 1: All humans are fish.
- Assertion 2: I am a human being.
- Conclusion: I am a fish.

this reasoning is valid.
The conclusion is true, based on **assertions that are assumed to be true**.

- Assertion 1: All cars made after 1991 have seat belts.
- Assertion 2: My car has a seat belt.
- Conclusion: My car was made after 1991.

This is an invalid assertion.
Assertion 1 does not exclude the possibility that cars with seat belts were made before 1991. It only confirms that all cars made after 1991 have seat belts.

- Assertion 1: All cars made after 1991 have seat belts.
- Assertion 2: My car does not have a seat belt.
- Conclusion: My car was made before 1991.

This is a valid assertion.
Assertion 1 rules out the possibility that cars made after 1991 have no seat belts.

- Assertion 1: All cars made after 1991 have seat belts.
- Assertion 2: All cars made before 1991 do not have a seat belt.
- Assertion 3: My car has a seat belt.
- Conclusion: My car was made after 1991.

This is a valid assertion.
Assertion 2 rules out the possibility that seat belts were present in cars before 1991.

- Assertion 1: All bicycles are blue.
- Assertion 2: All motorcycles are green.
- Conclusion: My car is red.

This is an invalid reasoning. Assertion 1 and 2 do not exclude the possibility that the car could be any other color than red.

## Deductive and inductive logic

Deductive assertions show conclusively that the conclusion is true, if all the assertions are true.
Inductive assertions show that a conclusion is likely, but not certain, to be true.

### Deductive examples

- Assertion 1: All humans are mammels.
- Assertion 2: All mammels are animals.
- Conclusion: All humans are animals.
  <br/>
  <br/>
- Assertion 1: The food this evening will be either wok or french fries.
- Assertion 2: The food this evening will not be french fries..
- Conclusion: The food this evening will be wok.
  <br/>
  <br/>
- Assertion 1: If water is leaking through the roof, water is leaking through the ceiling.
- Assertion 2: If water is leaking through the ceiling, the floor gets wet.
- Conclusion: When water is leaking through the roof, the floor gets wet..

### Inductive examples

- Assertion 1: It has never happened in history that a student has not passed.
- Conclusion: It is highly likely, but not certain, that all students will succeed this year.
  <br/>
  <br/>
- Assertion 1: Every morning for the past ten days, the neighbor's rooster has been crowing.
- Conclusion: Probably the cock crows tomorrow morning.

#### Strong inductive assertions

Again, the assumption is that the assertions are true. A strong inductive assertion provides a conclusion that is most likely true.

- Assertion 1: All humans born so far have been born with eleven toes.
- Conclusion: Most likely, the next human born is also going to have eleven toes.

This is a strong inductive assertion.

#### Weak inductive assertions

- Assertion 1: A box contains one hundred apples.
- Assertion 2: Three randomly selected apples are ripe.
- Conclusion: All the apples in the box are likely to be ripe.

This is a weak inductive assertion. It can become a strong inductive assertion by increasing the number of apples inspected.

- Assertion 1: A box contains one hundred apples.
- Assertion 2: Eighty randomly selected apples are ripe.
- Conclusion: All the apples in the box are likely to be ripe.

This is a strong inductive assertion.

In the examples above, it is clear whether it is a weak or strong inductive assertion. There are cases where it is less clear, for example when fifty apples are inspected, does that make the conclusion more or less likely?

## Categorical logic - Venn-diagram

In this form of logic, categories are formed. This involves the use of the words "all," "some," and "no/none".

- All people are mammals.

- Some people live to be a hundred years old.

- No fish is a mammal.

Each category can be represented by a circle. A Venn diagram is a group of circles, where each circle is a category and the overlapping parts of circles represent the relationship between categories.

A Venn diagram with two categories.

![Venn-diagram-2](/img/blog/venn-diagram-2.png)

- The pink category are all dogs.
- The yellow category are all pets.
- The overlapping red section are all dogs that are also pets.

A Venn diagram with three categories.

![Venn-diagram-3](/img/blog/venn-diagram-3.png)

- The pink category are all dogs.
- The yellow category are all pets.
- The light blue circle are all animals with black fur.
- The overlapping red section are all dogs that are also pets.
- The overlapping blue section are all dogs with black fur.
- The overlapping green section are all pets with black fur.
- The overlapping black section are all dogs that are also pets and have black fur.

Combinations are also possible:

- The pink with the red combined represent all dogs that are also a pet but do not have black fur.
- The pink with the red and the blue combined do not include dogs that are both a pet and have a black fur.
