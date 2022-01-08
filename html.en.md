---
postUuid: 7c23a80c-e26a-424c-adf7-6a87bae20909
title: HTML
slug: html
tags:
  - HTML
categories:
  - Frontend
---

## Basic structure

HTML stands for HyperText Markup Language. It is a markup language used to create web pages, it describes the structure of a web page.

Elements exist out of begin and end tags.

An example of an element is `<title>Example 1</title>`, with `<title>` being the begin tag and `</title>` being the end tag.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Example 1</title>
  </head>
  <body>
    <h1>Basic structure</h1>
    <p>
      This example contains the essential elements to show a basic HTML page
      with a title and some content.
    </p>
  </body>
</html>
```

To view this as a web page:

- create a new file called `index.html`
- copy the content of the example above into the file
- save the file and open it in your browser

![basic structure](/img/blog/basic-structure.en.png)

## DOCTYPE

```html
<!DOCTYPE html>
<!-- ... -->
```

Everything between `<!-- -->` is ignored by the browser, this is called a comment.

The DOCTYPE is used to define the version of the HTML standard that the page is written in. This is how the browser knows what to do with the page, in this case, it will know that it is an HTML5 page.

`<!DOCTYPE html>` is the declaration for `HTML5`.

Older declarations are a bit more complicated:

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

This is because older declarations must refer to a DTD (Document Type Definition).

Since HTML5 is the default these days, it suffices to remember `<!DOCTYPE html>`.

## Elements

An element exists out of a begin and an end tag.

```html
<head>
  <title>Example 1</title>
</head>
```

`<head></head>` the `head`-element.
`<head>` the begin tag of the `head`-element.
`</head>` de end tag of the `head`-element.

There are some elements that are not closed by an end tag. These are called self-closing elements.

`<br>`

The `<br>`-element will add an empty line to the page.

A self-closing element can also be closed by a `/` at the end of the tag.

`<br />`

Both options are valid HTML5.

### `<head>`-tag

```html
<!-- ... -->
<head>
  <title>Example 1</title>
</head>
<!-- ... -->
```

The element `<head>` contains information about the page. In the example above, the title of the page `Example 1` can be found in the `<head>` section. The title element contains the content `Example 1`.

It is also possible to add other elemnts in the `<head>`-element:

- `<style>`-tags to refer to CSS files, which are used to style the page (e.g. a pink background instead of a white background). There will be a separate post about CSS.
- `<script>`-tags to refer to JavaScript files. There will be a separate post about JavaScript.
- `<meta>`-tags to add extra information about the page.

A couple of `<meta>`-tag examples:

```html
<!-- ... -->
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>
<!-- ... -->
```

The `charset` refers to the kind of character encoding that is used in the page. The `UTF-8` is the most common encoding.

The `viewport` refers to the size of the page. The `width=device-width` means that the page will be displayed in the same size as the device it is being viewed on. Which is necessary for mobile devices.

### Headings

To display different sizes of headings, use the `<h1>`-element to display a heading of level 1, `<h2>` for level 2, `<h3>` for level 3, and so on. This goes up to `<h6>` for level 6.

```html
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```

![headings](/img/blog/headings.en.png)

### Paragraphs

A paragraph shows that a piece of text belongs together.

```html
These two lines look identical in the browser, yet there is a difference!
<p>These two lines look identical in the browser, yet there is a difference!</p>
```

De second line describes to the browser that the text that is shown, is a paragraph. The first line simply shows the text, the browser does not get any information about what the text means.

![paragraph](/img/blog/paragraphs.en.png)

### Links

Through links, hyperlinks, it is possible to link to other elements or total different pages.

```html
<a href="https://9gag.com/gag/a9W9WyL">This is the shown text.</a>
```

Note that a link that is not yet visited, will be shown in blue. As soon as the link has been visited, it will be shown in purple. Dit happens automatically.

![link](/img/blog/link.en.png)
![visited link](/img/blog/link-visited.en.png)

### Images

```html
<img
  src="https://img-9gag-fun.9cache.com/photo/a9W9WyL_700bwp.webp"
  alt="A meme of someone"
/>
```

The `<img>`-tag is a self closing element.

`src` gives the `source` where the image can be found.

`alt` gives the **alt**ernative text that is shown when the image cannot be loaded.

## Attributes

Elements can have attributes. In the examples above this can be seen in a couple places already:

```html
<meta charset="UTF-8" />
<!-- charset is an attribute with the value "UTF-8" -->

<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<!-- name is an attribute with the value "viewport" -->
<!-- content is an attribute with the value "width=device-width, initial-scale=1.0" -->
```

```html
<a href="https://9gag.com/gag/a9W9WyL">This is the shown text.</a>
<!-- hreft is an attribute with as value the location of the website -->
```

```html
<img
  src="https://img-9gag-fun.9cache.com/photo/a9W9WyL_700bwp.webp"
  alt="A meme of someone"
/>
<!-- src is an attribute with as value the image that must be shown -->
<!-- alt is an attribute with as value the text that must be shown -->
```

## Lists

There are two types of lists:

- Lists with ordered items.
- Lists with unordered items.

**O**rdered **L**ist, `<ol>`-element is used to show a list with numbers.

**U**nordered **L**ist, `<ul>`-element is used to show a list without numbers.

In both cases an item can be added with the **L**ist **I**tem: `<li>`.

```html
<h1>Students</h1>

<ul>
  <li>Huey</li>
  <li>Dewey</li>
  <li>Louie</li>
</ul>

<h1>Students</h1>

<ol>
  <li>Huey</li>
  <li>Dewey</li>
  <li>Louie</li>
</ol>
```

## Tables

A table is a collection of several elements.

`<table>`: in between the begin and end tag of the `table`-element are all the other elements
`<tr>`: **t**able **r**ow, this refers to a row of the table
`<th>`: **t**able **h**eader, this refers to the header of the table
`<td>`: **t**able **d**ata, this refers to a single cell in the table

The elements `<th>` and `<td>` are both one cell in the table. The difference is that `<th>` will make the content bold and centered. `<td>` will align the content to the left without making it bold.

The borders of the table can be made visible through CSS. This is not handled in this post.

```html
<table>
  <tr>
    <th>First name</th>
    <th>Last name</th>
    <th>Age</th>
  </tr>
  <tr>
    <td>Bart</td>
    <td>Duisters</td>
    <td>29</td>
  </tr>
  <tr>
    <td>Mark</td>
    <td>Duisters</td>
    <td>29</td>
  </tr>
</table>
```

![table](/img/blog/table.en.png)

```html
<table>
  <tr>
    <th colspan="2">Name</th>
    <th>Age</th>
  </tr>
  <tr>
    <td>Bart</td>
    <td rowspan="2">Duisters</td>
    <td>29</td>
  </tr>
  <tr>
    <td>Mark</td>
    <td>29</td>
  </tr>
</table>
```

![table with colspan and rowspan](/img/blog/table-span.en.png)

The attributes `colspan` and `rowspan` can be used to make the table more flexible, by defining how many cells a cell should span.

## Text formatting

It is possible to style text with HTML elements.
It is also possible to style text with CSS (not handled in this post).

```html
<b>b - bold</b>
```

```html
<strong>strong - important text</strong>
```

```html
<i>i - italic</i>
```

```html
<em>em - emphasized</em>
```

```html
sub <sub> subscript</sub> & sup <sup>superscript</sup>
```

```html
<mark>mark - highlight</mark>
```

```html
<del>del - delete</del>
```

```html
<ins>ins - insert</ins>
```

![text formatting](/img/blog/text-formatting.en.png)

## Elements: block & inline

It is important to understand that placing HTML elements in a .html file, does not mean that it will be shown at that place on the page in the browser.

HTML elements have a default `display` property. There are two possible values: `block` and `inline`.

Placing two block elements under or next to each other in the .html file has nothing to do with how it will be shown in the browser. They will be shown under each other in the browser.

Placing two inline elements under or next to each other in the .html file has nothing to do with how it will be shown in the browser. They will be shown next to each other in the browser.

### Block

An element with as `display` property `block` will be shown on a new line and takes up the full width of the available room (left and right).

An example is the `<div>`-element.

```html
<div>First element</div>
<div>Second element</div>
```

![div](/img/blog/div.en.png)

All elements with as `display` property `block`:

`<address>`
`<article>`
`<aside>`
`<blockquote>`
`<canvas>`
`<dd>`
`<div>`
`<dl>`
`<dt>`
`<fieldset>`
`<figcaption>`
`<figure>`
`<footer>`
`<form>`
`<h1>-<h6>`
`<header>`
`<hr>`
`<li>`
`<main>`
`<nav>`
`<noscript>`
`<ol>`
`<p>`
`<pre>`
`<section>`
`<table>`
`<tfoot>`
`<ul>`
`<video>`

### Inline

An element with as `display` property `inline` place the content on the same line and take as much room as is necessary to show the content.

An example is the `span`-element.

```html
<span>First element</span> <span>Second element</span>
```

![span](/img/blog/span.en.png)

All elements with as `display` property `inline`:

`<a>`
`<abbr>`
`<acronym>`
`<b>`
`<bdo>`
`<big>`
`<br>`
`<button>`
`<cite>`
`<code>`
`<dfn>`
`<em>`
`<i>`
`<img>`
`<input>`
`<kbd>`
`<label>`
`<map>`
`<object>`
`<output>`
`<q>`
`<samp>`
`<script>`
`<select>`
`<small>`
`<span>`
`<strong>`
`<sub>`
`<sup>`
`<textarea>`
`<time>`
`<tt>`
`<var>`

## Semantic elements

Compare the HTML below, specifically look at the elements in the `<body>`-element.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <div class="header">A header</div>
    <div class="nav">
      <a href="#section1">Go to section 1</a>
      <a href="#section2">Go to section 2</a>
    </div>
    <div id="section1">
      <h1>Section 1</h1>
      <div>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </div>
    </div>
    <div id="section2">
      <h1>Section 2</h1>
      <div>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </div>
    </div>
    <div class="footer">A footer</div>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <header>A header</header>
    <nav>
      <a href="#section1">Go to section 1</a>
      <a href="#section2">Go to section 2</a>
    </nav>
    <section id="section1">
      <h1>Section 1</h1>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </p>
    </section>
    <section id="section2">
      <h1>Section 2</h1>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce sit amet
        sem erat. Phasellus pellentesque nisl lorem, a lacinia dolor lacinia at.
        Maecenas interdum sapien ut tellus porttitor pellentesque. Nam nec risus
        vitae lacus porttitor porta. Fusce vitae dolor vel lorem aliquet
        porttitor varius ut odio. Nulla vel neque mi. Quisque et magna ut libero
        semper luctus. Phasellus interdum libero vel dolor tincidunt pulvinar.
        Curabitur commodo condimentum facilisis. Ut tempus tortor in sodales
        dapibus. Nam suscipit nisl non purus aliquam ornare. Donec vestibulum
        dignissim lorem, vitae venenatis enim finibus vel. Nulla scelerisque
        laoreet ligula et hendrerit. Sed ac porta ligula.
      </p>
    </section>
    <footer>A footer</footer>
  </body>
</html>
```

Both examples are valid HTML. The difference is that the first example uses `<div>` elements.
The second example uses `semantic` elements. `Semantic element` means that the name of the element describes what the purpose of the element is.

Semantic elements are easier to index by a search engine. This potentially makes the page better for SEO (Search Engine Optimization).
