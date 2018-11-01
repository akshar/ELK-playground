# Search DSL

## Search DSL component:
* Query context
* Filter context

Both can be combined to form complex queries

* Match Query:

* match everything
```
GET /courses/_search
{
	"match_all": {}
}
```

_source = relevenacy score

* match specific field
```
GET /courses/_search
{
	"query": {
		"match": {"name": "foo"}
	}
}
```

* get doc by field ( NOT null check)

```
GET /courses/_search

{
	"query": {
		"exists": {"field": "name"}
	}
}

```


* And query ( must be wrapped with bool query)

```
GET /courses/_search
{
	"query": {
		"bool": {
			"must": [
				{"match": {"name": "computer"}},
				{"match": {"room": "c8"}}
				]
			}
		}
}
```


* combining AND and NOT query( should be wrapped with bool query)
* must match computer and not include room c12

```
GET /courses/_search
{
	"query": {
		"bool": {
			"must": [
				{"match": {"name": "computer"}}
			],
			"must_not": [
				{"match": {"room": "c8"}}
			]
		}
	}
}
```

[bool query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)



## Multi match

```
	GET /courses/_search
	{
		"query": {
			"multi_match": {
				"fields": ["name","professor.department"],
				"query": "accounting"
			}
		}
	}
```

* search in fields name, professor['department'] for string "accouting"

* search phrase
```
	GET /courses/_search
	{
		"query": {
			"match_phrase": {
				"course_description": "lorem ipsum dolret"
			}
		}
	}
```

use match_phrase_prefix to search by partial tokens


## Range query:
(gte >= lte <= or use gt or lt)
```
GET courses/_search
{
	"query": {
		"range": {
			"students_enrolled": {
				"gte": 10,
				"lte": 20
			}
		}
	}
}
```
* can range over date as well.

convert should to must: "minimum_should_match": 1 => part of bool query
