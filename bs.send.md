# `[@bs.send]`

* [`[@bs.send]`](#bssend)
* [`[@bs.send.pipe]`](#bssendpipe)

## `[@bs.send]`

`bs.send` is used to call a function on an object.

For example, say you created a `div` and a `button`:

```reason
type element;
[@bs.scope "document"] [@bs.val] external createElement: string => element = "";

let div = createElement("div");
let button = createElement("button");
```

How can we append the `button` to the `div`?

Doing this doesn't work:

```reason
div.appendChild(button); // Error: The record field appendChild can't be found.
```

That's what `bs.send` is for:

```reason
[@bs.send] external appendChild: (element, element) => element = "";

appendChild(div, button);
```

which compiles to:

```js
div.appendChild(button);
```

In general, `bs.send` external looks like this:

```reason
[@bs.send] external fn: (obj, arg1, arg2, arg3, ... , argN) => ...;
```

You call `fn` like this:

```reason
fn(obj, arg1, arg2, arg3, ... , argN);

/*
  or equivalently:

  obj->fn(arg1, arg2, arg3, ... , argN);
*/
```

and BS compiles this to:

```js
obj.fn(arg1, arg2, arg3, ... , argN);
```

What if you want the object to come after the arguments like so:

```reason
fn(arg1, arg2, arg3, ... , argN, obj);

/*
  or equivalently:

  obj |> fn(arg1, arg2, arg3, ... , argN);
*/
```

That's what `bs.send.pipe` is for.

## `[@bs.send.pipe]`

`bs.send.pipe`, like `bs.send`, is used to call a function on an object.

Unlike `bs.send`, `bs.send.pipe` takes the object as an argument:

```reason
[@bs.send.pipe: obj] external fn: (arg1, arg2, arg3, ... , argN) => ...;
```

You call `fn` like this:

```reason
fn(arg1, arg2, arg3, ... , argN, obj);

/*
  or equivalently:

  obj |> fn(arg1, arg2, arg3, ... , argN);
*/
```

and BS compiles this to:

```js
obj.fn(arg1, arg2, arg3, ... , argN);
```



