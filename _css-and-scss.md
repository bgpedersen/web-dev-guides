# CSS and SCSS

## Contents

- [CSS and SCSS](#css-and-scss)
  - [Contents](#contents)
  - [Architecture](#architecture)
    - [Architecture: Reset and normalize](#architecture-reset-and-normalize)
    - [Architecture: Structuring your sass projects](#architecture-structuring-your-sass-projects)
  - [Responsive](#responsive)
    - [Responsive: Writing media-queries with sass mixins](#responsive-writing-media-queries-with-sass-mixins)
  - [CSS GRID](#css-grid)
    - [CSS GRID: Preventing a grid blowout](#css-grid-preventing-a-grid-blowout)
    - [CSS GRID: Responsive Layouts Using CSS Grid](#css-grid-responsive-layouts-using-css-grid)
    - [CSS GRID: Prevent content overflow grid container](#css-grid-prevent-content-overflow-grid-container)
  - [Tips and tricks](#tips-and-tricks)
    - [Tips and tricks: Control scroll in elements](#tips-and-tricks-control-scroll-in-elements)

## Architecture

### Architecture: Reset and normalize

Easy reset using [reset-css](https://www.npmjs.com/package/reset-css). Install `npm install reset-css --save`and import `@import '~reset-css';`
or create your own reset.css with

```css

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, menu, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
main, menu, nav, output, ruby, section, summary,
time, mark, audio, video {
 margin: 0;
 padding: 0;
 border: 0;
 font-size: 100%;
 font: inherit;
 vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
footer, header, hgroup, main, menu, nav, section {
 display: block;
}
/* HTML5 hidden-attribute fix for newer browsers */
*[hidden] {
    display: none;
}
body {
 line-height: 1;
}
menu, ol, ul {
 list-style: none;
}
blockquote, q {
 quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
 content: '';
 content: none;
}
table {
 border-collapse: collapse;
 border-spacing: 0;
}

```

### Architecture: Structuring your sass projects

- [Structuring your sass projects](https://itnext.io/structuring-your-sass-projects-c8d41fa55ed4)

## Responsive

### Responsive: Writing media-queries with sass mixins

- [Writing media-queries with sass mixins](https://itnext.io/writing-media-queries-with-sass-mixins-3ea591ea3ea4)

## CSS GRID

- [Grid Malven - CSS Grid visual web guide](http://grid.malven.co/)

### CSS GRID: Preventing a grid blowout

- [Preventing a grid blowout](https://css-tricks.com/preventing-a-grid-blowout/)

### CSS GRID: Responsive Layouts Using CSS Grid

- [Look Ma, No Media Queries! Responsive Layouts Using CSS Grid](https://css-tricks.com/look-ma-no-media-queries-responsive-layouts-using-css-grid)

### CSS GRID: Prevent content overflow grid container

Fix by using `max-width: 100%;` and `overflow: hidden;` on your child elements. Example:

```css
#imgEditContainer {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  column-gap: 5px;
  justify-items: center;
}

.img {
  max-width: 100%;
  max-height: 100%;
  overflow: hidden;
}
```

## Tips and tricks

### Tips and tricks: Control scroll in elements

Set overflow: hidden on body, to stop scroll set on body.

```css
body {
  overflow: hidden;
}
```

Add `overflow: auto` on parent element of the element that should have scroll. If using angular components, use :host to set overflow on the host div:

```scss
:host {
  overflow: auto;
}

.container {
  height: 5000px;
}
```
