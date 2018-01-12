# Chapter 10: Introducing CSS

<em>Thinking inside the box</em>
Testing pages on different browsers: BrowserShots.org
CSS bugs: PositionIsEverything.net
<hr />

* A CSS rule contains two parts:
** a selector
** a declaration

### CSS selectors

* universal selector: apply to all elements in the document
** \* {}
* type selector: match element names
** h1, h2, h3 {}
* class selector: match an element whose class attribute has a value that matches the one specified after the period symbol
** .note {}
** p.note {}
* id selector: match an element whose id attribute has a value that matches the one specified after the hash symbol
** \#introduction {}
* child selector: match an element that is a direct child of another
** li>a {}: target any &lt;a&gt; elements that are children of an &lt;li&gt; element(but not other &lt;a&gt; elements in the page)
* descendant selector: match an element that is a descendent of another specified element(not just a direct child of that element)
** p a {}: target any &lt;a&gt; elements that sit inside a &lt;p&gt; element, even if there are other elements nested between them

##### these two are less commonly used

* adjacent sibling selector: match an element that is the next sibling of another
** h1+p {}
* general sibling selector: match an element that is a sibling of another, although it does not have to be the directly preceding element
** h1\~p {}

### How CSS rules cascade

* last rule: if two selectors are identical, the latter of the two will take precedence
* specificity: if one selector is more specific than the others, the more specific rule will take precedence over more general ones
** h1 is more specific than \*
** p b is more specific than p
** p\#intro is more specific than p
* important: you can add <em>!important</em> after any property value to indicate that it should be considered more important than other rules that apply to the same element
### Inheritance
* use value <em>inherit</em> to inherit from their parent element
# Chapter 11: Color
color picking tool: colorschemedesigner.com
check contrast: www.snook.ca/technical/colour_contrast/colour.html
<hr />
### Foreground color: color
* RGB values
** e.g. rgb(100,100,90)
* HEX codes
** e.g. \#ee3e80
* color names: there are 147 predefined color names
** e.g. DarkCyan
* HLSA(CSS3)
* Hue
* Saturation: refer to the amount of gray in a color.
* Brightness: refer to how much black is in a color
### Background color: background-color
### Contrast
### CSS3: opacity
* opacity(apply to the element and any of its child elements): 0.0 - 1.0
* rgba(only apply to the element, not child elements): fourth value <em>alpha</em> specifies the opacity
* fallback solution
** specify a color as a hex code, color name or RGB value, followed by the rule that specifies an RGBA value.
### CSS3: HSL(hue, saturation, lightness) colors
* Hue: is often represented as a color circle(0 - 360 degrees)
* Saturation: is represented as a percentage
* Lightness(is sometimes referred to as <em>luminosity</em>): is represented as percentage(0% is black, 100% is white)
### CSS3: HSL and HSLA
* hsl
* hsla: fourth value represents transparency, denoted by a, known as <em>alpha</em>
# Chapter 12: Text
* open source fonts:
** www.fontsquirrel.com
** www.fontex.org
** www.openfontlibrary.org
* commercial fonts
** www.typekit.com
** www.kernest.com
** www.fontspring.com
* convert format
** www.fontsquirrel.com/fontface/generator
<hr />
* The properties that allow you to control the appearance of text can be split into two groups:
** those that directly affect the font and its appearance(including the typeface, whether it is regular, bold or italic, and the size of the text)
** those that would have the same effect on text no matter what font you were using(including the color of text and the spacing between words and letters)
### Typeface terminology
* serif(In print, serif fonts were traditionally used for long passages of text because they were considered easier to read.)
** serif fonts have extra details on the ends of the main strokes of the letters. These details are known as serifs
* sans-serif(Screens have a lower resolution than print. So, if the text is small, sans-serif fonts can be clearer to read.)
** sans-serif fonts have straight ends to letters, and therefore have a much cleaner design
* monospace(Monospace fonts are commonly used for code because they align nicely, making the text easier to follow.)
** every letter in a monospace(or fixed-width) font is the same width. (Non-monospace fonts have different widths.)
* weight
** light
** medium
** bold
** black
* style
** normal
** italic have a cursive aspect to some of the lettering
** oblique take the normal style and put it on an angle
* stretch
** condensed(or narrow) thinner and closer
** regular 
** extended thicker and further apart
### Choose a typeface for your website
* serif
** Georgia
** Times
** Times New Roman
* sans-serif
** Arial
** Verdana
** Helvetica
* monospace: every letter in a monospace typeface is the same width.
** Courier
** Courier New
* cursive
** Comic Sans MS
** Monotype Corsiva
* fantasy: are usually decorative fonts and are often used for titles. Not designed for long bodies of text
** Impact
** Haettenschweiler
* e.g. font-family: Georgia, Times, serif;
##### specifying typefaces
* font-family
##### size of type 
* font-size
a pixel roughly equates to a point because a point corresponds to 1/72 of an inch
** pixels: the default size of text in browsers is 16px
** percentages
** ems an em is equivalent to the width of a letter m
##### units of type size
* pixels(twelve pixel scale)
** h1 24px
** h2 18px
** h3 14px
** body 12px
* percentages
** h1 200%
** h2 150%
** h3 117%
** body 75%
* ems
** h1 1.5em
** h2 1.3em
** h3 1.17em
** body 100%
** p 0.75em
* pixels(sixteen pixel scale)
** h1 32px
** h2 24px
** h3 18px
** body 16px
* percentages
** h1 200%
** h2 150%
** h3 133%
** body 100%
* ems
** h1 2em
** h2 1.5em
** h3 1.125em
** body 100%
** p 1em
### More font choice
* @font-face: allows you to use a font, even if it is not installed on the computer of the person browsing, by allowing you to specify a path to a copy of the font
** e.g.
@font-face {
font-family: ‘’;
src: url(‘’);
}
** font-family
** src
** format
##### the various font formats should appear in your code in this order:
* eot
* woff
* ttf/otf
* svg
### Bold
* font-weight
** normal
** bold
### Italic
* font-style
** normal
** italic
** oblique
### Uppercase and lowercase
* text-transform
** uppercase
** lowercase
** capitalize
### Underline and strike
* text-decoration
** none
** underline
** overline
** line-through
** blink
### Leading
* line-height
** a good starter setting is around 1.4 to 1.5em
### Letter and word spacing
* letter-spacing: <strong>Kerning</strong> is the term typographers use for the space between each letter
* word-spacing: the default gap between words is set by the typeface(often around 0.25em)
### Alignment
* text-align
** left
** right
** center
** justify: this indicates that every line in a paragraph, except the last line, should be set to take up the full width of the containing box
### Vertical alignment
* vertical-align: It is not intended to allow you to vertically align text in the middle of block level elements such as &lt;p&gt; and &lt;div&gt;, although it does have this effect when used with table cells(the &lt;td&gt; and &lt;th&gt; elements). It is more commonly used with inline element such as &lt;img&gt;, &lt;em&gt;, or &lt;strong&gt; elements.
** baseline
** sub
** super
** top
** text-top
** middle
** bottom
** text-bottom
### Indenting text
* text-indent: the amount you want the line indented by can be specified usually in pixels or ems
### CSS3: Drop shadow
* text-shadow: takes three lengths and one color for the drop shadow
** first length: indicates how far to the left or right the shadow should fall
** second length: indicates the distance to the top or bottom that the shadow should fall
** third length: optional and specifies the amount of blur that should be applied to the drop shadow
** color
### First letter or line
* :first-letter
* :first-line
##### Above are known as <strong>pseudo-elements</strong>
##### pseudo-class acts like an extra value for a class attribute(when they are used, they should appear in this order: :link, :visited, :hover, :focus, :active)
### Styling links
* :link for links that have not yet been visited
* :visited allows you to set styles for links that have been clicked on
### Responding to users
* :hover
* :active
* :focus
### Attribute selectors
* existence
** [] matches a specific attribute
** p[class]
* equality
** [=] matches a specific attribute with a specific value
** p[class=“dog”]
* space
** [~=] matches a specific attribute whose value appears in a space-separated list of words
** p[class~=“dog”]
* prefix
** [\^=] matches a specific attribute whose value begins with a specific string
** p[attr\^”d”]
* substring
** [\*=] matches a specific attribute whose value contains a specific substring
** p[attr\*”do”]
* suffix
** [$=] matches a specific attribute whose value ends with a specific string
** p[attr$”g”]
# Chapter 13: Boxes
<hr />
### Box dimensions
* width
* height
### Limiting width and height
* min-width
* max-width
* min-height
* max-height
### Overflowing content
* overflow: tells the browser what to do if the content contained within a box is larger than the box itself
** hidden: hides any extra content that does not fit in the box
** scroll: add a scrollbar to the box so that users can scroll to see the missing content
### Border, margin and padding
### Border width
* border-width
** thin
** medium
** thick
** border-top-width
** border-right-width
** border-bottom-width
** border-left-width
** border-width: para1, para2, para3, para4
### Border style
* border-style
** solid
** dotted
** dashed
** double
** groove
** ridge
** inset
** outset
** hidden/none
** border-top-style
** border-right-style
** border-bottom-style
** border-left-style
### Border color
* border-color
** border-top-color
** border-right-color
** border-bottom-color
** border-left-color
** border-color: para1, para2, para3, para4
### Shorthand
* border: allows you to specify the width, style and color of a border in one property(and the value should be coded in that specific order)
### Padding
* padding: is not inherited by child elements
** padding-top
** padding-right
** padding-bottom
** padding-left
** padding: para1, para2, para3, para4
** padding: para1, para2
### Margin
* margin: is not inherited by child elements
** margin-top
** margin-right
** margin-bottom
** margin-left
** margin: para1, para2, para3, para4
** margin: para1, para2
### Centering content
* e.g. margin: 10px auto 10px auto; this centers a box on the page
### Change inline/block
* display: allows you to turn an inline element into a block-level element or vice versa, and can also be used to hide an element from the page
** inline
** block
** inline-block: this causes a block-level element to flow like an inline element, while retaining other features of a block-level element
** none
### Hiding boxes
* visibility: allows you to hide boxes from users but it leaves a space where the element would have been
** hidden
** visible
### CSS3: Border images
* border-image: -moz-border-image; -webkit-border-image;(helps earlier versions of Chrome, Firefox, and Safari display the effect)
** url of the image
** where to slice the image
** what to do with the straight edges:stretch, repeat, round
### CSS3: Box shadows
* box-shadow: allows you to add a drop shadow around a box. It must use at least the first of these two values; -moz-border-shadow; -webkit-border-shadow;
** horizontal offset: negative values position the shadow to the left of the box
** vertical offset: negative values position the shadow to the top of the box
** blur distance: if omitted, the shadow is a solid line like a border
** spread of shadow: if used, a positive value will cause the shadow to expand in all directions, and a negative value will make it contract
** inset: can also be used before these values to create an inner-shadow
### CSS3: Rounded corners
* border-radius: -moz-border-radius; -webkit-border-radius;
** border-top-right-radius
** border-bottom-right-radius
** border-bottom-left-radius
** border-top-left-radius
** border-radius: para1 para2 para3 para4
### CSS3: Elliptical shapes
* border-radius
** border-radius: para1 para2 para3 para4(horizontal) / para1 para2 para3 para4(vertical)
# Chapter 14: Lists, tables and forms
* To get select boxes, radio buttons, and checkboxes to look consistent across all browsers: http://formalize.me
<hr />
### Bullet point styles
* list-style-type
* unordered lists
** none
** disc
** circle
** square
* ordered lists
** decimal
** decimal-leading-zero
** lower-alpha
** upper-alpha
** lower-roman
** upper-roman
### Images for bullets
* list-style-image
### Positioning the marker
* list-style-position: indicates whether the marker should appear on the inside or the outside of the box containing the main points
** outside(default)
** inside
### List shorthand
* list-style: acts as a shorthand for list styles, allowing you to express the markers’ style, image and position properties in any order.
### Table properties
* properties
** width
** padding
** text-transform
** letter-spacing, font-size
** border-top, border-bottom
** text-align
** background-color
** :hover
* tips
** give cells padding
** distinguish headings
** shade alternate rows
** align numerals
### Border on empty cells
* empty-cells
** show
** hide
** inherit
### Gaps between cells
* border-spacing: allows you to control the distance between adjacent cells
* border-collapse: border-spacing will be ignored and cells pushed together, and empty-cells properties will be ignored.
** collapse
** separate
### Styling text inputs
* commonly used properties
** font-size
** color
** background-color
** border
** border-radius
** :focus
** :hover
** background-image
### Styling submit buttons
* commonly used properties
** color
** text-shadow
** border-bottom
** background-color
** :hover
### Styling fieldsets and legends
* commonly used properties
** width
** color
** background-color
** border
** border-radius
** padding
### Aligning form controls: problem
### Cursor styles
* cursor
** auto
** crosshair
** default
** pointer
** move
** text
** wait
** help
** url(“cursor.gif”)
### Web developer toolbar
# Chapter 15: Layout
* 960GS: www.960.gs
* CSS frameworks:
** blueprints.org
** lessframework.com
** developer.yahoo.com/yui/grids/
<hr />
### Key concepts in positioning elements
##### Building blocks
* block-level elements: start on a new line
** &lt;h1&gt;
** &lt;p&gt;
** &lt;ul&gt;
** &lt;li&gt;
* inline elements: flow in between surrounding text
** &lt;img&gt;
** &lt;b&gt;
** &lt;i&gt;
##### Containing elements
* If one block-level element sits inside another block-level element then the outer box is known as the containing or parent element
##### Controlling the position of elements
* positioning schemes
** normal flow
** relative positioning
** absolute positioning: absolutely positioned elements move as users scroll up and down the page
* box offset
** fixed positioning: this is a form of absolute positioning that positions the element in relation to the browser window. elements with fixed positioning do not affect the position of surrounding elements and they do not move when the user scrolls up or down the page.
** floating elements: floating an element allows you to take that element out of normal flow and position it to the far left or right of a containing box. the floated element becomes a block-level element around which other content can flow.
* z-index: allows you to control which box appears on top
### Normal flow
* position:static(By default, you do not need to specify this)
### Relative positioning
* position:relative: moves an element in relation to where it would have been in normal flow
* offset properties
** top
** right
** bottom
** left
### Absolute positioning
* position: absolute
### Fixed positioning
* position:fixed this is a type of absolute positioning that requires the position property to have a value of fixed
### Overlapping elements
* If boxes do overlap, the elements that appear later in the HTML code sit on top of those that are earlier in the page.
* z-index: sometimes is referred to as the stacking context
### Floating elements
* float
### Using float to place elements side-by-side
### Clearing floats
* clear: allows you to say that no element(within the same containing element) should touch the left or right-hand sides of a box.
** left: the left-hand side of the box should not touch any other elements appearing in the same containing element.
** right: the right-hand side of the box will not touch elements appearing in the same containing element.
** both: neither the left nor right-hand sides of the box will touch elements appearing in the same containing elements.
** none: elements can touch either side
### Parents of floated elements: problem
* If a containing element <em>only</em> contains floated elements, some browsers will treat it as if it is zero pixels tall
### Parents of floated elements: solution
* add two CSS rules to the containing element
** overflow:auto
** width: 100%
### Creating multi-column layouts with floats
* width: this sets the width of the columns
* float: this positions the columns next to each other
* margin: creates a gap between the columns
### Page sizes
* The area of the page that users would see without scrolling was often referred as being “above the fold”
* It is assumed that users would see the top 570 - 600 pixels of a page without scrolling down 
### Fixed width layout
### Liquid layout
### Layout grids
* 960 pixel grid
** 12 column grid: each column has a margin set to 10 pixels, which creates a gap of 20 pixels between each column and 10 pixels to the left and right-hand sides of the page.
![layout grids](images/960grid.jpg)
![layout grids_2](images/960grid_2.jpg)
### CSS frameworks
##### Introducing the 960.GS CSS framework
##### Multiple style sheets
* @import
** Inside a CSS file, use @import url(“”) to import other style sheets
** If a stylesheet uses the @import rule, it should appear before other rules
# Chapter 16: Images
<hr />
### Controlling sizes of images in CSS
* small portrait: 220 * 360
* small landscape: 330 * 210
* feature photo: 620 * 400
### Aligning images using CSS
* the float property is added to the class that was created to represent the size of the image
* new classes are created with names such as align-left or align-right to align the images to the left or right of the page
### Centering images using CSS
* make it into a block-level element
* on the containing element
** use text-align property with a value of center
* on the image itself
** use the margin property and set the values of the left and right margins to auto
* refer to the &lt;figure&gt; in HTML5
### Background images
* background-image
** syntax:background-image: url(“”)
** can be specified to an element like body or p
### Repeating images
* background-repeat
** repeat
** repeat-x
** repeat-y
** no-repeat
* background-attachment
** fixed: stays in the same position on the page
** scroll: moves up and down as the user scrolls up and down the page
### Background position
* background-position usually have two values: horizontal position and vertical position
** left top
** left center
** left bottom
** center top
** center center
** center bottom
** right top
** right center
** right bottom
** If only one value is specified, the second will default to center
** You can use a pair of pixels or percentages. These represent the distance from the top left corner of the browser window. The top left corner is equal to 0% 0%.
### Shorthand
* background
** background-color
** background-image
** background-repeat
** background-attachment
** background-position
* CSS3 supports the use of multiple background images by repeating the background shorthand
** e.g.
div {
background:
url(example-1.jpg)
top left no-repeat,
url(example-2.jpg)
bottom left no-repeat,
url(example-3.jpg)
center top repeat-x;
}
### Image rollovers and sprites
* advantage: the web browser only needs to request one image rather than many images, which can make the web page load faster
### CSS3: gradients
* background-image
# Chapter 17: HTML5 Layout
<hr />
### Headers and footers
* &lt;header&gt;
* &lt;footer&gt;
### Navigation
* &lt;nav&gt;
### Articles
* &lt;article&gt;
### Asides
* &lt;aside&gt;
### Sections
* &lt;section&gt;
### Heading groups
* &lt;hgroup&gt;
### Figures
* &lt;figure&gt;
* &lt;figcaption&gt;
* usages:
** images
** videos
** graphs
** diagrams
** code samples
** text that supports the main body of an article
### Sectioning elements
* &lt;div&gt;
** used as a wrapper
### Linking around block-level elements
* place an &lt;a&gt; element around a block level element that contains child elements
### Helping older browsers understand
* to help older browsers, you should include the rule of that new elements should be rendered as block-level elements
# Chapter 18: Process and design
* online wireframe tools:
** http://gomockingbird.com
** http://lovelycharts.com
<hr />
### Who is the site for
* target audience: individuals
* target audience: companies
### Site maps
### Wireframes
### Visual hierarchy
* size
* color
* style
### Grouping and similarity
* grouping
** proximity
** closure
** continuance
** white space
** color
** borders
* similarity
**  consistency
**  headings
### Designing navigation
* concise: no more than eight links
* clear: choose single descriptive words for each link
* selective: primary navigation should only reflect the sections or content of the site
* context: lets the user know where they are in the website at that moment
* interactive
* consistent
# Chapter 19: Practical information
* search engine optimization
* using analytics to understand how people are using your site after it has launched
* putting your site on the web
<hr />




