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

## Assumptions
These tips assume:

* Node.js v4+ installed
* npm 2+ (or [Yarn](http://yarnpkg.com), but you'll have to swap commands) installed
* JavaScript as the language
* An API ready for documentation

### Nodejs API
I will not include instructions for authoring a Node / Express API.  [Chris Sevilleja](https://github.com/sevilayha) has already written a fantastic [API How-To at Scotch.io](https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4), to get you started if you need it.

### Inclusion in a Project
Once you have a Node.js project to document, include `apiDoc` in your project as a dev-dependency, with ```npm install --save-dev apiDoc```.  This command will install `apiDoc` into _node_modules_, along with its own `apiDoc-core` dependency.  Take a moment to validate the apiDoc installation, and add some documentation lines to any one of the javascript files in the project.
{% highlight javascript %}
/**
 * @api {get} apiDataEndpoint Get data from this endpoint
 * @apiName getApiData
 * @apiGroup Site Data
 * @apiVersion 0.0.1
 *
 **/
{% endhighlight %}

With `apiDoc` installed locally and the above comment added to a `*.js` file in the project, run the documentation builder to test the install:
```./node_modules/.bin/apidoc```

**note**: simply running the `apidoc` command will attempt to build documentation for the entire project. If this is an existing project with many files, limit this validation run by limiting source directory(s) with the `-i` flag:
 ```./node_modules/.bin/apidoc -i ./lib/api```
 Further control the documentation build by also specifying the output location with the `-o` flag:
 ```./node_modules/.bin/apidoc -i ./lib/api -o ./apidoc-pages/ -f js```


### apidoc-plugin-*

Convention sets in right from the start.