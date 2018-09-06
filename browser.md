# Browser

## CSS

### Fallback for `object-fit: cover;`

https://medium.com/@primozcigler/neat-trick-for-css-object-fit-fallback-on-edge-and-other-browsers-afbc53bbb2c3

### Feature Detection

If you don't want to include Modernizer, [`element.style[propName] !== undefined` should be enough](https://github.com/Modernizr/Modernizr/blob/master/src/testProps.js). Probably you can use `document.body` or something.
