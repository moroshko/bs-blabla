# `[@bs.set]`

`bs.set` is used to set a property of an object.

For example, say you created a `div` element:

```reason
type element;
[@bs.scope "document"] [@bs.val] external createElement: string => element = "";

let div = createElement("div");
```

How can we set its properties, e.g. `scrollTop`?

Doing this doesn't work:

```reason
div.scrollTop = 100; // Error: The record field scrollTop can't be found.
```

That's what `bs.set` is for:

```reason
[@bs.set] external setScrollTop: (element, int) => unit = "scrollTop";

setScrollTop(div, 100);
```

which compiles to:

```js
div.scrollTop = 100;
```

Note how we defined `scrollTop` as a function that takes an `element` and a `value`, and BS compiled it to `element.scrollTop = value`.