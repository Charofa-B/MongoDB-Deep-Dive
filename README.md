# Architecture And Overview

<br><br>

* ### [MongoDB Architecture](#mongo-architecture)
* ### [Data Types](#data-type)

<br><br>

## [MongoDB Architecture](#mongo-architecture)

<br>

MongoDB is a popular NoSQL database that uses a flexible, document-oriented data model. Its architecture is designed to provide scalability, flexibility, and high performance.

<img src='./MongoDB-Architecture.png' width=400 height=400 />

### Document Model 
MongoDB stores data in flexible, JSON-like documents we call it `BSON`. These documents can have varying structures, which means each record in a collection (similar to a table in relational databases) does not need to have the same schema.
### Driver & Storage Engine
#### **Driver**
MongoDB drivers are client libraries that enable applications to interact with MongoDB databases. These drivers provide APIs and tools for developers to connect to MongoDB, perform database operations, and handle data in their preferred programming language.
#### **Storage Engine**
MongoDB's storage engine is responsible for managing data storage and retrieval on disk.MongoDB offers pluggable storage engines, allowing users to choose the one that best suits their workload requirements. The default storage engine for MongoDB is `WiredTiger`

InMemory Instead of storing documents on disk, the engine uses in-memory for more predictable data latencies. It uses 50% of physical RAM minimum 1 GB as default. It requires all its data. When dealing with large datasets, the in-memory engine may not be the most suitable choice.

#### **Server**
The MongoDB server acts as the heart of the system. Each instance of the MongoDB server, known as `mongod`, handles client requests, manages data storage, and performs various database operations.

#### **Data Storage in MongoDB**
* **Collections** : Represents Group of documents.

* **Documents** : Represents individual Records hold the data as `BSON` data a `Json like type`.

<br><br>

## [Data Types](#data-type)
* **String**: UTF-8 encoded strings.
* **Integer**: 32-bit signed integers.
* **Double**: 64-bit floating-point numbers.
* **Boolean**: Boolean value (true or false).
* **ObjectId**: Unique identifier consisting of a 12-byte hexadecimal string.
* **Date**: Date and time value.
* **Array**: List of values.
* **Object**: Embedded documents.
* **Null**: Null value.
* **Regular Expression**: Regular expression pattern.
* **Binary Data**: Binary data in various formats (e.g., generic, function, UUID).
* **Timestamp**: Timestamp for operations within a MongoDB cluster.
* **Decimal128**: 128-bit decimal floating-point number.