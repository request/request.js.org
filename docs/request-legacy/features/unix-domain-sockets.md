# UNIX Domain Sockets

`request` supports making requests to [UNIX Domain Sockets](https://en.wikipedia.org/wiki/Unix_domain_socket). To make one, use the following URL scheme:

```js
/* Pattern */ 'http://unix:SOCKET:PATH'
/* Example */ request.get('http://unix:/absolute/path/to/unix.socket:/request/path')
```

Note: The `SOCKET` path is assumed to be absolute to the root of the host file system.

---

[back to README.md](../../README.md#table-of-contents)
