# MongoDB Tools

<br><br>

* ### [Definition](#small-definition)
* ### [Export Data](#export-data)
* ### [Import Data](#import-data)
* ### [Set Backup Data](#dump-data)
* ### [Restore Data Using Backup](#restore-data-using-backup)
* ### [MongoStat](#mongostat)
* ### [MonogTop](#monogtop)
* ### [#Mongofiles](#mongofiles)

<br><br>

## [Export Data](#export-data)
### Export Data As Json Format
```sh
# If you dont Include --fields then you'll export all data fields
# Using Domain And Port:
> mongoexport --host=Hostname --port=Port-Number --db=DB-Name --type=json --fields=Field1,Field2,... --collection=Collection-name --out=PathName/filename.json

# Using Uri:
> mongoexport --uri="url-path" --db=DB-Name --type=json --collection=Collection-Name --out=PathName/filename.json
```
### Export Data As Json Array Format
```sh
# --pretty To make It More Readable
# Using Domain And Port:
> mongoexport --host=Hostname --port=Port-Number --db=DB-Name --type=json --collection=Collection-name --out=PathName/filename.json --jsonArray --pretty

# Using Domain And Port:
> mongoexport --uri="url-path" --db=DB-Name --type=json --collection=Collection-Name --out=PathName/filename.json --jsonArray --pretty
```
### Export Data As CSV
```sh
# --pretty To make It More Readable
# Using Domain And Port:
> mongoexport --host=Hostname --port=Port-Number --db=DB-Name --type=csv --collection=Collection-name --fields=Field1,Field2... --out=PathName/filename.csv

# Using Domain And Port:
> mongoexport --uri="url-path" --db=DB-Name --type=csv --collection=Collection-name --fields=Field1,Field2... --out=PathName/filename.csv
```

<br>

## [Import Data](#import-data)
```sh
# From Json File
> mongoimport --uri="url-path" --db=DB-Name --collection=Collection-name --file=PathName/filename.csv --jsonArray

#From Csv File
> mongoimport --uri="url-path" --db=DB-Name --collection=Collection-name --type=csv --file=PathName/filename.csv --headerline
```

<br>

## [Set Backup Data](#dump-data)
```sh
# Set Specific DB As Backup
> mongodump --uri="url-path" --db=DB-Name --out=PathName

# Set Specific DB As Copressed Backup
> mongodump --uri="url-path" --db=DB-Name --out=PathName --gzip

# Set All DB As Backup
> mongodump --uri="url-path" --out=PathName
```

## [Restore Data Using Backup](#restore-data-using-backup)
```sh
> mongorestore --uri="url-path" --db=DB-Name --drop --out=PathName

# Restore Data Using Compressed Backup
> mongorestore --uri="url-path" --db=DB-Name --drop --out=PathName --gzip
```

## [MongoStat](#mongostat)
provides a quick overview of the status and performance metrics of a MongoDB server. It displays real-time statistics about MongoDB's connections, operations, memory usage, and other important metrics.
```sh
> mongostat --host <hostname> --port <port> --username <username> --password <password> --authenticationDatabase <authDB>
```


## [MonogTop](#monogtop)
command-line utility used to track and report the read and write activity of a MongoDB instance at the collection level. It displays information about the amount of time (in milliseconds) spent reading and writing to each collection in a MongoDB database.
```sh
> mongotop --host <hostname> --port <port> --username <username> --password <password> --authenticationDatabase <authDB> <interval>
```


## [#Mongofiles](#mongofiles)
* Allows users to work with files stored in a MongoDB GridFS system. 
* GridFS is a specification used in MongoDB for storing and retrieving large files, such as images, videos, and documents, that exceed the BSON document size limit of 16MB.
### Uploading a File
```sh
> mongofiles put /path/to/local/file --host <hostname> --port <port> --username <username> --password <password> --authenticationDatabase <authDB>
```

### Listing Files
To list files stored in `GridFS`, you can use the list command.
```sh
> mongofiles list --host <hostname> --port <port> --username <username> --password <password> --authenticationDatabase <authDB>
```

### Downloading a File
```sh
> mongofiles get <filename> --host <hostname> --port <port> --username <username> --password <password> --authenticationDatabase <authDB>
```

### Deleting a File
```sh
> mongofiles delete <filename> --host <hostname> --port <port> --username <username> --password <password> --authenticationDatabase <authDB>
```