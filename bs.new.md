# `[@bs.new]`

When you need use the JS `new` operator, use `bs.new`.

For example:

```reason
type t;
[@bs.new] external createDate: unit => t = "Date";

let date = createDate();
```

compiles to:

```js
var date = new Date();
```

When the object is not available on the global scope and needs importing, add `bs.module`:

```reason
type t;
[@bs.module] [@bs.new] external book: unit => t = "Book";

let myBook = book();
```

compiles to:

```js
var Book = require("Book");
var myBook = new Book();
```
