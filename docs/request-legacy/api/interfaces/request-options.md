# RequestOptions

The `RequestOptions` _interface_ describes the options of [the `request`'s
main function](../main-module#call) and all of its specialized
versions ([`.get`](../main-module#requestget-uri-options-callback),
[`.post`](../main-module#requestpost-uri-options-callback),
[`.delete`](../main-module#requestdelete-uri-options-callback), ...).

**All the properties are optional (except if you do not provide an `uri`
argument to the `request` call, then the property `uri` is required).**

Here is a thematic overview of the supported properties:

- General
  - [uri](#uri)
  - [url](#url)
  - [baseUrl](#baseurl)
  - [method](#method)
  - [headers](#headers)
- Query String
  - [qs](#qs)
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

- Type: `string` | [`Url`][node-url]

A fully qualified (scheme, domain and path) [URI string][rfc-uri] or a parsed
[`Url`][node-url] object from [`url.parse()`][node-url-parse] for the target
of the request.

If [`.baseUrl`](#baseurl) is defined, `.uri` must be a a `string` and is allowed
to represent a relative URI.

### Example:

```js
import url from 'url';
import request from 'request';

// Fully qualified URI
request.get({uri: "http://example.com/api"});

// Parsed Url object
const parsedUrl = url.parse("http://example.com/api");
request.get({uri: parsedUrl});
```

## `url`

Alias for [`.uri`](#uri).

_Note_: If you define both `.uri` and `.url`, `.uri` will be used and `.url`
ignored.

## `baseUrl`

- Type: `string`

Fully qualified (scheme, domain and path) [URI string][rfc-uri] used as the base
URL.
This is especially useful with
[`request.defaults`](../main-module.md#requestdefaultsoptions), for example
when you want to do many requests to the same domain.

When `.baseUrl` is given, [`.uri`](#uri) must also be a `string`.

### Example

```js
import request from 'request';

// Wrapper configured for the API of example.com
const requestWrapper = request.defaults({baseUrl: "https://example.com/api/"});

// Perform a GET request with the wrapped API
const pendingRequest = requestWrapper.get({uri: "/endpoint?test=true"});

// Log the full URI of the GET request
console.log(pendingRequest.href); // https://example.com/api/endpoint?test=true
```

## `method`

- Type: `string`
- Default: `"GET"`

The [HTTP request method][wikipedia-http-request-methods] to use.
There are no restrictions on the name of method.

**TODO**: Is the case normalized ?

## `headers`

- Type: `object`
- Default: `{}`

A dictionary of additional HTTP request headers to send.
The keys (`string`) are used as header names, the values are converted to
strings.

**TODO**: Check if the values are really converted to strings, how are handled
  non-string (JSON.stringify ?).

**TODO**: What is the interaction with `request.defaults` (replaces or extends).

---

## `qs`

- Type: `object`

A dictionary containing _querystring_ (qs) parameters to be appended to the
`uri`.


**Interaction with `request.defaults`**: The current options are deeply merged
to the wrapper's options (which is the base object). This means that atomic
values (strings, numbers, booleans, null, undefined, NaN, Infinity, ...) are
replaced (current options override the wrapper's options), plain objects are
merged recursively. Arrays are extended up to the max of both then values at
the same index are merged.
**TODO**: How are non-plain objects merged ?
**TODO**: What happens if the options have the same key with different types ?
**TODO**: Move the explanation about options merging to a common place.

**Interaction with `.uri`**: The data in `.qs` is merged to the query-string
data eventually in `.uri`. If a key is both defined in `.uri` and `.qs`, the one
defined in `.qs` is used.

### Example

```js
import request from 'request';

// Simple usage
const simpleRequest = request.get({
  uri: 'https://example.com/api/issues',
  qs: {perPage: 50, page: 2}
});
console.log(simpleRequest.url.href); // https://example.com/api/issues?perPage=50&page=2

// Interaction with defaults and uri
const issuesRequestWrapper = request.defaults({qs: {perPage: 50}});
const complexRequest = issuesRequestWrapper.get({
  uri: 'https://example.com/api/issues?category=documentation',
  qs: {category: "fiz"}
});
console.log(complexRequest.url.href); // https://example.com/api/issues?category=documentation&perPage=50&page=50
```

## `qsParseOptions`

- Type: [`object`][gh-qs-parse] | `{seq?: string; eq?: string; options?: object}`
- Default: **TODO**

This option controls the way query strings are parsed, for example in `.uri`.
(**TODO**: check if it is used for `uri`)

**This option depends on the chosen library to handle query string
serialization. This choice is determined
by [the `.useQuerystring` option](#usequerystring)).**


- **If [`.useQuerystring`](#usequerystring) is `false` (default)**:  
  Query strings will be parsed with the [parse][gh-qs-parse] function from
  [the `qs` library][gh-qs]. `.qsParseOptions` is directly passed to
  [`qs.parse`][gh-qs-parse] and allows you to configure the way the query
  string is parsed. See the [`qs.parse` documentation][gh-qs-parse]
  for more details.
  (**TODO**: include the most important parts here)

- **If [`.useQuerystring`](#usequerystring) is set to `true`**:  
  Query strings will be parsed with the [parse][node-querystring-parse] function
  from [Node's `querystring` module][node-querystring]. You can pass an
  object with the following interface:
  - `sep` (type: `string`, optional, default: `'&'`): The substring used to
    delimit key and value pairs in the query string (second argument of
    [`querystring.parse`][node-querystring-parse]).
  - `eq` (type: `string`, optional, default: `'='`): The substring used to
    delimit keys and values in the query string. (third argument of
    [`querystring.parse`][node-querystring-parse]).
  - `options` (type: [`object`][node-querystring-parse], optional,
    default: `{}`): Additional options (last argument of
    [`querystring.parse`][node-querystring-parse]).

## `qsStringifyOptions`

- Type: [ `object` ][gh-qs-stringify]
- Default: **TODO**

This option controls the way query string data are stringify'ed.
(**TODO**: check if it is used for `uri`)

**This option depends on the chosen library to handle query string
serialization. This choice is determined
by [the `.useQuerystring` option](#usequerystring)).**


- **If [`.useQuerystring`](#usequerystring) is `false` (default)**:  
  Query string data are stringify'ed with the [stringify][gh-qs-stringify]
  function from [the `qs` library][gh-qs]. `.qsStringifyOptions` is directly
  passed to [`qs.parse` documentation][gh-qs-parse] and allows you to configure
  the way the query string data are serialized. See the [`qs.stringify`
  documentation][gh-qs-stringify] for more details.
  (**TODO**: include the most important parts here)

- **If [`.useQuerystring`](#usequerystring) is set to `true`**:  
  Query string data are stringify'ed with the
  [stringify][node-querystring-stringify] function from [Node's
  `querystring` module][node-querystring]. You can pass an
  object with the following interface:
  - `sep` (type: `string`, optional, default: `'&'`): The substring used to
    delimit key and value pairs in the query string (second argument of
    [`querystring.stringify`][node-querystring-stringify]).
  - `eq` (type: `string`, optional, default: `'='`): The substring used to
    delimit keys and values in the query string. (third argument of
    [`querystring.stringify`][node-querystring-stringify]).
  - `options` (type: [`object`][node-querystring-stringify], optional,
    default: `{}`): Additional options (last argument of
    [`querystring.stringify`][node-querystring-stringify]).

### Example:

Change the way arrays are converted to query strings: `foo=bar&foo=baz`
instead of the default `foo[0]=bar&foo[1]=baz`.


```js
import request from 'request';

const pendingRequest = request.get({
  uri: 'https://example.com',
  qs: {'foo': ['bar', 'baz']},
  qsStringifyOptions: {
    arrayFormat: 'repeat'
  }
})
console.log(pendingRequest.url.href); // https://example.com/?foo=bar&foo=baz
```

## `useQuerystring`

- Type: `boolean`
- Default : `false`

`request` can use two libraries to stringify and parse your query strings:
[Node's `querystring` module][node-querystring] or the preferred [third-party
library `qs`][gh-qs]. The implication on "stringification" and parsing are
respectively explained in [`.qsStringifyOptions`](#qsstringifyoptions) and
[`.qsParseOptions`](#qsparseoptions).
                          

- By default or if `.useQuerystring` is `false`, `request` will
  use [`qs`][gh-qs].
- If `.useQuerystring` is set to `true`, `request` will use [Node's
  `querystring`][node-querystring]

**Note**: The options [`.qsStringifyOptions`](#qsstringifyoptions) and
[`.qsParseOptions`](#qsparseoptions) depend on the library you choose
with this flag.

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


[gh-qs]: https://github.com/hapijs/qs
[gh-qs-parse]: https://github.com/hapijs/qs#parsing-objects
[gh-qs-stringify]: https://github.com/ljharb/qs#stringifying
[node-querystring]: https://nodejs.org/api/querystring.html
[node-querystring-parse]: https://nodejs.org/api/querystring.html#querystring_querystring_parse_str_sep_eq_options
[node-querystring-stringify]: https://nodejs.org/api/querystring.html#querystring_querystring_stringify_obj_sep_eq_options
[node-url]: https://nodejs.org/dist/latest-v6.x/docs/api/url.html
[node-url-parse]: https://nodejs.org/dist/latest-v6.x/docs/api/url.html#url_url_parse_urlstring_parsequerystring_slashesdenotehost
[rfc-uri]: https://tools.ietf.org/html/rfc3986
[wikipedia-http-request-methods]: https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods
