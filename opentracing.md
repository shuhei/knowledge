# OpenTracing

http://opentracing.io/

- Read [the spec](https://github.com/opentracing/specification/blob/master/specification.md). It's tiny.
- [Towards Turnkey Distributed Tracing](https://medium.com/opentracing/towards-turnkey-distributed-tracing-5f4297d1736) - API in programming languages is defined in the spec. How to encode spans are not.

## Node.js HTTP server/client

Node.js HTTP API provides timings of:

1. Response status code and headers sent/arrived
2. Response body fully sent/arrived

How should we create spans for them? One idea is to start a span at the beginning of a request arrives/starts, log when response headers are sent/arrived and finish the span when the response body is fully sent/arrived.
