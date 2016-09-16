# RequestOptions

`RequestOptions` describes the options supported by the `request` function.
All the properties are optional (except if you do not provide an `uri` argument to the `request` call, then the property
`uri` is required).

- General
  - [uri](#uri)
  - [url](#url)
  - [baseUrl](#baseurl)
  - [method](#method)
  - [headers](#headers)
- Query String
  - [qs](#qs-object)
  - [qsParseOptions](#qsparseoptions)
  - [qsStringifyOptions](#qsstringifyoptions)
  - [useQuerystring](#usequerystring)
- Forms
  - [body](#body)
  - [form](#form)
  - [formData](#formdata)
  - [multipart](#multipart)
  - [preambleCRLF](#preamblecrlf)
  - [postambleCRLF](#postamblecrlf)
  - [json](#json)
  - [jsonReviver](#jsonreviver)
  - [jsonReplacer](#jsonreplacer)
- Authentication
  - [auth](#auth)
  - [oauth](#oauth)
  - [hawk](#hawk)
  - [aws](#aws)
  - [httpSignature](#httpsignature)
- Redirections
  - [followRedirect](#followredirect)
  - [followAllRedirects](#followallredirects)
  - [maxRedirects](#maxredirects)
  - [removeRefererHeader](#removerefererheader)
- Request headers
  - [encoding](#encoding)
  - [gzip](#gzip)
  - [jar](#jar)
- Agent
  - [agent](#agent)
  - [agentClass](#agentclass)
  - [agentOptions](#agentoptions)
  - [forever](#forever)
  - [pool](#pool)
  - [timeout](#timeout)
- Network
  - [localAddress](#localaddress)
  - [proxy](#proxy)
  - [strictSSL](#strictssl)
  - [tunnel](#tunnel)
  - [proxyHeaderWhiteList](#proxyHeaderwhiteList)
  - [proxyHeaderExclusiveList](#proxyheaderexclusivelist)
- Others
  - [time](#time)
  - [har](#har)
  - [callback](#callback)

---

## `uri`

- Type: `string` | [`Url`](https://nodejs.org/dist/latest-v6.x/docs/api/url.html)

Fully qualified uri or a parsed url object from `url.parse()`.

## `url`

- Type: `string` | [`Url`](https://nodejs.org/dist/latest-v6.x/docs/api/url.html)

Alias for [`uri`](#uri)

## `baseUrl`

- Type: `string`

Fully qualified uri string used as the base url. Most useful with `request.defaults`, for example when you want to do
many requests to the same domain.  If `baseUrl` is `https://example.com/api/`, then requesting `/end/point?test=true`
will fetch `https://example.com/api/end/point?test=true`. When `baseUrl` is given, `uri` must also be a string.

## `method`

- Type: `string`
- Default: `"GET"`

The HTTP method to use.

## `headers`

- Type: `Object`
- Default: `{}`

The HTTP headers to send.

---

## `qs`

- Type: `Object`

A dictionary containing querystring values to be appended to the `uri`.

## `qsParseOptions`

- Type: `Object`

Object containing options to pass to the [qs.parse](https://github.com/hapijs/qs#parsing-objects) method. Alternatively
pass options to the [querystring.parse](https://nodejs.org/docs/v0.12.0/api/querystring.html#querystring_querystring_parse_str_sep_eq_options)
method using this format `{sep:';', eq:':', options:{}}`.

## `qsStringifyOptions`

- Type: `Object`

Object containing options to pass to the [qs.stringify](https://github.com/hapijs/qs#stringifying) method. Alternatively
pass options to the  [querystring.stringify](https://nodejs.org/docs/v0.12.0/api/querystring.html#querystring_querystring_stringify_obj_sep_eq_options)
method using this format `{sep:';', eq:':', options:{}}`. For example, to change the way arrays are converted to query
strings using the `qs` module pass the `arrayFormat` option with one of `indices|brackets|repeat`.

## `useQuerystring`

- Type: `boolean`
- Default : `false`

If true, use `querystring` to stringify and parse querystrings, otherwise use `qs`. Set this option to `true` if you
need arrays to be serialized as `foo=bar&foo=baz` instead of the default `foo[0]=bar&foo[1]=baz`.

---

## `body`

- Type: `Buffer` | `string` | `ReadStream` | `JSONSerializableObject`

Entity body for PATCH, POST and PUT requests. If `json` is `true`, then `body` must be a JSON-serializable object.

## `form`

- Type: `Object` | `string`

When passed an object or a querystring, this sets `body` to a querystring representation of value, and adds
`Content-type: application/x-www-form-urlencoded` header. When passed no options, a `FormData` instance is returned (and
is piped to request). See [Forms](guides/forms.md) guide.

## `formData`

- Type: **TODO**

Data to pass for a `multipart/form-data` request. See [Forms](guides/forms.md) guide.

## `multipart`

- Type: `Array<Object>` | `{chunked: Boolean, data: Array<Object>}`

Array of objects which contain their own headers and `body` attributes. Sends a `multipart/related` request. See
[Forms](guides/forms.md) guide.

Alternatively you can pass in an object `{chunked: false, data: []}` where `chunked` is used to specify whether the
request is sent in [chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding). In non-chunked
requests, data items with body streams are not allowed.

## `preambleCRLF`

- Type: `boolean`
- Default : `false`

Append a newline/CRLF before the boundary of your `multipart/form-data` request.

## `postambleCRLF`

- Type: `boolean`
- Default : `false`

Append a newline/CRLF at the end of the boundary of your `multipart/form-data` request.

## `json`

- Type: `boolean`
- Default : `false`

Sets `body` to JSON representation of value and adds `Content-type: application/json` header.  Additionally, parses the
response body as JSON.

## `jsonReviver`

- Type: `Function`

A [reviver function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) that
will be passed to `JSON.parse()` when parsing a JSON response body.

## `jsonReplacer`

- Type: `Function`

A [replacer function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
that will be passed to `JSON.stringify()` when stringifying a JSON request body.

---

## `auth`

- Type: [`AuthOptions`](#authoptions)

See [HTTP Authentication](guides/http-authentication.md) guide.

## `oauth`

- Type: [`OAuthOptions`](#oauthoptions)

Options for OAuth HMAC-SHA1 signing. See [OAuth Signing](guides/oauth-signing.md) guide.

## `hawk`

- Type: `Object`

Options for [Hawk signing](https://github.com/hueniverse/hawk). The `credentials` key must contain the necessary signing
info, [see hawk docs for details](https://github.com/hueniverse/hawk#usage-example).

## `aws`

- Type: `Object`

Object containing AWS signing information. Should have the properties `key`, `secret`. Also requires the property
`bucket`, unless you’re specifying your `bucket` as part of the path, or the request doesn’t use a bucket (i.e. GET
Services). If you want to use AWS sign version 4 use the parameter `sign_version` with value `4` otherwise the default
is version 2. **Note:** you need to `npm install aws4` first.

## `httpSignature`

- Type: `Object`

Options for the [HTTP Signature Scheme](https://github.com/joyent/node-http-signature/blob/master/http_signing.md) using
[Joyent's library](https://github.com/joyent/node-http-signature). The `keyId` and `key` properties must be specified.
See the docs for other options.

---

## `followRedirect`

- Type: `boolean`
- Default: `true`

Follow HTTP 3xx responses as redirects. This property can also be implemented as function which gets `response` object
as a single argument and should return `true` if redirects should continue or `false` otherwise.

## `followAllRedirects`

- Type: `boolean`


Follow non-GET HTTP 3xx responses as redirects.

## `maxRedirects`

- Type: `number`
- Default: `10`

The maximum number of redirects to follow.

## `removeRefererHeader`

- Type: `boolean`
- Default: `false`

Removes the referer header when a redirect happens.
**Note:** if true, referer header set in the initial request is preserved during redirect chain.

---

## `encoding`

- Type: `string` | `null` | `undefined`
- Default: `undefined`

Encoding to be used on `setEncoding` of response data. If `null`, the `body` is returned as a `Buffer`. Anything else
**(including the default value of `undefined`)** will be passed as the [encoding](http://nodejs.org/api/buffer.html#buffer_buffer)
parameter to `toString()` (meaning this is effectively `utf8` by default). (**Note:** if you expect binary data, you
should set `encoding: null`.)

## `gzip`

- Type: `boolean`
- Default: `false`

If `true`, add an `Accept-Encoding` header to request compressed content encodings from the server (if not already
present) and decode supported content encodings in the response.  **Note:** Automatic decoding of the response content
is performed on the body data returned through `request` (both through the `request` stream and passed to the callback
function) but is not performed on the `response` stream (available from the `response` event) which is the unmodified
`http.IncomingMessage` object which may contain compressed data. See [examples](./examples.md).

## `jar`

- Type: `boolean` | `RequestCookieJar`
- Default: `false`

If `true`, remember cookies for future use (or define your custom cookie jar; see examples section)

---

## `agent`

- Type: [`http.Agent`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) | [`https.Agent`](https://nodejs.org/dist/latest-v6.x/docs/api/https.html#https_class_https_agent)

The `http(s).Agent` instance to use

## `agentClass`

- Type: **TODO**

Alternatively specify your agent's class name

## `agentOptions`

- Type: **TODO**

Pass its options to your agent's class.
**Note:** for HTTPS see [tls API doc for TLS/SSL options](http://nodejs.org/api/tls.html#tls_tls_connect_options_callback)
and the [TLS/SSL guide](guides/tls-ssl-protocol.md#using-optionsagentoptions).

## `forever`

- Type: `boolean`
- Default: `false`

Set to `true` to use the [forever-agent](https://github.com/request/forever-agent).
**Note:** Defaults to `http(s).Agent({keepAlive:true})` in node 0.12+

## `pool`

- Type: `Object`

An object describing which agents to use for the request. If this option is omitted the request will use the global
agent (as long as your options allow for it). Otherwise, request will search the pool for your custom agent. If no
custom agent is found, a new agent will be created and added to the pool. **Note:** `pool` is used only when the `agent`
option is not specified.

- A `maxSockets` property can also be provided on the `pool` object to set the max number of sockets for all agents
  created (ex: `pool: {maxSockets: Infinity}`).
- Note that if you are sending multiple requests in a loop and creating multiple new `pool` objects, `maxSockets` will
  not work as intended. To work around this, either use [`request.defaults`](#requestdefaultsoptions) with your pool
  options or create the pool object with the `maxSockets` property outside of the loop.

## `timeout`

- Type: `number`

Integer containing the number of milliseconds to wait for a server to send response headers (and start the response
body) before aborting the request. Note that if the underlying TCP connection cannot be established, the OS-wide TCP
connection timeout will overrule the `timeout` option ([the default in Linux can be anywhere from 20 to 120
seconds](http://www.sekuda.com/overriding_the_default_linux_kernel_20_second_tcp_socket_connect_timeout)).

---

## `localAddress`

- Type: `string`

Local interface to bind for network connections.

## `proxy`

- Type: **TODO**

An HTTP proxy to be used. Supports proxy Auth with Basic Auth, identical to support for the `url` parameter (by
embedding the auth info in the `uri`).

## `strictSSL`

- Type: `boolean`
- Default: `false`

If `true`, requires SSL certificates be valid. **Note:** to use your own certificate authority, you need to specify an
agent that was created with that CA as an option.

## `tunnel`

- Type: `boolean` | `undefined`
- Default: `undefined`

Controls the behavior of [HTTP `CONNECT` tunneling](https://en.wikipedia.org/wiki/HTTP_tunnel#HTTP_CONNECT_tunneling)
as follows:

- `undefined` (default) - `true` if the destination is `https`, `false` otherwise
- `true` - always tunnel to the destination by making a `CONNECT` request to the proxy
- `false` - request the destination as a `GET` request.

## `proxyHeaderWhiteList`

- Type: **TODO**

A whitelist of headers to send to a tunneling proxy.

## `proxyHeaderExclusiveList`

- Type: **TODO**

A whitelist of headers to send exclusively to a tunneling proxy and not to destination.

---

## `time`

- Type: `boolean`

If `true`, the request-response cycle (including all redirects) is timed at millisecond resolution, and the result
provided on the response's `elapsedTime` property.


## `har`

- Type: **TODO**

A [HAR 1.2 Request Object](http://www.softwareishard.com/blog/har-12-spec/#request), will be processed from HAR format
into options overwriting matching values *(see the [HAR 1.2 section](guides/support-for-har-1.2.md) for details)*

## `callback`

- Type: `Function`

Alternatively pass the request's callback in the options object.

The callback argument gets 3 arguments:

1. An `error` when applicable (usually from [`http.ClientRequest`](http://nodejs.org/api/http.html#http_class_http_clientrequest) object)
2. A [`Response`](#../classes/response.md) object
3. The third is the `response` body (`String` or `Buffer`, or JSON object if the `json` option is supplied)
