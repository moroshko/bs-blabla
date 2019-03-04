# `[@bs.scope]`

`bs.scope` is used with other attributes like [`bs.val`](./bs.val.md) and [`bs.module`](./bs.module.md) to access "deep" values. 

For example:

```reason
[@bs.scope "Math"] [@bs.val] external floor: float => int = "";
[@bs.scope ("location", "origin")] [@bs.val] external originLength: string = "length";
[@bs.scope "CONSTANTS"] [@bs.module "MyModule"] external initialDays: int = "";

Js.log(floor(3.4));
Js.log(originLength);
Js.log(initialDays);
```

compiles to:

```js
var MyModule = require("MyModule");

console.log(Math.floor(3.4));
console.log(location.origin.length);
console.log(MyModule.CONSTANTS.initialDays);
```

**Note:** The order of `bs.*` attributes doesn't matter.
