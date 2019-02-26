# `[@bs.unwrap]`

Say you have the following JS function:

```js
function padLeft(padding, str) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + str;
  }
  if (typeof padding === "string") {
    return padding + str;
  }
  throw new Error("Expected padding to be number or string");
}
```

Note how `padding` can be either a number or a string.

Here is how you'd bind to this function:

```reason
[@bs.val]
external padLeft: (
  [@bs.unwrap] [`Int(int) | `Str(string)],
  string
) => string = "";

padLeft(`Int(7), "eleven");
padLeft(`Str("7"), "eleven");
```

which compiles to:

```js
padLeft(7, "eleven");   /* => "       eleven" */
padLeft("7", "eleven"); /* => "7eleven" */
```

Note how `bs.unwrap` simply strips the polymorphic variant constructor (i.e. it unwraps the wrapped value).

Now, any attempts to pass a padding that's not a number or a string, will result in compile error.

```reason
padLeft(true, "eleven");      /* => compile error */
padLeft(`Int(7.5), "eleven"); /* => compile error */
```

Alternatively, you might consider to create two separate functions that bind to the same `padLeft`:

```reason
[@bs.val] external padLeftWithSpaces: (int, string) => string = "padLeft";
[@bs.val] external padLeftWithString: (string, string) => string = "padLeft";

padLeftWithSpaces(7, "eleven");
padLeftWithString("7", "eleven");
```

The compiled output is the same:

```js
padLeft(7, "eleven");   /* => "       eleven" */
padLeft("7", "eleven"); /* => "7eleven" */
```
