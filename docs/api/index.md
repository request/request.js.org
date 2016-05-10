# API

Full API documentation in the style of the current Request README

## Call

````js
request ([uri], [options], [callback]) => Request
````

- `uri`
  - Type: {`string`}
  - Optional

  If you do not supply a string `uri`, then you **MUST** set the `uri` property on the `options` object.

- `options`
  - Type: {[`RequestOptions`](./interfaces/request-options.md)}
  - Optional

- `callback`
  - Type: {`Function (err, res, body) => any`}
    - `err` {`Error` | `null`}: An eventual error
    - `res` {[`Response`](./classes/response.md)}: The server's response
    - `body` {`sintr` | `Buffer` | `JSONObject`}
  - Optional

- **return**
  - Type: {[`Request`](./classes/request.md)}


This function performs an HTTP request and returns a [Request](./classes/request.md) object.


## Functions

The `request` namespace exposes the following functions:

### request.defaults(options)

- `options`
  - Type: {[`RequestOptions`](./interfaces/request-options.md)}
  - Optional
- **return**
  - Type: {`RequestAPI`}

This method **returns a wrapper** around the normal request API that defaults
to whatever options you pass to it.

**Note:** `request.defaults()` **does not** modify the global request API;
instead, it **returns a wrapper** that has your default settings applied to it.

**Note:** You can call `.defaults()` on the wrapper that is returned from
`request.defaults` to add/override defaults that were previously defaulted.

For example:
```js
//requests using baseRequest() will set the 'x-token' header
var baseRequest = request.defaults({
  headers: {'x-token': 'my-token'}
})

//requests using specialRequest() will include the 'x-token' header set in
//baseRequest and will also include the 'special' header
var specialRequest = baseRequest.defaults({
  headers: {special: 'special value'}
})
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

### request.del (uri, options, callback) / request.delete (uri, options, callback)

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

### request.cookie

Function that creates a new cookie.

```js
request.cookie('key1=value1')
```
### request.jar()

Function that creates a new cookie jar.

```js
request.jar()
```
