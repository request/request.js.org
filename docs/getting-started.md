# Getting Started

Request is a powerful library that covers almost any use case of doing http(s) requests.

Crawling a webpage:

```js
var request = require('request')

request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage.
  }
})
```

Calling an API:

```js
var options = {
  method: 'POST',
  uri: 'https://truckdb.com/api/truck',
  json: {
    make: 'Ford',
    model: 'VelociRaptor'
  },
  headers: {
    'X-API-Key': 'horsepower'
  }
}

request(options, function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the API response.
  }
})
```

## Choose Your Flavor!

The options you pass to the `request` function are always the same. But how you execute the request comes in different flavors.

### The Traditional Flavor: *Callbacks*

Handling the response in a callback is the primary choice:

``` js
request(options, function (error, response, body) {
  // Process the response in this callback.
})
```

 - Also consider *Promises* if your code will do many requests in sequence/parallel. It will help you fight callback hell.
 - Also consider *Streams* if your code will upload/download megabytes of data. It will reduce the memory footprint.

If you decide to use *Callbacks* then continue reading about the [available options](#todo-add-link) and [callback parameters](#todo-add-link).

### The Modern Flavor: *Promises*

Handling the response via *Promises* adds all convenience promises provide:

``` js
var request = require('request-promise') // Promise capabilities are actually added by a separate library

request(options)
  .then(function (body) (
    // Process a successful response here.
  ))
  .catch(function (error) {
    // Handle any error here.
  })
```

 - Also consider *Callbacks* if you want to use Request in old browsers and prefer to not increase the page weight by the required Promise implementation.
 - Also consider *Streams* if your code will upload/download megabytes of data. It will reduce the memory footprint.

If you decide to use *Promises* then continue reading about the [available options](#todo-add-link) and the [Promise support](#todo-add-link).

### The Refined Flavor: *Streams*

If, for example, your request downloads 3 megabytes of data then the code will need 3 megabytes of memory to hold the response until it got processed. Imagine that you are implementing a server that does approx. 100 of such requests in parallel then it will need 300 megabytes of memory just to hold the downloaded data. But if your server runs in a container with 512 megabytes of memory it might become very tight.

This is where streams are very useful: While the data is downloaded, small chunks can be processed one by one through the stream. Since only the downloaded chunks that are not processed yet are kept in memory, the memory footprint gets much smaller.

``` js
var request = require('request') // Streams are again supported by the request library itself

request(options)
  .pipe(fs.createWriteStream('snap.png')) // The image is saved to the disk chunk by chunk.
```

If you decide to use *Streams* then continue reading about the [available options](#todo-add-link) and the [Streaming support](#todo-add-link).
