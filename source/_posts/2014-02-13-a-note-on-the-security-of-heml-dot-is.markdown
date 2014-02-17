---
layout: post
title: "A note on the security of Heml.is"
date: 2014-02-13 15:15:20 +0100
comments: true
categories: security
---
Heml.is advertises itself as a secure messenger. But how secure is it? Let's look into this.
<!-- more -->
The main thing that caught my eye when looking at the frequently asked questions was the following:

{% blockquote %}
The fundamental benefits of Heml.is will be the app together with our infrastructure, which is what really makes the system interesting and secure.
{% endblockquote %}

Further down they state that:

{% blockquote %}
The way to make the system secure is that we can control the infrastructure. Distributing to other servers makes it impossible to give any guarantees about the security.
{% endblockquote %}

This shows that Heml.is relies on security through obfuscation. This is not a good practice, as it is possible that there would be security issues. These would not be obvious, however. But we have nothing to fear because:

{% blockquote %}
We’ll have audits from trusted third parties on our platforms regularily.
{% endblockquote %}

In other words: "just trust us".

Now, doesn't that [sound familiar](http://techcrunch.com/2013/07/31/dont-worry-trust-us/)?
