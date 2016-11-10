---
layout: post
title:  "Jason vs. Anchor Tags – Getting Results with the preventDefault method"
date:   2010-05-13 18:22:49 -0800
categories: blog
tags: AJAX JAVASCRIPT JQUERY MODAL PREVENT-DEFAULT
author: jason_corns
---

In another post, [Jason vs. Page Weight – Accessibility Series, pt. 2]({% post_url 2010-04-26-vs-page-weight-accessibility-part-2 %}), I talked about a solving page weight problem I had with a client.<!--more-->  In the end, the client decided to put a lot of data on a single page.  In my opinion, this would have been a great time to use AJAX to pull in the data rather than just pushing it all to the page at load.  The problem with AJAX is that it relies on JavaScript (reliance may, in fact, be too weak a term, honestly) and so would not have been ideal for search engines and would not have provided accessible page content.  We could fix that by linking to pages with additional content, but we really want to use AJAX to show the data on a single page for all visitors with JavaScript enabled.  So, we just do both.

By leveraging the [preventDefault method](http://api.jquery.com/category/events/event-object/#post-201) available in the jQuery library (this is actually a JavaScript call, and exists outside of jQuery.  See the [W3C write up](http://www.w3.org/TR/2001/WD-DOM-Level-3-Events-20010823/events.html#Events-Event-preventDefault).) we can provide a page that is static, accessible HTML content by default, but picks up some dynamic interest at page load.  Note: some of the following examples presuppose a single domain; using AJAX to load content from other pages can easily get into [cross-site scripting](http://en.wikipedia.org/wiki/Cross-site_scripting) issues.  If you control the server being accessed, you can manage the cross-origin request permissions by setting the [HTTP access control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

Take the following HTML code for example:

{% highlight html %}
<div class="targetLinks">
	<ul>
		<li><a href="somepage.html">Some Page</a></li>
		<li><a href="anotherpage.html">Another Page</a></li>
	</ul>
</div>
{% endhighlight %}

No magic here.  Two links to two different pages (for simplicity, these are two pages within the same domain).

## The Task

Our task is to display content from a different page in a modal dialog.  Just to make it more fun, we’ll pull a specific part of the target page in our modal (denoted in each page by a div with ID of “target”).  To add that last little bit of shine to the action, we’ll prevent the URL from changing (more appropriately, we’ll stop any calls to change the _window location_).

We’ll need three things for this tutorial:

* [jQuery](http://jquery.com)
* [jQueryUI](https://jqueryui.com/) - _core_ plus _dialog_ will do (for a simple Modal)

Let’s take a look at the code required to attain our goal, then we’ll break it down piece by piece.

{% highlight javascript %}
$(document).ready(function() {
	$('.targetLinks').find('ul li a').on('click', function(eventMarker) {
		var $dialog = $('<div></div>')
			.load($(this).attr('href') + " #target")
			.dialog({
				autoOpen: false,
				modal:true,
				title: $(this).attr('title'),
				width: 500,
				height: 300
			});
		$dialog.dialog('open');
		eventMarker.preventDefault();
	});
});
{% endhighlight %}

If you’ve seen jQuery snippets before, you should be familiar with the _$(document.ready)_, which executes the wrapped code only after the document has finished loading.

```$('.targetLinks').find('ul li a').on('click', function(eventMarker) {```

This line of code does two things:

1. Traverses the DOM to find the elements with class “targetLinks” that also contain the UL LI A descendant path and establishes a jQuery array of elements representing the matched `<a>` tags.
2. Assigns a click event to each `<a>` in the returned array of elements.  The click event is an anonymous function that takes the DOM Event and assigns it to an argument called “eventMarker”.

```var $dialog = $('<div></div>')```

Next, we establish a variable, _$dialog_, as a pointer to a newly created DIV element. Why a new div? The jQuery UI dialog, like many other jQuery plugins, destroy the element used as the selector when the dialog closes.  We could target an existing DIV as the dialog, but when the modal closes, the existing DIV will be destroyed (lost for all script).

```.load($(this).attr('href') + " #target")```


We continue the method chain, established on our newly created div, by calling the jQuery .load() method.  Load expects a URL (same site URI, of course), which we have passed by calling $(this) (the clicked anchor) and calling the .attr() method asking for the href.

```.dialog({
	autoOpen: false,
	modal:true,
	title: $(this).attr('title'),
	width: 500,
	height: 300
});```

Still continuing the method chain, we call the [.dialog()](http://jqueryui.com/demos/dialog/) method on the `$dialog` variable (div that was created).  This established the UI Framework necessary to make the dialog work.  We set a few options to the dialog (see the [UI documentation for the dialog](http://jqueryui.com/dialog/#options) for more options), again referencing _$(this)_ (the clicked anchor) and calling the [.attr()](http://api.jquery.com/attr/) method asking for the title of the anchor to set to the dialog title.

```$dialog.dialog('open');```

Here, we have separated the _open_ command from the dialog instantiation.  This doesn’t need to be the case, here, but may prove to be a useful practice later.  If you were waiting to see your dialog, you should see it now.

`eventMarker.preventDefault()`

This is the moment we’ve been waiting for.  We have been following happily along wherever the DOM actions will take us but now, we are going to take control of the clicked anchor tag, and force it to do what we want.  Remember, _eventMarker_ is the event argument passed in initially.  Here, we are calling the JavaScript preventDefault method on the event.  Since our event is the result of the click of an anchor, the default action should be to alter the window location.  Instead, we have truncated the event prior to changing the address bar.  As you might imagine, this same method can be used for bookmark links as well – in the end, the preventDefault() method will prevent the hash from being appended to the URL.

### Why _preventDefault()_ instead of return false;?

Whenever  you run a return from a JavaScript function, you exit the function – completely preempting any further actions.  With _preventDefault()_, further processing can be accomplished after the method call, within the same function.  Note that _eventMarker.stopPropogation()_ or _eventMarker.cancelBubble=true_ should be used to completely cancel event bubbling.

### So what?

Why would the methods in this post be useful?  With this code,  we are unobtrusively adding click event handlers to targeted links, but we are doing so without actually coding “onclick=’…’” or setting href=”javascript:function()”.  If your visitors have [JavaScript disabled](http://www.webdesignerdepot.com/2010/02/how-to-plan-for-the-absence-of-javascript/), then they will get the benefit of linked content.  If JavaScript is enabled, they will get the benefit of dynamic content display, without leaving the page.  _Win-win_.