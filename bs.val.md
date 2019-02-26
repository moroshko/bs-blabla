# `[@bs.val]`

`bs.val` allows you to access JS values that are available on the global scope.

For example:

```reason
[@bs.val] external setTimeout: (unit => unit, int) => float = "setTimeout";

setTimeout(() => Js.log("Hello"), 1000);
```

When BS and JS names match, you can use a `""` shorthand:

```reason
[@bs.val] external setTimeout : (unit => unit, int) => float = "";
[@bs.val] external clearTimeout: int => unit = "";
```

Let's make sure that `clearTimeout` can only get `timeoutID`s returned by `setTimeout`, not any `int`.

Abstract types to the rescue!

```reason
type timeoutID;
[@bs.val] external setTimeout: (unit => unit, int) => timeoutID = "";
[@bs.val] external clearTimeout: timeoutID => unit = "";
```

What if the value we want to access is on another global object, e.g. `Math.floor`, `Date.now`, or even `location.origin.length`?

By adding `bs.scope` we can access "deep" values. For example:

```reason
[@bs.scope "Math"] [@bs.val] external floor: float => int = "";
[@bs.scope "Date"] [@bs.val] external now: unit => int = "";
[@bs.scope ("location", "origin")] [@bs.val] external originLength: string = "length";

let low = floor(3.4);
let timestamp = now();
Js.log(originLength);
```

compiles to:

```js
var low = Math.floor(3.4);
var timestamp = Date.now();
console.log(location.origin.length);
```

**Note:** The order of `bs.scope` and `bs.val` doesn't matter.
