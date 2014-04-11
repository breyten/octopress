---
layout: post
title: "Automatically create draft posts using Octopress"
date: 2014-03-24 12:41:54 +0100
comments: true
published: true
categories: [octopress]
---
{% img left /images/posts/draft.png 150 %}
[Octopress](http://octopress.org/) has support for draft posts, which is [covered](http://octopress.org/docs/blogging/) in the documentation.

<!-- more -->

You mark a post as a draft using the `published` directive:

``` yaml
published: false
```


However, new posts do not include this directive. In order to change this behaviour, you have to edit the `Rakefile`. Scroll to the `:new_post` task. At the end of the task there are a couple of lines which write the YAML front matter into the file. You can add more directives there. I've put the published directive just after the [comments directive](https://github.com/imathis/octopress/blob/master/Rakefile#L116). It looks like this:

``` ruby
puts "Creating new post: #{filename}"
open(filename, 'w') do |post|
  post.puts "---"
  post.puts "layout: post"
  post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
  post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
  post.puts "comments: true"
  post.puts "published: false"
  post.puts "categories: "
  post.puts "---"
end
```

Now all new posts will be draft posts. Just make sure you don't forget to mark them as published after you're done editing.
