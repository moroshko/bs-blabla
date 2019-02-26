# `[@bs.string]`

You can use `bs.string` together with polymorphic variant to constrain function's parameters.

For example:

```reason
[@bs.module "fs"]
external readFileSync: (
  ~name: string,
  [@bs.string] [`utf8 | `ascii]
) => string = "";

readFileSync(~name="data.txt", `utf8);
```

compiles to:

```js
var Fs = require("fs");

Fs.readFileSync("data.txt", "utf8");
```

`bs.string` compiled `` `utf8 `` to the string `"utf8"`.

Now, if we try pass an unknown encoding:

```reason
readFileSync(~name="data.txt", `binary);
```

we'll get a compile error.

When your string causes a syntax error in the polymorphic variant (this can happen, for example, when the string starts with a number or contains a special character), use `bs.as`:

```reason
[@bs.module "fs"]
external readFileSync: (
  ~name: string,
  [@bs.string] [`utf8 | `ascii | [@bs.as "ucs-2"] `ucs_2]
) => string = "";

readFileSync(~name="data.txt", `ucs_2);
```

This compiles to:

```js
var Fs = require("fs");

Fs.readFileSync("data.txt", "ucs-2");
```
