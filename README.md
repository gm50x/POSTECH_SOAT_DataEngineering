# Mongodb - Document

```bash
# Connect to mongodb
mongosh --host {host} --port {port} -u {username} -p {password}
# change database:
use {DatabaseName}
# create collection:
db.createCollection("{CollectionName}")
# insert an object into a colleciton
db.boletos.insertOne({
  "customer": "John Doe",
  "due_date": "2023-05-15",
  "payment_date": "2023-05-14",
  "amount_paid": 250.75,
  "status": "Paid",
  "payment_method": "Credit Card",
  "invoice_number": "INV-123456",
  "description": "Monthly subscription",
  "notes": "Payment made on time",
  "items": [
    {
      "product": "Subscription",
      "quantity": 1,
      "unit_price": 250.75
    }
  ]
})
db.boletos.insertOne({
  "customer": "Jane Doe",
  "due_date": "2023-05-15",
  "payment_date": "2023-05-14",
  "amount_paid": 250.75,
  "status": "Paid",
  "payment_method": "Credit Card",
  "invoice_number": "INV-123457",
  "description": "Monthly subscription",
  "notes": "Payment made on time",
  "items": [
    {
      "product": "Subscription",
      "quantity": 1,
      "unit_price": 129.75
    }
  ]
})
# Querying...
db.boletos.find()
db.boletos.find({ customer: { $eq: "John Doe" }})
db.boletos.find({ amount_paid: { $lte: 150.00 }})

```

# Redis - Key Value

```bash
# Connecting to redis
docker exec -it redis /bin/sh
redis-cli -a {password}

# Shows databases there will be 16 dbs by default
CONFIG GET databases

# Diagnostico do redis
INFO keyspace

# Changing databases (there are 16 dbs by default)
SELECT 0
SELECT 1
# ...remaining selects

# Show all keys
KEYS *

# Show keys in namespace
KEYS {namespace}:*

# Setting a key-value
SET {keyname} {keyvalue}

# Getting the value from a key
GET {keyname}

# Seting a key value with expiration
SETEX {keyname} {secondsToExpire} {keyvalue}

# Deleting a key
DEL {keyname}

# Get Multiple Keys
MGET {key1} {key2}

# Type of a key
TYPE {keyname}

# Get TTL for a key
TTL {keyname}

# Create a list
RPUSH {keyname} "item1" "item2" "item3"

# Get a range in a list
LRANGE {keyname} {position-start} {position-end}


# Namespaces, just prefix your keyname with the namespace
SET {namespace}:{subnamespace}:{keyname}

# Commands that did not work but are mentioned
SMEMBERS
ZRANGE


# this did not work
# Seting a key value with expiration at given unix timestamp
SETEXAT {keyname} {unixTimestamp} {keyvalue}
```

# Cassandra - Columnar
```bash
# Connecting to Cassandra
cqlsh -u {user} -p {password}

# Describe Keyspaces
DESC keyspaces
DESCRIBE keyspaces

# Create a Keyspace
CREATE KEYSPACE {keyspacename} 
WITH replication = {
  'class': '{SimpleStrategy | NetworkTopologyStrategy}',
  'replication_factor': {integer}
};

# Setup keyspace
USE {keyspacename}

# Create a table (similar to SQL)
CREATE TABLE {tablename} (
  <col> <type>,
  <otherCol> <type>,
  ...
  PRIMARY KEY (key column)
);

CREATE TABLE hello_cassandra (
  id INT,
  name TEXT,
  PRIMARY KEY (id)
);

# Check tables
DESC tables;

# Insert a single record
INSERT INTO hello_cassandra (
  id, name
) VALUES ( 1, 'Jack Sparrow' );

# Insert multiple records
BEGIN BATCH
  INSERT INTO hello_cassandra (id, name) VALUES ( 2, 'David Jones' );
  INSERT INTO hello_cassandra (id, name) VALUES ( 3, 'Hector Barbossa' );
APPLY BATCH;


# QUERYING...
SELECT * FROM hello_cassandra;
SELECT * FROM hello_cassandra WHERE id = 1;

# Update a record
UPDATE hello_cassandra SET name = 'William Turner' WHERE id = 1;

# Deleting a record...
DELETE FROM hello_cassandra WHERE id = 1;

# Example
CREATE KEYSPACE ecommerce
WITH replication = {
  'class': 'SimpleStrategy',
  'replication_factor': 4
};

CREATE TABLE charges (
  customer_id UUID,
  charge_id UUID,
  amount DECIMAL,
  created_at TIMESTAMP,
  status TEXT,
  coupon TEXT,
  PRIMARY KEY (customer_id, charge_id)
);

BEGIN BATCH
  INSERT INTO charges (customer_id, charge_id, amount, created_at, status, coupon) 
  VALUES ( e5b52892-9911-431a-a807-1643d0bf0c67, 29451f30-ad94-4334-80c8-cba4c1a40da1, 1056.00, toUnixTimestamp(now()), 'ACTIVE', '#blackfriday');
  INSERT INTO charges (customer_id, charge_id, amount, created_at, status) 
  VALUES ( b42fdaad-9868-4d5f-90d0-e54e8b5ffb2a, 277e9eef-c821-403c-a02a-b55e9d41a7a2, 943.56, toUnixTimestamp(now()), 'ACTIVE');
  INSERT INTO charges (customer_id, charge_id, amount, created_at, status, coupon) 
  VALUES ( e42397c1-e581-423e-a3b1-0a472cfe3977, f4b30186-f551-4043-ad8b-cd73ad223b64, 211.10, toUnixTimestamp(now()), 'CANCELLED', '#compremais' );
  INSERT INTO charges (customer_id, charge_id, amount, created_at, status, coupon) 
  VALUES ( ee869ecf-1136-493a-a84e-821c40fa74a5, a0b3d797-ed2d-4f5f-b729-3826c1a54dcd, 2259.99, toUnixTimestamp(now()), 'ACTIVE', '#compremais' );
  INSERT INTO charges (customer_id, charge_id, amount, created_at, status) 
  VALUES ( e5b52892-9911-431a-a807-1643d0bf0c67, aa67fe3f-4a52-40b0-9cc0-a9581caf8c95, 98.90, toUnixTimestamp(now()), 'ACTIVE');
APPLY BATCH;

```

# Neo4J - Graph

```bash
# navigate to localhost:7474 and login with user neo4j and pass fiap@neo4j

# See databases
SHOW DATABASE

# Creating a new Database (Does not work on community)
CREATE DATABASE {name}

# Changing database
:use {database name}

# Creating a Node
CREATE (:label {prop:value,otherprod:othervalue})

# Creating a related Nodes
CREATE(:label {prop:value}) -[:relationship]->(:label2 {prop2:value2})

# Relating nodes
MATCH(n:label), (m:label) WHERE n.prop = value AND m.prop = value CREATE (n)-[:relationship]->(m)_

# Updating a node
MATCH(:Label{prop:value}) SET n.prop2 = value;

# Deleting nodes
MATCH(n:label{prop:value}) DETACH DELETE n

# Delete all nodes
MATCH(n) DETACH DELETE n

# Insert multiple nodes and relationships
CREATE 
  (n:label:{prop:value}),
  (m:label:{prop:value}),
  (n:label)-[:relationship]->(m:label),
  (n:label)-[:other-relationship]->(m:label)

# Multiple Relations at once
MATCH (n:label)(m:label) WHERE n.prop = value AND m.prop = value

# Constraining
CREATE CONSTRAINT ON (n:label) ASSERT n.prop IS UNIQUE
CREATE CONSTRAINT {constriant_name} FOR (n:label) REQUIRE n.prop IS UNIQUE

# Indexing
CREATE INDEX {name} FOR (n:label) ON (n.prop)


# Example
CREATE (n:Pessoa{name: 'Luke Skywalker'})
CREATE (n:Pessoa{name: 'Anakin Skywalker'})

MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Anakin Skywalker' and m.name = 'Luke Skywalker' CREATE (n) -[:FatherOf]-> (m)

MATCH(n:pessoa{name: 'Luke Skywalker'}) SET n.age = 19
CREATE (n:Pessoa{name: 'Obiwan Kenobi'})-[:Teaches]->(m:Pessoa {name: 'Padmé Amidala'})
CREATE (n:Pessoa{name: 'Yoda'})-[:BornIn]->(m:Planet {name: 'Dagobah'})
CREATE (n:Pessoa{name: 'Rey'})

MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Yoda' and m.name = 'Luke Skywalker' CREATE (n) -[:Teaches]-> (m)
MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Yoda' and m.name = 'Obiwan Kenobi' CREATE (n) -[:Teaches]-> (m)
MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Anakin Skywalker' and m.name = 'Padmé Amidala' CREATE (n) -[:MariedTo]-> (m)
MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Padmé Amidala' and m.name = 'Luke Skywalker' CREATE (n) -[:MotherOf]-> (m)
MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Obiwan Kenobi' and m.name = 'Luke Skywalker' CREATE (n) -[:Teaches]-> (m)
MATCH(n:Pessoa),(m:Pessoa) WHERE n.name = 'Luke Skywalker' and m.name = 'Rey' CREATE (n) -[:Teaches]-> (m)

# Querying...
MATCH(n:Pessoa) WHERE n.age is null return n;
MATCH(n:Pessoa) WHERE n.age = 19 return n;

# Querying by relationship
MATCH(n:Pessoa)-[:Teaches]->(m:Pessoa) return n,m
MATCH(n:Pessoa)-[:Teaches]->(m:Pessoa)-[:MotherOf]->(o:Pessoa) return n,m,o

# Querying by path e.g:(Yoda Teaches Obiwan who Luke who teaches Rey) *3  means Three Levels
MATCH path = (n:Pessoa)-[:Teaches*2]->(m:Pessoa) return path
MATCH path = (n:Pessoa)-[:Teaches*3]->(m:Pessoa) return path

```