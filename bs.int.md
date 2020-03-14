# `[@bs.int]`

You can use `bs.int` together with polymorphic variant to constrain function's parameters.

For example:

```reason
[@bs.val]
external log: (
  ~status: [@bs.int] [[@bs.as 200] `ok | [@bs.as 400] `bad_request | [@bs.as 401] `unauthorized],
  ~message: string
) => string = "";

log(~status=`unauthorized, ~message="Not allowed");
```

compiles to:

```js
log(401, "Not allowed");
```

`bs.int` compiled `` `unauthorized `` to the number `401`.

Now, if we try pass an unknown status:

```reason
log(~status=`not_found, ~message="This page is not found");
```

we'll get a compile error.

Note how we used `bs.as` to better control the numbers that `bs.int` compiles to.

If we omitted all the `bs.as` above, `bs.int` would compile:

- `` `ok `` to `0`
- `` `bad_request `` to `1`
- `` `unauthorized `` to `2`
