# TLS/SSL Protocol

TLS/SSL Protocol options, such as `cert`, `key` and `passphrase`, can be
set directly in `options` object, in the `agentOptions` property of the `options` object, or even in `https.globalAgent.options`. Keep in mind that, although `agentOptions` allows for a slightly wider range of configurations, the recommended way is via `options` object directly, as using `agentOptions` or `https.globalAgent.options` would not be applied in the same way in proxied environments (as data travels through a TLS connection instead of an http/https agent).

```js
var fs = require('fs')
    , path = require('path')
    , certFile = path.resolve(__dirname, 'ssl/client.crt')
    , keyFile = path.resolve(__dirname, 'ssl/client.key')
    , caFile = path.resolve(__dirname, 'ssl/ca.cert.pem')
    , request = require('request');

var options = {
    url: 'https://api.some-server.com/',
    cert: fs.readFileSync(certFile),
    key: fs.readFileSync(keyFile),
    passphrase: 'password',
    ca: fs.readFileSync(caFile)
    }
};

request.get(options);
```

## Using `options.agentOptions`

In the example below, we call an API requires client side SSL certificate
(in PEM format) with passphrase protected private key (in PEM format) and disable the SSLv3 protocol:

```js
var fs = require('fs')
    , path = require('path')
    , certFile = path.resolve(__dirname, 'ssl/client.crt')
    , keyFile = path.resolve(__dirname, 'ssl/client.key')
    , request = require('request');

var options = {
    url: 'https://api.some-server.com/',
    agentOptions: {
        cert: fs.readFileSync(certFile),
        key: fs.readFileSync(keyFile),
        // Or use `pfx` property replacing `cert` and `key` when using private key, certificate and CA certs in PFX or PKCS12 format:
        // pfx: fs.readFileSync(pfxFilePath),
        passphrase: 'password',
        securityOptions: 'SSL_OP_NO_SSLv3'
    }
};

request.get(options);
```

It is able to force using SSLv3 only by specifying `secureProtocol`:

```js
request.get({
    url: 'https://api.some-server.com/',
    agentOptions: {
        secureProtocol: 'SSLv3_method'
    }
});
```

It is possible to accept other certificates than those signed by generally allowed Certificate Authorities (CAs).
This can be useful, for example,  when using self-signed certificates.
To require a different root certificate, you can specify the signing CA by adding the contents of the CA's certificate file to the `agentOptions`.
The certificate the domain presents must be signed by the root certificate specified:

```js
request.get({
    url: 'https://api.some-server.com/',
    agentOptions: {
        ca: fs.readFileSync('ca.cert.pem')
    }
});
```

---

[back to README.md](../../README.md#table-of-contents)
