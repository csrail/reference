---
layout: default
title: programming
---

# Object oriented programming
```js
const Consumable = (obj = {}) => {
  const name = obj['name'];
  const description = obj['description'];
  const price = obj['price'];
  const { getCourse } = Food(obj);
  const { getStyle } = Beverage(obj['style']);

  const getName = () => { return name }
  const getDescription = () => { return description }
  const getPrice = () => { return price}
  
  return { getName, getDescription, getPrice }
```

Consumable was initially defined with positional parameters to create a Consumable object with properties, something like `const Consumable = (name, description, price) => {}`.  This architecture works with the expectation that the arguments received are always in the same order and format.  'Order' defined here in terms of parameter `a_1` being in argument position 1, parameter `a_2` being in argument position 2, parameter `a_3` being in argument position 3, stated: `a_1, a_2, a_3, ...a_n : 1, 2, 3, ...n`. 'Format' defined  in terms of the expectation the data type of `a_1, a_2, ...a_n` will always be the data type we expect of them.  This expectation of the 'same order and format' cannot be satisfied if there is a requirement for the program to be adapted and expanded to meet the changing needs of users: i.e. any software used in production with a strong user base.  Keywords in argument for better architecture: 'adapted and expanded', 'changing needs'.

To improve upon this architecture, Consumable is defined to use a single object parameter instead of positional parameters: `const Consumable = (obj = {}) => {}`.  `obj` is a symbol, and the human-readable expectation is that `obj` is an object so the argument will be used in that manner.  However, to Consumable, `obj` is just a symbol and Consumable is ignorant beyond what `obj` actually is.  It should be noted that Consumable can be initialised without an `obj` argument by setting the default value of the `obj` parameter to `{}`, an empty object.  This is useful because Consumable can now be initialised, without arguments "needing to be needed", and passed to other objects to share its methods -- this pattern seems familiar... a mixin pattern perhaps?  The parameters `a_1, a_2, a_3, ...a_n` have been encapsulated into a single parameter `obj`, and their values can be extracted by assigning the values to properties; the object passed into Consumable will take the form of: `{k_1: a_1, k_2: a_2, k_3: a_3, ...k_n: a_n}`, where `k` refers to a named property the value `a` belongs to.  Values can then be set to variables inside Consumable through an object-key pattern `const k_1 = obj['k_1']` which equates to `a_1`; more formally stated: `const k = obj['k']` gets `a`.

Two objects understand the existence of the `k` property: the anonymous object passed as an argument to Consumable, and Consumable as an object.  If Consumable were passed to another object and the `k` variable made available (declared as `const k = obj['k'])`, whenever the value `a` is needed, you could call `k` in the new object.  The new object calling `k`, would then also be indirectly tied to the `k` property.  At least three objects now know about the existence of `k` property, if another object uses Consumable's `k` property, then the count for objects knowing the existence of `k` property increases by 1.

To state the case clearly:
- `property_k` is contained in an anonymous object.
- Consumable object takes anonymous object as an argument.
- Consumable object takes `property-k` and declares the variable `constant-k`, initialising it with the value `object[property-k]`.
- Consumable exposes `constant-k`, making it available for calling if other objects include Consumable in its composition.
- A new X object is initialised which includes Consumable, the X object calls `constant-k` to do something.
- Another Y object is initialised which includes Consumable, the Y object calls `constant-k` to do something.
- Both X and Y indirectly know about `property-k` through this chaining.
- At least four objects know about `property_k` either directly or indirectly: the anonymous object, Consumable, X and Y.
- A Z object is initialised and includes Consumable, the Z object calls `constant-k` to do something.
- Five objects now know about `property_k`: the anonymous object, Consumable, X, Y and Z.

This handling of `property_k` can be improved.  This is achieved by decreasing the exposure of `property_k` through a getter function.  The getter function returns the value of `constant-k` which means a callable function now represents `property-k` whereas previously a callable variable `constant-k` represented `property-k`.  `constant-k` is now wrapped inside a `get-constant_k` getter function: this makes objects composed with Consumable and needing to use `constant-k` ignorant of `property-k`, these objects only know about `get-constant_k`.

separation
variable
`Food(obj)`


Readings 
- []()