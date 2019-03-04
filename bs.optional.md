# `[@bs.optional]`

`bs.optional` can be used with [`bs.deriving abstract`](./bs.deriving.md#bsderiving-abstract) to mark fields as optional.

For example:

```reason
[@bs.deriving abstract]
type person = {
  name: string,
  [@bs.optional] age: int
};

let misha = person(~name="Misha", ());
```

compiles to:

```js
var misha = {
  name: "Misha"
};
```