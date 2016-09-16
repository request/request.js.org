# Request

## Prototype Chain

- [`stream.Stream`](https://github.com/nodejs/node/blob/master/lib/stream.js#L22)
- [`events.EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter)

## Methods

### `.toJSON ()`

This override allows you to safely use `JSON.stringify` on a `Request` instance.

- **return**
  - Type: [`RequestJSON`](../interfaces/request-json.md)


## Internal methods

### `.debug (...args)`

Logs the arguments to the console (`console.error`) if the `NODE_DEBUG` environment variable is set.

### `.init (options)`

**TODO**

### `.getNewAgent ()`

**TODO**

### `.start ()`

**TODO**

### `.onRequestError (error)`

**TODO**

### `.onRequestResponse (response)`

**TODO**

### `.readResponseBody (response)`

**TODO**

### `.abort ()`

**TODO**

### `.pipeDest (dest)`

**TODO**

### `.qs (q, clobber)`

**TODO**

### `.form (form)`

**TODO**

### `.multipart (multipart)`

**TODO**

### `.json (val)`

**TODO**

### `.getHeader (name, headers)`

**TODO**

### `.enableUnixSocket ()`

**TODO**

### `.auth (user, pass, sendImmediately, bearer)`

**TODO**

### `.aws (opts, now)`

**TODO**

### `.httpSignature (opts)`

**TODO**

### `.hawk (opts)`

**TODO**

### `.oauth (_oauth)`

**TODO**

### `.jar (jar)`

**TODO**

### `.pipe (dest, opts)`

**Override**

**TODO**

### `.write ()`

**TODO**

### `.end (chunk)`

**TODO**

### `.pause ()`

**TODO**

### `.resume ()`

**TODO**

### `.destroy ()`

**TODO**

## Static properties

### `.debug`

- Type: `boolean`

### `.defaultProxyHeaderWhiteList`

### `.defaultProxyHeaderExclusiveList`
