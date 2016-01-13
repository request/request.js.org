
# Request

```js
var request = require('request')
request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage.
  }
})
```

- [Gitter Chat][gitter]
- [NPM Package][npm]
- [Promises Interface][request-promise]
- [Changelog][changelog]
- [Request Next][request-next]


  [too-much-stress]: images/tms.gif (Too Much Stress)
  [analog-nico]: https://github.com/analog-nico
  [gitter]: https://gitter.im/request/request
  [npm]: https://www.npmjs.com/package/request
  [request-promise]: https://github.com/request/request-promise
  [request-next]: https://github.com/request/core
  [changelog]: https://github.com/request/request/blob/master/CHANGELOG.md
