# CSS

## Fallback for `object-fit: cover;`

https://medium.com/@primozcigler/neat-trick-for-css-object-fit-fallback-on-edge-and-other-browsers-afbc53bbb2c3

## Feature Detection

If you don't want to include Modernizer, [`element.style[propName] !== undefined` should be enough](https://github.com/Modernizr/Modernizr/blob/master/src/testProps.js). Probably you can use `document.body` or something.

## line-height

`line-height: normal` (default) depends on the font. Set `line-height` relative to the font size (for example `line-height: 1.75`) to avoid page jumps with web fonts. For accessibility, `line-height` should be at least `1.5`.

Read [Deep dive CSS: font metrics, line-height and vertical-align](https://iamvdo.me/en/blog/css-font-metrics-line-height-and-vertical-align) for more details!
