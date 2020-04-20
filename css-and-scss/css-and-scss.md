# CSS and SCSS

## Architecture

### Reset and normalize

Easy reset using [reset-css](https://www.npmjs.com/package/reset-css). Install and import `@import 'reset-css';`

```bash
npm install reset-css
```

Set global border-box and height

```scss
html {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: inherit;
}

html,
body {
  height: 100%;
}
```

### Structuring sass projects

- [Structuring your sass projects](https://itnext.io/structuring-your-sass-projects-c8d41fa55ed4)

## Responsive

### Media-queries mixins

- [Writing media-queries with sass mixins](https://itnext.io/writing-media-queries-with-sass-mixins-3ea591ea3ea4)

## CSS GRID

- [Grid Malven - CSS Grid visual web guide](http://grid.malven.co/)

### Preventing a grid blowout

- [Preventing a grid blowout](https://css-tricks.com/preventing-a-grid-blowout/)

### Without media queries

- [Look Ma, No Media Queries! Responsive Layouts Using CSS Grid](https://css-tricks.com/look-ma-no-media-queries-responsive-layouts-using-css-grid)

### Prevent content overflow

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

### Control scroll in elements

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
