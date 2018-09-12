+++
title         = "Semantic Css Establishing a Visual Outline"
slug          = "semantic-css-visual-outline"
date          = 2010-08-16T20:08:33-08:00
tags          = ["css","coding","OOCSS","code-sample","tips"]
image         = "images/posts/features/bar-cafe-chairs.jpg"
+++

Let's move beyond simply linting our code, and delve into creating a visual outline of our CSS.<!--more-->  Anyone who writes code, anything from simple UI shifts through complex exotic joins, inherits a responsibility to write reasonable code.  Good code solves a problem or answers a question; well-written code lives on, open and accessible to future developers.

Before we begin, let me say that the delivered stylesheet should be minified and free of excess spaces, tabs, semicolons and lengths of 0px instead of “0″, using tools like the [YUI compressor](http://developer.yahoo.com/yui/compressor/) or the [CSS Drive Online CSS Compression Utility](http://www.cssdrive.com/index.php/main/csscompressor/).

I have read and been part of many discussions about the [format of a CSS document](http://stackoverflow.com/questions/72911/whats-the-best-way-to-organize-css-rules).  I happen to prefer to keep my declarations line-separated, alphabetized within contextually appropriate groups.  I also like to keep the entire ruleset on one line, unless the ruleset is too long (I know, that’s subjective, but it is my blog).

Recently, though, I’ve had to work on some interesting and complex designs.  As the DOM depth grew, I found myself getting lost in my own HTML.  The solution, for me, was a simple one, both easy to implement and easy to back out of (especially when using the tools listed above).

My CSS formatting rules were category based:

1. [Reset CSS](http://developer.yahoo.com/yui/reset/), if necessary/desired (note, I don’t always use a full reset library)
1. HTML declarations
1. ID rules
1. class rules

Beyond that, I found my formatting rules lacking.  Last-in mental flow within category left me lost and confused.  To buttress my existing formatting rules, I adopted a visual outline in my stylesheet that matched the indent rules of my HTML.  For example, the following HTML:

```html
<div id="nav">
   <ul>
      <li><a href="#" title="some local navigation link">Navigation Item 1</a></li>
      <li><a href="#" title="some local navigation link">Navigation Item 2</a></li>
      <li><a href="#" title="some local navigation link">Navigation Item 3</a>
         <ul>
            <li><a href="#" title="some local navigation link">Sub-Navigation Item 1</a></li>
            <li><a href="#" title="some local navigation link">Sub-Navigation Item 2</a></li>
            <li><a href="#" title="some local navigation link">Sub-Navigation Item 3</a>
         </ul>
      </li>
      <li><a href="#" title="some local navigation link">Navigation Item 4</a></li>
      <li><a href="#" title="some local navigation link">Navigation Item 5</a></li>
      <li><a href="#" title="some local navigation link">Navigation Item 6</a></li>
   </ul>
</div>
```

Would be supported by this CSS:

```css
#nav {border:1px solid #ccc;}
	#nav ul {list-style-type:none;margin:0;padding:0;}
		#nav ul li {display:inline;list-style-type:none;margin:0;padding:0;}
		#nav ul li:hover {position:relative;}
			#nav ul li a {display:inline-block;color:#000;padding:4px 12px;text-decoration:none;}
			#nav ul li a:hover {text-decoration:underline;}
			#nav ul li ul {background-color:#666666;border:1px solid #333333;display:none;position:absolute;left:5px;top:100%;width:200px;}
			#nav ul li ul li {display:block;width:200px;}
			#nav ul li:hover ul {display:block;}
			#nav ul li ul li a {color:#fff;}
```

There’s nothing new or magical here. In fact, this isn’t even a very pretty implementation of a flyout menu. The point here, though, is that my CSS has been formatted to mimic the indentation and format of the HTML.  Clearly, “li” is a child of “ul” is a child of “#nav” and so on.  As your style sheet grows, you’ll be glad you took the extra steps to maintain a mock of your HTML formatting.

Don’t forget – the final step before publishing to production should, at the very least, be minification!

Bonus content: this concept can be rewritten to use Object Oriented CSS ([stubbornella@github](http://github.com/stubbornella/oocss/wiki) and [object-oriented CSS](http://oocss.org/)).  Check back later for that post!

*Photo by Igor Starkov from Pexels*
