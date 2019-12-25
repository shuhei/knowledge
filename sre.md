# SRE

## Reliability Patterns for Microservices

- retry
- circuit breaker
- fallback

See:

- https://gist.github.com/lmineiro/a2a080e12037e1bb94bc45a90c49033b
- https://youtu.be/8RKmy9JIU54

More?

- concurrency limit
  - adaptive concurrency limit
    - https://medium.com/@NetflixTechBlog/performance-under-load-3e6fa9a60581
    - https://github.com/Netflix/concurrency-limits

## Performance Modeling

- Queing Theory https://en.wikipedia.org/wiki/Queueing_theory
  - [M/M/1 queue](https://en.wikipedia.org/wiki/M/M/1_queue)
  - [M/M/c queue](https://en.wikipedia.org/wiki/M/M/c_queue)

## Weekly Review

- Check error counts/rates and latencies over a week
- If there are peaks, explain why they happenned and how to mitigate them

## Monitoring

- [Everything You Know About Latency Is Wrong](https://bravenewgeek.com/everything-you-know-about-latency-is-wrong/)
  - Don't average percentiles
  - Don't ignore p99, p99.9, p99.99 as exceptions

### Histogram

- [Your Latency Metrics Could Be Misleading You — How HdrHistogram Can Help](https://medium.com/hotels-com-technology/your-latency-metrics-could-be-misleading-you-how-hdrhistogram-can-help-9d545b598374)
- [
Histogram for Time-Series Metrics on Node.js](https://shuheikagawa.com/blog/2018/12/29/histogram-for-time-series-metrics-on-node-js/)

HDR Histogram is basically a big array with dynamic resizing.

- 0-2047 covers 0-2047 with precision 1
- 2048-3071 covers 2048-4095 with precision 2
- 3072-4095 covers 4097-8191 with precision 4
- and so on

## Load Balancing Algorithms

- [Deterministic Aperture](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/daperture-load-balancer.html)
