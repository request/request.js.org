# HTTP Authentication

```js
request.get('http://some.server.com/').auth('username', 'password', false);
// or
request.get('http://some.server.com/', {
  'auth': {
    'user': 'username',
    'pass': 'password',
    'sendImmediately': false
  }
});
// or
request.get('http://some.server.com/').auth(null, null, true, 'bearerToken');
// or
request.get('http://some.server.com/', {
  'auth': {
    'bearer': 'bearerToken'
  }
});
```

If passed as an option, `auth` should be a hash containing values:

- `user` || `username`
- `pass` || `password`
- `sendImmediately` (optional)
- `bearer` (optional)

The method form takes parameters
`auth(username, password, sendImmediately, bearer)`.

`sendImmediately` defaults to `true`, which causes a basic or bearer
authentication header to be sent.  If `sendImmediately` is `false`, then
`request` will retry with a proper authentication header after receiving a
`401` response from the server (which must contain a `WWW-Authenticate` header
indicating the required authentication method).

Note that you can also specify basic authentication using the URL itself, as
detailed in [RFC 1738](http://www.ietf.org/rfc/rfc1738.txt).  Simply pass the
`user:password` before the host with an `@` sign:

```js
var username = 'username',
    password = 'password',
    url = 'http://' + username + ':' + password + '@some.server.com';

request({url: url}, function (error, response, body) {
   // Do more stuff with 'body' here
});
```

Digest authentication is supported, but it only works with `sendImmediately`
set to `false`; otherwise `request` will send basic authentication on the
initial request, which will probably cause the request to fail.

Bearer authentication is supported, and is activated when the `bearer` value is
available. The value may be either a `String` or a `Function` returning a
`String`. Using a function to supply the bearer token is particularly useful if
used in conjunction with `defaults` to allow a single function to supply the
last known token at the time of sending a request, or to compute one on the fly.

---

[back to README.md](../../README.md#table-of-contents)
