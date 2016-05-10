# Proxies

If you specify a `proxy` option, then the request (and any subsequent
redirects) will be sent via a connection to the proxy server.

If your endpoint is an `https` url, and you are using a proxy, then
request will send a `CONNECT` request to the proxy server *first*, and
then use the supplied connection to connect to the endpoint.

That is, first it will make a request like:

```
HTTP/1.1 CONNECT endpoint-server.com:80
Host: proxy-server.com
User-Agent: whatever user agent you specify
```

and then the proxy server make a TCP connection to `endpoint-server`
on port `80`, and return a response that looks like:

```
HTTP/1.1 200 OK
```

At this point, the connection is left open, and the client is
communicating directly with the `endpoint-server.com` machine.

See [the wikipedia page on HTTP Tunneling](https://en.wikipedia.org/wiki/HTTP_tunnel)
for more information.

By default, when proxying `http` traffic, request will simply make a
standard proxied `http` request.  This is done by making the `url`
section of the initial line of the request a fully qualified url to
the endpoint.

For example, it will make a single request that looks like:

```
HTTP/1.1 GET http://endpoint-server.com/some-url
Host: proxy-server.com
Other-Headers: all go here

request body or whatever
```

Because a pure "http over http" tunnel offers no additional security
or other features, it is generally simpler to go with a
straightforward HTTP proxy in this case.  However, if you would like
to force a tunneling proxy, you may set the `tunnel` option to `true`.

You can also make a standard proxied `http` request by explicitly setting
`tunnel : false`, but **note that this will allow the proxy to see the traffic
to/from the destination server**.

If you are using a tunneling proxy, you may set the
`proxyHeaderWhiteList` to share certain headers with the proxy.

You can also set the `proxyHeaderExclusiveList` to share certain
headers only with the proxy and not with destination host.

By default, this set is:

```
accept
accept-charset
accept-encoding
accept-language
accept-ranges
cache-control
content-encoding
content-language
content-length
content-location
content-md5
content-range
content-type
connection
date
expect
max-forwards
pragma
proxy-authorization
referer
te
transfer-encoding
user-agent
via
```

Note that, when using a tunneling proxy, the `proxy-authorization`
header and any headers from custom `proxyHeaderExclusiveList` are
*never* sent to the endpoint server, but only to the proxy server.


## Controlling proxy behaviour using environment variables

The following environment variables are respected by `request`:

 * `HTTP_PROXY` / `http_proxy`
 * `HTTPS_PROXY` / `https_proxy`
 * `NO_PROXY` / `no_proxy`

When `HTTP_PROXY` / `http_proxy` are set, they will be used to proxy non-SSL requests that do not have an explicit `proxy` configuration option present. Similarly, `HTTPS_PROXY` / `https_proxy` will be respected for SSL requests that do not have an explicit `proxy` configuration option. It is valid to define a proxy in one of the environment variables, but then override it for a specific request, using the `proxy` configuration option. Furthermore, the `proxy` configuration option can be explicitly set to false / null to opt out of proxying altogether for that request.

`request` is also aware of the `NO_PROXY`/`no_proxy` environment variables. These variables provide a granular way to opt out of proxying, on a per-host basis. It should contain a comma separated list of hosts to opt out of proxying. It is also possible to opt of proxying when a particular destination port is used. Finally, the variable may be set to `*` to opt out of the implicit proxy configuration of the other environment variables.

Here's some examples of valid `no_proxy` values:

 * `google.com` - don't proxy HTTP/HTTPS requests to Google.
 * `google.com:443` - don't proxy HTTPS requests to Google, but *do* proxy HTTP requests to Google.
 * `google.com:443, yahoo.com:80` - don't proxy HTTPS requests to Google, and don't proxy HTTP requests to Yahoo!
 * `*` - ignore `https_proxy`/`http_proxy` environment variables altogether.

---

[back to README.md](../../README.md#table-of-contents)
