# `[@bs.deriving]`

* [`[@bs.deriving accessors]`](#bsderiving-accessors)
* [`[@bs.deriving abstract]`](#bsderiving-abstract)

## `[@bs.deriving accessors]`

Reason records are represented as arrays in JS.

For example:

```reason
type person = {
  name: string,
  age: int,
  country: string,
};

let misha: person = {
  name: "Misha",
  age: 18,
  country: "Australia",
};
```

compiles to:

```js
var misha = ["Misha", 18, "Australia"];
```

Imagine that you converted some JS code to Reason and it uses the record above. The code that isn't converted to Reason yet wants to access `misha`'s fields. 

In order to get the `country`, for example, you would need to know to access `misha[2]`.

You shouldn't need to worry about the array representation of a record or the indices where certain fields live.

That's exactly what `bs.deriving accessors` is for! It creates getter functions for all fields of the annotated record.

For example:

```reason
[@bs.deriving accessors]
type person = {
  name: string,
  age: int,
  country: string,
};

let misha: person = {
  name: "Misha",
  age: 18,
  country: "Australia",
};
```

compiles to:

```js
function name(param) {
  return param[0];
}

function age(param) {
  return param[1];
}

function country(param) {
  return param[2];
}

var misha = ["Misha", 18, "Australia"];
```

Now, to get `misha`'s `country` we can use the generated accessor: `country(misha)`

Similarly, imagine your new Reason code has the following variant:

```reason
type action =
  | Add(string)
  | Toggle(int)
  | Delete(int)
  | DeleteAll;
```

and you want to create `Delete(12)` from JS, for example.

By adding `bs.deriving accessors`, you get a creation function for every variant constructor.

```reason
[@bs.deriving accessors]
type action =
  | Add(string)
  | Toggle(int)
  | Delete(int)
  | DeleteAll;
```

Now, in JS you can do:

```js
let actions = [
  add("Drink coffee"), 
  toggle(34),
  delete(12),
  deleteAll
];
```

## `[@bs.deriving abstract]`

Reason records are represented as arrays in JS:

```reason
type person = {
  name: string,
  age: int
};

let misha: person = {
  name: "Misha",
  age: 18
};

Js.log(misha); /* => ["Misha",18] */
```

To represent them as objects, add `bs.deriving abstract`:

```reason
[@bs.deriving abstract]
type person = {
  name: string,
  age: int
};
```

`bs.deriving abstract` converts a record to an abstract type, and also creates a creation function:

```reason
let misha = person(~name="Misha", ~age=18);

Js.log(misha); /* => {"name":"Misha","age":18} */
```

You can also mark fields as optional with [`bs.optional`](./bs.optional.md) and rename fields with [`bs.as`](./bs.as.md).

### Getters

`bs.deriving abstract` generates a getter function for every field.

Continuing the example above:

```reason
Js.log(nameGet(misha)); /* => "Misha" */
Js.log(ageGet(misha));  /* => undefined */
```

or, equivalently:

```reason
Js.log(misha->nameGet); /* => "Misha" */
Js.log(misha->ageGet);  /* => undefined */
```

### Setters

Field values can't be updated unless makred with `mutable`.

`bs.deriving abstract` generates a setter function for every mutable field.

For example:

```reason
[@bs.deriving abstract]
type person = {
  name: string,
  [@bs.optional] mutable age: int,
  mutable country: string
};

let misha = person(~name="Misha", ~country="United States", ());

ageSet(misha, 18);
misha->countrySet("Australia");
```

compiles to:

```js
var misha = {
  name: "Misha",
  country: "United States"
};

misha.age = 18;
misha.country = "Australia";
```

**Note:** `bs.*` must come before `mutable`.
