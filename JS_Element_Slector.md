``` md


| Method                     | Selects By                     | Returns                                  | Type              | Live/Static | Use Case                                      | Example |
|----------------------------|--------------------------------|------------------------------------------|------------------|-------------|----------------------------------------------|---------|
| `getElementById()`         | `id`                           | Single Element or `null`                 | `HTMLElement`    | -           | Selecting a unique element by its ID.         | `document.getElementById("myDiv")` |
| `querySelector()`          | CSS Selector (`#id, .class, tag`) | First Matching Element or `null`   | `HTMLElement`    | -           | Selecting a single element using CSS.        | `document.querySelector(".myClass")` |
| `getElementsByClassName()` | `class`                        | HTMLCollection (Multiple Elements)       | `HTMLCollection` | **Live**    | Selecting multiple elements by class name.   | `document.getElementsByClassName("myClass")` |
| `getElementsByTagName()`   | `tag`                          | HTMLCollection (Multiple Elements)       | `HTMLCollection` | **Live**    | Selecting all elements of a certain tag.     | `document.getElementsByTagName("p")` |
| `querySelectorAll()`       | CSS Selector                   | NodeList (Multiple Elements)             | `NodeList`       | **Static**  | Selecting multiple elements using CSS.       | `document.querySelectorAll("div p")` |
| `getElementsByName()`      | `name` attribute               | NodeList (Multiple Elements)             | `NodeList`       | **Live**    | Selecting form elements by `name` attribute. | `document.getElementsByName("username")` |

```

### Using `getElementById()`
```js
let element = document.getElementById("myDiv");
console.log(element.innerText);
```

### Using `querySelector()`
```js
let element = document.querySelector(".myClass");
console.log(element.innerText);
```

### Using `getElementsByClassName()`
```js
let elements = document.getElementsByClassName("myClass");
console.log(elements[0].innerText);
```

### Using `getElementsByTagName()`
```js
let elements = document.getElementsByTagName("p");
console.log(elements[0].innerText);
```

### Using `querySelectorAll()`
```js
let elements = document.querySelectorAll("div p");
elements.forEach(el => console.log(el.innerText));
```

### Using `getElementsByName()`
```js
let elements = document.getElementsByName("username");
console.log(elements[0].value);
```
```

