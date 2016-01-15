---
layout: post
title:  "Jason vs. Page Weight – Accessibility Series, pt. 2"
date:   2010-05-13 21:55:34 -0800
categories: blog accessibility
---

When the topic of Web Accessibility arises, I immediately think of a standard set of keywords: ADA/508 compliance, information architecture, JavaScript, well-formed HTML, ALT text, etc.  This list represents the things that, as I have been taught over the past few years, have significant impact on the accessibility of a page and a site.

Recently, during an accessibility assessment, I discovered a new factor to consider: page weight.  Page Weight should not be a new concept to any web designer.  Ever since the days of dial-up dominance, we have been scrutinizing every paragraph, abusing calculators to determine the most appropriate image collection, mapping the dimension and location of every new icon and rollover in our sprite, clocking speed tests and ripping out every non-essential bit of white-space we could find.  I admit that I have become a little lax, allowing more and larger images on my page for the sake of design, all the while whispering silent apologies to the dial-up community.


During the aforementioned assessment, we paid special attention to one set of pages in particular.  Our project, the public-facing site of a university, has a section outlining each of the degree programs.  The challenge the client faced is one that many education websites face: how to appropriately define a program and all its structural elements, without destroying the visitor’s sense of connection to the program itself.  The client’s decision was to provide all the program details on one page, rather than just linking from the program page to course description pages.  Program details include the course subject, required course list, course descriptions and, in some cases, subject description.  For a full program page, we discovered the amount of information could be….daunting.  With nine or ten requirement categories and as many as fourteen course requirements per category, we were looking at a number of pages with an HTML weight of 130 kb.  Once the images were added in, we were well above 200 kb in page weight which could take up to 30 seconds to load on a 56 Kbps modem.

As it turns out, the page weight is a necessary evil for this particular page.  We had two options, in order to maintain an accessible data set: put all the information on one page or link to it all from one page.  All the data on one page makes the navigation of the program easier for the site visitor, but overloads the ideal weight.  Linking from the program page to each course would have delivered a light page but would have forced a lot of usage of the back button.

So what is the ideal solution?  For this client, the massive page was the preferred solution.

Alternatively, we could have used a bit of JQuery magic to deliver a lightweight page, with links to additional content, that was also slick and usable. I cover the details of this option in an upcoming post (Jason vs. Anchor Tags – Getting Results with the PreventDefault method) but the idea is this:

Provide the user with an appropriate amount of content – an amount that would adequately describe the program- and supply links to the detail pages of each course.  When a visiting user has JavaScript disabled, the content would behave as you might expect: the visitor reads the program description, clicks a link and navigates to a course description page.

When JavaScript is available, however, we capture the click event of the anchor and, since we’re in the same domain as the source content, pull the source content into a modal dialog (like ColorBox, if you’re in the market for a good one) via AJAX request and prevent the default action of the link from firing.  We’ll use the HREF property of the anchor as the URI to the source.  Further, we’ll use the anchor’s class as the selector target (See jQuery’s class selector for more details).

A few things to note -

We did not have to include any onclick property to return false at the click, which helps us maintain our unobtrusive JavaScript.
Since we would be relying on the anchor’s class when attaching click events, we can flag any anchor to be skipped just by adding another name to a compound class of the anchor OR omitting a target class name.
Degradation is smooth and predictable, since we’re just talking about anchors with standard href properties.  Without the JavaScript to cause the Modal onclick, the link will fire and redirect the visitor to the uri referenced in the href.
With the JavaScript in place, we will have taken a very large page and trimmed it down to a manageable size, and successfully merged accessibility with usability.

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
