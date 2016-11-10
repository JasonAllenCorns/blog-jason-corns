---
layout: post
title:  "How to: apiDoc plugin"
date:   2016-09-21 8:54:23 -0800
categories: blog
author: jason_corns
tags: [apiDoc, code, api, node, documentation]
image:
  feature: /apidoc/apidoc_banner.png
  credit: inveris
  creditlink: https://github.com/apidoc/apidoc
---

Documentation is tedious.  It's necessary, but tedious.  When recently tasked with creating a useful, wide-audience documentation site for a Node API server, I discovered [apiDoc](http://http://apidocjs.com/){:target="_blank"}, which is targeted to this purpose.  Following a vernacular similar to [jsDoc](http://usejsdoc.org){:target="_blank"} but addressing the higher API document, apiDoc creates a beautiful blend of in-file documentation and distributed (html) documentation.<!--more-->

While it was very close to what I needed, apiDoc did not have a few preferred fields, like "author".  Luckily, [@rottmann](https://github.com/rottmann){:target="_blank"} has created an extensible system in the apiDoc suite.  Let's step through the process of creating the [apidoc-plugin-author](https://www.npmjs.com/package/apidoc-plugin-author) plugin.

## apiDoc Suite
First, an understanding of the apiDoc structure.  The documentation page, at [https://github.com/apidoc/apidoc](https://github.com/apidoc/apidoc){:target="_blank"}, is actually a single page application built on handlebars templates and ingests a JSON structure as defined by [https://github.com/apidoc/apidoc-spec](https://github.com/apidoc/apidoc-spec){:target="_blank"}.   [https://github.com/apidoc/apidoc-core](https://github.com/apidoc/apidoc-core){:target="_blank"} is the engine behind the documentation, and generates the JSON fixture for the documentation S.P.A.  Creating and displaying our plugin is a matter of writing a custom parser to consume the custom param and coding a custom(ized) template for apiDoc.

## Getting Started

### Assumptions and Understandings


### Nodejs API
I will not include instructions for authoring a Node / Express API.  [Chris Sevilleja](https://github.com/sevilayha) has already written a fantastic [API How-To at Scotch.io](https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4), to get you started if you need it.

### Inclusion in a Project



### apidoc-plugin-*

Convention sets in right from the start.