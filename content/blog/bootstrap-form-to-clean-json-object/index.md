---
title: "[Web Dev] Bootstrap Wizard that produces a clean JSON Object"
date: "2021-12-09T03:10:00.000Z"
description: "A simple HTML-CSS-JS Wizard that guides the user through a multi-tab form and ends in a clean JSON object. Server-side ready."
categories: [webdev]
comments: true
image:
  feature: https://media.slid.es/uploads/kouceylahadji-1/images/174949/json_logo-555px__1_.png
  credit: kouceylahadji
---

![Smithsonian Image](https://images.unsplash.com/photo-1610986602538-431d65df4385?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1207&q=80)

## What's the use ?

Willing to separate most efficiently the server-side logic from the client-side, i wanted to showcase a simple bootstrap 5 based wizard.

### I used

- Bootstrap 5 CSS
- Bootstrap 5 JS Bundle (with popper.js)
- [Mohsen's JSON Formatter](https://azimi.me/json-formatter-js/) (thanks to him for the great work)

### The workflow

Using a nav-tab container [based on bootstrap documentation](https://getbootstrap.com/docs/5.1/components/navs-tabs/#javascript-behavior), i splitted a basic form in order to lighten the text seen by the user. This helps categorizing the information gathered and, also, this becomes a wizard this way.

At the end, the form isn't submitted like a standard HTML form.
1. A new submit function called `dataToJSON()` is triggered
2. A sub-function is then browsing the `#wizard` form for `input` and `select` elements
    - note that and exception is made for `checkbox` and `radio` types simply because i needed to test whether these had `checked == true` before considering the value submitted
3. Another sub-function called `populateObj()` takes four parameters from the browsing function : an object `obj`, the index `index` of the browsing function, the `id` of the input, the `value` of the input so it can populate/hydrate the object based on the form values

**The browsing function :**
```
_formData.forEach(data => {
        if (data.localName == "input" && data.type != "radio" && data.type != "checkbox") {
            populateObj(_objData, i, data.id, data.value);
        } else if (data.type == "radio" && data.checked == true) {
            populateObj(_objData, i, data.id, data.value);
        } else if (data.type == "checkbox" && data.checked == true) {
            populateObj(_objData, i, data.id, data.value);
        } else if (data.localName == "select") {
            populateObj(_objData, i, data.id, data.value);
        }
        i++;
    });
```
**The populate function :**
```
function populateObj(obj, index, id, value) {
    obj[index] = {
        "id": id,
        "value": value
    };
}
```

### See the result in a CodePen

<iframe height="686" style="width: 100%;" scrolling="no" title="JSON Wizard" src="https://codepen.io/hotlinedelite/embed/abLZZEj?default-tab=result&theme-id=light" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/hotlinedelite/pen/abLZZEj">
  JSON Wizard</a> by tleclercq (<a href="https://codepen.io/hotlinedelite">@hotlinedelite</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>