Middleman Startup
=========

## Getting started

Assuming you have [RVM](https://rvm.io/) and [Bundler](http://bundler.io/) already installed, `cd` into the project directory and run

    $ bundle
    $ middleman s

This runs a web server, preview your project at [http://localhost:4567](http://localhost:4567)

To build your project, run

    $ middleman build

Which compiles your templates into the `build` directory using the settings in `config.rb`. The build directory is ignored in `.gitignore`.   

All assets minifed by default. For a different build config, please see the [Middleman](http://middlemanapp.com/) documentation.

Custom settings added by this framework in `config.rb` are...

    set :use_typekit, true
    set :use_jquery, false

...use Typekit or jQuery respectively. If not using jQuery, [Tire JS](http://tirejs.com/) is included for easy DOM selectors.

======================================

## CSS Architecture

- All site assets (css, javascript, images, fonts) lie within the `/assets` folder
- Use **always** use the [BEM notation](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) for CSS classes and module structure. Use classes generously and pragmatically
- **You should be able to tell exactly what element you are affecting** from CSS and front-end only, **without looking at the source!**
- Use the SMACSS notation for state e.g. `is-hidden`, `is-active` or `is-current`
- Always prefix JS hooks or injected classes with `js-` e.g. `id="js-nav"` or `class="js-added-class"` 
- **Only use IDs for JS identifiers**, nothing else
- We are building modules, not pages

### Configuration

- All Sass configuration is located in  `assets/css/config`
- **Global variables** are defined in `config/vars` and separated into (major) breakpoints `breakpoints.sass`, colours `colours.sass`, typography `typography.sass` and units `units.sass`
- Add custom mixins to `mixins.sass`. Make use of existing and Compass mixins if possible
- Add all `z-index` values discreetly in `z-index.sass` to easily see stacking context (avoids guessing e.g. `z-index: 99999`)
- Place **module-specific variables** at the top of each object partial, to easily view them in context

### Media Queries

- This project uses Jake Archibald's [Sass-IE Framework](http://jakearchibald.github.io/sass-ie/) framework. 
- **Always create media queries at module-level using the `respond-min`, `respond-max` or `respond-min-max` mixins**
- This means IE8 will recieve a perfect fallback stylesheet stripping out media queries and including styles up to the pixel value defined in `old-ie.css.sass`
- Define all **major breakpoints** in `breakpoints.sass` using unitless pixel values. These are then converted into em-based media queries using the `respond` mixins
- Define **minor breakpoints** at module level, in the relevant object partial (again using unitless pixel values)

### Units

- Defined in `units.sass`
- Try to use the base unit `$unit` in `units.sass` as a reference for all arbitrary measurements
- Pre-defined variables are given for the baseline unit `$unit--baseline`, default gutters `$unit--gutter` as well as double and half units
- Use the `em()` function to convert units into `ems` e.g. `margin-top: em($unit * 4)`
- Making all measures in `ems` around the base unit means we can easily change sizing/spacing globally

### Partialling

- All Sass partials in the `/base`, `/generic`, and `/objects` folders **are automatically imported** using Sass globbing. This occurs *in alphabetical order*
- Base elements (default selectors) are separated into category partials in `/base`
- Generic elements and helpers should be defined in `/generic`
- Objects (or modules) are defined in `/objects` using the module name as the filename. **Every selector within an object partial should be prefixed with the module name (as-per BEM)**
- Always check if you can re-use a module (and/or use a modifier) before creating a new one

### Helpers

- Use `helpers.sass` for all helper classes or generic `@extends` and shared properties
- Make use of helper classes to quickly prototype by adding classes to markup. **Use discrection whether to keep these classes**, or `@extend` a selector

### Grid System

- Base grid configuration given in `grid.sass`
- Use `.grid` as a wrapper and `.grid__unit` for each grid cell
- Then use a modier to define grid breakpoints and gutters (if needed)
- **Always nest moodules *inside* grid units** so you can move a module into a new grid without it breaking!
- **Define grids on a per-module basis** but consider abstraction and re-use first
- [Read more about this](http://dbushell.com/2013/03/19/on-responsive-layout-and-grids/)

### Opt-In Typography

- Be default, all typographical elements are *reset* to avoid over-writing default text styling over and over
- **Use the `.copy` class as a wrapper** on a block of copy text which requires regular heading, list, paragraph etc. styling
- [Read more about this](http://dbushell.com/2012/04/18/scoping-typography-css/) 
- A [double-stranded heading hierachy](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css/) is also used. Make a `h4` look like a `h2` using the `.as-h2` class

### Browser Quirks

- Add all browser quirks / fixes in context to the element you're fixing, not in a separate stylesheet
- **Always test for browser capability first** using the Modernizr-added `<html>` classes. *Are we fixing IE8, or browsers that don't support RGBA?*
- Create all IE fixes using conditional `<html>` classes and place them within the relavent object partial
- Use the `old-ie` mixin to hide styles from all browsers that aren't IE8 and lower

### Technical Debt

- Make use of `TODO.md` to document all technical debt
- Use `generic/shame.css` as a place to store sub-optimal CSS that was rushed, needs refactoring or requires further thought