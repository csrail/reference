---
layout: default
title: js
---

## Understanding .bind

- The below code block demonstrates how `this` changes inside a constructor function of a __class__ and a __callback function__.
- The statement on line 15 will affect how line 20 is parsed.
- Without `.bind(this)`, the `this` in `#removeBook` refers to the event that was triggered when clicking `#button`.
- With `.bind(this)`, the `this` in `#removeBook` refers to the `LibraryController` instance.
```js
class LibraryController {
    #library = []
    #books = [
        new Book(),
        new Book(),
        new Book(),
    ]
    #button = document.querySelector('button')
    
    constructor() {
        for (const book of #books) {
            this.#library.push(book)
        }
        
        this.#button.addEventListener('click', this.#removeBook.bind(this))
    }
    
    #removeBook() {
        // logic here to get the index of the book to remove from the #this.library model
        this.#library.splice(index, 1)
        // logic here to remove the node object from the GUI display
    }
}
```