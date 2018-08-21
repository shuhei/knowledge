# Webpack

## Webpack 4

### Extracing CSS from JS files

- Use [extract-text-webpack-plugin@next](https://github.com/webpack-contrib/extract-text-webpack-plugin)
  - [mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin) cannot extract one CSS file from multiple JS chunks without creating an extra JS chunk
- [Hash based on CSS file content](https://github.com/webpack-contrib/extract-text-webpack-plugin/issues/763#issuecomment-377990665)
