# GOV.UK template, GOV.UK frontend toolkit & GOV.UK elements SCSS refactor

---

The purpose of this:

- Decide on filenames and naming conventions
- Take the existing scss from the GOV.UK template, GOV.UK frontend toolkit and GOV.UK elements and apply ITCSS principles
- Rewrite classnames using BEM to give greater flexibility than provided by GOV.UK elements

---

Things to do:

* Use BEM as a naming convention
* BEM stands for `Block__Element--Modifier`, not `Block__Element__Element--Modifier`. So, avoid multiple element level naming.
* Use Normalize
* Remove duplicate resets (compare Normalize to GOV.UK template base styles and GOV.UK elements reset)
* Use single-level-deep classes, reduce nesting
* Reduce the size of the codebase
* Remove CSS which isn't being used, remove bloat and waste
* Add documentation for each layer
* Remove placeholders % where used
* Decide on use of prefixes o- object ?
* Decide how to set widths for form-element__control

---

The files in assets/scss have been ordered following ITCSS principles:

### Settings

High level settings, global variables

For example colour values, spacing units, globally available variables
This layer can be used by the entire project.

### Tools

Mixins and functions
Global mixins come after the settings
Need to come before the rest of the prohect and can affect the entire project

For example globally available font sizes
tools.typography.scss
Decide which mixins are global ones

### Generic

Ground zero styles, normalize, resets box sizing
the * selector for box-sizing
This layer affects a lot of the dom

### Base (HTML elements)

Unclassed HTML elements (type selectors)
More explicit than the reset layer
h1 - h6, basic links, lists

### Objects

This layer is optional.
Aim to work towards adding this layer.

This is the objects layer, put the media object in here.
Try to create a generic list object

    .ui-list {}
    .ui-list__item {}

Choose agnostic names.

### Components

Fully designed chunks of UI
Private mixins sit in relevant file
This is where the ui-list (in the objects layer above) becomes a product list

    .products-list {}
    .products-list__item {}

### Overrides, helpers and utilities

Affect one piece of the dom at a time
Very high specificity, usually carry !important;
e.g. helper class

    .one-half {
      width: 50% !important;
    }

This is the most specific layer.

We are progressively adding and never undoing.
Causes rules to trickle together and we are adding on layers, not undoing.

------------------------------------------------------------------------------------------

### Namespaces:

* Decide on use of [namespaces/prefixes](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/) o- object ?
* Do we want to use a c- prefix for components?

Namespaces we are using:

Component          c-               c-message-bar
Object (layout)    o-               o-site-width-container
Utility            u-               u-one-half
States             is-              is-visible
                   has-             has-loaded
JS hooks           js-              js-enabled

For JS hooks, we currently use

    .js-enabled .js-hidden, would this make more sense as
    .js-enabled .is-hidden { display: none; }

### States:

    <!-- standalone state hook -->
    <div class="c-card is-active">
        […]
    </div>

    <!-- or BEM modifier -->
    <div class="c-card c-card--is-active">
        […]
    </div>

Use standalone classes to apply states to components.

Examples of standalone classes:

    .is-active
    .has-loaded
    .is-visible
    .is-hidden
    .is-disabled
    .is-expanded
    .is-collapsed


------------------------------------------------------------------------------------------

### Questions to answer

- Print stylesheet & how do the mixins for this work?
- What units do we want to use rem/em/px a combo? At the moment we use 15px, this means we can't vertically align things properly
- Names for sizes of things?

    .button--big or .button--large
    .font-small or .font-alpha

------------------------------------------------------------------------------------------

### Other things consider (out of scope for this prototype)

- Can we reduce the size of the font files?
- Can we fallback to PNGs in template/images and instead use SVG?

------------------------------------------------------------------------------------------

### Futher reading

https://www.smashingmagazine.com/2016/06/battling-bem-extended-edition-common-problems-and-how-to-avoid-them/
https://github.com/chris-pearce/css-guidelines#state-hooks

------------------------------------------------------------------------------------------

### ITCSS Principles:

- Order stylesheets from generic to specific, this helps you manage the cascade
- You're also then doing less overriding as you move further down the set of stylesheets
- Manages source order and makes it specificity based
- Filters explicitness
- Check to see how much waste it removed (reduced number of strikethroughs for each element)
- Add documentation, if you need to edit type selectors, edit overrides, look in this layer.
- Open to extension, but closed to modification

### ITCSS tl;dr
- Write CSS in specificity order
- All rulesets should only ever add to and inherit from other styles
- DO NOT UNDO CSS later in the stylesheet
- Overrides and helpers need to come last
  This prevents a spike in the specificity graph
- Add extra layers as you need
- Media queries sit with the rules that they affect
