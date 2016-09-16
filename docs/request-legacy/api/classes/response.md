# `Response`

A `Response` instance is currently constructed by extending an `http.IncomingMessage`.
This document describes the methods and properties added by the `request` package.

## Prototype Chain

- [`http.IncomingMessage`](https://nodejs.org/api/http.html#http_class_http_incomingmessage)
- [`stream.Readable`](https://nodejs.org/api/stream.html#stream_class_stream_readable)
- [`events.EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter)

## Methods

### `.toJSON ()`

This override allows you to safely use `JSON.stringify` on a `Response` instance.

- **return**
  - Type: [`ResponseJSON`](../interfaces/response-json.md)
