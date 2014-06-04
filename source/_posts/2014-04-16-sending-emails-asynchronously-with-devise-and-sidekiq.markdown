---
layout: post
title: "Sending emails asynchronously with Devise and Sidekiq"
date: 2014-04-16 12:36:18 +0200
comments: true
published: true
categories: [rails, devise, sidekiq]
---
{% img left /images/posts/sidekiq.png 150 %}
If you're building a website that supports user logins and use [Devise](https://github.com/plataformatec/devise) to manage
that, you'll soon find out that it sends out emails synchronously by default.
This is not a good practice, as it's not something that a user visiting your
site should have to wait for. There is a solution to make this work asynchronously,
however.

<!-- more -->

There is a [page](https://github.com/plataformatec/devise/wiki/How-To%3A-Send-devise-emails-in-background-%28Resque%2C-Sidekiq-and-Delayed%3A%3AJob%29) in the Devise wiki that describes how to configure it for sending
emails asynchronously. It involves using the [devise-async](https://github.com/mhfs/devise-async) gem and it's rather flexible and supports Resque, Sidekiq and Delayed::Job.
You should start by following the instructions on that page if you're using Sidekiq.

However, there is a thing that you might overlook: by default it'll enqueue the
jobs in the `mailer` queue. This means you'll have to either configure you Sidekiq worker to
look for jobs in that queue as well, or instruct devise-async to
enqueue in a different queue.

The latter is simple and is described in the README:

``` ruby
# config/initializers/devise_async.rb
Devise::Async.queue = :my_custom_queue
```

Configuring the Sidekiq worker to [look into more than one queue](https://github.com/mperham/sidekiq/wiki/Advanced-Options) is only slightly
more complicated and looks something like the following:

``` yaml
---
:concurrency: 5
:pidfile: tmp/pids/sidekiq.pid
staging:
  :concurrency: 10
production:
  :concurrency: 20
:queues:
  - default
  - mailer
  - [myqueue, 2]
```

You should save this as `config/sidekiq.yml` and then restart the worker as well
as the server and then your emails will be sent asynchronously.
