# Basic Commands

<br><br>

* ### [Databases Navigation](#databases-navigation)
* ### [Perfermence Traking](#perfermence-traking)
* ### [Collections Commands](#collectiona-basic-commands)
* ### [Documents Commands](#documents-basic-commands)
* ### [Fields Commands](#fields-commands)


<br><br>

## [Databases Navigation](#databases-navigation)
### Show Current Database
```sh
> db
```
### Show All Databases
```sh
> show dbs
```
### Create/Switch Database
```sh
> use db_name
```
### Drop Database
```sh
> db.dropDatabase()
```

<br>

## [Perfermence Traking](#perfermence-traking)
### Enable Profiling
**Level 0** : Profiling is off. No operations are logged.

**Level 1** : Profiling is on for all database operations. This means that all operations, regardless of their execution time, are logged.

**Level 2** : Profiling is on for operations that take longer than a specified threshold. This threshold is set using the `slowms` parameter. Operations that take longer than this threshold are logged.
****
****
```sh
# This Command Sets The Profiling Level To Capture All Operations (1) That Take Longer Than 100 Milliseconds
db.setProfilingLevel(1, { slowms: 100 })
```

### Analyze Profiler Output
Enable query profiling and analyze the output
```sh
# This Command Will Show The Last 10 Operations Captured By The profiler. You can adjust the limit as needed.
db.system.profile.find().sort({ ts: -1 }).limit(10)
```

### Use Explain
```sh
# Work Only With Read Functions
# This command will provide detailed information about how MongoDB executes the query.
> db.collectionName.explain().find({ "Your query" })

# Execution state
# This option provides detailed execution statistics including the number of documents scanned, the number of documents returned, execution time, and more.
> db.CollectionName.explain("executionStats").find({ /* Your query */ })

# Query Planner
# This option provides information about the query plan chosen by MongoDB, including which indexes were considered and why a particular plan was selected.
> db.CollectionName.explain("queryPlanner").find({ /* Your query */ })

# All Plan Execution
# This option provides information about the execution of all plans considered by the query planner. It can be useful for understanding why certain plans were rejected.
> db.CollectionName.explain("allPlansExecution").find({ /* Your query */ })

# Verbose Output
# This option provides a detailed breakdown of the execution plan, including index usage, index bounds, and more.
> db.CollectionName.explain("verbose").find({ /* Your query */ })
```



<br>

## [Collections Commands](#collectiona-basic-commands)
### Show Collections
```sh
> show Collections
```
### Create Collection
```sh
> db.createCollection("Collection Name")
```
### Update Collection Name
```sh
> db.CollectionName.renameCollection("New Name")
```
### Clean Collection
```sh
> db.CollectionName.deleteMany({})
```
### Delete Collection
```sh
> db.CollectionName.drop()
```

<br>

## [Documents Commands](#documents-basic-commands)
### Show All Documents
```sh
> db.CollectionName.find()
```
### Show All Documents Formated
```sh
> db.CollectionName.find().pretty()
```
### Include Only Specific Fields
```sh
> db.CollectionName.find({}, { field1: 1, field2: 1, _id: 0 })
```
### Show All Documents With Limited Number Condition
```sh
> db.CollectionName.find().limit(number).pretty()
```
### Skip Documents
```sh
# Skip number N Then return others
> db.CollectionName.find().skip(N) 
```
### Show All Documents Under Selected Conditions
```sh
# Condition {FieldX:ValueY}
> db.CollectionName.find({FieldX:ValueY})

# Arthimatic Operation
# Greater Then X 
> db.CollectionName.find({Fields : { $gt : X }})

# Greater Or Equal to X
> db.CollectionName.find({Fields : { $gt : X }})

# Less Then X
> db.CollectionName.find({Fields : { $gt : X }})

# Less Or Equal To X
> db.CollectionName.find({Fields : { $gt : X }})
```
### Show All Documents In Sorted Way
```sh
# asd
> db.CollectionName.find().sort({filedToBeSortWith:1})

# desc
> db.CollectionName.find().sort({filedToBeSortWith:-1})
```
### Show One Document
```sh
# Condition {FieldX:ValueY}
> db.CollectionName.findOne({FieldX:ValueY})
```
### Get Documents By Condition
```sh
> db.collection.find({ $or: [conditionA, conditionB] })
> db.collection.find({$and: [conditionA, conditionB]})
```
### Insert Document
```sh
# JsonData : {Key:Value}
> db.CollectionName.insert({...JsonData}) 
```
### Insert Multiple Documents
```sh
# JsonData : {Key:Value}
> db.CollectionName.insertMany([{...JsonData},{...JsonData},...])
```
### Count All Documents
```sh
> db.CollectionName.find().count()

# With Conditions
> db.CollectionName.find({filedToBeSortWith:1}).count()
```
### Delete Documents
```sh
# If There Is Multiple Documents With The Same Condition {FieldX:ValueY}
# Then All Documents Will Be Deleted
> db.CollectionName.remove({FieldX:ValueY})
```

<br>

## [Fields Commands](#fields-commands)
### Check If Field Exists
```sh
> db.CollectionName.find({ fieldName: { $exists: true } })
```
### Update Fields
```sh
# If There Is Multiple Documents With The Same Condition {FieldX:ValueY}
# Then Only First Documents Will Be Updated
> db.CollectionName.update({"FieldX:ValueY"}, {$set: {New Update}})
```
### Increment And Decement Int Fields
```sh
# X Is Number of Increment
> db.CollectionName.update({"FieldX:ValueY"},{$inc:{Int-Field:X}})

> db.CollectionName.update({"FieldX:ValueY"},{$inc:{Int-Field:-X}})
```
### Rename Field
```sh
> db.CollectionName.update({"FieldX:ValueY"},{$rename:{Old-Field-Name:New-Field-Name}})
```
