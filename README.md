[![view on npm](http://img.shields.io/npm/v/sort-array.svg)](https://www.npmjs.org/package/sort-array)
[![npm module downloads](http://img.shields.io/npm/dt/sort-array.svg)](https://www.npmjs.org/package/sort-array)
[![Build Status](https://travis-ci.org/75lb/sort-array.svg?branch=master)](https://travis-ci.org/75lb/sort-array)
[![Dependency Status](https://badgen.net/david/dep/75lb/sort-array)](https://david-dm.org/75lb/sort-array)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](https://github.com/feross/standard)

# sort-array

Isomorphic, load-anywhere function to sort an array by scalar, deep or computed values in any standard or custom order.

```js
const sortArray = require('sort-array')
```

## Synopsis

Sort an array of primitives, for example strings.

```js
> const partsOfTheDay = ['twilight', 'afternoon', 'morning', 'evening']

> sortArray(partsOfTheDay)
[ 'afternoon', 'evening', 'morning', 'twilight' ]
```

The default sort order is `asc`. You can also specify `desc` or the name of a property from the `customOrders` object. For example, sort parts of the day by the order in which they occur.

```js
> sortArray(partsOfTheDay, {
  order: 'time',
  customOrders: {
    time: ['morning', 'afternoon', 'evening', 'twilight']
  }
})
[ 'morning', 'afternoon', 'evening', 'twilight' ]
```

Sort by a computed field, i.e. a computed value that doesn't exist in the input dataset. Define your computed fields in the `options.computed` object, each value being a function which takes an array member as input and returns the primitive value to be sorted by. In this example we sort by `total` (the name of the computed field supplied in `options.computed`).

```js
> const repositories = [
  { name: '75lb/sort-array', openIssues: 0, closedIssues: 4 },
  { name: 'lwsjs/local-web-server', openIssues: 4, closedIssues: 80 },
  { name: 'jsdoc2md/jsdoc-api', openIssues: 3, closedIssues: 47 }
]

> sortArray(repositories, {
  by: 'total',
  order: 'desc',
  computed: {
    total: repository => repository.openIssues + repository.closedIssues
  }
})
[
  { name: 'lwsjs/local-web-server', openIssues: 4, closedIssues: 80 },
  { name: 'jsdoc2md/jsdoc-api', openIssues: 3, closedIssues: 47 },
  { name: '75lb/sort-array', openIssues: 0, closedIssues: 4 }
]
```

You can use computed fields to sort by values deep in an object structure.

```js
> const data = [
  { inner: { number: 2 } },
  { inner: { number: 3 } },
  { inner: { number: 1 } }
]

> sortArray(data, {
  by: 'number',
  computed: {
    number: row => row.inner.number
  }
})
[
  { inner: { number: 1 } },
  { inner: { number: 2 } },
  { inner: { number: 3 } }
]
```

Sort by multiple columns using multiple custom orders.

```js
> const attributes = [
  { skill: 'accuracy', confidence: 'medium' },
  { skill: 'power', confidence: 'high' },
  { skill: 'speed', confidence: 'low' },
  { skill: 'power', confidence: 'low' },
  { skill: 'speed', confidence: 'high' },
  { skill: 'accuracy', confidence: 'low' },
  { skill: 'speed', confidence: 'medium' },
  { skill: 'accuracy', confidence: 'high' },
  { skill: 'power', confidence: 'medium' }
]

> sortArray(attributes, {
  by: ['skill', 'confidence'],
  order: ['skill', 'confidence'],
  customOrders: {
    skill: ['accuracy', 'speed', 'power'],
    confidence: ['low', 'medium', 'high'],
  }
})
[
  { skill: 'accuracy', confidence: 'low' },
  { skill: 'accuracy', confidence: 'medium' },
  { skill: 'accuracy', confidence: 'high' },
  { skill: 'speed', confidence: 'low' },
  { skill: 'speed', confidence: 'medium' },
  { skill: 'speed', confidence: 'high' },
  { skill: 'power', confidence: 'low' },
  { skill: 'power', confidence: 'medium' },
  { skill: 'power', confidence: 'high' }
]
```

Please visit [the sort-array wiki](https://github.com/75lb/sort-array/wiki) for more examples.

<a name="module_sort-array"></a>

## sort-array
Isomorphic, load-anywhere function to sort an array by scalar, deep or computed values in any standard or custom order.

**Example**  
```js
const sortArray = require('sort-array')
```
<a name="exp_module_sort-array--sortArray"></a>

### sortArray(array, [options]) ⇒ <code>Array</code> ⏏
**Kind**: Exported function  
**Returns**: <code>Array</code> - Returns the array that was passed in.  

| Param | Type | Description |
| --- | --- | --- |
| array | <code>Array</code> | The input array to sort. It is sorted in place. |
| [options] | <code>object</code> | Sort options. |
| [options.by] | <code>Array.&lt;string&gt;</code> | One or more property names or computed fields to sort by. Specifying property names is only relevant when sorting an array of objects. |
| [options.order] | <code>Array.&lt;string&gt;</code> | One or more sort orders. Specify `asc`, `desc` or a property name from the `options.customOrders` object. |
| [options.customOrders] | <code>object</code> | A dictionary object containing one or more custom orders. Each custom order value must be an array defining the order expected values must be sorted in. |
| [options.computed] | <code>object</code> | A dictionary object containing one or more computed field functions. The function will be invoked once per item in the array. Each invocation will receive the array item as input and must return a primitive value by which the array can be sorted. |


## Load anywhere

This library is compatible with Node.js, the Web and any style of module loader. It can be loaded anywhere, natively without transpilation.

Node.js:

```js
const arrayify = require('sort-array')
```

Within Node.js with ECMAScript Module support enabled:

```js
import arrayify from 'sort-array'
```

Within an modern browser ECMAScript Module:

```js
import arrayify from './node_modules/sort-array/dist/index.mjs'
```

Old browser (adds `window.sortArray`):

```html
<script nomodule src="./node_modules/sort-array/dist/index.js"></script>
```

* * *

&copy; 2015-19 Lloyd Brookes \<75pound@gmail.com\>. Documented by [jsdoc-to-markdown](https://github.com/jsdoc2md/jsdoc-to-markdown).
