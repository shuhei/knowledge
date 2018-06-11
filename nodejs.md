# Node.js

## Event Loop

Read [The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/).

1. `setTimeout`, `setInterval`
2. `poll`: IO callbacks
3. `setInterval`
4. `close` event handlers

Microtasks such as `process.nextTick()` and promise callbacks are eagerly executed before proceeding to the next phase. [An experiment here](https://gist.github.com/shuhei/34efae93e288ebf20a46de633a53ae9d).

## Profiling

### Flame Graph

Available only on Linux?

- [Node.js Frame Graphs on Linux](http://www.brendangregg.com/blog/2014-09-17/node-flame-graphs-on-linux.html) - CPU frame graph with Node.js
- [CPU Frame Graph](http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html) - CPU frame graph in general
- [Node.js in Flame](https://medium.com/netflix-techblog/node-js-in-flames-ddd073803aa4) - A nice use case of CPU frame graph

## Logging of HTTP Server

### Useful low-level packages

- [on-headers](https://github.com/jshttp/on-headers
) - `ServerResponse` is about to write HTTP headers
- [on-finished](https://github.com/jshttp/on-finished) - `ServerResponse` is finished
  - A caveat: When a response is finished, it can be one of the following cases:
    - Normal case: The response is fully sent.
    - Edge case: The socket is disconnected before response status code and headers are set.
      - Note that `res.statusCode === 200` in this case while `200` is not sent to the client. It's caused by `ServerResponse.prototype.statusCode === 200`.
      - Check `res.headersSent` and `res.res._header` to detect this case. Learned from [morgan](https://github.com/expressjs/morgan).
    - Edge case: The socket is disconnected after response status code and headers are set but before a response body is fully sent.

### Case studies

Express middlewares:

- [response-time](https://github.com/expressjs/response-time) - Using `on-headers` to measure response time. Note that the response time does **NOT** include the period from `res.writeHead()` to `res.end()`.
- [morgan](https://github.com/expressjs/morgan) - Using `on-headers` to measure response time. Using `on-finished` to write logs.
