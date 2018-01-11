# Chapter 2: Text
&lt;hr /&gt; this is called an empty element
<hr />
### semantic markups
&lt;strong&gt; text are bold
&lt;em&gt; text are in italic
&lt;blockquote&gt; markup quotations: text will be indented
&lt;q&gt; is used for short quotes(many people avoid using this)
&lt;abbr&gt;(HTML5) a title attribute on the opening tag is used to specify the full term.
&lt;acronym&gt;(HTML4)
&lt;cite&gt;(not for person’s name in HTML5, but in HTML4 it’s OK)
&lt;dfn&gt;(Safari and Chrome don’t change text appearance)
&lt;address&gt; contains details for the author of the page
##### changes to content
&lt;ins&gt; the content has been inserted
&lt;del&gt; the content has been deleted
&lt;s&gt; indicates something that is no longer accurate or relevant(but should not be deleted)
# Chapter 3: Lists
* Numbered lists
* Bullet lists
* Definition lists
<hr />
### ordered lists
&lt;ol&gt; denotes ordered lists
&lt;li&gt; stands for list item
&lt;ul&gt; denotes unordered lists
&lt;dl&gt; denotes definition lists
&lt;dt&gt; is used to define terminology
&lt;dd&gt; is used to contain the definition
# Chapter 4: Links
<strong>SYNTAX:</strong>&lt;a href=“http://www.imdb.com”&gt;IMDB&lt;/a&gt;
<hr />
URL stands for Uniform Resource Locator
### email links
syntax: &lt;a href=“mailto:abc@example.org”&gt;Email abc&lt;/a&gt;
### target
open the link in a new window
syntax: &lt;a href=“https://google.com target=“_blank””&gt;Google it!&lt;/a&gt;
##### link to a specific part of another page
syntax: &lt;a href=“https://google.com/#top”&gt;Google it!&lt;/a&gt;
# Chapter 5: Images
* choose the right format
* optimize images for websites
<hr />
### stock photos
* www.istockphoto.com
* www.gettyimages.com
* www.veer.com
* www.sxc.hu
* www.fotolia.com
### adding images
<strong>&lt;img&gt;</strong>
* src
* alt
* title
* height
* width
* align: is removed from HTML5 and new websites should use CSS to control the alignment of images
##### Align images horizontally
** left: allow text to flow around its right-hand side
** right: allow text to flow around its left-hand side
##### Align images vertically
** top: This aligns the first line of the surrounding text with the top of the image
** middle: Same with the middle of the image
** bottom: Same with the bottom of the image
##### Three rules for creating images
Adobe Elements
<hr />
1. save images in the right format
** JPEG: picture with many different colors
** GIF or PNG: picture has an area that is filled with exactly the same color, aka flat color.
** JPEG, GIF and PNG are <strong>bitmap</strong> images
2. save images at the right size
** tools: Photoshop or GIMP
** cropping images: portrait and landscape
3. use the correct resolution:
** images created for the web should be saved at a resolution of 72 ppi(72 pixels per inch).
** images in print are usually at a resolution of 300 dpi(dots per inch)
** JPEG at 300 dpi = 1526kb
** JPEG at 72 ppi = 368kb
** Vector images are resolution-independent, commonly created by Adobe Illustrator 
** Scalable Vector Graphics(SVG)
### HTML5: figure and figure caption
&lt;figure&gt; contain images and their caption
&lt;figcaption&gt;
# Chapter 6: Tables
<hr />
### basic table structure
* &lt;table&gt;
* &lt;tr&gt;
* &lt;td&gt;
* &lt;th&gt;
##### spanning columns
* colspan can be used on a &lt;th&gt; or &lt;td&gt; element and indicates how many columns that cell should run across
* rowspan is the same
### long tables
* &lt;thead&gt;
* &lt;tbody&gt;
* &lt;tfoot&gt;
##### old code:
* width
* spacing
* border
* background
# Chapter 7: Forms
information from a form is sent in name/value pairs, so it is important to always use name and value attributes in elements.
* type=“text”
** the value is user input
* type=“checkbox” or type=“radio”
** the value needs to be specified
<hr />
### form controls:
* Adding text
** Text input(single-line)
** Password input
** Text area(multi-line)
* Making choices
** Radio buttons
** Checkboxes
** Drop-down boxes
* Submitting forms
** Submit buttons
** Image buttons
* Uploading files
** File upload
##### form structure
* &lt;form&gt;
** action: every form requires an action attribute. Its value is the URL for the page on the server that receive the information in the form when it is submitted.
** method: get or post
<strong>get</strong> method is ideal for:
short forms(such as search boxes)
when you are just retrieving data from the web server(not sending information that should be added to or deleted from a database)
<strong>post</strong> method is used if your form:
allows users to upload a file
is very long
contains sensitive data(e.g. passwords)
adds information to, or deletes information from, a database
<strong>By default the form will use the get method</strong>
** id
##### text input
* &lt;input&gt;
** type=“text”
** name
** size display characters(should not be used on new forms)
** maxlength limits length of user’s input characters
##### password input
For fully security, the server needs to be set up to communicate with user’s browsers using Secure Sockets Layer(SSL).
* &lt;input&gt;
** type=“password”
** name
** size
** maxlength
##### text area
* &lt;textarea&gt;
##### radio button
* &lt;input&gt;
** type=“radio”
** name
** value
** checked
##### checkbox
* &lt;input&gt;
** type=“checkbox”
** name
** value
** checked
##### drop down list box
* &lt;select&gt;
** name
* &lt;option&gt;
** value
** selected
##### multiple select box
* &lt;select&gt;
** size
** multiple
##### file input box
* &lt;input&gt;
** type=“file”
##### submit button
* &lt;input&gt;
** type=“submit”
** name
** value
##### image button
* &lt;input&gt;
** type=“image”
##### button and hidden controls
* &lt;button&gt;
* &lt;input&gt;
** type=“hidden”
##### labelling form controls
best places to place labels on form controls:
* Above or to the left:
** text inputs
** text areas
** select boxes
** file uploads
* To the right
** individual checkboxes
** individual radio buttons
* &lt;label&gt; can be used in two ways:
Warp around both the text description and the form input.
Be kept separate from the form control and use the for attribute to indicate which form control it is a label for.
** for
##### grouping form elements
* &lt;fieldset&gt; group related form controls inside the &lt;fieldset&gt; element
* &lt;legend&gt;
##### HTML5: Form validation
##### HTML5: Date input
* &lt;input&gt;
** type=“date”
##### HTML5: Email and URL input
* &lt;input&gt;
** type=“email”
** type=“url”
##### HTML5: Search input
* &lt;input&gt;
** type=“search”
** placeholder
# Chapter 8: Extra markup
* The different versions of HTML and how to indicate which version you are using
* How to add comments to your code
* Global attributes, which are attributes that can be used on any element, including the class and id attributes
* Elements that are used to group together parts of the page where no other element is suitable
* How to embed a page within a page using frames
* How to add information about the web page using the &lt;meta&gt; element
* Adding characters such as angled brackets and copyright symbols
<hr />
XHTML can be used with other data formats such as Scalable Vector Graphics(SVG) - a graphical language written in XML, MathML(used to mark up mathematical formulae), and CML(used to mark up chemical formulae)
### comments in HTML
* &lt;!-- --&gt;
### id attribute
* its value should start with a letter or an underscore
* no two elements on the same page have the same value for their id attributes
* the id attribute is known as a <strong>global attribute</strong>
### class attribute
### block elements
* examples of block elements are:
** &lt;h1&gt;
** &lt;p&gt;
** &lt;ul&gt;
** &lt;li&gt;
### inline elements
* examples of inline elements are:
** &lt;a&gt;
** &lt;b&gt;
** &lt;em&gt;
** &lt;img&gt;
### grouping text and elements in a bock
* &lt;div&gt;
### grouping text and elements inline
* &lt;span&gt;: acts like an inline equivalent of the &lt;div&gt; element
** contain a section of text where there is no other suitable element to differentiate it from its surrounding text
** contain a number of inline elements
### iframes
* &lt;iframe&gt; stands for inline frame
** src
** height
** width
** scrolling(not supported in HTML5) three values:
yes(to show scrollbars)
no(to hide scrollbars)
auto(to show them if only needed)
** frame border(not supported in HTML5) two values:
1: indicates that a border should be shown
0: indicates that no border should be shown
** seamless(new in HTML5, usually does not need a value)
### information about pages
* &lt;meta&gt; it is an empty element. Its attributes:
name
content
http-equiv(usually used with content):
author
pragma: prevent the browser from caching the page
expires
** description maximum 155 characters
** keywords
** robots
noindex can be used if this page should not be added.
nofollow can be used if search engines should add this page in their results but not any pages that it links to.
### escape characters
<em>HTML And CSS</em> page. 194
# Chapter 9: Flash, Video and Audio
* Prototype
* script.aculo.us
* jQuery
<hr />
### Understanding video formats and players
### HTML5: adding video to your pages
* &lt;video&gt; two formats: H264(IE and Safari only) and WebM(Firefox and Opera)
** src
** poster specify an image to show while the video is downloading or until the user tells the video to play
** width, height
** controls when used, this attribute indicates that the browser should supply its own controls for playback
** autoplay
** loop
** preload: 
none; the browser should not load the video until the user presses play
auto; download the video when the page loads
metadata the browser should just collect information such as the size, first frame, track list, and duration
##### multiple video sources
* &lt;source&gt; when used, it replaces src attribute
** src
** type
** codecs
### HTML5: adding HTML5 audio to your pages
* &lt;audio&gt; MP3:Safari 5+, Chrome 6+, IE9
Ogg Vorbis:Firefox 3.6, Chrome 6, Opera 1.5, IE9
** src
** controls
** autoplay
** preload
** loop
##### multiple audio sources
* &lt;source&gt;
** src
** type
