# svg-icon-experiment

This project demos two SVG icon approaches:

### Inline

A [SVG sprite sheet](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)
with some manual optimizations. HTML is minified with
[html-minifier](https://github.com/kangax/html-minifier).

* URL: https://swashcap.github.io/svg-icon-experiment/inline.html
* Source: [src/inline.html](src/inline.html)
* HTML size: 2.1 kB (gzipped)
* Bytes transferred: 2.5 kB (gzipped)

#### localhost, Fast 3G

* TTFB: ~570 ms
* `DOMContentLoaded`: ~620 ms

#### GitHub Pages, Slow 3G

* TTFB: ~2.00 s
* `DomContentLoaded`: ~2.00 s

### External

SVGs in individual `img` elements. SVGs are minified with
[svgo](https://github.com/svg/svgo), HTML is minified with
[html-minifier](https://github.com/kangax/html-minifier).

* URL: https://swashcap.github.io/svg-icon-experiment/external.html
* Source: [src/external.html](src/external.html)
* HTML size: 1.0 kB (gzipped)
* Bytes transferred: 11.1 kB (gzipped)

Loading experience:

![simulated external loading](img/external-loading.gif)

#### localhost, Fast 3G

* TTFB: ~570 ms
* `DOMContentLoaded`: ~620 ms

#### GitHub Pages, Slow 3G

* TTFB: ~2.00 s
* `DomContentLoaded`: ~2.00 s

## Setup

1. Ensure [Node.js](https://nodejs.org/en/) (>=12.16) is installed.
2. Install dependencies:

  ```shell
  npm install
  ```

## Build

```shell
npm run clean && npm run build
```
