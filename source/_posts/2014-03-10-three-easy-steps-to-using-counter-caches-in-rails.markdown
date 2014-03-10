---
layout: post
title: "Three easy steps to using counter caches in Rails"
date: 2014-03-10 15:34:05 +0100
comments: true
published: false
categories: [rails]
---
I recently started using counter caches in Rails. This seems like a simple concept, but the manual is a bit misleading in gettings these to work correctly. The [manual](http://guides.rubyonrails.org/active_record_basics.html#naming-conventions) reads:

{% blockquote %}
Used to cache the number of belonging objects on associations. For example, a comments_count column in a Post class that has many instances of Comment will cache the number of existent comments for each post.
{% endblockquote %}

This suggest that there are things autmagically working for you and might lead you to believe that you don't have to do anything on your part to get it working. This is unfortunately not true. So here are the steps you need to do to get a counter cache to behave like you expect it to.

Let's start by using the same example as in the manual: we want to count the number of comments on the blog post.

The model difinitions look like this:

``` ruby
class Post < ActiveRecord::Base
  has_many :comments
end

class Comments < ActiveRecord::Base
  belongs_to :post
end
```

The first thing you have to do is adding a `comments_count` column to the Post model. You can do that using a migration. Now, you need to explicitly tell Rails that you want to use a cache counter for this colum, like so:

``` ruby
  belongs_to :post, :counter_cache => true
```

The only problem ramaining now is that the counts are wrong if there are already posts and comments in the database. This is because the way the counter caches work is that they increment or decrement the counter when you're saing or deleting an instance of the model. You can fix this by resetting the counter caches manually:

``` ruby
Post.each { |post| Post.reset_counters(post.id, :comments) }
```





