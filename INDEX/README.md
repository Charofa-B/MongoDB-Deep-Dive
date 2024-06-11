Index In MongoDB

<br>

* ### [List All Indexes](#list-all-indexes)
* ### [Create Index](#create-index)
* ### [Create Text Index](#create-text-index)
* ### [Create Composite Index](#create-composite-index)
* ### [Create Geospatial Indexes](#create-geospatial-indexes)
* ### [Create TTL Indexes](#create-ttl-indexes)
* ### [Query Hint](#query-hint)
* * ### [Track Hint](#track-hint)
* ### [Drop Index](#drop-index)
* ### [Drop All Indexes](#drop-all-indexes)

<br><br>

## [List All Indexes](#list-all-indexes)
```sh
> db.CollectionName.getIndexes()
```

<br>

## [Create Index](#create-index)
```sh
> db.CollectionName.createIndex(keys, options)

# Options List
## { name: "compound_index" } => Specifies the name of the index. If not provided, MongoDB generates a name based on the fields indexed.

## {background:true} => the index creation will run in the background without blocking other database operations. Default is `false`.

## {unique:true} => ensures that the indexed field(s) contain unique values across the collection. Default is `false`.

## {sparse:true} => the index will only include documents that contain the indexed field(s). This can be useful for creating indexes on fields that may not exist in all documents. Default is `false`.

## { partialFilterExpression: { "status": { $eq: "active" } } } => Allows creating a partial index based on a filter expression. Only documents that match the filter expression will be included in the index. This option is available starting from MongoDB 3.2.

## { expireAfterSeconds: 3600 } => Specifies a TTL (time-to-live) for the documents in the collection. Documents that exceed this time limit will be automatically deleted. This option is used in conjunction with a TTL index.

## { weights: { "title": 10, "content": 5 } } => Assigns a weight to each field when creating a text index. This is used to control the relevance score of documents returned by text search queries.


## { collation: { locale: "en_US", strength: 2 } } => Specifies the collation for the index. This determines the rules for string comparison, such as case sensitivity and accent sensitivity.
```

## [Create Text Index](#create-text-index)
This function creates a text index on the specified field for text search operations.
```sh
> db.CollectionName.createIndex({ field: "text" })
```

## [Create Composite Index](#create-composite-index)
function creates a compound index on multiple fields. The order of fields in the index definition matters for query optimization.
```sh
> db.CollectionName.createIndex({ title: 1, author: 1 })
```

## [Create Geospatial Indexes](#create-geospatial-indexes)
function creates a geospatial index on a field containing GeoJSON objects for geospatial queries.
```sh
> db.CollectionName.createIndex({ location: "2dsphere" })
```

## [Create TTL Indexes](#create-ttl-indexes)
function creates a TTL (Time-To-Live) index to automatically expire documents after a specified time.
```sh
> db.CollectionName.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```

## [Query Hint](#query-hint)
technique used to force the query optimizer to use a specific index when executing a query. This can be useful in scenarios where the optimizer's automatic selection is not ideal for certain queries, and you know a better index choice that will improve performance.
```sh
# You can combine multiple indexes within the hint
> db.CollectionName.find({ <field: value> }).sort({ <Sort Element> }).hint({ <IndexField>: 1 });
```

## [Track Hint](#track-hint)
* returns the documents in the order they are currently stored on disk at the time the query is executed
* Useful in testing how data retrieval varies with and without using indexes, especially for performance analysis.
```sh
> db.CollectionName.find({ <field: value> }).hint({ indexField: 1 })

# Reverse the result
> db.CollectionName.find({ <field: value> }).hint({ indexField: -1 })
```


## [Drop Index](#drop-index)
```sh
> db.CollectionName.dropIndex(index)
```

## [Drop All Indexes](#drop-all-indexes)
```sh
> db.CollectionName.dropIndexes()
```