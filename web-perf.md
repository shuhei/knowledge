# Web Performance

- Read [Web Fundamentals](https://developers.google.com/web/fundamentals/performance/why-performance-matters/).

## Runtime Optimization

- Remove reflow.

### React

- Don't render hidden elements.
- Find bottlenecks with [User Timing on the development mode](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab).

## File Size Optimization

### Webpack

- Use [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) and remove unnecessary bytes.
- If you find modules that you don't know why they are necessary, check [stats.json](https://webpack.js.org/api/stats/) to find out why. `webpack-bundle-analyzer` has options to generate it.
- Take advantage of [sideEffects: false](https://github.com/webpack/webpack/tree/master/examples/side-effects) for webpack.
  - If side-effect-free packages don't have the flag in their `package.json`s, you can also set the flag with `modules.rules`. Use `test: /node_modules\/some-module-name-here/` to match against npm packages.

### Images

- Use `precision: 1` option of [SVGO](https://github.com/svg/svgo) for SVG images. The option works very well but it's not well-known.
