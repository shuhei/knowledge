# Web Performance

- Remove reflow.
- (React) Don't render hidden elements.
- (React) Find bottlenecks with [User Timing on the development mode](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab).
- Read [Web Fundamentals](https://developers.google.com/web/fundamentals/performance/why-performance-matters/).

## File Size Optimization

- Use [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) and remove unnecessary bytes.
- Use `precision: 1` option of [SVGO](https://github.com/svg/svgo) for SVG images. The option works very well but it's not well-known.
