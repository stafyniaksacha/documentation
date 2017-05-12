# Basic queries

We will present here the most common ways to use the different search queries,
together with the options that modify their behaviour.
For more details about these options you can find more informations in the
[Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.x/query-dsl.html).

## The search endpoint (and the `match_all` query)

The search endpoint allows different query parameters to control the output of the search.
By defaut, the search endpoint will return the **10** first results, sorted by score.
The `match_all` query returns all the documents in the collection,
it can be useful with other queries in a `bool` query for instance.

### Without query parameters

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "match_all": {}
  }
}'
```

Reply:

```json
{
  "took" : 4,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 5,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "3",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Smith",
        "title" : "I hate fish",
        "body" : "They do not bring the ball back",
        "tags" : [ "pet", "animal", "fish" ],
        "status" : "pending",
        "publish_date" : "2017-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 1.0,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "5",
      "_score" : 1.0,
      "_source" : {
        "author" : "Will Smith",
        "title" : "I admire lions",
        "body" : "They are so regal",
        "tags" : [ "wild animal", "animal", "lion" ],
        "status" : "published",
        "publish_date" : "2016-08-02"
      }
    } ]
  }
}
```

Returns all the documents in the blogpost collection (because we have less than 10).

### With `from` and `size` query parameters

To change this behaviour, 2 query parameters are available : `from` and `size`:

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty&from=3&size=2" -d '{
  "query": {
    "match_all": {}
  }
}'
```

Reply:

```json
{
  "took" : 28,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 5,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 1.0,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "5",
      "_score" : 1.0,
      "_source" : {
        "author" : "Will Smith",
        "title" : "I admire lions",
        "body" : "They are so regal",
        "tags" : [ "wild animal", "animal", "lion" ],
        "status" : "published",
        "publish_date" : "2016-08-02"
      }
    } ]
  }
}
```

Returns the 4th and 5th result of the search query.
This is very useful when you want to paginate the results.

### The `scroll` query parameter

The `scroll` query parameter is useful when dealing with huge data sets,
or when you want to be sure the data set will not change during your processing. We recommend you to read the
[Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.x/search-request-scroll.html)
for more details.

## The `ids` query

Returns the documents with the matching `id`.

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "ids": {
      "values": ["2", "4"]
    }
  }
}'
```

Reply:

```json
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 1.0,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    } ]
  }
}
```


## The `query_string` query

The `query_string` query is a way to "talk" directly to the core engine of Elasticsearch.
If you are used to use Solr, it will look familiar.

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "query_string": {
      "query": "_id:1 OR _id:2"
    }
  }
}'
```

Reply:

```json
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 0.35355338,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : 0.35355338,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : 0.35355338,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      }
    } ]
  }
}
```


## The `match` query

The `match` query is the one you want to use to perform a full text search.
The query you use (here: "hate cake") is analyzed (lowercased, tokenized ...)
and then is applied against the analyzed version of the field (which is also lowercased, tokenized...).
As a result, the choice of the analyzer applied to a field is very important. To know more about analyzers,
we recommend you to read the
[Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.x/analysis-analyzers.html).

It results in a set of documents where a score is applied.

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "match": {
      "title":"hate cake"
    }
  }
}'
```

Reply:

```json
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.2201192,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 1.2201192,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "3",
      "_score" : 0.23384948,
      "_source" : {
        "author" : "John Smith",
        "title" : "I hate fish",
        "body" : "They do not bring the ball back",
        "tags" : [ "pet", "animal", "fish" ],
        "status" : "pending",
        "publish_date" : "2017-08-03"
      }
    } ]
  }
}
```

You can see that the second document does not contain cake at all but is still matching.
This is because, by default, the `match` query operator applies a `or` operand to the provided searched terms.
To return documents matching all tokens, you have to use the `and` operator:

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "match": {
      "title": {
        "query": "hate cake",
        "operator": "and"
      }
    }
  }
}'
```

Reply:

```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 1.2201192,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 1.2201192,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    } ]
  }
}
```

## The `prefix` query

The `prefix` query matches all the documents where the given field has a value that begins with the given string.
In the following example, we want to match all the documents where the value of field status begins with `pub`:

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "prefix": {
      "status":"pub"
    }
  }
}'
```

Reply:

```json
{
  "took" : 107,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "5",
      "_score" : 1.0,
      "_source" : {
        "author" : "Will Smith",
        "title" : "I admire lions",
        "body" : "They are so regal",
        "tags" : [ "wild animal", "animal", "lion" ],
        "status" : "published",
        "publish_date" : "2016-08-02"
      }
    } ]
  }
}
```


## The `range` query

The `range` query matches all the documents where the value of the given field is included within the specified range.
In the following example, we want to match all the document where `published_date` is included within the two specified dates:

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "range": {
      "publish_date": {
        "gte": "2016-08-01",
        "lte": "2016-08-31",
        "format": "yyyy-MM-dd"
      }
    }
  }
}'
```

Reply:

```json
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 3,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "5",
      "_score" : 1.0,
      "_source" : {
        "author" : "Will Smith",
        "title" : "I admire lions",
        "body" : "They are so regal",
        "tags" : [ "wild animal", "animal", "lion" ],
        "status" : "published",
        "publish_date" : "2016-08-02"
      }
    } ]
  }
}

```

## The `term` query

The `term` query is used to find exact matches on the *indexed* value of a field.
It should not be used on analyzed fields: the analyzed value that is indexed is a modified version of the input value.
Analyzers are explain during the cookbook you can come back when it is clearer.

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "term": {
      "status": "pending"
    }
  }
}'
```

Reply:

```json
{
  "took" : 4,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.5108256,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : 1.5108256,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "3",
      "_score" : 1.5108256,
      "_source" : {
        "author" : "John Smith",
        "title" : "I hate fish",
        "body" : "They do not bring the ball back",
        "tags" : [ "pet", "animal", "fish" ],
        "status" : "pending",
        "publish_date" : "2017-08-03"
      }
    } ]
  }
}
```

## The `terms` query

Behaves exactly like `term`, but with multiple possible exact matches.

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "terms": {
      "status": ["pending", "archived"]
    }
  }
}'
```

Reply:

```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 3,
    "max_score" : 0.7524203,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 0.7524203,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : 0.46769896,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "3",
      "_score" : 0.46769896,
      "_source" : {
        "author" : "John Smith",
        "title" : "I hate fish",
        "body" : "They do not bring the ball back",
        "tags" : [ "pet", "animal", "fish" ],
        "status" : "pending",
        "publish_date" : "2017-08-03"
      }
    } ]
  }
}

```


## The `exists` query


The `exists` query matches the documents where a given field is present:

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "exists": {
      "field": "author"
    }
  }
}'
```

Reply:

```json
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 5,
    "max_score" : 1.0,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "3",
      "_score" : 1.0,
      "_source" : {
        "author" : "John Smith",
        "title" : "I hate fish",
        "body" : "They do not bring the ball back",
        "tags" : [ "pet", "animal", "fish" ],
        "status" : "pending",
        "publish_date" : "2017-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : 1.0,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      }
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "5",
      "_score" : 1.0,
      "_source" : {
        "author" : "Will Smith",
        "title" : "I admire lions",
        "body" : "They belong to the Savanna",
        "tags" : [ "wild animal", "animal", "lion" ],
        "status" : "published",
        "publish_date" : "2016-08-02"
      }
    } ]
  }
}
```

## The `missing` query

The `missing` query has been removed in Elasticsearch 5.x. Elasticsearch recommends to use the `exists` query in a `must_not`
occurence of a `bool` compound query (and this will introduce you to the `bool` query :-) ).

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "author"
        }
      }
    }
  }
}'
```

Reply:

```json
{
  "took" : 4,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 0,
    "max_score" : null,
    "hits" : [ ]
  }
}
```

## Sorting the result set

If you want to sort your result set in a different order than the `_score` default sort
or compound the `_score` sort with other fields, you can specify the sort order alongside to the query:

```json
curl -g -X POST "http://localhost:9200/example/blogpost/_search?pretty" -d '{
  "query": {
    "match_all": {}
  },
  "sort": [
    {"status": {"order": "asc"}}
  ]
}'
```

Reply:

```json
{
  "took" : 9,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "hits" : {
    "total" : 5,
    "max_score" : null,
    "hits" : [ {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "4",
      "_score" : null,
      "_source" : {
        "author" : "Jane Doe",
        "title" : "I hate cheese cake",
        "body" : "I prefer chocolat cake",
        "tags" : [ "food", "cake" ],
        "status" : "archived",
        "publish_date" : "1985-08-03"
      },
      "sort" : [ "archived" ]
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "1",
      "_score" : null,
      "_source" : {
        "author" : "John Doe",
        "title" : "I love cats",
        "body" : "They are so cute",
        "tags" : [ "pet", "animal", "cat" ],
        "status" : "pending",
        "publish_date" : "2016-08-03"
      },
      "sort" : [ "pending" ]
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "3",
      "_score" : null,
      "_source" : {
        "author" : "John Smith",
        "title" : "I hate fish",
        "body" : "They do not bring the ball back",
        "tags" : [ "pet", "animal", "fish" ],
        "status" : "pending",
        "publish_date" : "2017-08-03"
      },
      "sort" : [ "pending" ]
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "2",
      "_score" : null,
      "_source" : {
        "author" : "John Doe",
        "title" : "I like dogs",
        "body" : "They are loyal",
        "tags" : [ "pet", "animal", "dog" ],
        "status" : "published",
        "publish_date" : "2016-08-01"
      },
      "sort" : [ "published" ]
    }, {
      "_index" : "example",
      "_type" : "blogpost",
      "_id" : "5",
      "_score" : null,
      "_source" : {
        "author" : "Will Smith",
        "title" : "I admire lions",
        "body" : "They are so regal",
        "tags" : [ "wild animal", "animal", "lion" ],
        "status" : "published",
        "publish_date" : "2016-08-02"
      },
      "sort" : [ "published" ]
    } ]
  }
}

```

If the `_score` is not used in the sort, it is not calculated and nullified in the reply.