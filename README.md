# sassSandbox
My Sass sandbox.

    Sass = "better" CSS.

Sass is a stylesheet language interpreted (preprocessed) into CSS.
I use the newer syntax called SCSS.

Some features of SCSS are planned to become standard CSS in the future, but for now pre-processing with SCSS Sass can improve writing CSS files.

Use http://www.sassmeister.com/
or do something like this in command-line/terminal:

`sass input.scss output.css`

or do something like this in command-line/terminal, using node:

`node-sass input.scss output.css`


The `.map` source map file that's created maps each line in the CSS file to the corresponding line in the Sass file.

The `.sass-cache` cache folder that's created is used to speed up re-compilation.

# tutorial:
http://tutorialzine.com/2016/01/learn-sass-in-15-minutes/

# Sass features shown through described code examples:
http://sass-lang.com/guide and http://tutorialzine.com/2016/01/learn-sass-in-15-minutes/

* preprocessing
* variables (so you change properties in only one place)
* nesting
* partials (e.g. to have small separate Sass files for re-use)
* imports
* mixins (like constructor classes.  Can be used for prefixes for Firefox and other vendors)
* extend/inheritance (e.g. classes overwriting properties)
* operators (for pre-calculating properties)
* function (you can make your own or use built-in ones, e.g. darken($color, $amount) )