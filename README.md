# Learning SASS / SCSS

Just one of the things I'm learning. <https://github.com/hchiam/learning>

```text
Sass = "better" CSS.
```

Sass is a stylesheet language interpreted (preprocessed) into CSS.
I like the newer syntax called SCSS (basically a superset of vanilla CSS).

Some features of SCSS are planned to become standard CSS in the future, but for now pre-processing with SCSS Sass can improve writing CSS files.

Use <http://www.sassmeister.com/>
or do something like this in command-line/terminal:

```bash
npm install -g sass
sass input.scss output.css
```

You can also have `sass` watch for file changes to signal it to re-compile the CSS output files:

```bash
sass --watch input.scss output.css
```

The `.map` source map file that's created maps each line in the CSS file to the corresponding line in the Sass file.

The `.sass-cache` cache folder that's created is used to speed up re-compilation.

SASS/SCSS is conceptually similar to [Less](https://github.com/hchiam/learning-less).

## Tutorial

<http://tutorialzine.com/2016/01/learn-sass-in-15-minutes/>

## Sass features shown through described code examples

<http://sass-lang.com/guide> and <http://tutorialzine.com/2016/01/learn-sass-in-15-minutes/>

- preprocessing
- variables (so you change properties in only one place)
- nesting
- partials (e.g. to have small separate Sass files for re-use)
- imports
- mixins (like constructor classes. Can be used for prefixes for Firefox and other vendors)
- extend/inheritance (e.g. classes overwriting properties)
- operators (for pre-calculating properties)
- function (you can make your own or use built-in ones, e.g. darken($color, $amount) )

## YouTube Tutorial I'm Following

<https://www.youtube.com/watch?v=roywYSEPSvc>

(Uses VSCode SCSS extension "Live Sass Compiler" instead of `node`/`gulp`/etc. Just click the "Watch Sass" button on the bottom of the window. It'll auto-generate and auto-update the `.css` and `.css.map` files.)

(Another recommended VSCode extension: "Live Server". Just hit "Go Live" on the bottom of the window.)

Online CSS clip-path maker: <https://bennettfeely.com/clippy>

### Highlights of [/youtube-tutorial/css/main.scss](https://github.com/hchiam/learning-sass/blob/master/youtube-tutorial/css/main.scss)

- `@include desktop` SCSS mixin to make media query less verbose (less typing for you).

  ```scss
  @mixin desktop {
    @media (min-width: #{$desktop}) {
      @content;
    }
  }

  ... #bg {
    clip-path: polygon(100% 0, 100% 76%, 45% 100%, 0 100%, 0 0);
    // other properties

    ... @include desktop {
      // override when wider than $desktop min-width:
      clip-path: polygon(0 0, 84% 0, 53% 100%, 0% 100%);
    }
  }
  ```

- (Pure CSS:) CSS grid example that uses `grid-template-areas` and 2 `grid-area`s:

  ```css
  grid-template-areas: "primary card"; /* will place primary on left of card */
  ... section#card {
    grid-area: card;
  }
  ... section#primary {
    grid-area: primary;
  }
  ```

- SCSS map of `$colors: (...)` to group related variables together.
- SCSS function to make `map-get` less verbose.

## Map and loop and breakpoints example

```scss
$grid-breakpoints: (
  "sm": 576px,
  "md": 768px,
  "lg": 992px,
  "xl": 1200px,
  "xxl": 1400px,
);

@each $grid-breakpoint, $grid-breakpoint-size in $grid-breakpoints {
  @media (min-width: $grid-breakpoint-size) {
    .w-#{$grid-breakpoint}-unset {
      width: unset !important;
    }

    .text-#{$grid-breakpoint}-wrap {
      white-space: normal !important;
    }

    .text-#{$grid-breakpoint}-nowrap {
      white-space: nowrap !important;
    }
  }
}
```

## `@import` will eventually be replaced with [`@use`](https://sass-lang.com/documentation/at-rules/use) and [`@forward`](https://sass-lang.com/documentation/at-rules/forward)

Tip from [coder coder](https://youtu.be/dOnYNEXv9BM):

- [`@forward`](https://sass-lang.com/documentation/at-rules/forward) for styles
- [`@use`](https://sass-lang.com/documentation/at-rules/use) for mixins, variables, etc.

But I might likely just use `@use` since it can do what `@forward` can.

## SCSS looping though CSS variable colour names

```scss
/** colors.scss */

$colors: (
  red,
  green,
  blue,
);

$color-count: length($colors);

/** not only SASS variables, but also set up CSS variables ("--..."), which can be accessed by JS: */

@mixin get-css-color-variables {
  $count: length($colors);
  --color-count: #{$count};
  @for $i from 1 through $count {
    --color-#{$i}: #{nth($colors,$i)};
  }
}
```

So for example:

```scss
@import 'colors.scss';
.container {
  @include get-css-color-variables;
  // can also use $color-count to use in a @for loop in this file too for other stuff
}
```

to generate something like:

```css
.container {
  --color-count: 3;
  --color-1: red;
  --color-2: green;
  --color-3: blue;
}
```

## unintuitive difference between comment types when using CSS grid template areas in SASS/SCSS

  ```scss
  /* in SCSS: */
  grid-template-areas:
    "top right" // top
    "mid right" // middle
    "bot right"; // bottom
  ```

- Seems to behave differently than

    ```scss
    /* in SCSS: */
    grid-template-areas:
      "top right" /* top */
      "mid right" /* middle */
      "bot right"; /* bottom */
    ```

## "partial selectors" in variables

You can put "partial selectors" in SCSS variables:

```scss
$partialSelector: ".some-class";
.something:has(#{$partialSelector}:nth-child(5)) {
```

```scss
$isImageEmpty: ":has(.image-container img[src=''])";
$isPhoneEmpty: ".contact-container .phone:empty";
$isEmailEmpty: ".contact-container .email:empty";
$isContactInfoEmpty: ":has(#{$isPhoneEmpty}):has(#{$isEmailEmpty})":
&:not(#{$isImageEmpty})#{$isContactInfoEmpty} {
```

## helper mixin for writing media queries and container queries at the same time

could be helpful for things like a page of small previews that need to simulate mobile/table/desktop views:

```scss
/**
example usage:
.something {
    @include custom-query("min-width: 62.5rem") {
        background: red; // this will go into the mixin's @content "placeholders"
    }
}
the $condition must be a string, otherwise complex conditions with brackets or functions may not transpile properly for the container query
*/
@mixin custom-query($condition: /**must be string*/ 'min-width: 1500px', $container-name: custom-query, $container-type: inline-size) {
    @media screen and ($condition) {
        @content;
    }

    container-type: $container-type;
    container-name: $container-name;

    @container #{$container-name} (#{$condition}) {
        @content;
    }
}
```
