# Mapping & Mapping types

* mapping = Mapping is the process of defining how a document, and the fields it contains, are stored and indexed. For instance, use mappings to define. (types and properties)


* settings and mappings need to be defined while creating an index
```
PUT /customers
{
	"settings": {
		"number_of_shards": 2 (defaults to 5),
		"number_of_replicas": 1
	},
	"mappings": {}
}
```

* mapping types: Each index has one or more mapping types, which are used to divide the documents in an index into logical groups. User documents might be stored in a user type, and blog posts in a blogpost type.

(customers = index | TABLE, online = _type | gender, age, name etc  = fields | columns)
* To define fields:
(NOTE: ES versions >= 7 will only have one mapping type)

```
PUT /customers
{
	"mappings": {
		"online": {
			"properties": {
				"gender": {
					"type": "text",
					"analyzer": "standard"
				},
				"age": {
					"type": "integer"
				},
				"totat_spent": {
					"type":  "float"
				},
				"is_new": {
					"type": "blooean"
				},
				"name": {
					"type": "text",
					"analyzer":  "standard"
				}
			}
		}
	},
	"settings": {
		"number_of_shards": 2,
		"number_of_replicas": 1
	}
}

```


* Indexing a doc:
```
PUT /customers/online/1
{
	"name": "foo",
	"gender": "male",
	"age": 10,
	"address": "Indiranagar",
	"is_new": true,
	"total_spent": 50.314
 }

```
* Here extra field `address` is added.ES will add that mapping. To prevent this and enforce strict mapping. pass flag `dynamic`

* if set to `false`- indexing field will be ignored for `true` - indexing field will throw error.

* To modify the mapping
```
GET customers/_mappings/online
	{
		"dynamic": false
	}
```
