# Web Performance

- Remove reflow.
- (React) Don't render hidden elements.
- (React) Find bottlenecks with [User Timing on the development mode](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab).
- Read [Web Fundamentals](https://developers.google.com/web/fundamentals/performance/why-performance-matters/).

## File Size Optimization

- Use [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) and remove unnecessary bytes.
- Take advantage of [sideEffects: false](https://github.com/webpack/webpack/tree/master/examples/side-effects) for webpack. If side-effect-free packages don't have the flag in their `package.json`s, you can also set the flag with `modules.rules`.
- Use `precision: 1` option of [SVGO](https://github.com/svg/svgo) for SVG images. The option works very well but it's not well-known.
