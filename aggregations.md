```
GET /vehicles/cars/_search 
{
    "size": 5,
    "from": 2,
    "query": {
        "match_all": {}
    },
    "sort": [
       { "price": {"order": "asc"}}
    ]
}
```

(query infetch = get_all -> sort -> size/from)
size = number of doc to return, from : offset [doc](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-from-size.html)

## Aggregations:

* get count

```
GET vehicles/cars/_count
{
    "query": {
        "match": {"make": "foo"}
    }
}
```

aggs syntax: (aggs -> aggs_name -> aggs_type -> field)
```
GET vehicles/cars/_search
{
  "aggs": {
    "NAME": {
      "AGG_TYPE": {}
    }
  }
}
```

```
GET vehicles/cars/_search
{
    "aggs": {
        "popular_cars": {
            "terms": {
                "field": "make.keyword"
            },
            "aggs": {
                "avg_price": {
                    "avg": {
                        "field": "price"
                    }
                },
                "max_price": {
                    "max": {
                        "field": "price"
                    }
                }
            }
        }
    }
}
```

will give :

```
 "aggregations": {
        "popular_cars": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
                {
                    "key": "dodge",
                    "doc_count": 5,
                    "avg_price": {
                        "value": 18900
                    }
                }
            ]
```


