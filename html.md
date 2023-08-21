---
layout: default 
title: html 
---

## Using async and defer in the script tag when loading javascript file
```html
<script src="script.js"></script>
<script src="script.js" defer></script>
<script src="script.js" async></script>
```

- `no option` html loading pauses while js script is fetched; when the js file is fetched it's executed, then the html file unpauses and continues loading.
- `defer` the html loads; the js script is fetched at the same time; the js file executes only after the html fully loads.
- `async` the html loads; the js script is fetched at the same time; when the js file fetch finishes, the js file is executed immediately.

[Diagram](https://html.spec.whatwg.org/images/asyncdefer.svg)