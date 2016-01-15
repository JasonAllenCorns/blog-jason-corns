---
layout: post
title:  "Jason vs. the CSS Triangle: Seeing My Own Shadow"
date:   2013-03-28 11:14:23 -0800
categories: blog
---

A few years ago, I stumbled upon the CSS Triangle - a clever abuse of borders to achieve a triangle shape using no images.  This led, almost immediately, to things like fancy tooltips.

I like things that are clever and simple, so I was immediately drawn to the idea.  The border trick, however, does not play well with box-shadow, and I desperately wanted a shadow on the triangle marker (“desperate” may be too strong a term, here). The borders are still technically there, so the box-shadow will show around the rectangular space occupied by the element, not just the triangular space occupied by the border.

After a few minutes of tinkering, I came up with the following solution.  Given an element with classname “hpContent”, consider the following “before” pseudo-element.  It’s a ton of CSS for this effect; a necessary evil required to support the various browser prefixes.

{% highlight css %}
.hpContent::before {
    content: "";
    background: #f9fcf7;
    border-radius: 0;
    position:  absolute;
    left: -12px;
    top:  18px;
    height: 24px;
    width: 24px;
    -webkit-box-shadow:  -5px 5px 6px 2px rgba(122, 122, 122, .4);
    box-shadow:  -5px 5px 6px 2px rgba(122, 122, 122, .4);

    transform: rotate(45deg);
    -ms-transform: rotate(45deg); /* IE 9 */
    -webkit-transform: rotate(45deg); /* Safari and Chrome */
    -o-transform: rotate(45deg); /* Opera */
    -moz-transform: rotate(45deg); /* Firefox */

    background: -moz-linear-gradient(45deg,  rgba(249,252,247,1) 0%, rgba(249,252,247,1) 50%, rgba(249,252,247,0) 51%, rgba(249,252,247,0) 100%);
    background: -webkit-gradient(linear, left bottom, right top, color-stop(0%,rgba(249,252,247,1)), color-stop(50%,rgba(249,252,247,1)), color-stop(51%,rgba(249,252,247,0)), color-stop(100%,rgba(249,252,247,0)));
    background: -webkit-linear-gradient(45deg,  rgba(249,252,247,1) 0%,rgba(249,252,247,1) 50%,rgba(249,252,247,0) 51%,rgba(249,252,247,0) 100%);
    background: -o-linear-gradient(45deg,  rgba(249,252,247,1) 0%,rgba(249,252,247,1) 50%,rgba(249,252,247,0) 51%,rgba(249,252,247,0) 100%);
    background: -ms-linear-gradient(45deg,  rgba(249,252,247,1) 0%,rgba(249,252,247,1) 50%,rgba(249,252,247,0) 51%,rgba(249,252,247,0) 100%);
    background: linear-gradient(45deg,  rgba(249,252,247,1) 0%,rgba(249,252,247,1) 50%,rgba(249,252,247,0) 51%,rgba(249,252,247,0) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f9fcf7', endColorstr='#00f9fcf7',GradientType=1 );

}
{% endhighlight %}

Renders as this:

![alt text](/img/css-corner-full-width.png "CSS corner with drop-shadow")

 

 

That little triangle on the left?  That’s the result of the CSS block above.  The rounded corners and general box-shadow are just part of the surrounding Proof-of-Concept.

And voila.  Take that, PNGs.

Standard InternetExplorer disclaimer, blah blah blah.  This solution uses box-shadow and multi-stop gradients – two things that are not fully supported by IE.

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
