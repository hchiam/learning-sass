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
