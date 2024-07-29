## terminologies and objects

### [cascade and origin](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade#origin_types) -- the specificity issue prerequisite
Cascade: algorithm that defines how user agents combine property values originating from different sources.
- defines the origin and layer that takes precedence with declarations
- core of CSS (the name suggests it)
Rule of thumb: when a selector matches an element -> property value from the origin with the highest precedence gets applied (even if that of lower precedence has more specificity)

origin types of css (the stylesheets)
- user-agent stylesheet: default style sheets from browsers (as user-agents)
- author style sheets: from web developers
- user stylesheets: some websites, users can choose to override the styles using a custom user stylesheets (dark theme, accessibility...)

cascading order:
1. relevance
2. origin and importance
3. specificity
4. scoping proximity
5. order of appearance

order of precedence by origin and importance for criteria 2:
1. user-agent normal styles
2. user normal styles
3. author normal styles
4. styles being animated (@keyframe animations)
5. author important styles
6. user important styles
7. user-agent important styles
8. styles being transitioned

### Selectors
> A selector targets HTML to apply styles to content

A selector list, combining the selectors, separated by comma:
```css
h1, .special {
  color: blue;
}
```

Types of selectors:
- type selectors: target an HTML element such as `h1`, [universal selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Type_Class_and_ID_Selectors#type_selectors) *
- Class selectors target an element that has a specific value for its class attribute `.class_selector`
- ID selectors target an element that has a specific value for its id attribute `#unique`
- attribute selectors: this group of selectors gives you different ways to select elements based on the presence of a certain attribute on an element
- pseudo-classes  style certain states of an element `a:hover`
- pseudo-elements select a certain part of an element rather than the element itself: `p::first-line`
- combinators: combine other selectors in order to target elements within our documents: `article > p`

Class selectors hacks:
- target classes on particular elements:
```css
span.highlight {
  background-color: yellow;
}

h1.highlight {
  background-color: pink;
}
```
- target elements with more than one classes applied:
```html
<div class="notebox">
    This is an informational note.
</div>

<div class="notebox warning">
    This note shows a warning.
</div>

<div class="notebox danger">
    This note shows danger!
</div>

<div class="danger">
    This won't get styled — it also needs to have the notebox class
</div>
```
```css
.notebox {
  border: 4px solid #666;
  padding: .5em;
}

.notebox.warning {
  border-color: orange;
  font-weight: bold;
}

.notebox.danger {
  border-color: red;
  font-weight: bold;
}
```

ID selectors hacks:
- Using the same ID multiple times in a document may appear to work for styling purposes, but don't do this. It results in invalid code, and will cause strange behavior in many places. (an ID can be used only once per page, and elements can only have a single id value applied to them)

[Attribute selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Attribute_selectors) hacks:
- presence and value selectors:selection of an element based on the presence of an attribute alone (for example href), or on various different matches against the value of the attribute.
  - `a[title]`
  - equal: `a[href="https://example.com"]`
  - exactly or contains a value: `p[class~="special"]`
  - exactly value or begins with value immediately followed by a hyphen `div[lang|="zh"]`
- substring matching selectors:
  - Matches elements with an attribute, whose value begins with value. `li[class^="box-"]`
  - Matches elements with an attr attribute whose value ends with value. `li[class$="-box"]`
  - Matches elements with an attr attribute whose value contains value anywhere within the string. `li[class*="box"]`
- use flag i `li[class^="a" i]` to achieve case-insensitivity

Combinators note:
- descendant combinator select all elements matched by the second selector `body article p`
- child combinator (`>`) matches only those elements matched by the second selector that are the direct children of elements matched by the first. `article > p`
- next-sibling combinator (`+`) matches only those elements matched by the second selector that are the next sibling element of the first selector. `p + img`
- subsequent-sibling combinator (`~`) select siblings of an element even if they are not directly adjacent. `p ~ img`
- css nesting modules can create complex combinators, tho idk why need this
- but declaring too specific selectors with combinators will be hard to reuse the CSS rules since you have made the selector very specific to the location of that element in the markup.

Cascade Specificity - when two selectors select the same HTML element, CSS use rules to select the stronger selector
- more info [here](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)
Rules:
- cascade rule: when two rules from the same cascade layer apply and both have equal specificity, the one that is defined last in the stylesheet is the one that will be used.
- specificity: between class and element, specificity says that class is more specific than element selector, so it override element selector's style
- inheritance: some CSS property values set on parent elements are inherited by their child elements, and some aren't, the former is most likely due to a different style is applied directly to them (not speicificity, since they are different elements)

More intricacies concerning cascade, specificity, and inheritance:
- controlling inheritance: some CSS property cannot be inherited, but CSS provides five special universal property __values__ for controlling inheritance (`inherit`, `unset`, `initial`, `revert`, `revert-layer`)
- shorthand `all` __property__ applies one of these inheritance values to (almost) all properties at once
- inheritance is the reason why nested elements appear to have the same property values as parent element, but there are some rules in which property values are applied: source order -> specificity -> importance
- source order: more than one rule, all of which have exactly the same weight, then the one that comes last in the CSS will win.
- when they have different weights, look at specificity: selectors with more weights
- selector rule vs a singular property when thinking about specificity rule: "Something to note here is that although we are thinking about selectors and the rules that are applied to the text or component they select, it isn't the entire rule that is overwritten, only the properties that are declared in multiple places." E.g.
```css
/* the element selector here isn't entirely overidden */
h2 {
  font-size: 2em; /* font-size is overidden by the more specific class selector "small" */
  color: #000; /* color is overidden by the more specific class selector bright */
  font-family: Georgia, 'Times New Roman', Times, serif;
}

.small {
  font-size: 1em;
}

.bright {
  color: rebeccapurple;
}
```
```html
<h2>Heading with no class</h2>
<h2 class="small">Heading with class of small</h2>
<h2 class="bright">Heading with class of bright</h2>
```
- inline styles are more specific than anything else
- !important flag after a property value will cause this property to override whatever that is before it, E.g.
```css

#winning {
  background-color: red;
  border: 1px solid black;
}

.better {
  background-color: gray;
  border: none !important; /* nothing will show for border */
}

p {
  background-color: blue;
  color: white;
  padding: 5px;
}
```
```html
<p class="better">This is a paragraph.</p>
<p class="better" id="winning">One selector to rule them all!</p>
```

### cascade layers

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
