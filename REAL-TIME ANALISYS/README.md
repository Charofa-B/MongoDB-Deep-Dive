# Real-Time Ingestion and Processing

<br>

* MongoDB allows continuous collection and `analysis of real-time data`.
* Supports `change streams` to `monitor` data changes (inserts, updates, deletes).
* Tools like `Apache Kafka` or Pulsar can feed real-time data into MongoDB, acting as pipelines.
* MongoDB can store and process data received via `WebSocket`.
* Flexible `schemas` help structure the database according to application needs, suitable for real-time data.

<br>

## Real-Time Analytics Code With Python
For this example, we'll use a collection named `orders` to simulate an `e-commerce` scenario where new orders are placed.

```py
import asyncio
from pymongo import MongoClient
from pymongo.errors import PyMongoError
import datetime

client = MongoClient('mongodb://localhost:27017/')
db = client['ecommerce']
collection = db['orders']

async def process_change(change):
    # Extract information from the change event
    operation_type = change['operationType']
    full_document = change.get('fullDocument', {})
    print(f"Operation Type: {operation_type}, Document: {full_document}")

    if operation_type == 'insert':
        # Perform real-time analytics, e.g., calculate total sales
        order_amount = full_document.get('amount', 0)
        print(f"New order received. Amount: ${order_amount}")
        
        # Update the real-time total sales in another collection
        db['analytics'].update_one(
            {"_id": "total_sales"},
            {"$inc": {"total": order_amount}},
            upsert=True
        )

    async def monitor_changes():
    try:
        async with client.start_session() as session:
            async for change in collection.watch(session=session):
                await process_change(change)
    except PyMongoError as e:
        print(f"Error watching changes: {e}")


asyncio.run(monitor_changes())

```