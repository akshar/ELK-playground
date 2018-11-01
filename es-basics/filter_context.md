# Filter Query

* Documents are not scored inside filter context
* filter query should be wrapped with bool query
* filter queries are cached.
* use filter when doing exact match

```
	GET /courses/_search
	{
		"query": {
			"bool": {
				"filter": {
					"bool": {
						"must": [
							{"match": {"name": "foo"}}
						]
					}
				},

					"must": [{"match": {"name": "bar"}}]
			}
		}
	}
```

2nd must outside the filter context, results of filter will acquire score.


 ```
GET /courses/_search
{
  "query": {
    "bool": {
      "filter": {
        "bool": {
          "must": [
            {"match": {"name": "accounting"}}
            ]
        }
      },
      "must": [
        {"match": {"room": "e3"}}
      ]
    }
  }
}
```

Below query will score document which matches room = e3 + will show in ascending order and will also have documents with zero score matching the filter query context (anyway all returned doc should statisfy filter query)
```
{
  "query": {
    "bool": {
      "filter": {
        "bool": {
          "must": [
            {
              "range": {
                "students_enrolled": {
                  "gte": 12
                }
              }
            }
          ]
        }
      },

      "should": [
          {"match": {"room": "e3 "}}
        ]
    }
  }
}
```
