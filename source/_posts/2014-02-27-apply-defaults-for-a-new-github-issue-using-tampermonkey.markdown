---
layout: post
title: "Apply defaults for a new Github issue using Tampermonkey"
date: 2014-02-27 14:48:25 +0100
comments: true
categories: 
---
I've been working a lot with [Github](https://www.github.com)'s issue tracker lately. Not perfect by all means, but it does a decent job. But it could be a lot better, especially when you're creating a new issue.

<!-- more -->

For me, the most important ones are the following:

* No default assignee
* No default milestone

To fix this I have created a small script for use with [Tampermonkey](http://tampermonkey.net/) to change this behavior into (for me) saner defaults. It assigns the issue to the logged in user, and sets the milestone to the one which is the soonest.

Here is the source code:

``` javascript
// ==UserScript==
// @name       GithubNewIssueDefaults
// @namespace  http://yerb.net/
// @version    0.1
// @description  Make a new issue have sane defaults -- milestone & assignee
// @match      https://github.com/*/*/issues/new
// @copyright  2013, Breyten Ernsting
// @require http://code.jquery.com/jquery-latest.js
// ==/UserScript==

$(document).ready(function(){
  setTimeout(function() {
    var user = $('#user-links a.name').attr('href').substr(1);
    var assignee_input = $('input[name="issue[assignee]"][value="' + user + '"]');
  	$('.js-composer-assignee-picker span[role="button"]').click();
    assignee_input.click();
    $('.js-composer-milestone-picker span[role="button"]').click();
    $('.js-composer-milestone-picker').find('*[role="menuitem"]').click();
  }, 1);
});
```