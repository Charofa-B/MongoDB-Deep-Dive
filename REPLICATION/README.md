# Data Replica In MongoDB

<br>

* ### [MongoDB Replica Set](#mongodb-replica-set)
* ### [Replica Set Setups](#replica-set-process)
* ### [Election Process](#election-process)
* ### [Configuring and Managing Replica Sets](#configuring-and-managing-replica-sets)
* ### [Add Server To Replica Sets](#add-server-to-replica-sets)
* ### [Change Primary](#change-primary)

<br><br>

## [MongoDB Replica Set](#mongodb-replica-set)
A replica set in MongoDB is a group of MongoDB servers that maintain the same data set. It provides redundancy and automatic failover to ensure high availability.
### Real Life Example
Suppose you have an e-commerce platform that relies on a MongoDB database to store product information, user data, and order history. To ensure that your platform remains available even if one of your database nodes fails, you decide to set up a MongoDB replica set.

<br>

## [Replica Set Setups](#replica-set-process)
A replica set consists of multiple MongoDB instances:
1. **Primary Node** : The primary node is the main node that receives all write operations. It's the only member of the replica set that can accept write operations.

2. **Secondary Nodes** : Secondary nodes replicate data from the primary node asynchronously. They can serve read operations, but they cannot accept write operations directly.

3. **Arbiter (Optional)** : An arbiter is a lightweight member of the replica set that participates in elections to break ties when determining the primary node. It does not store data and is typically used to achieve an odd number of votes in the replica set configuration.

### Data Synchronization
* MongoDB ensure that Data remains consistent across all nodes in the replica
* This sync is auto with primary node replication data to secondary nodes in real-time

### OpLog (Operation Log)
* The `OpLog` is cretical component for data sync
* It records all write operations executed on the primary node
* Secondary nodes `tail` the `OpLog` to `capture these operations and apply them maintain data consistency`

<br>

## [Election Process](#election-process)
1. **Triggering an Election** : An election is triggered when the primary node becomes unavailable or steps down

2. **Heartbeat Mechanism** : Replica set members regularly send "heartbeat" signals to each other to confirm that they are online and operational.

3. **Candidate Nodes** : When an election is triggered, eligible secondary nodes become candidates for the new primary. An eligible node is one that is up-to-date with the primary node's oplog (operation log).

4. **Voting Process** : 
* * Each node in the replica set has a vote, including the primary and secondary nodes. Arbiter nodes also participate in the voting but do not store data.
* * A node that wants to become the primary requests votes from the other nodes.
* * Each node evaluates the request based on several factors, including the candidate's oplog position (how current its data is) and the node's priority setting.

5. **Majority Vote**
To be elected as the new primary, a candidate must receive votes from a majority of the voting members of the replica set.

6. **Election Timeout**
If no candidate receives a majority of votes within a certain period, the election process starts over. Nodes continue to request votes until a primary is elected.

7. **New Primary**
* * Once a node receives a majority of votes, it becomes the new primary.
* * The new primary node begins accepting write operations, and the other nodes start replicating from it.

8. **Failover and Failback**
If the original primary node comes back online, it will join as a secondary node. It will not automatically become primary again unless another election is triggered.

<br>

## [Configuring and Managing Replica Sets](#configuring-and-managing-replica-sets)

1. Select the directory where MongoDB will store its data files.
```sh
> mkdir -p "/data/rs1" "/data/rs2" "/data/rs3"
```

2. Select the directory where MongoDB will store its logs messages.
```sh
> mkdir -p "/logs/rs1" "/logs/rs2" "/logs/rs3"
```

3. Start MongoDB Servers 
```sh
# Server 1
> mongod --replSet <Replica Name> --port <PortNumber 1> --dbpath "/data/rs1" --logpath "/logs/rs1" --logappend

# Server 2
> mongod --replSet <Replica Name> --port <PortNumber 2> --dbpath "/data/rs2" --logpath "/logs/rs2" --logappend

# Server 3
> mongod --replSet <Replica Name> --port <PortNumber 3> --dbpath "/data/rs3" --logpath "/logs/rs3" --logappend 

# --logappend :  tells MongoDB to append new log entries to the existing log file rather than overwriting it.
# is a good practice to ensure that previous log data is retained.
```

4. Start Mongo shell for selecting server And `initiate replica sets` 
```sh
# Start mongosh
> mongosh --port <PortNumber 1>
...

> rs.initiate({
  _id: "<Replica Name>",
  members: [
    { _id: 1, host: "host:<PortNumber 1>", priority: 2 },
    { _id: 2, host: "host:<PortNumber 2>", priority: 1 },
    { _id: 3, host: "host:<PortNumber 3>", priority: 1 }
  ]
});

# The Server With Heigher Periority Will Be The Primary
```

5. Verify the Replica Set Status
```sh
> rs.status();

# You ll recive full report about status and all servers and its periorities

# the command line will start with text like this if you are in primary server: 
# <Replica Name> [direct: primary] test> 

# Else you ll find it like this
# <Replica Name> [direct: secondary] test>
```

6. Track The Primary And Secondary
```sh
> rs.isMaster();
```

7. Read Data From Secondary
When run the command the `driver` or the `MongoDB query router` (if you are using sharding with mongos) will decide which secondary to use based on several factors.
```sh
> rs.slaveOk();
```

By setting up a replica set and understanding the replication process, you can ensure data redundancy, high availability, and fault tolerance for your MongoDB deployment.

<br>

## [Add Server To Replica Sets](#add-server-to-replica-sets)
```sh
#in mongosh
> rs.add("<host>:<PortNumber>")
```

<br>

## [Change Primary](#change-primary)
1. Step Down the Current Primary
```sh
# Connect to the primary node
> mongosh --port <Primary_Port>

> rs.stepDown()
```

2. Set Node Priorities
```sh
# Connect to any member of the replica set
> mongosh --port <Any_Member_Port>

# Modify the priorities, for example:
> cfg.members[0].priority = 1;
> cfg.members[1].priority = 2; # Increase to make this node more likely to be primary

# Reconfigure the replica set
> rs.reconfig(cfg);
```

3. Trigger a Re-election
```sh
# If still connected to the primary, run:
> rs.stepDown()

# Or connect to the new preferred node and check status
> mongosh --port <Preferred_Node_Port>
> rs.status()
```