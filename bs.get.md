# `[@bs.get]`

`bs.get` is used to get a property of an object.

For example, say you created a `div` element:

```reason
type element;
[@bs.scope "document"] [@bs.val] external createElement: string => element = "";

let div = createElement("div");
```

How can we access its properties, e.g. `scrollTop`?

Doing this doesn't work:

```reason
let top = div.scrollTop; // Error: The record field scrollTop can't be found.
```

That's what `bs.get` is for:

```reason
[@bs.get] external scrollTop: element => float = "";

let top = scrollTop(div);
```

which compiles to:

```js
var top = div.scrollTop;
```

Note how we defined `scrollTop` as a function that takes an `element`, and BS compiled it to `element.scrollTop`.