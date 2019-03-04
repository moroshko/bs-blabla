# `[@bs.val]`

`bs.val` allows you to access JS values that are available on the global scope.

For example:

```reason
[@bs.val] external setTimeout: (unit => unit, int) => int = "setTimeout";

setTimeout(() => Js.log("Hello"), 1000);
```

**Tip:** When BS and JS names match, you can use a `""` shorthand:

```reason
[@bs.val] external setTimeout : (unit => unit, int) => int = "";
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

That's what `bs.scope` is for. [Check it out!](./bs.scope.md)
