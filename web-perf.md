# Web Performance

- Read [Web Fundamentals](https://developers.google.com/web/fundamentals/performance/why-performance-matters/).

## Runtime Optimization

- Remove reflow.

### Measurement with Chrome DevTools

- Use _Disable Cache_, _CPU Throttling_, _Network Throttling_, etc.
- When you record screenshots with reload on _Performance_ tab, the previous page is also recorded in screenshots before the next page is displayed. To avoid this, delete `<body>` on _Element_ tab before reloading the page.

### React

- Don't render hidden elements.
- Find bottlenecks with [User Timing on the development mode](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab).

## File Size Optimization

### Webpack

- Use [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) and remove unnecessary bytes.
- If you find modules that you don't know why they are necessary, check [stats.json](https://webpack.js.org/api/stats/) to find out why. `webpack-bundle-analyzer` has options to generate it.
- Take advantage of [sideEffects: false](https://github.com/webpack/webpack/tree/master/examples/side-effects) for webpack.
  - If side-effect-free packages don't have the flag in their `package.json`s, you can also set the flag with `modules.rules`. Use `test: /node_modules\/some-module-name-here/` to match against npm packages.
- When using webpack for production bundles, keep ES modules instead of transpiling them into CommonJS
  - [`babel-loader` seems to do this by default](https://github.com/babel/babel-loader/pull/660), but be careful with old versions (update to the latest!)

### Images

- Use `precision: 1` option of [SVGO](https://github.com/svg/svgo) for SVG images. The option works very well but it's not well-known.

## Resources

- [Chasing Waterfalls](https://chasingwaterfalls.io/) (podcast)
