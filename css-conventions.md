# CSS & CSS Preprocessor Conventions

## Grouping & Ordering

### Rules should be ordered based on specificity.

**Order should be:**

1. vendors (e.g. Bootstrap, jQuery UI)
2. vendor-overrides (to re-declare some vendor CSS, if needed)
3. settings (variables and configs, e.g. colors, fonts, font-sizes)
4. tools (e.g. mixins, functions)
5. reset (e.g. normalize.css and box-sizing)
6. base (HTML elements, e.g. h1-h6 styles)
7. layout (wrapping and constraining elements, e.g. grid, sections, body, header, footer)
8. components (e.g. buttons, date selector, stepper)
9. pages (page specific styles)
10. overrides (hacks and things we are not proud of)

### Properties should be ordered based on functionality.

- Increases readability, understanding, and the ability to find duplicate properties.
- SASS or LESS includes should be placed above all properties within a rule. This is to improve readability and to make sure properties from includes cannot override specific properties.

**Order should be:**

1. Positioning
2. Box behavior and properties
3. Sizing
4. Color appearance
5. Special content types
6. Text
7. Visual properties
8. Print

See [Zen CSS Properties](https://code.google.com/p/zen-coding/wiki/ZenCSSPropertiesEn) for further reading. Tools like [CSScomb](http://csscomb.com) can be used to automate ordering.

## Spacing

### Indents should be done with one tab, not spaces.

- Tabs allow developers with different preferences in indentation size to change how the code looks.
- It is impossible to half-indent with tabs.
- Requires less interaction.

**Right:**
```css
selector {
	property: value;
}
```

**Wrong:**
```css
selector {
 property: value;
}
```

### Rules should be separated by one empty line.

**Right:**
```css
selector {
	property: value;
}

selector {
	property: value;
}
```

**Wrong:**
```css
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

### Selectors should be comma separated and on a separate line.

- Makes it easier to find and optimize selectors.

**Right:**
```css
selector,
selector {
	property: value;
}
```

**Wrong:**
```css
selector, selector {
	property: value;
}
```

### Properties should be on a separate line.

- There is one exception: when there is a large amount of selectors with one specific property. Only then it is allowed to place selectors and properties on one line.

**Right:**
```css
selector {
	property: value;
	property: value;
	property: value;
}
```

**Wrong:**
```css
selector {
	property: value; property: value;

	property: value;
}
```

### Strings should be quoted with a single quote.

**Right:**
```css
selector {
	font-family: 'Helvetica Neue Light', 'Helvetica', 'Arial', sans-serif;
	background-image: url('/images/image.jpg');
}
```

**Wrong:**
```css
selector {
	font-family: "Helvetica Neue Light", "Helvetica", "Arial", sans-serif;
	font-family: Helvetica Neue Light, Helvetica, Arial, sans-serif;
	background-image: url(/images/image.jpg);
}
```

### Value lists should be written on the same line.

**Right:**
```css
selector {
	font-family: 'Helvetica Neue Light', 'Helvetica', 'Arial', sans-serif;
}
```

**Wrong:**
```css
selector {
	font-family:
		'Helvetica Neue Light',
		'Helvetica',
		'Arial',
		sans-serif;
}
```

### Other spacing conventions

- Add an empty line at end of file.
- No spaces between property and colon.
- Add a space between colon and value.
- Remove spaces before and after a selector combinator.
- Add a space between selector and opening brace.
- Add a line break after the opening brace.
- Remove spaces before selector delimiter.
- Add a line break before closing brace.
- Trim trailing spaces or tabs.
- Align prefixes.

## Naming

### Write selectors in lowercase, and separate different words within a name with hyphens.

**Right:**
```css
selector-name {}
```

**Wrong:**
```css
SelectorName {}
SELECTOR_NAME {}
```

### Names of selectors or variables should follow 'BEM Methodology' honed by Nicolas Gallagher.

- Enables you to write modular based CSS, which makes it easier to manage larger projects that last a while.
- Name of an element is a combination of the surrounding block and element, divided with two underscores '__'.
- Name of a modified block or element is a combination between the name and the modifier, divided with two hyphens ‘--‘.
- Modification means a different version of a block or element.
- Modifiers are timeless. Use 'states' if a 'different version' is temporarily.
- A block can be nested within another block if that block is often used by itself. The nested block has at least two class names: (1) the block name itself and (2) as an element of the surrounding block.

**Right:**
```css
.block {}
.block__element {}
.block--modifier {}
.site-search {} // Block
.site-search__field {} // Element
.site-search--full {} // Modifier
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

See [BEM](https://bem.info) and [CSS Wizardry](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) for further reading.

### Names of variants (within a rhythm) for selectors or variables should follow 'city block sizes'.

- The standard variant of a pattern gets the modifier '100'.
- There are two standard variants, one with and one without a number. Especially useful when you start patterns without variants and you have to add variants later on in a project. You don't want to change all the names in your project.
- Smaller variants get the modifiers ’90', '80', '70',…
- Large variants get the modifiers '200', '300',…
- Numbers don't resemble exact size ratios, because that would be descriptive naming: button--200 is not necessary twice as large as button--100.

**Right:**
```css
selector--90 {
	property: value;
}

selector {
	property: value;
}

selector--100 {
	property: value;
}

selector--200 {
	property: value;
}
```

**Wrong:**
```css
selector--small {
	property: value;
}

selector--medium {
	property: value;
}

selector--large {
	property: value;
}
```

See ['How to build the perfect pattern library'](http://www.slideshare.net/WolfBruening/how-to-build-the-perfect-pattern-libraryy) for further reading.

### Names of selectors or variables should be written in full.

- Don't shorten selector or variable names.
- Shorter selector names could make it more difficult to understand selector names.
- Shorter selector names don't affect file size that much, because of GZIP.
- There is one exception: use 'nav' instead of 'navigation'.

**Right:**
```css
.button {
	property: value;
}
```

**Wrong:**
```css
.btn {
	property: value;
}
```

### Names of variables should start with the property or type.

- Makes it easier to group them and to use autocomplete in development tools.

**Right:**
```css
@font-family-monospace: ;
@color-blue: ;
```

**Wrong:**
```css
@monospace-font-family: ;
@blue-color: ;
```

### Names of variables for colors should be written both descriptive and functional.

- Start with descriptive variables, then set functional variables.
- City block sizes can be used for functional names. Higher number means darker, lower number means lighter. You can use LESS or SASS functions to control the variants, but it is also possible to use descriptive names for variants.
- Use variables even for black or white.

**Right:**
```css
@color-silver: rgb(50, 50, 50);
@color-primary: @color-silver;
@color-primary--90: lighten(@color-primary--100, 10%)
@color-primary--100: @color-primary;
@color-primary--200: darken(@color-primary--100, 10%)
```

**Wrong:**
```css
@color-primary: rgb(50, 50, 50);
```

See ['Name that Color'](http://chir.ag/projects/name-that-color/) for example for finding descriptive names.

### Names of media queries should be based on human ergonomics.

- There are major and minor ranges or breakpoints.
- Major ranges should be based on human ergonomics.
- If human ergonomics is not directly applicable, describe media queries as objects close to the human body as possible.
- Minor ranges (a.k.a. tweak points) are based on size differences within major ranges.
- Breakpoints should only be used if the content requires it.
- Makes it easier to add new media queries in the future, for example 'couch', 'wrist', or 'glasses'.
- Breakpoints are currently based on size differences, but don't have to be.

**Visual presentation of the breakpoints:**
```
──palm─┤──lap──┤─desk────────────
───portable────┤
       ├──lap-and-up─────────────
```

**Right:**
```css
@breakpoint-palm:       ~"only screen and (max-width:440px)";
@breakpoint-palm--s:     ~"only screen and (max-width:320px)";
@breakpoint-palm--m:     ~"only screen and (min-width:321px) and (max-width:380px)";
@breakpoint-palm--l:     ~"only screen and (min-width:381px) and (max-width:440px)";
@breakpoint-lap:        ~"only screen and (min-width:441px) and (max-width:1024px)";
@breakpoint-lap--s:      ~"only screen and (min-width:441px) and (max-width:600px)";
@breakpoint-lap--m:      ~"only screen and (min-width:601px) and (max-width:800px)";
@breakpoint-lap--l:      ~"only screen and (min-width:801px) and (max-width:1024px)";
@breakpoint-lap-and-up: ~"only screen and (min-width:441px)";
@breakpoint-portable:   ~"only screen and (max-width:1024px)";
@breakpoint-desk:       ~"only screen and (min-width:1025px)";
@breakpoint-desk--s:     ~"only screen and (min-width:1025px) and (max-width:1200px)";
@breakpoint-desk--m:     ~"only screen and (min-width:1201px) and (max-width:1600px)";
@breakpoint-desk--l:     ~"only screen and (min-width:1601px)";
```

**Wrong:**
```css
@breakpoint-small: ;
@breakpoint-smedium: ;
@breakpoint-medium: ;
@breakpoint-large: ;
```

See ['Responsive grid systems; a solution?'](http://csswizardry.com/2013/02/responsive-grid-systems-a-solution/) and ['Tweakpoints'](http://adactio.com/journal/6044/) for further reading.

## Selectors

### Do not use ID's, use classes.

**Right:**
```css
.class {
	property: value;
}
```

**Wrong:**
```css
#id {
	property: value;
}
```

### Use states as separate classes and add them to existing selectors.

- States differ from modifiers. Modifiers are timeless, states are temporarily.
- States never contain global styling. Style states always in combination with other selectors.
- States are allowed to be nested with LESS or SASS. Usage is similar to pseudo-classes and pseudo-elements.
- States always start with 'is'.
- Preferred state names are listed below.

**Right:**
```css
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
selector.is-dragging {}
selector.is-dropped {}
selector.is-selected {}
selector.is-filled {}
selector.is-empty {}
```

**Wrong:**
```css
.is-hidden {
	display: none;
}

.hidden {
	display: none;
}
```

### Never reference ‘js-‘ prefixed class names from CSS files.

- Classes starting with js- are used exclusively for javascript files.

## Nesting

### Do not nest selectors deeper than 3 levels.

- Even when using the BEM methodology, you cannot avoid cascading. Think about content where you have little control, such as user generated content. In these cases it is allowed to nest selectors, but never deeper than 3 levels.

**Right:**
```CSS
.user-content td p {
	property: value;
}
```

**Wrong:**
```css
.user-content table td p {
	property: value;
}
```

### Do not nest rules with LESS or SASS.

- Nesting rules with LESS or SASS makes it more difficult to optimize your CSS.

**Right:**
```CSS
selector {
	property: value;
}

selector selector {
	property: value;
}
```

**Wrong:**
```css
selector {
	property: value;
	selector {
		property: value;
	}
}
```

### Do nest pseudo-classes, pseudo-elements, media queries, and states with LESS or SASS.

- Makes sure style and behavior of the same selector are grouped.
- Nested pseudo-classes, pseudo-elements, media queries, and states should not be separated by one empty line.

**Right:**
```css
selector {
	property: value;
	&:hover {
		property: value;
	}
	@media @breakpoint {
		property: value;
	}
}
```

**Wrong:**
```css
selector {
	property: value;
}

selector:hover {
	property: value;
}

@media @breakpoint {
	selector {
		property: value;
	}
}
```

## Values

### Color units should be written in 'RGB' or 'RGBa'.

- LESS or SASS should convert RGB to hex color codes to reduce file size.
- RGB has the advantage over HSL, because it is better and more consistently available in other systems like brand guidelines, graphic applications, or color systems.

**Right:**
```css
rgb(50, 50, 50);
rgba(50, 50, 50, 0.2);
```

**Wrong:**
```css
#FFF;
#FFFFFF;
white;
hsl(120, 100%, 50%);
hsla(120, 100%, 50%, 1);
rgb(50,50,50);
rgba(50,50,50,0.2);
```

### Absolute sizes should be written in pixels.

- A 'CSS px' is a reference pixel intended to scale in size depending on the 'typical' distance of an observer from the display.
- Webbrowsers scale a design in pixels the same way they scale designs in em's or rem's. Zooming even triggers media queries based on pixels. Essentially, webbrowsers make the reference pixel larger or smaller.
- Makes it easier to integrate images or other media formats based on pixels in your layout.
- Makes sure you don't get nummers like '0.29310344827586204em' (this is an actual used number).
- Development time in pixels is much less.
- Fonts and line-heights look more identical between browsers.
- Sizes in ems are allowed, but only in specific cases: when it should actually be based on the current font-size. For example a max width of 30 ems to make sure lines never get to long.

See ['W3C Recommendations about lengths'](http://www.w3.org/TR/CSS21/syndata.html#length-units) for further reading.

### Z-indexes are limited to 12 levels.

- The z-index starts at -100 and is not higher than 1000. Steps are 100.
- Hundreds are used to make sure we could add a new number if needed. But will probably never happen. If more z-index are needed, rethink your code.

**Right:**
```css
selector {
	z-index: 200;
}
```

**Wrong:**
```css
selector {
	z-index: 248;
}

selector {
	z-index: 9999;
}
```

## Comments

### Use LESS/SASS commenting style '//' for comments pointless for debugging.

- Applicable for commenting code not visible in CSS, such as mixins, or variables. This is to make sure you don't have loose comments floating around in dev version: LESS/SASS style comments are also always compiled out.
- Comments outside rules should be separated by one empty line.
- Do not add extra dividers to mark a comment. If needed, change your code coloring to identify comment blocks.
- Use markdown within comments.
- Do not add line breaks for wrapping.

**Right:**
```css
// # Colors

@color-blue: rgb(0, 0, 255); // #0000ff
```

**Wrong:**
```css
/* -- colors -- */
@color-blue: rgb(0, 0, 255); /* #0000ff */
```

### Use CSS commenting style '/* */' for comments usefull for debugging.

- Applicable for commenting code visible in CSS, such as rules, selectors, properties, or values.
- Preserve CSS style comments during compiling LESS or SASS code for dev versions.
- CSS style comments should be removed during compiling LESS or SASS code for live versions.
- Comments outside rules should be separated by one empty line.
- For comments outsides rules, start and end syntaxes should be on a separate line. Even for single line comments, because of consistency and making transitions between single and multi line (adding and removing lines) easier.
- Comments inside rules can be written as 'single line comment': syntaxes and comment on the same line.
- Do not add extra dividers to mark a section. If needed, change your code coloring to identify comment blocks.
- Do not indent comments or start every line with special characters such as *s.
- Use markdown within comments.
- Do not add line breaks for wrapping.

**Right:**
```css
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
```css
/*===
* -- Heading 1 --
* Paragraph
===*/
```

### Write actions inside the CSS as: "Action (date/situation)".

- Always include a date or situation when it should be changed.
- Always start actions with a verb.

**Right:**
`````css
selector {
	property: value; /* Remove hack (after we stop Internet Explorer 9 support) */
	property: red; /* Change value to blue (4 april 2015) */
}
```

## Further reading

Guidelines are inspired by [sass-guideline.es](http://sass-guidelin.es/#syntax--formatting), [cssguidelin.es](http://cssguidelin.es), [Zen CSS Properties](https://code.google.com/p/zen-coding/wiki/ZenCSSPropertiesEn), and [Medium's Less Coding Guidelines](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06).
