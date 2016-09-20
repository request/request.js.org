# ResponseJSON

This _interface_ describes the JSON object obtained when serializing a
[`Response`](../classes/response.md) instance with its
[`.toJSON()`](../classes/response.md#tojson) method or passing it to
[`JSON.stringify`][mdn-json-stringify] and then parsing the result.

## `statusCode` (`number`)

The [HTTP status code][wikipedia-http-status-codes] of the server's response.

## `body`

A string or a JSON object.

**TODO**: Check the possible values of body (string, array ?? (from a
buffer ?), JSON object) and add a description.

## `headers` (`object`)

The dictionary of the headers attached to the HTTP response. It is a plain
object representing a map between header names (`string`) and their values
(`string`).

**TODO**: check if the value of the `cookies` property is a `string` too.

## `request` ([`RequestJSON`](./request-json.md))

The request associated to this response.


[mdn-json-stringify]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify
[wikipedia-http-status-codes]: https://en.wikipedia.org/wiki/List_of_HTTP_status_codes
