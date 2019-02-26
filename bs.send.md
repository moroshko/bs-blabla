# `[@bs.send]`

`bs.send` is used to call a function inside an object.

JS example:

```js
element.appendChild(anotherElement);
```

You type and use it like so:

```reason
fn(obj, arg1, arg2, arg3, ...)
```

and BS compiles it to:

```js
obj.fn(arg1, arg2, arg3, ...)
```

For example:

```reason
type element;
[@bs.scope "document"] [@bs.val] external createElement: string => element = "";
[@bs.send] external appendChild: (element, element) => element = "";

let div = createElement("div");
let button = createElement("button");

appendChild(div, button)
```

compiles to:

```js
var div = document.createElement("div");
var button = document.createElement("button");

div.appendChild(button);
```
