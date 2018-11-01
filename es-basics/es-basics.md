# ES

DB    |     ES
-----------------
TABLE  | Index
ROW    | Document
COLUMN | Field

index > type > Documents > Fields
=======================================================================
Inserting = indexing

# Index a document:
```
PUT /{index}/{type}/{id}
		{
			"field": "value"
		}
```
E.g:
```PUT /vehicles/car/1
		{
			"make": "foo",
			"color": "white",
			"price": 314
		}
```
# get document: `GET /vehicles/car/1/_source`   => get only source, ignore metada.

# to check if document exists: `HEAD /vehicles/car/1` => 200 -OK

* Documents are immutable. creates a new doc on every reindex

# updating specific fields:
```
POST /vehicles/car/1/_update
		{
			"make": "bar"
		}
```

# Deleting a docuemnt:
	Delete /vehicles/car/1


# get index schema:
	GET /index_name e.g GET /vehicles
# Delete index - don't
	DELETE /index_name

# List all indices
	GET /_cat/indices?v
