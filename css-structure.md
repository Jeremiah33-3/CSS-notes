## terminologies and objects
### Selectors
> A selector targets HTML to apply styles to content

Cascade Specificity - when two selectors select the same HTML element, CSS use rules to select the stronger selector
- cascade rule says that the later style will override previous ones
- between class and element, specificity says that class is more specific than element selector, so it override element selector's style
- more info [here](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)

### Properties and values
> Properties: These are human-readable identifiers that indicate which stylistic features you want to modify. For example, font-size, width, background-color. Values: Each property is assigned a value. This value indicates how to style the property.

These are the basic CSS components. When a property is paired with a value, this pairing is called a CSS declaration. They are case-insensitive, and separated by a colon.

Some values can exist in the form of functions:
- calc(), [transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) property's functions

### @rules
> CSS @rules (pronounced "at-rules") provide instruction for what CSS should perform or how it should behave. Some @rules are simple with just a keyword and a value. For example, @import imports a stylesheet into another CSS stylesheet.

For example:
- `@import` rule can import a stylesheet into another stylesheet
- `@media` create [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries), which uses conditional logic for applying CSS styling. E.g.
```css
body {
  background-color: pink;
}

@media (min-width: 30em) {
  body {
    background-color: blue;
  }
}
```

### Shorthands
> Some properties like font, background, padding, border, and margin are called shorthand properties. This is because shorthand properties set several values in a single line.

Sort of like a function accepting multiple arguments
E.g.
```css
/* In 4-value shorthands like padding and margin, the values are applied
   in the order top, right, bottom, left (clockwise from the top). There are also other
   shorthand types, for example 2-value shorthands, which set padding/margin
   for top/bottom, then left/right */
padding: 10px 15px 15px 5px;
```
which is equivalent to:
```css
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

### Comments
No comments, just `/* */`



## Miscellaneous 
To change default behaviour of elements: 
```css
li {
  list-style-type: none;
}
```

Styling things based on their location in a document;
- descendant combinator:
```css
li em {
  color: rebeccapurple; /* select only <em> element inside <li> tag */
}
```
- next-sibling combinator
```css
h1 + p {
  font-size: 200%; /* select <p> element next to the h1 element (same indentation) */
}
```

Styling things based on states:
- html elements might have different states (e.g. <a> (anchor) element. This has different states depending on whether it is unvisited, visited, being hovered over, focused via the keyboard, or in the process of being clicked (activated).)
```css
a:link {
  color: pink;
}

a:visited {
  color: green;
}

a:hover {
  text-decoration: none;
}
```

You can also combine combinators and selectors together:
```css
/* selects any <span> that is inside a <p>, which is inside an <article>  */
article p span {
}

/* selects any <p> that comes directly after a <ul>, which comes directly after an <h1>  */
h1 + ul + p {
}

/* multiple types */
body h1 + p .special {
  color: yellow;
  background-color: black;
  padding: 5px;
}
```
