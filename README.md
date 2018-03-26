# turbo-http

A low level http library for Node.js based on [turbo-net](https://github.com/mafintosh/turbo-net)

```
npm install turbo-http
```

[![build status](https://travis-ci.org/mafintosh/turbo-http.svg?branch=master)](https://travis-ci.org/mafintosh/turbo-http)

WIP, this module is already *really* fast but there are some HTTP features
missing and easy perf gains to be had :D :D :D

On my laptop I can serve simple hello world payloads at around 100k requests/seconds compared to 10k requests/second using node core.

## Usage

``` js
const turbo = require('turbo-http')

const server = turbo.createServer(function (req, res) {
  res.setHeader('Content-Length', '11')
  res.write(Buffer.from('hello world'))
})

server.listen(8080)
```

## API

#### `server = turbo.createServer([onrequest])`

Create a new http server. Inherits from [the turbo-net tcp server](https://github.com/mafintosh/turbo-net#server--turbocreateserveroptions-onsocket)

#### `server.on('request', req, res)`

Emitted when a new http request is received.

#### `res.statusCode = code`

Set the http status

#### `res.getHeader(name)`

Get a http header

#### `res.setHeader(name, value)`

Set a http header

#### `res.hasHeader(name)`

Returns true if the header identified by name. Note that the header name matching is case-insensitive.

#### `res.getHeaders()`

Returns a shallow copy of the current outgoing headers.

#### `res.writeHead(statusCode[, statusMessage][, headers])`

Set a response header to the request. The status code is a 3-digit
HTTP status code, like 404. The last argument, `headers`, are the
response headers. Optionally one can give a human-readable
`statusMessage` as the second argument.

#### `res.write(buf, [length], [callback])`

Write a buffer. When the callback is called, the buffer
has been *completely* flushed to the underlying socket and is safe to
reuse for other purposes

#### `res.writev(buffers, [lengths], [callback])`

Write more that one buffer at once.

#### `res.end([buf], [length], [callback]`)

End the request. Only needed if you do not provide a `Content-Length`.

#### `req.url`

Request url

#### `req.method`

Request method

#### `req.socket`

Request [turbo-net](https://github.com/mafintosh/turbo-net) socket

#### `value = req.getHeader(name)`

Get a request header.

#### `headers = req.getAllHeaders()`

Get all request headers as an object.

#### `headers = req.headers`

Get all request headers as an object.

#### `req.ondata(buffer, start, length)`

Called when there is data read. If you use the buffer outside of this function
you should copy it.

#### `req.onend()`

Called when the request is fully read.

## Acknowledgements

This project was kindly sponsored by [nearForm](http://nearform.com).

## License

MIT
