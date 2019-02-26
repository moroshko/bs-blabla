# `[@bs.module]`

Use `bs.module` when you need to `require` something.

For example:

```reason
[@bs.module] external uuidv4: unit => string = "uuid/v4";

Js.log(uuidv4());
```

compiles to:

```js
var V4 = require("uuid/v4");

console.log(V4());
```

By passing a string to `bs.module` you can bind to a specific function in the module.

For example:

```reason
type t;
[@bs.module "cheerio"] external load: string => t = "";

let q = load("<h2 class='title'>Hello world</h2>");
```

compiles to:

```js
var Cheerio = require("cheerio");

var q = Cheerio.load("<h2 class='title'>Hello world</h2>");
```

**Note:** The string provided to `bs.module` can be anything, e.g.: `"./path/to/my_module"`.

### Dealing with ES6 `export default`

Say that your project has the following `my_module.js` that is compiled with Babel:

```js
export default "fancy stuff";
```

You'd bind to it like this:

```reason
[@bs.module "./path/to/my_module"] external fancyStuff: string = "default";

Js.log(fancyStuff); /* => "fancy stuff" */
```

This compiles to:

```js
var My_module = require("./path/to/my_module");

console.log(My_module.default);
```
