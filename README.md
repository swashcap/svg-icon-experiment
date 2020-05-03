# svg-icon-experiment

This project demos two SVG icon approaches:

### Inline

A [SVG sprite sheet](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)
with some manual optimizations. HTML is minified with
[html-minifier](https://github.com/kangax/html-minifier).

* URL: https://swashcap.github.io/svg-icon-experiment/inline.html
* Source: [src/inline.html](src/inline.html)
* HTML size: 1.7 kB (gzipped)
* Bytes transferred: 2.0 kB (gzipped)

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
* Bytes transferred: 10.8 kB (gzipped)

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

## Tips for small icons

This projects icons were created in [Sketch](https://www.sketch.com), exported
as SVGs, then hand-tuned for minimal size. Here's a set of guidelines for
producing assets that minify well:

### Graphics editor

* Ensure every border position is set to "center" in [the border
  position](https://www.sketch.com/docs/styling/#how-to-set-a-border-position)
  setting. SVG doesn't offer border positioning; matching SVG's center border
  position ensures shape and line points don't have to be recalculated.
* Set shapes and points on the pixel grid (ex: `1` or `1.5` instead of arbitrary
  values like `1.051664`). This outputs smaller files and ensures that SVG point
  optimization doesn't alter the icon.
* Prefer basic shapes, like the Rectangle and Oval, and Vector-drawn shapes.
  These are often easy to group into a single SVG `path`.
* Avoid shape combinations (Union, Subtract, etc.) and shape flatten.
* Minimize grouping.
* Set the artboard size such that the most common border width among icons is
  `1`. This takes advantage of SVG's default and ensures the `stroke-width`
  property can be omitted.
* If an icon can be made up entirely with Vector tool lines, do it. A good
  example is the classic hamburger menu icon: it can be created with three
  rounded rectangles, but it can be optimized better if its made of three lines.

### Manual tuning

* Remove Sketch's Artboard groups if possible:

  ```shell
  <g id="My Artboard" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
    <!-- ... -->
  </g>
  ```

  These often aren't needed but aren't possible to optimize with svgo.
* If your icon set is monochromatic, change the `stroke` and `fill` colors to
  `currentColor`.

  ```diff
  - <path stroke="#000000" />
  + <path stroke="currentColor" />
  ```

  The SVG icon will take on the `color` of its parent when used inline.
  ([ref](https://css-tricks.com/cascading-svg-fill-color/)).
* Ensure that elements of similar type have the same attributes. For example:

  ```svg
  <svg viewBox="0 0 12 12" version="1.1" xmlns="http://www.w3.org/2000/svg">
    <polyline
      fill="none"
      points="6 3 6 9"
      stroke-linecap="round"
      stroke-linejoin="round"
      stroke="currentColor"
    />
    <polyline
      fill="none"
      points="2.5 6.5 6 9.5 9.5 6.5"
      stroke-linecap="round"
      stroke-linejoin="round"
      stroke="currentColor"
    />
  </svg>
  ```

  svgo will optimize both `polyline`s into a `path`, but hey have to have the
  same `stroke*` attributes.
* You might be tempted to pull common attributes to a parent `g`, but be
  careful: this can prevent svgo from optimizing many elements into a `path` in
  most cases.
* Experiment! Try a few options and run it through svgo to see what works best.
