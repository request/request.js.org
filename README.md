
# Request

```js
var request = require('request');
request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage.
  }
})
```

![tms][too-much-stress]

> That's the home page BTW


Support / Contribute stuff can go here I think, [@analog-nico][analog-nico]?

  - Link to Gitter chat
  - Link to repos, contribution guidelines
  - Link to Request architecture docs etc.


  [too-much-stress]: images/tms.gif (Too Much Stress)
  [analog-nico]: https://github.com/analog-nico
