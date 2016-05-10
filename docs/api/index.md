# API

Full API documentation in the style of the current Request README

## Function Call

````js
request (
  [string uri],
  [Object options],
  [Function callback]
) -> Request
````

If you do not supply a string `uri`, then you **MUST** set the `uri` property on the `options` object.
It sends an HTTP request and returns a [Request](#Request) object.

## Functions

The `request` namespace exposes the following functions.

### request.defaults(options)

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

### request.put

Same as `request()`, but defaults to `method: "PUT"`.

```js
request.put(url)
```

### request.patch

Same as `request()`, but defaults to `method: "PATCH"`.

```js
request.patch(url)
```

### request.post

Same as `request()`, but defaults to `method: "POST"`.

```js
request.post(url)
```

### request.head

Same as `request()`, but defaults to `method: "HEAD"`.

```js
request.head(url)
```

### request.del / request.delete

Same as `request()`, but defaults to `method: "DELETE"`.

```js
request.del(url)
request.delete(url)
```

### request.get

Same as `request()` (for uniformity).

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
