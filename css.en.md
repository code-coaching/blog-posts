---
postUuid: bbca8ab3-0a51-44f2-a377-2978f8a4ca4f
title: CSS
slug: css
tags:
  - CSS
categories:
  - Frontend
---

HTML contains the structure of a page. CSS contains the styling of a page.

CSS is an abbreviation for **C**ascading **S**tyle **S**heets. `Cascading` is what a waterfall does, it flows from a higher part to a lower part.

HTML takes care of the elements on a web page. CSS takes care of how elements are displayed.

This can be better understood with a demo, see this [demo](https://www.w3schools.com/CSS/CSS_intro.asp). In the demo are five variations of the same HTML, with different CSS.

## HTML elements

Some HTML elements add styling themselves. To show that an HTML element can be rebuilt identical to an element with default styling, we start from an element without styling and apply CSS to it to get to an identical result.

```html
<h1>Heading 1 with h1 element</h1>
<div>Heading 1 with CSS</div>
<div style="font-size: 28px; font-weight: 700;">Heading 1 with CSS</div>
```

![recreate h1 element with CSS](/img/blog/html-css-duplicate.en.png)

## Coupling CSS

There are three ways to couple CSS to an HTML file: `inline`, `internal` and `external`.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <!-- External CSS -->
    <!-- Content of external.css
    .blue {
        font-size: 28px;
        color: blue;
    }
    -->
    <link rel="stylesheet" type="text/css" href="external.css" />

    <!-- Internal CSS -->
    <style>
      div {
        font-size: 28px;
        color: red;
      }
    </style>
  </head>

  <body>
    <!-- 
        - External CSS is looked at (because it is first loaded in the <head>)
            - External CSS has no matching selector, nothing is applied
        - Internal CSS is looked at (because it is loaded second in the <head>)
            - Internal CSS has a matching selector: div
            - The styling `font-size: 28px;` and `color: red;` are applied
        - Inline CSS is applied
            - Element has no inline CSS, nothing is applied
    -->
    <div>Big red text</div>

    <!-- 
        - Exernal CSS is looked at (because it is first loaded in the <head>)
            - External CSS has no matching selector, nothing is applied
        - Internal CSS is looked at (because it is loaded second in the <head>)
            - Internal CSS has a matching selector: div
            - The styling `font-size: 28px;` and `color: red;` are applied
        - Inline CSS is applied
            - Element contains inline CSS
            - The styling `font-size: 28px;` and `color: deeppink;` are applied
            - This overrides the earlier applied styling `font-size: 28px;` and `color: red;`
    -->
    <div style="font-size: 28px; color: deeppink;">Big pink text</div>

    <!--
        - External CSS is looked at (because it is first loaded in the <head>)
            - External CSS has a matching selector: .blue (this equals to class='blue')
            - The styling `font-size: 28px;` and `color: blue;` are applied
        - Internal CSS is looked at (because it is loaded second in the <head>)
            - The styling is NOT applied, a class selector is more specific than an element selector
        - Inline CSS is applied
            - Element has no inline CSS, nothing is applied
    -->
    <div class="blue">Big blue text</div>
  </body>
</html>
```

Result:

![CSS Cascading](/img/blog/css-cascading.en.png)

## Selectors

With inline CSS the CSS is directly applied to the element on which the style attribute is placed.

With internal and external CSS selectors must be used to apply CSS rules on elements.

### Element

It is possible to select elements based on the name of an element. The `body` element can be selected with the word `body`. A `div` element can be selected with the word `div`.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* 
      * This will select ALL elements with the tag <body></body>
      */
      body {
        /* Change the background color to black */
        background-color: black;
      }

      /* 
      * This will select ALL elements with the tag <div></div>
      */
      div {
        /* Change the background color to pink */
        background-color: deeppink;
        /* Change the margin (the white space around the element) to 10px (default 0px) */
        margin: 10px 10px 10px 10px;
      }
    </style>
  </head>

  <body>
    <div>First element</div>
    <div>Second element</div>
    <div>Third element</div>
  </body>
</html>
```

![CSS selector element](/img/blog/css-selector-element.en.png)

### class

It is possible to select elements based on the class of an element.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* 
      * This selector will select ALL elements with the class 'element'
      */
      .element {
        background-color: deeppink;
        margin: 10px 10px 10px 10px;
      }
    </style>
  </head>

  <body>
    <div class="element">First element</div>
    <div class="element">Second element</div>
    <div class="element">Third element</div>
  </body>
</html>
```

![CSS selector class](/img/blog/css-selector-class.en.png)

### id

It is possible to select elements based on the id of an element.

```html
<!DOCTYPE html>
<html lang="nl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* 
      * This selects ALL elements with the class 'element'
      */
      .element {
        background-color: deeppink;
        margin: 10px;
      }

      /*
      * This selects ALL elements with the id 'element-2'
      */
      #element-2 {
        background-color: blue;
        color: white;
      }
    </style>
  </head>

  <body>
    <div class="element">First element</div>
    <!-- 
      Note: The second div has both the class 'element' and the id 'element-2'. Because CSS applies the styling in a cascading way, it will first apply the styling of the class '.element' and then the styling of the id '#element-2'. 
    -->
    <div class="element" id="element-2">Second element</div>
    <div class="element">Third element</div>
  </body>
</html>
```

![CSS selector id](/img/blog/css-selector-id.en.png)

## Box model

Box model is the term used to describe the `content`, `padding`, `border` and `margin`.

When viewing the `inspector` of the browser, the box model can be viewed. The `inspector` is opened by right-clicking in the browser and then clicking `inspect`.

When an element is clicked in the DOM, the box model is shown in the browser. The colors correspond to the box model that can be seen at the bottom right. The colors are:

- `content`: the blue color
- `padding`: the purple color
- `border`: the dark gray color
- `margin`: the yellow color

![Box model](/img/blog/box-model.png)

Changes can be added to the selected element in the browser.

```css
element {
  /* Add CSS here */
}
```

To better illustrate the box model, CSS can be added to the selected element.

![Box model custom](/img/blog/box-model-custom.png)

The padding is now 20 pixels.
The border now has a red color and is 1px in size.
The mapping is now 40 pixels.

Translated with www.DeepL.com/Translator (free version)
