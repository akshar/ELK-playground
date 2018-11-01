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

OR use *stats*

GET /vehicles/cars/_search
{
    "aggs": {
        "popular_cars": {
            "terms": {
                "field": "make.keyword"
            },
            "aggs": {
                "car_stats": {
                    "stats": {
                        "field": "price"
                    } 
                }
            }
        }
    }
}

* We are creating a bucket named *popular_cars* (1st aggs) and generating mertrices (2nd aggs query)


* Date range bucket:
```
GET /vehicles/cars/_search
{
    "aggs": {
        "popular_cars": {
            "terms": {
                "field": "make.keyword"
            },
            "aggs": {
                "sold_date_range": {
                    "range": {
                        "field": "sold",
                        "ranges": [
                            {"from": "2016-01-01",
                            "to": "2016-05-18" },
                            {"from": "2016-05-18",
                            "to": "2017-01-01" }
                        ]
                    },
                    "aggs": {
                        "avg_price": {
                            "avg" : {
                                "field": "price"
                            }
                        }
                    }
                }
            }
        }
    }
}
```
popular_cars bucket => sold_date_range bucket = aggregate by date range + calcuate avg price for both date range


GET /vehicles/cars/_search
{
    "aggs": {
        "car_condition": {
            "terms": {
                "field": "condition.keyword"
           },
           "aggs": {
               "average_price": {
                   "avg": {
                       "field": "price"
                   }
               },
               "make": {
                   "terms": {
                       "field": "make.keyword"
                   },
                   "aggs": {
                       "min_max_stats": {
                           "stats": {
                               "field": "price"
                           }
                       } 
                   }
               }
           }
        }
    }
}