# rfc-js-destructuring-expression
A proposal for JS to support destructuring as expressions.

It would be useful if destructuring could be used as expressions, returning an object with the subset of the fields indicated.

This could replace the need for helpers like `lodash.pick`, [`pick-deep`](https://github.com/strikeentco/pick-deep), etc.

It's almost like a GraphQL query - something you might get in [`graphql-anywhere`](https://www.npmjs.com/package/graphql-anywhere) or [`graphql-object`](https://github.com/AndersDJohnson/graphql-object). Syntax is also familar to tools like [`updeep`](https://www.npmjs.com/package/updeep).

```js
const object = {
  a: 1,
  nest: {
    b: 2,
    c: 3
  }
}

const subset = pick {
  a,
  nest: {
    c
  }
} = object

// Now `subset` would now be an object:

{
  a: 1,
  nest: {
    c: 3
  }
}
```

With this, you could simplify a helper function to:

```js
const getMyFields = object => pick { a, nest: { c } } = object
```

instead of:

```js
const getMyFields = object => {
 const { a, nest: { c } } = object
 
 return { a, nest: { c } }
}
```

To handle optionality, you could use `= {}` defaults or consider my ["optional destructuring" proposal](https://github.com/AndersDJohnson/rfc-js-optional-destructuring).

I could see a runtime version of this using Proxy objects (perhaps something like my [`stub-obj-proxy`](https://github.com/AndersDJohnson/stub-obj-proxy)), perhaps with a Babel plugin to optimize it for production runtime use.

