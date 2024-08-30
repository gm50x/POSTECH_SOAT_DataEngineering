# Mongodb

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

# Redis

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
