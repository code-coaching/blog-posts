---
postUuid: e64b851a-b794-45b4-baee-6e38fdf289f7
title: Quasar Tour of Heroes - Intro
slug: quasar-toh-intro
tags:
  - Quasar
  - Vue
  - Tour of Heroes
categories:
  - Frontend
---

Expected Prior Knowledge:
- Basic JavaScript
- Basic HTML
- Basic CSS
- Node / NPM
- Git

This module is about the Quasar Framework and thus Vue. More specifically Vue3. In many cases, a to-do list is created as the first application to showcase the functionality of a framework. We are **not** going to do this! So what's going to happen? A choice is made for an extended application to showcase not just the basic functionality of the framework, but also some concepts.

Angular, one of the other modern frameworks, has a very good tutorial named Tour of Heroes to show the functionality of the framework and to teach basic concepts. This tutorial takes the [Tour of Heroes](https://angular.io/tutorial/) as the basis with Quasar as the framework. Then the tutorial will be extended to also show the functionalities of a production-ready Vue3 application.

The basic concepts are:
- routing (the navigation)
- layouts 
- pages
- smart/dumb components
- linking data in the HTML
- services to collect data

The additional concepts are:
- i18n (internationalization) - translations
- linking the data in the services with an API (both with `fetch` and with `axios`)
- linking a store (both `Vuex` and `Pinia`).

A number of techniques will also be used to facilitate theming (CSS variables) and global styling (using the correct CSS units). TypeScript will be used.

The base that will be built first, will look like this:
![Tour of Heroes basic](https://angular.io/generated/images/guide/toh/toh-anim.gif)

The module consists of separate parts. Each part can be followed separately if certain knowledge is already present. It is recommended to follow these parts in the order they are built.
