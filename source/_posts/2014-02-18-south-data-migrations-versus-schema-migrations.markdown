---
layout: post
title: "South data migrations versus schema migrations"
date: 2014-02-18 16:42:39 +0100
comments: true
categories: [django, south]
---
[South](http://south.aeracode.org/) makes a distinction between schema migrations and data migrations. But why is that a good thing?

<!-- more -->

South [defines](http://south.readthedocs.org/en/latest/tutorial/part3.html#data-migrations) a schema migrations as "migrations which change the layout of your columns and indexes". This is complemented by the notion of data migrations which are "used to change the data stored in your database to match a new schema, or feature".

But, as Branko Vukelic [notes](http://www.brankovukelic.com/2013/02/south-migration-with-dynamically.html), you can also dynamically generate data in a schema migration -- although his solution is outdated because of South's dry run feature. Making it work is trivial, however:

``` python
if not db.dry_run:
    foos = orm['foos.Foo'].objects.all()
    for foo in foos:
        foo.slug = slugify(foo.name)
        foo.save()
```

There is of course, a drawback when doing it this way: _you can never regenerate the data without running the schema migration_. Data migrations are also marked for no-dry-run by default, so your code ends up a bit simpler.