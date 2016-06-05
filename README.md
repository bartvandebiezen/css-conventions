# CSS and CSS Preprocessor Conventions

The goal of these conventions are to create reusable stylesheets and to keep them maintainable, transparent, readable, and scalable. And to make it as painless as possible to work with.

Although Less is used for the examples, it does not matter if you use Less, Sass, or even plain CSS. Choose a language based on the nature of your project. Each language has its own strengths.

For these conventions a [CSScomb config file is available](configs/csscomb.json).

## Table of Contents
- [CSS Terminology](#css-terminology)
- [Grouping and Ordering](#grouping-and-ordering)
- [Spacing](#spacing)
- [Naming](#naming)
- [Selectors](#selectors)
- [Nesting](#nesting)
- [Declarations](#declarations)
- [Values](#values)
- [Comments](#comments)

## CSS Terminology

This document assumes you are familiar with the terminology for CSS and CSS preprocessors. A short refresher:

- **Rule** = combination of selector group and declaration block.
- **Selector group** = All the selectors within a rule. Selectors within a selector group are separated by a comma.
- **Selector** = Pointer to an element in your HTML page.
- **Simple selector** = Part of a selector. Simple selectors within a selector are separated by a combinator.
- **Combinator** = Describes the relation between simple selectors.
- **Property** = Aspect of the selector you are styling.
- **Value** = Setting of the property.
- **Declaration** = Combination of a property and its value.
- **Declaration block** = All the declarations within a rule.

**Rule:**
```
selector,
simple-selector > simple-selector {
	property: value;
	property: value;
	property: value;
}
```

## Grouping and Ordering

### Rules should be ordered based on specificity

- If you use Less or Sass you should create different directories for each group.
- Avoid sub-folders to group files within a group.
- Rules within vendors, utilities, or layout are [immutable](http://csswizardry.com/2015/03/immutable-css/).

**Order should be:**

1. **vendors** (e.g. Bourbon, Bootstrap, jQuery UI)
2. **fonts** (font-facing)
3. **settings** (variables and configs, e.g. colors, fonts families, font sizes, line heights)
4. **utilities** (e.g. your mixins, functions, and tools)
5. **vendor-overrides** (to re-declare some vendor CSS with help of settings and utilities, if needed)
6. **reset** (e.g. [Reset CSS](http://meyerweb.com/eric/tools/css/reset/) or [normalize.css](http://necolas.github.io/normalize.css/), and [box-sizing](http://www.paulirish.com/2012/box-sizing-border-box-ftw/))
7. **layout** (immutable cosmetic-free objects, e.g. grid, body with a sticky footer)
8. **typography** (e.g. headings, text, code, lists, quotes)
9. **controls** (e.g. buttons, segmented-controls, pull-down-menus, text-inputs)
10. **components** (designed interface blocks: bars, dialogs, indicators, views)
11. **pages** (page specific styles. Before you add page specific styles, consider modified components)
12. **themes** (for many projects non-existent)
13. **overrides** (hacks and things you are not proud of)

See [The Specificity Graph](http://csswizardry.com/2014/10/the-specificity-graph/) and [CSS Specificity Graph Generator](http://jonassebastianohlsson.com/specificity-graph/) for more information about CSS specificity.

### For Less or Sass, use separate files for different rule groups

**File names should be:**

1. **vendors**: original directories and file names.
2. **fonts**: name of typeface.
3. **settings**: name of property or use.
4. **utilities**: name of function or mixin.
5. **vendor-overrides**: same as vendor's directories or file names.
6. **reset**: original file name or function name.
7. **layout**: name of container or function.
8. **typography**: name of typography group.
9. **controls**: name of control.
10. **components**: name of block.
11. **pages**: name of page or template.
12. **themes**: name of the theme.
13. **overrides**: depends on type.

### Do not use @import for CSS files

- Importing CSS files has a negative impact on performance.
- Create a link to the CSS file in the HTML head instead, or better: download the CSS file, place it in the correct group and change the CSS extension to the extension of your preprocessor. This forces the compiler to add the rules into your own CSS file.

### Properties should be ordered based on functionality

- Increases readability, understanding, and the ability to find duplicate properties.
- Sass or Less includes should be placed first, before all declarations within a rule. This is to improve readability and to assure declarations from includes cannot override specific declarations.

**Order should be:**

1. Positioning
2. Box behavior and properties
3. Sizing
4. Color appearance
5. Special content types
6. Text
7. Visual properties
8. Print

Tools like [CSScomb](http://csscomb.com), in combination with [a CSScomb config file](configs/csscomb.json) based on these conventions, can be used to automate ordering.

### Place rules in media queries, unless applicable for all media queries

Only rules applicable to all media queries should be placed outside a media query. All other rules should be written per media query. This makes sure you never have to overwrite rules in an other media query, the code is easier to read, and it [improves render performance](http://meiert.com/en/blog/20080419/reset-style-sheets-are-bad/). This means that you should not write CSS rules mobile first and change the rules for bigger viewports. You write CSS rules meant for a viewport only for that specific viewport. This does not mean you shouldn't design mobile first.

**Right:**
```CSS
.class {
	background-color: red;
}

@media screen and (max-width: 440px) {
    .class {
        padding: 20px;
    }
}

@media screen and (min-width: 441px) and (max-width: 1040px) {
    .class {
        padding: 40px;
    }
}

@media screen and (min-width: 1041px) {
    .class {
        padding: 60px;
    }
}
```

**Wrong:**
```CSS
.class {
	background-color: red;
	padding: 20px;
}

@media screen and (min-width: 441px) {
    .class {
        padding: 40px;
    }
}

@media screen and (min-width: 1041px) {
    .class {
        padding: 60px;
    }
}
```

## Spacing

### Indents should be done with one tab, not spaces

- Tabs allow developers with different preferences in indentation size to change how the code looks.
- It is impossible to half-indent with tabs.
- Requires less interaction.

**Right:**
```CSS
selector {
	property: value;
}
```

**Wrong:**
```CSS
selector {
 property: value;
}
```

### Rules should be separated by one empty line

**Right:**
```CSS
selector {
	property: value;
}

selector {
	property: value;
}
```

**Wrong:**
```CSS
selector {
	property: value;
}
selector {
	property: value;
}


selector {
	property: value;
}
```

### Selectors, separated by a comma, should be placed on separate lines

- Makes it easier to find and optimize selectors.

**Right:**
```CSS
selector,
simple-selector simple-selector,
selector {
	property: value;
}
```

**Wrong:**
```CSS
selector, simple-selector simple-selector, selector {
	property: value;
}
```

### Declarations should be on a separate line

- Exception: when there is a large amount of selectors with similar properties. Only in this case it is allowed to place selectors and declarations on a single line.

**Right:**
```CSS
selector {
	property: value;
	property: value;
	property: value;
}
```

**Wrong:**
```CSS
selector {
	property: value; property: value;

	property: value;
}
```

### Strings should be quoted with a single quote

**Right:**
```CSS
selector {
	font-family: 'Helvetica Neue Light', 'Helvetica', 'Arial', sans-serif;
	background-image: url('/images/image.jpg');
}
```

**Wrong:**
```CSS
selector {
	font-family: "Helvetica Neue Light", "Helvetica", "Arial", sans-serif;
	font-family: Helvetica Neue Light, Helvetica, Arial, sans-serif;
	background-image: url(/images/image.jpg);
}
```

### Value lists should be written on the same line

- Exception: only very long comma separated values, such as font-face URLs and gradients, may be written on different lines to improve readability.

**Right:**
```CSS
@font-face {
	font-family: 'Source Sans Pro';
	font-weight: 400;
	font-style: normal;
	src: url('../fonts/SourceSansPro/SourceSansPro-Regular.eot');
	src: url('../fonts/SourceSansPro/SourceSansPro-Regular.eot?#iefix') format('embedded-opentype'),
		 url('../fonts/SourceSansPro/SourceSansPro-Regular.woff2') format('woff2'),
		 url('../fonts/SourceSansPro/SourceSansPro-Regular.woff') format('woff'),
		 url('../fonts/SourceSansPro/SourceSansPro-Regular.ttf') format('truetype');
}

selector {
	font-family: 'Source Sans Pro', 'Helvetica', 'Arial', sans-serif;
}
```

**Wrong:**
```CSS
selector {
	font-family:
		'Helvetica Neue Light',
		'Helvetica',
		'Arial',
		sans-serif;
}
```

### Other spacing conventions

- Add spaces before and after a selector combinator (e.g. ' > ').
- Remove spaces before selector delimiter (',').
- Add a space between selector and opening brace ('{').
- Add a line break after the opening brace ('{').
- No spaces between property and colon (':').
- Add a space between colon (':') and value.
- Comma-separated values should include a space after each comma.
- Comma-separated values within parentheses ('()') should include a space after each comma.
- Parentheses ('()') should not be padded with spaces.
- Align prefixes.
- Add a trailing semi-colon (';') after the last declaration.
- Add a line break before closing brace ('}').
- Trim trailing spaces or tabs.
- Add an empty line at end of file.

Tools like [CSScomb](http://csscomb.com), in combination with [a CSScomb config file](.csscomb.json) based on these conventions, can be used to automate spacing.

## Naming

### Write selectors in US English

- CSS is also written in US English, e.g. color (US English) instead of colour (British English).

### Write selectors in lower-case and use hyphen-delimited syntax

- CSS is a hyphen-delimited syntax.
- Makes it easier to read and scan.

**Right:**
```CSS
selector-name {}
```

**Wrong:**
```CSS
SelectorName {}
SELECTOR_NAME {}
```

See ['CSS: CamelCase Seriously Sucks!'](http://csswizardry.com/2010/12/css-camel-case-seriously-sucks/) for further reading.

### Names of selectors or variables should follow 'BEM Methodology' honed by Nicolas Gallagher

- Enables you to write modular based CSS, which makes it easier to manage larger projects that last a while.
- Name of an element is a combination of the surrounding block and element, divided with two underscores '\_\_'.
- Name of a modified block or element is a combination between the name and the modifier, divided with two hyphens '--'.
- Modification means a different version of a block or element.
- Modifiers are timeless. Use 'states' if a 'different version' is temporarily.
- A block can be nested within another block if that block is often used by itself. The nested block has at least two class names in this order: (1) as an element of the surrounding block and (2) the block name itself. This is called a 'mix'. The element class should only define contextual positioning, never styling.
- A class does not reflect the full trail of the DOM. Only one block or element is allowed within a class name.
- When is a block a modified block or a complete new block? Is it '.small-project' or 'project--small'? A modified block still inherits a lot of styling from the original block. A new block is completely independent from the original block. You determine the tipping point, but if in doubt create a new block.
- It is not allowed to give an element two block classes or two element classes.
- It is not allowed to place an element class from a block in another block.

**Right:**
```CSS
.block {}
.block__element {}
.block--modifier {}
.site-search {} /* Block */
.site-search__field {} /* Element */
.site-search--full {} /* Modifier */
.person {}
.person__hand {}
.person--female {}
.person--female__hand {}
.person__hand--left {}
.person {}
.hand {}
.female {}
.female-hand {}
.left-hand {}
```

**Wrong:**
```CSS
.block__element__element {}
.block__block__element {}
```

See [BEM](https://bem.info), [CSS Wizardry](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/), [Google's Web Fundamentals](https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations#use-block-element-modifier), and [Side Effects in CSS](http://philipwalton.com/articles/side-effects-in-css/) for further reading.

### Never style HTML elements directly, always use class names.

- Also applies to typographic elements such as 'h1' and 'p'. For user content or other content containing only HTML elements without class names, use the class '.content' as a container.
- Exception applies to rules in reset folder.

**Right:**
```CSS
.content h1 {}
.content p {}
.content ul {}
```

**Wrong:**
```CSS
h1 {}
p {}
ul {}
```

See [Modular CSS typography](http://thesassway.com/advanced/modular-css-typography) and [Side Effects in CSS](http://philipwalton.com/articles/side-effects-in-css/) for further reading.

### Names of variants (within a rhythm) for selectors or variables should follow 'city block sizes'

- The standard variant of a pattern gets the modifier '100'.
- There are two standard variants: with and without a number. Especially useful when you start patterns without variants and you have to add variants later on in a project. You don't want to change all the names in your project.
- Smaller variants get the modifiers '90', '80', '70',…
- Large variants get the modifiers '200', '300',…
- A modifier that removes paddings or height from a block can be written as '0', for example pane--0 is a section without padding-top and padding-bottom.
- Numbers don't resemble exact size ratios, because that would be descriptive naming: button--200 is not necessary twice as large as button--100.
- City block sizes for Less or Sass variables should be written as part of the name (right: '@line-height-200') and should not be written as BEM modifiers (wrong: '@line-height--200').

**Right:**
```CSS

.button {
	property: value;
}

.button--90 {
	property: value;
}

.button--100 {
	.button;
}

.button--200 {
	property: value;
}
```

**Wrong:**
```CSS
.button--small {
	property: value;
}

.button--medium {
	property: value;
}

.button--large {
	property: value;
}
```

See ['How to build the perfect pattern library'](http://www.slideshare.net/WolfBruening/how-to-build-the-perfect-pattern-libraryy) for further reading.

### Fractions used for grids and modifiers should be written out in full

City block sizes are used for rhythms, never for exact values or fractions. When you want to specify that an object should be exactly one third or two tenths of a width, add modifiers as: .class--one-third or .class--two-tenths. When you want to create a modifier for full width, use: .class--full.

**Right:**
```CSS
.column--one-third {}
.column--two-tenths {}
.column--full {}
```

**Wrong:**
```CSS
.column--1-3 {}
.column--2-10 {}
.column--1 {}

.column--80 {}
.column--90 {}
.column--100 {}
```

### Modifiers for different flex-grows should be written out in full

Depending on the 'grow value' (default is 1) and the amount of elements within a flex container, an element takes more or less space within the flex container. Flex-grow doesn't dictate a fixed size or fraction, it is a unit-less value. For more about flex-grow and Flexbox Layout see ['A Complete Guide to Flexbox'](https://css-tricks.com/snippets/css/a-guide-to-flexbox/). These unit-less values should be written out in full.

**Right:**
```CSS
.class--takes-two {
	flex-grow: 2;
}

.class--takes-three {
	flex-grow: 3;
}
```

**Wrong:**
```CSS
.class--2 {
	flex-grow: 2;
}

.class--3 {
	flex-grow: 3;
}
```

### Names of selectors or variables should be written out in full

- Don't shorten selector or variable names.
- Shorter selector names could lead to misunderstandings.
- Shorter selector names don't affect file size that much, because of GZIP.
- You may use an abbreviation for selectors if it is already used as an official HTML element, e.g. '.nav' instead of '.navigation', or '.h1' instead of '.heading-1'. An exception is 'ui'. You may use 'ui' instead of 'user-interface'.

**Right:**
```CSS
.button {
	property: value;
}
```

**Wrong:**
```CSS
.btn {
	property: value;
}
```

### Names of variables should start with the property or type

- Makes it easier to group them and to use autocomplete in development tools.

**Right:**
```Less
@font-family-monospace: ;
@color-blue: ;
```

**Wrong:**
```Less
@monospace-font-family: ;
@blue-color: ;
```

### Names of variables for colors should be written both descriptive and functional

- Start with descriptive variables, followed by the functional variables.
- City block sizes may be used for functional names. Higher number means darker, lower number means lighter. You can use Less or Sass functions to control the variants. It is also possible to use descriptive names for variants.
- Use variables even for black or white, because black and white could contain, depending on the styleguide, a tint.
- Good functional colors are: color-ui; color-ui-accent; color-heading; color-subheading; color-text; color-text-diap; color-cta; color-primary; color-secondary; color-tertiary; color-error; color-success; color-select; color-focus;

**Right:**
```Less
// Descriptive colors

@color-silver: rgb(50, 50, 50);

// Functional colors

@color-primary: @color-silver;
@color-primary-90: lighten(@color-primary-100, 10%)
@color-primary-100: @color-primary;
@color-primary-200: darken(@color-primary-100, 10%)
```

**Wrong:**
```Less
@color-primary: rgb(50, 50, 50);
```

See ['Name that Color'](http://chir.ag/projects/name-that-color/) for example for finding descriptive names.

### Local variables should start with the name of the block and end with a property

- Local variables are written directly above the block rule in the same file.
- Local variables may only be used in the block it is created for. Local variables written above the block rule are technically not local in Sass or Less and can be used everywhere. You should never do this.
- Local variables should function as 'settings' for a block.
- For local variables, combine name of the block and, in some cases, the element with the property. Only use hyphens as delimiters. Do not use the BEM notation for variables.
- Make as few settings as possible, and try to use block settings for elements. For example border colors are not always set on a block but on different elements within a block. If the border color should be the same for all these elements, create only one block variable.
- Use local variables mostly for colors.
- Use global functional colors, not descriptive colors, to set local variables.
- Within Sass, local variables are set as default variables with '!default'.

Example written for Sass:

**Right:**
```SCSS
$button-background-color: $color-secondary !default;
$button-border-color: $color-secondary-200 !default;
```

**Wrong:**
```SCSS
$button-background-color: $color-red !default;
$border-color-button: $color-secondary-200;
```

### Names of media queries should be based on the context of a device

- There are major and minor ranges.
- Major ranges should be based on the context of a device.
- Minor ranges (a.k.a. tweak points) are based on size differences within major ranges.
- Breakpoints should only be used if the content requires it.

**Visual presentation of ranges:**
```
---palm--|--hand--|--lap--|--desk---
```

**Right:**
```Less
@range-palm:      ~"only screen and (max-width:440px)";
@range-palm-s:    ~"only screen and (max-width:320px)";
@range-palm-m:    ~"only screen and (min-width:321px) and (max-width:360px)";
@range-palm-l:    ~"only screen and (min-width:361px) and (max-width:440px)";
@range-hand:      ~"only screen and (min-width:441px) and (max-width:680px)";
@range-hand-s:    ~"only screen and (min-width:441px) and (max-width:520px)";
@range-hand-m:    ~"only screen and (min-width:521px) and (max-width:600px)";
@range-hand-l:    ~"only screen and (min-width:601px) and (max-width:680px)";
@range-lap:       ~"only screen and (min-width:681px) and (max-width:1040px)";
@range-lap-s:     ~"only screen and (min-width:681px) and (max-width:800px)";
@range-lap-m:     ~"only screen and (min-width:801px) and (max-width:920px)";
@range-lap-l:     ~"only screen and (min-width:921px) and (max-width:1040px)";
@range-desk:      ~"only screen and (min-width:1041px)";
@range-desk-s:    ~"only screen and (min-width:1041px) and (max-width:1280px)";
@range-desk-m:    ~"only screen and (min-width:1281px) and (max-width:1600px)";
@range-desk-l:    ~"only screen and (min-width:1601px)";
```

**Wrong:**
```Less
@breakpoint-small: ;
@breakpoint-smedium: ;
@breakpoint-medium: ;
@breakpoint-large: ;
```

See ['Responsive grid systems; a solution?'](http://csswizardry.com/2013/02/responsive-grid-systems-a-solution/) and ['Tweakpoints'](http://adactio.com/journal/6044/) for further reading.

## Selectors

### Name selectors with reusability in mind

- When you write a selector name for a new object, ask yourself if you could not only use this object multiple times in your current project but also in future projects.
- Avoid names describing content or styling.

**Right:**
```CSS
.menu-item {}
.is-highlighted {}
.page-row {}
.sheet {}
```

**Wrong:**
```CSS
.menu-item-about-us {}
.red {}
.border-top {}
.border-right {}
```

### Never reference IDs from CSS files

- IDs are overly specific and unnecessary.
- Performance difference between classes and IDs is irrelevant.

**Right:**
```CSS
.class {
	property: value;
}
```

**Wrong:**
```CSS
#id {
	property: value;
}
```

See['Don't use IDs in CSS selectors?'](http://oli.jp/2011/ids/) for further reading.

### Never reference class names in your CSS files used by other applications or languages

- Never reference, for example, classes prefixed with 'js-', 'test-', or 'track-', which are used by Javascript, testing, or tracking tools.

### Use states as separate classes and add them to existing selectors

- States differ from modifiers. Modifiers are timeless, states are temporarily.
- States never contain global styling. Style states always in combination with other selectors. This makes it easier to reuse components in different projects.
- States are allowed to be nested with Less or Sass. Usage is similar to pseudo-classes and pseudo-elements.
- States always start with 'is'.
- Preferred state names are listed below.

**Right:**
```CSS
selector.is-visible {}
selector.is-hidden {}
selector.is-added {}
selector.is-removed {}
selector.is-active {}
selector.is-disabled {}
selector.is-collapsed {}
selector.is-expanded {}
selector.is-highlighted {}
selector.is-invalid {}
selector.is-valid {}
selector.is-touched {}
selector.is-untouched {}
selector.is-pristine {}
selector.is-dirty {}
selector.is-dragging {}
selector.is-dropped {}
selector.is-selected {}
selector.is-filled {}
selector.is-empty {}
selector.is-updating {}
selector.is-loaded {}
selector.is-loading {}
```

**Wrong:**
```CSS
.is-hidden {
	display: none;
}

.hidden {
	display: none;
}
```

## Nesting

### A selector may contain no more than three simple selectors

- Even when using the BEM methodology, you cannot avoid cascading. Think about content where you have little control, such as user generated content. In these cases it is allowed to cascade, but never deeper than three levels.

**Right:**
```CSS
.content td p {
	property: value;
}
```

**Wrong:**
```CSS
.content table td p {
	property: value;
}
```

### Do not nest rules with Less or Sass

- Nesting rules with Less or Sass makes it more difficult to optimize your CSS.

**Right:**
```CSS
simple-selector {
	property: value;
}

simple-selector simple-selector {
	property: value;
}
```

**Wrong:**
```Less
simple-selector {
	property: value;
	simple-selector {
		property: value;
	}
}
```

### Do nest pseudo-classes, pseudo-elements, media queries, states, and BEM block modifiers with Less or Sass

- Makes sure style and behavior of the same selector are grouped.
- Also BEM block modifiers (not BEM element modifiers) are nested within BEM elements. This makes it easier for block modifiers to influence different elements at the same time and keep the behavior of an element contained within the Less or Sass.
- Nested pseudo-classes, pseudo-elements, media queries, states, or BEM block modifiers should also be separated by an empty line.
- The behavior of a pseudo-element should be grouped. Place media queries with rules for a pseudo-element within that pseudo-element. As a result, you don't have to repeat the pseudo-element per media query.
- They way you nest pseudo-elements and pseudo-classes is very different. Pseudo-classes should be nested within media queries when its styling is different per media query. That means you have to repeat pseudo-classes for every media query. The styling of a pseudo-class is related to the styling of the selector per media query. Read [Pseudo-classes vs pseudo-elements](http://www.growingwiththeweb.com/2012/08/pseudo-classes-vs-pseudo-elements.html) to learn more about the difference between pseudo elements and pseudo classes.

**Right:**
```Less
selector {
	property: value;

	@media @range-lap {
		property: value;

		&:last-child {
			property: value;
		}

	}

	@media @range-desk {
		property: value;

		&:last-child {
			property: value;
			property: value;
		}

	}

	&::after {
		property: value;

		@media @range-lap {
			property: value;
			property: value;
		}

		@media @range-desk {
			property: value;
		}
	}

}
```

## Declarations

### All declarations should end with a semi-colon. Even the last declaration within a rule

- Makes it easier to reorder or add declarations.
- Removing the last semi-colon is needles optimization. Automate this with a compiler or script.

## Values

### Color units should be written in 'HSL' or 'HSLa'

- HSL or HSLa makes changing color values much easier.
- Less or Sass should convert HSL to hex color codes to reduce file size.

**Right:**
```SCSS
hsl(120, 100%, 50%);
hsla(120, 100%, 50%, 0.5);
```

**Wrong:**
```SCSS
#fff;
#ffffff;
white;
rgb(50, 50, 50);
rgba(50, 50, 50, 0.5);
```

### Absolute sizes should be written in pixels instead of ems or rems

- A 'CSS px' is a reference pixel intended to scale in size depending on the 'typical' distance of an observer from the display.
- Web browsers scale a design in pixels the same way they scale designs in ems or rems. Zooming even triggers media queries based on pixels. Essentially, web browsers make the reference pixel larger or smaller.
- Makes it easier to integrate images or other media formats based on pixels in your layout.
- Makes sure you don't get nummers like '0.29310344827586204em' (this is an actual used number).
- Sizing in pixels reduces development time.
- Sizes in ems or rems are allowed, but only when it should actually be based on the font-size. For example a max width of 30 ems to make sure lines never get too long.

See ['W3C Recommendations about lengths'](http://www.w3.org/TR/CSS21/syndata.html#length-units) for further reading.

### Z-indexes are limited to 12 levels

- The z-index starts at -100 and is limited to 1000. Steps are 100.
- Steps of hundreds are used to allow adding additional numbers. But will probably never happen. If more z-index are needed, rethink your code.

**Right:**
```CSS
selector {
	z-index: 200;
}
```

**Wrong:**
```CSS
selector {
	z-index: 248;
}

selector {
	z-index: 9999;
}
```

### Avoid adding units to zero-values

**Right:**
```CSS
selector {
	margin: 0;
}
```

**Wrong:**
```CSS
selector {
	margin: 0px;
}
```

### Always include a leading zero when a numeric value is less than 1

- Do not add trailing zeros.

**Right:**
```CSS
selector {
	margin: 0.5%;
}
```

**Wrong:**
```CSS
selector {
	margin: .50%;
}
```

### Link font weights to their correct CSS values

- Sometimes we encounter fonts that have been given cryptic file names: not containing any clue about the weight or typeface. Rewrite the file names, before you add these to your project. A good pattern would be: [typeface]-[weight].[extension].

**Font weights:**

- 100 = hair
- 200 = thin
- 300 = light
- 400 = normal (or regular, book)
- 500 = medium
- 600 = semi-bold (or demi-bold)
- 700 = bold
- 800 = black (or heavy)
- 900 = ultra

### Only use '!important' when you know up front it should always override a style

- Never use '!important' to fix an existing problem.

## Comments

### Use Less/Sass commenting style '//' for comments pointless for debugging

- Applicable for commenting code not visible in CSS, such as mixins, or variables. It assures you don't have loose comments floating around in dev version: Less/Sass style comments are always compiled out.
- Comments outside rules should be separated by a single empty line.
- Do not add extra dividers to mark a comment. If needed, change your code coloring to identify comment blocks.
- Use markdown within comments.
- Do not add line breaks for wrapping.

**Right:**
```Less
// # Colors

@color-blue: rgb(0, 0, 255); // #0000ff
```

**Wrong:**
```Less
/* -- colors -- */

@color-blue: rgb(0, 0, 255); /* #0000ff */
```

### Use CSS commenting style '/* */' for comments useful for debugging

- Applicable for commenting code visible in CSS, such as rules, selectors, properties, or values.
- Preserve CSS style comments during compiling Less or Sass code for dev versions.
- CSS style comments should be removed during compiling Less or Sass code for live versions.
- Comments outside rules should be separated by a single empty line.
- For comments outsides rules, start and end syntaxes should be on a separate line. Even for single line comments, because of consistency and making transitions between single and multi line (adding and removing lines) easier.
- Comments inside rules (a.k.a. inline comments) can be written as 'single line comment': syntaxes and comment on the same line.
- Do not add extra dividers to mark a section. If needed, change your code coloring to identify comment blocks.
- Do not indent comments or start every line with special characters such as asterisks.
- Use markdown within comments.
- Do not add line breaks for wrapping.

**Right:**
```CSS
/*
# Heading 1
Paragraph text with a [link](url).
*/

/*
Comment on one line outside a rule.
*/

selector {
	property: value; /* Comment on one line inside a rule */
}
```

**Wrong:**
```CSS
/*===
* -- Heading 1 --
* Paragraph
===*/
```

### Numbered labels may be used for inline comments

- Allowed when a rule contains inline comments that are too long to be readable.
- Allowed when a comment is applicable for multiple declarations.
- When using numbered labels for a rule, also use numbered labels for all other inline comments for that rule.

**Right:**
```CSS
/*
Image
1. Makes it responsive.
*/

img {
	max-width: 100%; /* [1] */
	height: auto; /* [1] */
}
```

### Never limit comment lines to 80 characters

- Some guidelines advice to limit lines to 80 characters for readability. Don't do this.
- Line breaks have or should have semantic value.
- Manually limiting line length adds unnecessary extra time for editing comments.
- Set your editor to wrap lines instead.

### Use special tags to mark comments

- Using consistent tags such as 'TODO' makes sure they can be easily found with text search.
- For TODOs include a date when it should be done.
- Always start TODOs with a verb.
- Move TODOs as soon as possible to your backlog.

**Available tags**
- TODO: a task that should be done in the near future.
- BUG: something that should be done as soon as possible.
- HACK: fix for a specific web browser or situation.
- DEBUG: A temporary comment.
- IDEA: Possible improvement.
- ???: Unclear, needs a better description.
- CRED: Credits for someone.

**Right:**
```CSS
selector {
	property: value; /* HACK: for Internet Explorer 9 */
	property: red; /* TODO 2015 04 04: Change value to blue */
}
```

### Use KSS for documenting CSS components

To make component based development, generating pattern libraries and reuse of components easier, it is advised to add documentation for each component. For this, you can use [KSS style commenting](https://github.com/kneath/kss) (also see [KSS-node](https://github.com/kss-node/kss-node) or [SC5](https://github.com/SC5/sc5-styleguide)). Because KSS has its own conventions, the conventions for comments written earlier in this document do not apply to KSS comments.

**Right:**
```css
/*
Buttons

A majority of buttons in the site are built from the same base class.

Markup: buttons.hbs

:hover     - Highlight the button when hovered.
:disabled  - Make the button change appearance to reflect it being disabled.
:active    - "Press" the button down when clicked.

Style guide: components - buttons
*/
```

See [KSS](https://github.com/kneath/kss), [KSS-node](https://github.com/kss-node/kss-node), and [SC5](https://github.com/SC5/sc5-styleguide) for further reading.

## Acknowledgements and Further Reading

- [sass-guideline.es](http://sass-guidelin.es/#syntax--formatting)
- [cssguidelin.es](http://cssguidelin.es)
- [Medium's Less Coding Guidelines](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06).
- [Chris Pearce CSS Guidelines](https://github.com/chris-pearce/css-guidelines)
- [Five Q CSS Guidelines](https://github.com/akwright/Five-Q-CSS-Guidelines)
