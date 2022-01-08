---
postUuid: 5a3d9f54-4f7c-45ed-bbd9-1bb3cb93a74b
title: JavaScript - Practice
slug: javascript-practice
tags:
  - JavaScript
  - JS
categories:
  - Frontend
---

## Couple JavaScript

JavaScript can be coupled in HTML. JavaScript **can** be loaded in the head-tag. But to be able to connect the JavaScript to the HTML-elements, JavaScript must be loaded **after** all HTML-elements, at the bottom of the body tag.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <!-- Couple CSS -->
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <!-- Here all HTML elements -->
    <h1 class="title">Example</h1>
    <h1 class="title">Example</h1>
    <h1 id="special">Example</h1>

    <!-- Internal JavaScript with script tag at the bottom of the body tag -->
    <script>
      console.log("This is printed when the page is loaded");
    </script>

    <!-- External JavaScript with script tag at the bottom of the body tag -->
    <script src="index.js"></script>
  </body>
</html>
```

```js
// This code is in the file called: index.js
console.log("This is also printed in the console when the page is loaded");
```

In the browser it can be seen that the JavaScript is correctly loaded:

![js](/img/blog/js.en.png)

### defer

Although earlier it was said that it is not possible to load JavaScript in the head tag (if it needs access to any of the elements in the body tag), there is a way to load JavaScript in the head tag. With the attribute `defer`.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <!-- Couple CSS -->
    <link rel="stylesheet" href="style.css" />
    <!-- Externe JavaScript koppelen met een script-tag gebruikmakende van het attribuut "defer" -->
    <script defer src="index.js"></script>
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="title">Example</h1>
    <h1 class="title">Example</h1>
    <h1 id="special">Example</h1>
  </body>
</html>
```

It is better to load the JavaScript with `defer` in the head tag, than it is to load it at the bottom of the body tag.

The difference is that when loading JavaScript in the body tag, the JavaScript is loaded **after** processing the HTML and then it is executed **after** processing the HTML. When loading JavaScript in the head tag with the attribute `defer`, the JavaScript is loaded **during** processing the HTML and then **after** processing the HTML the JavaScript is executed.

## DOM-manipulation

Document Object Model, all elements of the web page.

Elements can be added to the DOM by adding the elements in the HTML pages. This can also be done via JavaScript

There is a global object present in JavaScript in which all the information of the HTML page is available, including all the elements. This is the object called `document'.

In Firefox, when `document` is typed into the console, all properties and methods that exist on the object can be viewed.

![document](/img/blog/document.jpeg)

On this object there are methods present to query the elements of the DOM. Two of these methods are `document.querySelector()` and `document.querySelectorAll()`.

## querySelector & querySelectorAll

`document.querySelector()` returns the first element that matches the CSS-selector.
`document.querySelectorAll()` returns all elements that match the CSS-selector.

The selectors are the same selectors that are used in CSS.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- Here all HTML elements -->
    <h1 class="title">Title one</h1>
    <h1 class="title">Title two</h1>
    <h1 id="special">Title three</h1>

    <!-- In the example below internal JavaScript is used, this code could also be in external JavaScript. -->
    <script>
      const a = document.querySelector("h1");
      // Variable a contains one element, the h1 tag with the text "Title one"

      const b = document.querySelector(".title");
      // Variable b contains one element, the h1 tag with the text "Title one"

      const c = document.querySelector("#special");
      // Variable c contains one element, the h1 tag with the text "Title three"

      const d = document.querySelectorAll("h1");
      // Variable d contains a NodeList (kind of an array) with three elements, all h1 tags

      const e = document.querySelectorAll(".title");
      // Variable e contains a NodeList (kind of an array) with two elements, all h1 tags with the class "title" (the first two h1 tags)

      const f = document.querySelectorAll("#special");
      // Variable f contains a NodeList (kind of an array) with one element, the h1 tag with the id "special" (the third h1 tag)
    </script>
  </body>
</html>
```

The value of variable `a` and `d` printed in the console.

![querySelector-querySelectorAll](/img/blog/querySelector-querySelectorAll.en.png)

In Firefox it is possible to see which properties/methods exist on the element.

![querySelector-h1](/img/blog/querySelector-h1.en.png)

## classList

One of the existing properties on an element is `classList`. This is a property that contains all the classes that are applied to the element. This property is an object that contains methods, like `add()` and `remove()`.

![classList](/img/blog/classList.en.png)

Via the method `add()` it is possible to add a class to the element.
Via the method `remove()` it is possible to remove a class from the element.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <style>
      /* The element with this class will be styled */
      .added {
        color: green;
        font-size: 20px;
      }
    </style>
  </head>
  <body>
    <!-- Here all HTML-elements -->
    <h1 class="title">Example</h1>
    <h1 class="title">Example</h1>
    <h1 id="special">Example</h1>

    <script>
      const specialH1 = document.querySelector("#special");
      specialH1.classList.add("added"); // Here the class "added" is added to the class attribute of the element in the variable 'specialH1'
    </script>
  </body>
</html>
```

![class-added](/img/blog/class-added.en.png)

The `class="added"` is added to the DOM at the moment the JavaScript code is executed.

## setAttribute & removeAttribute

Via `setAttribute` it is possible to add an attribute to an element.
Via `removeAttribute` it is possible to remove an attribute from an element.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- Hier alle HTML-elementen -->
    <h1 class="title">Example</h1>
    <h1 class="title">Example</h1>
    <h1 id="special">Example</h1>

    <script>
      const specialH1 = document.querySelector("#special");
      specialH1.setAttribute("hidden", "");
    </script>
  </body>
</html>
```

![hidden](/img/blog/hidden.en.png)
Because the attribute `hidden` is added to the DOM at the moment the JavaScript code is executed, the element will not be shown in the browser.

### event handler

Via event handlers it is possible to add event listeners to an element. This allows the developer to couple a JavaScript function to an event.

An example is calling a JavaScript function when a click occurs on an element.

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <script defer src="index.js"></script>
  </head>
  <body>
    <h1 onclick="handleClick()">Example</h1>
    <div></div>
  </body>
</html>
```

index.js

```js
function handleClick() {
  const divEl = document.querySelector("div");
  divEl.innerText = "The header got clicked!";
}
```

## Conclusion

JavaScript allows developers to dynamically change the content of the DOM. This post has a couple of examples of how to do this. In reality documentation will be used to find out which methods are available on which elements.
