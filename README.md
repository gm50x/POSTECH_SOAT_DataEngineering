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

