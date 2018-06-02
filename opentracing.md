# OpenTracing

http://opentracing.io/

- Read [the spec](https://github.com/opentracing/specification/blob/master/specification.md) to grasp the ideas. It's tiny.
- Read [Semantic Conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) before implementing tags and logs.
- [Towards Turnkey Distributed Tracing](https://medium.com/opentracing/towards-turnkey-distributed-tracing-5f4297d1736) - API in programming languages is defined in the spec. How to encode spans are not.

## JavaScript

### Packages

- [JS API (TypeScript)](https://github.com/opentracing/opentracing-javascript)
- [Express middleware](https://github.com/opentracing-contrib/javascript-express)

### Node.js HTTP server/client

Node.js HTTP API provides timings of:

1. Response status code and headers sent (server) or arrived (client)
2. Response body fully sent/arrived

How should we create spans for them? One idea is to start a span at the beginning of a request arrives/starts, log when response headers are sent/arrived and finish the span when the response body is fully sent/arrived.
