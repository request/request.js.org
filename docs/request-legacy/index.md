# Request Legacy

This is the documentation for version 2 of `request`.

Request is designed to be the simplest way possible to make http calls. It supports HTTPS and follows redirects by default.

```js
var request = require('request');
request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage.
  }
});
```

## [API](./api/index.md)

## Installation

```shell
npm install --save request@2
```

## Usage

### CommonJS (Node.js)

```js
var request = require("request");
```

### Javascript ES6 (Typescript)

```ts
import * as request from "request";
```

Note that the exported value is a *function* instead of a [namespace object][namespace-object].
This is a popular way of exporting non-ES6 modules, but this is also a violation of the ES6 spec.
When using the ES6 syntax, make sure to check how your environment or transpiler handles this case.

[namespace-object]: http://www.ecma-international.org/ecma-262/6.0/index.html#sec-module-namespace-objects
