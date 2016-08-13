# Custom HTTP Headers

HTTP Headers, such as `User-Agent`, can be set in the `options` object.
In the example below, we call the github API to find out the number
of stars and forks for the request repository. This requires a
custom `User-Agent` header as well as https.

```js
var request = require('request');

var options = {
  url: 'https://api.github.com/repos/request/request',
  headers: {
    'User-Agent': 'request'
  }
};

function callback(error, response, body) {
  if (!error && response.statusCode == 200) {
    var info = JSON.parse(body);
    console.log(info.stargazers_count + " Stars");
    console.log(info.forks_count + " Forks");
  }
}

request(options, callback);
```

---

[back to README.md](../../README.md#table-of-contents)
