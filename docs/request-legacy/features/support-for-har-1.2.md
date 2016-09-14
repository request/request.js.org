# Support for HAR 1.2

The `options.har` property will override the values: `url`, `method`, `qs`, `headers`, `form`, `formData`, `body`, `json`, as well as construct multipart data and read files from disk when `request.postData.params[].fileName` is present without a matching `value`.

a validation step will check if the HAR Request format matches the latest spec (v1.2) and will skip parsing if not matching.

```js
  var request = require('request')
  request({
    // will be ignored
    method: 'GET',
    uri: 'http://www.google.com',

    // HTTP Archive Request Object
    har: {
      url: 'http://www.mockbin.com/har',
      method: 'POST',
      headers: [
        {
          name: 'content-type',
          value: 'application/x-www-form-urlencoded'
        }
      ],
      postData: {
        mimeType: 'application/x-www-form-urlencoded',
        params: [
          {
            name: 'foo',
            value: 'bar'
          },
          {
            name: 'hello',
            value: 'world'
          }
        ]
      }
    }
  })

  // a POST request will be sent to http://www.mockbin.com
  // with body an application/x-www-form-urlencoded body:
  // foo=bar&hello=world
```

---

[back to README.md](../../README.md#table-of-contents)
