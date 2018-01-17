# What is a DOM?
**DOM** stands for Document Object Model, which is known as an Application Programming Interface(**API**). <br />
The DOM is a way of conceptualizing the contents of a document. <br />
That's to say, almost everything in a document can be represented via DOM. <br />

# Frequently used DOM methods
* getElementById
* getElementsByTagName
* getElementsByClassName()
* getAttribute
* setAttribute

### DOM is a collection of nodes
DOM has a tree-like structure, with every node linked to each other. <br />
Nodes can be divided into three categories, and each of them has corresponding nodeType and nodeValue:

  Node(kind)  | nodeType(value) | nodeValue(actual content)
--------------|-----------------|--------------------------
 element node |        1        |
attribute node|        2        |
  text node   |        3        |  

*All attribute nodes are contained by elements*

### Getting and Setting attributes
* getAttribute can only be used on an element node
* setAttribute

# Node properties
Some node properties can be used to retrieve information:<br />
* firstChild
  * node.firstChild = node.childNodes[0]
* lastChild
  * node.lastChild = node.childNodes[node.childNodes.length - 1]

# Best Practices
* Graceful degradation
  * window.open(url,name,feaures)
  * javascript:pseudo-protocol (bad way of referencing JavaScript frm within your markup)
  * Inline event handlers (bad)
* Backward Compatibility
  * Object Detection
  * Browser sniffing
* Performace Considerations
  * Minimizing DOM access and markup
  * Assembling and placing scripts
  * Minification: scriptName.min.js
  * Some tools for minification: Douglas Crokford's JSMin; Yahoo!'s YUI Compressor; Google's Closure Compiler

# Creating Markup on the Fly
* Some old-school methods:
  * document.write
  * innerHTML
* DOM methods:
  * createElement
  * appendChild
  * createTextNode
  * insertBefore
  * insertAfter(create one yourself)
* Ajax
* request.readyState:
  * 0 for uninitialized
  * 1 for loading
  * 2 for loaded
  * 3 for interactive
  * 4 for complete

# CSS - DOM
We can view a web page as three sheets, which are implemented by HTML, CSS and JavaScript.
* Structure -> HTML
* Persentation -> CSS
* Behavior -> JavaScript <br />
**DOM can only retrieve inline stylistic properties**

# Animation
* Useful methods:
  * setTimeout("function",interval)
  * clearTimeout(variable)
