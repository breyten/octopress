---
layout: post
title: "Modeling a spreadsheet in Elasticsearch"
date: 2014-02-12 15:04:31 +0100
comments: true
categories: elasticsearch
---
I recently worked on a project that involved extracting information for Excel files and storing this information in [Elasticsearch](http://www.elasticsearch.org/). Doing this is quite fun and using some tricks you can emulate some of the basic functions of a spreadsheet with ease.
<!-- more -->
## Mapping the data
The first step is to map the data in Elasticsearch. For the sake of simplicity, I assume the following:

1. only a single worksheet.
2. No formulae

A worksheet is basically a collection of cells that have a row, a column, and a value. Hence, the resulting mapping looks like this:

``` json
{
  "spreadsheet": {
    "properties": {
      "row": {"type": "string", "index": "not_analyzed"},
      "column": {"type": "integer"},
      "value": {"type": "float"}
    }
  }
}
```

Now you can load it with some sample data of your choosing.

## Using facets to aggregate
[Facetting](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-facets.html) is a really powerful tool in Elasticsearch and here I'll show you how to use it to do aggregations.

Let's say we can to caculate the sum of A2 and A3. In Excel, you would simple use the SUM function to accomplish this. In Elasticsearch you use the [term stats facet](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-facets-terms-stats-facet.html) like this:

``` json
{
    "query": {
        "bool": {
            "must": [
               {"term": {
                  "row": {
                     "value": "A"
                  }
               }},
               {"range": {
                      "column": {
                         "from": 2,
                         "to": 3
                      }
               }}
            ]
        }
    },
    "facets": {
       "sum": {
           "terms_stats": {
              "key_field": "row",
              "value_field": "value"
           }
       }
    }
}
```

The result is returned in the total field of the sum facet. Doing a sum across rows is trivial (use a [range](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-range-query.html) or [terms](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-terms-query.html) query), and using the facet you can even compute sums for the same columns across rows.

You can do even more aggregations in one go using [facet filters](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-facets.html#_facet_filter), which makes Elasticsearch a powerful tool for emulating a spreadhseet, as long as you don't need complex formulae.