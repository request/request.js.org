# Timeouts

Most requests to external servers should have a timeout attached, in case the
server is not responding in a timely manner. Without a timeout, your code may
have a socket open/consume resources for minutes or more.

There are two main types of timeouts: **connection timeouts** and **read
timeouts**. A connect timeout occurs if the timeout is hit while your client is
attempting to establish a connection to a remote machine (corresponding to the
[connect() call][connect] on the socket). A read timeout occurs any time the
server is too slow to send back a part of the response.

These two situations have widely different implications for what went wrong
with the request, so it's useful to be able to distinguish them. You can detect
timeout errors by checking `err.code` for an 'ETIMEDOUT' value. Further, you
can detect whether the timeout was a connection timeout by checking if the
`err.connect` property is set to `true`.

```js
request.get('http://10.255.255.1', {timeout: 1500}, function(err) {
    console.log(err.code === 'ETIMEDOUT');
    // Set to `true` if the timeout was a connection timeout, `false` or
    // `undefined` otherwise.
    console.log(err.connect === true);
    process.exit(0);
});
```

[connect]: http://linux.die.net/man/2/connect

---

[back to README.md](../../README.md#table-of-contents)
