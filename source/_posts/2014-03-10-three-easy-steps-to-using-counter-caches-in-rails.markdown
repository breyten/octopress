---
layout: post
title: "Three easy steps to using counter caches in Rails"
date: 2014-03-13 17:04:05 +0100
comments: true
published: true
categories: [rails]
---
I recently started using counter caches in Rails. Getting them to work was a little bit more difficult than the manual suggests, however.

<!-- more -->

The [manual](http://guides.rubyonrails.org/active_record_basics.html#naming-conventions) reads:

{% blockquote %}
Used to cache the number of belonging objects on associations. For example, a comments_count column in a Post class that has many instances of Comment will cache the number of existent comments for each post.
{% endblockquote %}

Although the mechanism built into Rails does the hard part, you still have to implement perform a couple of steps or else it won't work  at all. Here are the steps you need to do to get a counter cache to behave.

Let's start by using the same example as in the manual: we want to count the number of comments on the blog post.

The model definitions look like this:

``` ruby
class Post < ActiveRecord::Base
  has_many :comments
end

class Comments < ActiveRecord::Base
  belongs_to :post
end
```

The first thing you have to do is adding a `comments_count` column to the Post model. You can do that using a migration. After running the migration you also need to instruct Rails that the column should be treated as a counter cache:

``` ruby
  belongs_to :post, :counter_cache => true
```

The only problem left is that the counts are off if there are already posts and comments in the database. This is because the counter caches are incremented or decremented when you create or delete an instance of the model. You can reset the counters for the posts in the following manner:

``` ruby
# Post.find_each { |post| Post.reset_counters(post.id, :comments) }
# The above (an earlier version of this post) is not correct, it should be:
Post.find_each { |post| Post.reset_counters(post.id, :comments) }
```

It's a good idea to make a rake task for it, so you can run it whenever there are problems with your cached counters.
