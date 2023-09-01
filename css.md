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

The `flex` property applied to flex items, manages their sizing.  `flex` is shorthand for three components working together --- `flex-grow`, `flex-shrink`, `flex-basis` --- so the declaration should always be with `flex` and not the individual components.

Free space is the relationship of the lengths between the parent container and the item inside it.  To calculate the free space, take the sum of item lengths and subtract it from the length of the parent container: `parent_container_length - Σ(item_1_length, item_2_length, ...item_n_length)`.  When the value is positive, this is called positive free space; when the value is negative, negative free space.

`flex-grow` relates the flex item sizes to positive free space in the main axis. The flex-item will first take on width/height properties assigned to it, then it will become inflexible or flexible when `flex-grow` is set to `0` or `1` respectively.  When inflexible, the flex item behaves like a block element.  When flexible, the flex item grows, __increasing its length__ to take up all the positive free space available in the main axis.  When all flex items share the same positive integer for the `flex-grow` property, they relate to each other in the same proportion, so nothing observable happens beyond the initial expansion to take up all positive free space.  If the flex container has a positive free space of 200px, and there are four flex items each with `flex-grow: 1`, then the free space distributed to each flex item is 200px/4 portions -> 50px/portion.  If instead one of the four flex items has `flex-grow: 2`, then there are five portions to distribute, 200px/5 portions --> 40px/portion.  Since the ratio is 1:1:1:2, then three of the flex items receive 40px each, and one flex item receives two portions worth, 80px.  Note: behaviour from `flex-grow` only takes effect when there is positive free space, when there is negative free space, behaviour is taken from `flex-shrink`.

`flex-shrink` relates the flex item sizes to negative free space in the main axis.  The flex-item will first take on the width/height properties assigned to it, then it will become inflexible or flexible when `flex-shrink` is set to `0` or `1` respectively.  When inflexible, the flex item line will overflow its parent container.  When flexible, the flex item will shrink, __decreasing its length__ so that there is no negative space left.  When all flex items share the same positive integer for the `flex-shrink` property, they relate to each other in the same proportion, so nothing observable happens beyond the initial contraction to remove all negative free space.  If the flex container has a negative free space of 200px, and there are four flex items each with `flex-shrink: 1`, then the free space subtracted from each flex item is 200px/4 portions -> 50px/portion.  If instead one of the four flex items has `flex-shrink: 2`, then there are five portions to distribute, 200px/5 portions --> 40px/portion.  Since the ratio is 1:1:1:2, then three of the flex items receive 40px each, and one flex item receives two portions worth, 80px.  Note: behaviour from `flex-shrink` only takes effect when there is negative free space, when there is positive free space, behaviour is taken from `flex-grow`.

`flex-basis`

flex wording | equivalent shorthand | summary | behaviour
|:-|:-|:-|:
flex: initial | flex: 0 1 auto | keep sizing but shrink when parent container shrinks | when Σ(flex item lengths) < parent container, flex item lengths are specified via their width/height property corresponding to the main axis; when Σ(flex item lengths) > parent container, flex item lengths shrink to fit their parent container; 
flex: auto | flex: 1 1 auto | consume all positive space, reduce all negative space | when Σ(flex item lengths) < parent container, flex item lengths grow to fit their parent container; when Σ(flex item lengths) > parent container, flex item lengths shrink to fit their parent container;
flex: none | flex: 0 0 auto | behave like a block, | keep sizing, if there is positive free space, parent container might feel roomy; if there is negative free space, items may overflow their parent container
flex: positive_number | flex: positive_number 1 0

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
- [mdn: gap](https://developer.mozilla.org/en-US/docs/Web/CSS/gap)
- [csswg-drafts: bleeding edge css features](https://github.com/w3c/csswg-drafts)