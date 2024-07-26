## terminologies and objects
### Selectors

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
