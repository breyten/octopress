---
layout: post
title: "Django locale middleware with no frills"
date: 2014-02-10 15:46:12 +0100
comments: true
categories: [django, python]
---
Django's built-in locale middleware is rather [sophisticated](https://docs.djangoproject.com/en/dev/topics/i18n/translation/#how-django-discovers-language-preference). It tries to guess your language according to the following heuristics:
<!-- more -->
1. A language prefix in the URL
2. A key in the user's session
3. A cookie
4. The Accept-Language header
5. The global LANGUAGE_CODE setting

However, in a lot of cases this is overkill and only using the LANGUAGE_CODE is enough. A solution to make it easier is to provide your own middleware. It turns out that the solution is actually quite simple and only requires you to set the language for the request as follows:

{% gist 7909059 %}