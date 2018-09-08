# Node.js

## Event Loop

Read [The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/).

1. `setTimeout`, `setInterval`
2. `poll`: IO callbacks
3. `setInterval`
4. `close` event handlers

Microtasks such as `process.nextTick()` and promise callbacks are eagerly executed before proceeding to the next phase. [An experiment here](https://gist.github.com/shuhei/34efae93e288ebf20a46de633a53ae9d).

## libuv

Node.js event loop is backed by [libuv](http://libuv.org/).

- network I/O: single thread with polling (epoll on Linux, kqueue on Mac and BSD, etc.)
- file I/O, DNS lookup: multiple threads using a thread pool

To know what's going on, use:

- `process._getActiveHandles()`: handles
- `process._getActiveRequests()`: requests

### Thread Pool

According to https://nodejs.org/api/cli.html#cli_uv_threadpool_size_size, the thread pool is used for:

- all `fs` APIs, other than the file watcher APIs and those that are explicitly synchronous
- `crypto.pbkdf2()`
- `crypto.randomBytes()`, unless it is used without a callback
- `crypto.randomFill()`
- `dns.lookup()`
- all `zlib` APIs, other than those that are explicitly synchronous

http://docs.libuv.org/en/latest/threadpool.html

`UV_THREADPOOL_SIZE`:

- `4` by default.
- `128` is the absolute maximum.
- The size is per process. When used with `cluster`, each worker has the size.

## DNS

- `dns.lookup()` -> `libuv`'s thread pool -> `getaddrinfo(3)`
  - Used by `net.connect()`, `http.request()`, etc. by default.
  - It doesn't have caching. [We should use a caching solution in OS](https://github.com/nodejs/node/issues/5893).
- `dns.resolve()` -> [c-ares](https://github.com/c-ares/c-ares)

## Async Hooks

- [Performance impact of async_hooks](https://github.com/nodejs/benchmarking/issues/181)
- https://github.com/bmeurer/async-hooks-performance-impact

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

## Garbage Collection

- [Garbage collection in V8, an illustrated guide](https://medium.com/@_lrlna/garbage-collection-in-v8-an-illustrated-guide-d24a952ee3b8) - Nice illustrated guide
- [A tour of V8: Garbage Collection
](http://jayconrod.com/posts/55/a-tour-of-v8-garbage-collection) - Detailed guide for V8 Garbage Collection

How to check:

- `node --trace-gc` logs GCs. Use `node --trace-gc --trace-gc-ignore-scavenger` if scavenge logs are too much.
  - It seems to be safe to use in production.
  - [v8-gc-log-parser](https://github.com/aliyun-node/v8-gc-log-parser)
- [Performance Timing API](https://nodejs.org/api/perf_hooks.html) from Node v10 provides GC events to JavaScript without native modules.
- Most of GC-related monitoring tools need native modules. OSS packages like [gc-stats](https://github.com/dainis/node-gcstats), and many enterprise monitoring solutions.

## Profiling

Resources:

- [thlorenz/v8-perf](https://github.com/thlorenz/v8-perf) - Lots of information about V8 & Node.js performance
- [Node Clinic](https://clinicjs.org/) - An Open Source Node.js performance profiling suite by NearForm
  - Work also on non-Linux machines
- [Nodejs Profiling Tools](https://jiajizhou.com/2015/04/15/nodejs-profiling-tools.html) - A good guide that covers many tools

### Visualization

#### CPU Flame Graph

Tools:

- [Node.js Frame Graphs on Linux](http://www.brendangregg.com/blog/2014-09-17/node-flame-graphs-on-linux.html) - CPU frame graph with Node.js
- [CPU Frame Graph](http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html) - CPU frame graph in general
- [0x](https://github.com/davidmarkclements/0x) - Another tool to generate CPU frame graph with nice features for Node.js
- [FlameScope](https://github.com/Netflix/flamescope) - Timescale + Flame Graph
- [Off-CPU Flame Graph](http://www.brendangregg.com/FlameGraphs/offcpuflamegraphs.html)
- [SpeedScope](https://github.com/jlfwong/speedscope) - A web-based tool that can do more than CPU Flame Graph

Introductions:

- [Node.js in Flame](https://medium.com/netflix-techblog/node-js-in-flames-ddd073803aa4) - A nice use case of CPU frame graph
- [Debugging Node.js Performance Issues in Production](https://www.elastic.co/videos/debugging-node-js-performance-issues-in-production-by-thomas-watson) - A talk with demo of CPU frame graph and core dump
- [Generating Node.js Flame Graphs](https://yunong.io/2015/11/23/generating-node-js-flame-graphs/) - says `–perf-basic-prof-only-functions` is safe to use in production as long as the file size growth is OK
- [State of Diagnostic Tools for Production Environment in the Node.js Ecosystem](https://github.com/mmarchini/nodejs-production-diagnostic-tools) - state-of-the-art practices of CPU Flame Graph with Linux `perf`

Gotchas:

- To profile a process in a container, check [Making FlameGraphs with Containerized Java](http://blog.alicegoldfuss.com/making-flamegraphs-with-containerized-java/).
- Caveats of `--perf-basic-prof-only-functions` https://gist.github.com/shuhei/6c261342063bad387c70af384c6d8d5c

#### 0x

`0x --visualize-only` can visualize two different kinds of input:

- `isolate-*-*-v8.log`
  - From V8
  - Available on all environments
  - Better JS function names
  - Doesn't include stuff under V8 like kernel, etc.
- `stacks.*.out`
  - From `perf record` and `perf script`
  - Available only on Linux
  - Some percentage of JS function names are unknown (TODO: Can we improve it?)
  - Includes everything including kernel code

## Debugging with Chrome

https://medium.com/@paul_irish/debugging-node-js-nightlies-with-chrome-devtools-7c4a1b95ae27

### Inspect

```
node --inspect index.js
```

And open chrome://inspect/ with Chrome. In addition to breakpoint debugging, Profiling and Memory snapshots are available.

Also, `USR1` signal activates Inspector API. `process._debugProcess(pid)` on Linux/Unix sends `USR1` signal to another process. You can invoke `process_debugEnd()` on the process via Inspector to stop debugging. However, the process exits somehow on Node 8 and 10...

### Tracing

https://nodejs.org/api/tracing.html

```
node --trace-events-enabled index.js
```

`node_trace.1.log` is generated at cwd.

1. Open chrome://tracing/.
2. Load the file.

## Performance Optimization

### Micro-Optimization

- https://github.com/fastify/fastify - Cool ideas
- https://github.com/pinojs/pino
- https://github.com/davidmarkclements/flatstr - Before passing a big concatenated string to non-V8 API

### Clustering

- https://stackoverflow.com/questions/12432442/node-js-targeting-a-cpu-core - `CPU cores - 1` workers work best???

## C++ Addons

https://nodejs.org/api/addons.html

- Build with `node-gyp`
- N-API is the new way of building addons and will be stable in future versions of Node.js
- NAN (Native Abstractions for Node.js) is obsolete?

## Production Best Practices

- [Node.js production checklist](https://speakerdeck.com/gergelyke/node-dot-js-production-checklist)
- [RisingStack Blog](https://blog.risingstack.com/)
