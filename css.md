---
layout: default
title: css
---

# grid module
- Two dimension stream.
- Layout suitable for large zones.

# flexbox module
- Single dimension stream.
- Layout suitable for small zones.

Format:
- `display: flex` sets the target to be a 'flex container', every child element within it behaves as a 'flex item' 
- there are two keys, main and cross, with three properties each: axis, start-end, size
  - `main axis`: `main start-end`, `main size`
  - `cross axis`: `cross start-end`, `cross size`
- flex items are placed in the flex-direction layout, not the block layout or inline layout; 
- `flex-direction` can be set to `row` or `column`
  - `row` is the default
  - reverse options exist
- flex items can follow logical order expected from reading a book, from left to right, top to bottom when `flex-direction` is set to `row`, and `flex-wrap` is set to `wrap`; without `flex-wrap`, flex items would continue to be placed in the direction of flex-direction even if they would overflow their container.

The __macro arrangement__ of flex items is governed by __flex-direction__ in relation to the flex containers, flex items, the main axis, and the cross axis.
- for the given construct: a single column with three rows in it, there would be one flex container, three flex items, and the container's flex-direction set to column.
- in the same construct, the first flex item could itself be a flex container and contain a single row of elements, with the container's flex-direction set to row.

The __micro arrangement__ of flex items is governed by the __alignment module__[^1] in relation to the flex containers, flex items, main axis, cross axis and whether there is a single row of items or more than one row of items enabled through flex-wrap.
- `justify-content` applied onto the flex container will space flex items out along the main axis.
- `align-content` applied onto the flex container will space flex items out along the cross axis, only observable when there are two or more streams of items as provisioned by flex-wrap being enabled.
- the behaviour of `justify-content` and `align-content` can be thought of as adjusting the flex container's padding; adjusting the padding around the flex items inside the container
- `align-items` applied onto the flex container will apply its ruleset to the flex item as `align-self`, i.e, set the property with `align-items`, but look up `align-self` documentation for behaviour
- the behaviour of `align-self` can be thought of as adjusting the flex item's margin
- although the `justify-items` and `justify-self` properties exists, they aren't for flexbox!

[^1]: The alignment module has relations to the writing mode which encompasses the [block axis](https://www.w3.org/TR/css-writing-modes-4/#block-axis) and [inline axis](https://www.w3.org/TR/css-writing-modes-4/#inline-axis)

flexbox expansion content filling, overflow


flexibility

Quick Recipe 1:
```css
.container {
    display: flex;
}

.item {
    
}
```


Readings:
- [w3C: flexbox](https://www.w3.org/TR/css-flexbox/)
- [w3C: css box alignment](https://www.w3.org/TR/css-align/)
- [w3C: main-axis alignment](https://www.w3.org/TR/css-flexbox/#justify-content-property)
- [w3C: cross-axis alignment](https://www.w3.org/TR/css-flexbox/#justify-content-property)
- [w3C: flexibility](https://www.w3.org/TR/css-flexbox/#flexibility)
- [w3C: alignment reference](https://www.w3.org/TR/css-align/#overview)
- [w3C: distribution reference](https://www.w3.org/TR/css-align/#distribution-values)
- [css tricks: flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [css tricks: flexbox grow, shrink, basis](https://css-tricks.com/understanding-flex-grow-flex-shrink-and-flex-basis/)
- [mdn: flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [csswg-drafts: bleeding edge css features](https://github.com/w3c/csswg-drafts)