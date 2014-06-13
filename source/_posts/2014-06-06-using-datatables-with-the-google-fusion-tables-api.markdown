---
layout: post
title: "Using DataTables with the Google Fusion Tables API"
date: 2014-06-06 23:48:41 +0200
comments: true
published: true
categories: [datatables, fusion tables]
---
{% img left /images/posts/datatables.png 150 %}
[DataTables](http://datatables.net/) is a UI widget that lets you interact with
tabular data. It has support for a feature called [Server Side Processing](http://datatables.net/manual/server-side),
where ajax calls are used to retrieve the relevant data. Examples show how to
query database servers, such as MySQL, but it is also possible to query
[Google Fusion Tables](https://support.google.com/fusiontables/answer/2571232), using the [API](https://developers.google.com/fusiontables/docs/v1/getting_started).

<!-- more -->

Google Fusion Tables is a tool for working with tabular data. It is often used
to clean datasets, but you can also use it to create visualisations of the data.
The Google Fusion API is extensive and it supports querying rows via SQL. Although
this is not a full implementation of the SQL standard, it has enough basic functionality
to be used with DataTables.

The biggest limitation of the SQL implementation in Fusion Tables is that it only
can combine conditions in the WHERE clause of the select statement using the AND
operator. As a consequence, we can only search in a single column with DataTables.

Otherwise the implementation of a Server Side Processing Script that communicates
with fusion tables is pretty straight forward. The source code is [available](https://github.com/openstate/DataTablesFusionTablesSSP) on Github.
