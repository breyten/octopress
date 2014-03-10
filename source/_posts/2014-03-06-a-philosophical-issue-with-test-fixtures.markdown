---
layout: post
title: "A philosophical issue with test fixtures"
date: 2014-03-06 16:23:20 +0100
comments: true
published: false
categories: [testing]
---
A lot of unit testing involves testing with data. For this purpose you create [test fixtures](http://en.wikipedia.org/wiki/Test_fixture). The way unit testing usually works is that these text fixtures are loaded for each unit test. Hence, the time it will take for your testing to complete is proportional with the number of test you write. You don't want to cut down on the number of tests, so you make the test fixtures as small as possible, all while trying to guarantee that your test fixtures represent the content of your live database to a certain degree.

Now, consider that there is a bug for which you have to add an entity to the test fixtures (because the entity exhibits a property that triggers said bug). You write a unit test for this bug and you look up where the problem is. You change the code and rerun your unit tests until you solved the bug.

You repeat this mentioned process several times -- it's a large project, so let's say 100 times for each entity. You end up with test fixtures where the number of instances of each entity that exhibit some form of exoticness in their properties (since they triggered bugs) is greater than those that were normal (to a certain degree).

At this point your test fixtures do not represent the content of your live database anymore. So what do you do? The way I see it you have two options:

1. Remove the bug-specific instances and unit tests. The result is that your test fixtures will again represent your live database. The only downside is that you can't guarantee that these bugs will not occur anymore.
2. You leave it as it is. The result is that your unit tests are less valuable since your test fixtures do not represent your live database anymore. But at least you're certain that the bugs are gone forever.

So, what's the best choice?