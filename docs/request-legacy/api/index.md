# RequestAPI

- [`(uri, [options], [callback])`](#call)
- [`(options, [callback])`](#call)
- [`defaults (options)`](#requestdefaultsoptions)
- [`put (uri, options, callback)`](#requestput-uri-options-callback)
- [`patch (uri, options, callback)`](#requestpatch-uri-options-callback)
- [`post (uri, options, callback)`](#requestpost-uri-options-callback)
- [`head (uri, options, callback)`](#requesthead-uri-options-callback)
- [`del (uri, options, callback)`](#requestdelete-uri-options-callback)
- [`delete (uri, options, callback)`](#requestdelete-uri-options-callback)
- [`get (uri, options, callback)`](#requestget-uri-options-callback)
- [`cookie (str)`](#requestcookie-str)
- [`jar ([store])`](#requestjar)

## Call

The exported `request` module is a function. You can *call* it directly with the following
signatures:


```
request (uri, [options], [callback]) => Request
request (options, [callback]) => Request
```

- `uri`
  - Type: `string` 

- `options`
  - Type: [`RequestOptions`](./interfaces/request-options.md)
  
  If you use the second signature (without the `uri` string), then you **MUST** set the `uri` property on the
  `options` object.

- `callback`
  - Type: `Function (err, res, body) => any`
    - `err` (`Error` | `null`): An eventual error
    - `res` ([`Response`](./classes/response.md)): The server's response
    - `body` (`string` | `Buffer` | `JSONObject`): **TODO**

- **return**
  - Type: { [`Request`](./classes/request.md) }
  
  An object representing the current request.


This function performs an HTTP request and returns a [Request](./classes/request.md) object.

By default, it performs a `GET` request to the provided `uri`. Check the
[`RequestOptions`](./interfaces/request-options.md) interface to get more information about the default
values.

## Functions

The `request` namespace exposes the following functions:

### request.defaults(options)

- `options`
  - Type: [`RequestOptions`](./interfaces/request-options.md)
  - Optional
- **return**
  - Type: [`RequestAPI`](#requestapi)

This method **returns a wrapper** around the normal request API that defaults
to whatever options you pass to it.

**Note:** `request.defaults()` **does not** modify the global request API;
instead, it **returns a wrapper** that has your default settings applied to it.

**Note:** You can call `.defaults()` on the wrapper that is returned from
`request.defaults` to add/override defaults that were previously defaulted.

#### Example
```js
// requests using baseRequest() will set the 'x-token' header
var baseRequest = request.defaults({
  headers: {'x-token': 'my-token'}
});

// requests using specialRequest() will include the 'x-token' header set in
// baseRequest and will also include the 'special' header
var specialRequest = baseRequest.defaults({
  headers: {special: 'special value'}
});
```

### request.put (uri, options, callback)

Alias for a [`request` call](#call), but defaults to `method: "PUT"`.

```js
request.put(url)
```

### request.patch (uri, options, callback)

Alias for a [`request` call](#call), but defaults to `method: "PATCH"`.

```js
request.patch(url)
```

### request.post (uri, options, callback)

Alias for a [`request` call](#call), but defaults to `method: "POST"`.

```js
request.post(url)
```

### request.head (uri, options, callback)

Alias for a [`request` call](#call), but defaults to `method: "HEAD"`.

```js
request.head(url)
```

### request.delete (uri, options, callback)

- Alias: `request.del`

Alias for a [`request` call](#call), but defaults to `method: "DELETE"`.

```js
request.del(url)
request.delete(url)
```

### request.get (uri, options, callback)

Alias for a [`request` call](#call) (for uniformity).

```js
request.get(url)
```

### request.cookie (str)

Function that creates a new cookie.

- `str`
  - Type: `string`

- **return**
  - Type: [`Cookie`][tough-cookie-cookie]

Note: this is a wrapper for [`tough-cookie`][tough-cookie]'s [`Cookie.parse`][tough-cookie-cookie-parse] function.


#### Example

```js
var cookie = request.cookie('key1=value1');
```

### request.jar([store])

Function that creates a new cookie jar.

- `store`
  - Type: [`Store`][tough-cookie-store]

- **return**
  - Type: [`CookieJar`](./classes/cookie-jar.md)

#### Example

```js
var jar = request.jar()
```

[tough-cookie]: https://github.com/SalesforceEng/tough-cookie
[tough-cookie-cookie]: https://github.com/SalesforceEng/tough-cookie#cookie
[tough-cookie-cookie-parse]: https://github.com/SalesforceEng/tough-cookie#cookieparsecookiestring-options
[tough-cookie-store]: https://github.com/SalesforceEng/tough-cookie#store
