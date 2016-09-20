# API

You likely want to check [the API of the main module](./module.md) or [the 
request options](./interfaces/request-options.md).

The **main module** is the object you import in your code (for example with
`require('request')`). It is the entry point for Request's API and exposes most
of this package's features.

Some functions return objects with methods, they are described in the
"**classes**" section. You should not instanciate them yourself.

Properties of simple container objects (such as options) are documented as
**interfaces**.

## Overview

- [Main module](./main-module.md)

  - [`(uri, [options], [callback])`](./main-module.md#call)
  - [`(options, [callback])`](./main-module.md#call)
  - [`defaults (options)`](./main-module.md#requestdefaultsoptions)
  - [`put (uri, options, callback)`](./main-module.md#requestput-uri-options-callback)
  - [`patch (uri, options, callback)`](./main-module.md#requestpatch-uri-options-callback)
  - [`post (uri, options, callback)`](./main-module.md#requestpost-uri-options-callback)
  - [`head (uri, options, callback)`](./main-module.md#requesthead-uri-options-callback)
  - [`del (uri, options, callback)`](./main-module.md#requestdelete-uri-options-callback)
  - [`delete (uri, options, callback)`](./main-module.md#requestdelete-uri-options-callback)
  - [`get (uri, options, callback)`](./main-module.md#requestget-uri-options-callback)
  - [`cookie (str)`](./main-module.md#requestcookie-str)
  - [`jar ([store])`](./main-module.md#requestjarstore)

- [Class `Request`](./classes/request.md)
- [Class `Response`](./classes/response.md)
- [Class `CookieJar`](./classes/cookie-jar.md)
- [Interface `RequestOptions`](./interfaces/request-options.md)
