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
