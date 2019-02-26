# `[@bs.variadic]`

To model a JS function that takes a variable number of arguments, and all arguments are of the same type, use `bs.variadic`:

```reason
[@bs.scope "Math"] [@bs.val] [@bs.variadic] external max: array(int) => int = "";

Js.log(max([|5, -2, 6, 1|]));
```

This compiles to:

```js
console.log(Math.max(5, -2, 6, 1));
```

Similarly to [`Function.apply()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) in JS, `bs.variadic` converts the arguments array in BS to separate arguments on the JS side.

If your function has mandatory arguments, you could use `bs.as` to add them:

```reason
[@bs.val] [@bs.variadic] external warn: ([@bs.as 404] _, [@bs.as "NOT_FOUND"] _, array(string)) => unit = "log";

warn([||]);
warn([|"this", "page", "is", "not", "found"|]);
```

This compiles to:

```js
log(404, "NOT_FOUND");
log(404, "NOT_FOUND", "this", "page", "is", "not", "found");
```