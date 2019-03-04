# `[@bs.as]`

`bs.as` can be used with [`bs.deriving abstract`](./bs.deriving.md#bsderiving-abstract) to rename fields. This is useful when field names are valid in JS, but not in BS.

For example:

```reason
[@bs.deriving abstract]
type button = {
  [@bs.as "type"] type_: string,
  [@bs.as "aria-label"] ariaLabel: string
};

let submitButton = button(~type_="submit", ~ariaLabel="Submit");
```

compiles to:

```js
var submitButton = {
  type: "submit",
  "aria-label": "Submit"
};
```
