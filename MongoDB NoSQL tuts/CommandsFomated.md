# MongoDB Commands Cheat Sheet

```shell
# General Commands

## Show Databases
show dbs

## Switch to/Create Database
use <nameOfDATABASE>

# Create Operations

## Insert One Document
db.<nameOfCollection>.insertOne(data, options)

# Example:
db.testbooks.insertOne({
  "title": "ABC1",
  "author": "XYZ1",
  "pages": 600,
  "genre": ["Fantasy", "Drama"],
  "rating": 8
})

## Insert Many Documents
db.<nameOfCollection>.insertMany(data, options)

# Example:
db.testbooks.insertMany([
  {
    "title": "ABC1",
    "author": "XYZ1",
    "pages": 600,
    "genre": ["Fantasy", "Drama"],
    "rating": 8
  },
  {
    "title": "ABC2",
    "author": "XYZ2",
    "pages": 700,
    "genre": ["Sci-Fi", "Adventure"],
    "rating": 9
  }
])

# Read Operations

## Find All Documents
db.<nameOfCollection>.find()

## Find One Document
db.<nameOfCollection>.findOne(filter, options)

# Example:
db.testbooks.findOne({title: "ABC1"})

## Pretty Print All Documents
db.<nameOfCollection>.find().pretty()

## Find with Condition
db.<nameOfCollection>.find({<key>: <value>}).pretty()

# Example:
db.testbooks.find({rating: {$gt: 8}}).pretty()

# Update Operations

## Update One Document
db.<nameOfCollection>.updateOne(filter, update, options)

# Example:
db.testbooks.updateOne({title: "ABC1"}, {$set: {author: "New Author"}})

## Update Many Documents
db.<nameOfCollection>.updateMany(filter, update, options)

# Example:
db.testbooks.updateMany({}, {$set: {fbloggedin: "yes"}})

## Unset/Delete Fields
db.<nameOfCollection>.updateMany({}, {$unset: {<fieldName>: ""}})

# Example:
db.students.updateMany({}, {$unset: {thumbnails: ""}})

# Delete Operations

## Delete One Document
db.<nameOfCollection>.deleteOne(filter, options)

# Example:
db.testbooks.deleteOne({title: "ABC1"})

## Delete Many Documents
db.<nameOfCollection>.deleteMany(filter, options)

# Example:
db.testbooks.deleteMany({})

## Drop Database
use <nameOfDATABASE>
db.dropDatabase()

## Drop Collection
db.<nameOfCollection>.drop()

# Additional Operations

## Saving Bandwidth
# Instead of using find().pretty(), use selective querying to reduce bandwidth
db.students.find({}, {email: 1})

# Example:
db.students.find({}, {email: 1, _id: 0, name: 1})

# Converting Results to Array
db.students.find({}, {<key>: 1, _id: 0}).toArray()

# Example:
db.students.find({}, {email: 1, _id: 0, name: 1}).toArray()

# Finding Nested Objects
db.oldstudents.find({"thumbnails.small": 50})

# Updating Nested Objects
db.oldstudents.updateOne({name: 'Hitesh'}, {$set: {"thumbnails.large": 500}})

# Updating Arrays
db.oldstudents.updateOne({}, {$set: {lastlogin: ["Mon", "Tue", "Wed", "Thurs", "Fri", "Sat"]}})

# Sorting
db.<nameOfCollection>.find().sort(<key>: 1/-1)

# Example:
db.students.find().sort({name: 1})

# Counting Documents
db.<nameOfCollection>.find().count()

# Example:
db.students.find().count()

# Limiting Results
db.<nameOfCollection>.find().limit(#Num)

# Example:
db.students.find().limit(4)

# Serial Operations (Sorting and Limiting)
db.students.find().sort({name: 1}).limit(4)

# Relations in MongoDB

# One to One Example
use youtube
db.users.insertOne({
  name: "Tharki",
  verified: "true",
  earnings: 10000
})
db.video.insertOne({
  topic:"Sexual Abuse",
  len: 4,
  creator: ObjectId('66a7e6235d561327fbb04bbf')
})

# Accessing Related Object
var vidUID = db.video.findOne().creator
db.users.findOne({_id: vidUID})

# One to Many Example
db.users.updateMany({}, {$set: {videos: ["Indian Porn", "Desi Porn", "MILFs"]}})

# Other Operations Example
db.comment.updateOne({replies: ["rep1", "rep2", "rep3"]}, {$set: {replies: [{_id: "rep1", text: "xyz 1"}, {_id: "rep2", text: "XYZ 2"}]}})















# MongoDB Commands Cheat Sheet

```shell
# General Commands

## Show Databases
show dbs

## Switch to/Create Database
use <nameOfDATABASE>

# Create Operations

## Insert One Document
db.<nameOfCollection>.insertOne(data, options)

### Example:
db.testbooks.insertOne({
  "title": "ABC1",
  "author": "XYZ1",
  "pages": 600,
  "genre": ["Fantasy", "Drama"],
  "rating": 8
})

## Insert Many Documents
db.<nameOfCollection>.insertMany(data, options)

### Example:
db.testbooks.insertMany([
  {
    "title": "ABC1",
    "author": "XYZ1",
    "pages": 600,
    "genre": ["Fantasy", "Drama"],
    "rating": 8
  },
  {
    "title": "ABC2",
    "author": "XYZ2",
    "pages": 700,
    "genre": ["Sci-Fi", "Adventure"],
    "rating": 9
  }
])

# Read Operations

## Find All Documents
db.<nameOfCollection>.find()

## Find One Document
db.<nameOfCollection>.findOne(filter, options)

### Example:
db.testbooks.findOne({title: "ABC1"})

## Pretty Print All Documents
db.<nameOfCollection>.find().pretty()

## Find with Condition
db.<nameOfCollection>.find({<key>: <value>}).pretty()

### Example:
db.testbooks.find({rating: {$gt: 8}}).pretty()

# Update Operations

## Update One Document
db.<nameOfCollection>.updateOne(filter, update, options)

### Example:
db.testbooks.updateOne({title: "ABC1"}, {$set: {author: "New Author"}})

## Update Many Documents
db.<nameOfCollection>.updateMany(filter, update, options)

### Example:
db.testbooks.updateMany({}, {$set: {fbloggedin: "yes"}})

## Unset/Delete Fields
db.<nameOfCollection>.updateMany({}, {$unset: {<fieldName>: ""}})

### Example:
db.students.updateMany({}, {$unset: {thumbnails: ""}})

# Delete Operations

## Delete One Document
db.<nameOfCollection>.deleteOne(filter, options)

### Example:
db.testbooks.deleteOne({title: "ABC1"})

## Delete Many Documents
db.<nameOfCollection>.deleteMany(filter, options)

### Example:
db.testbooks.deleteMany({})

## Drop Database
use <nameOfDATABASE>
db.dropDatabase()

## Drop Collection
db.<nameOfCollection>.drop()

# Additional Operations

## Saving Bandwidth
# Instead of using find().pretty(), use selective querying to reduce bandwidth
db.students.find({}, {email: 1})

### Example:
db.students.find({}, {email: 1, _id: 0, name: 1})

## Converting Results to Array
db.students.find({}, {<key>: 1, _id: 0}).toArray()

### Example:
db.students.find({}, {email: 1, _id: 0, name: 1}).toArray()

## Finding Nested Objects
db.oldstudents.find({"thumbnails.small": 50})

## Updating Nested Objects
db.oldstudents.updateOne({name: 'Hitesh'}, {$set: {"thumbnails.large": 500}})

## Updating Arrays
db.oldstudents.updateOne({}, {$set: {lastlogin: ["Mon", "Tue", "Wed", "Thurs", "Fri", "Sat"]}})

## Sorting
db.<nameOfCollection>.find().sort({<key>: 1/-1})

### Example:
db.students.find().sort({name: 1})

## Counting Documents
db.<nameOfCollection>.find().count()

### Example:
db.students.find().count()

## Limiting Results
db.<nameOfCollection>.find().limit(#Num)

### Example:
db.students.find().limit(4)

## Serial Operations (Sorting and Limiting)
db.students.find().sort({name: 1}).limit(4)

## Relations in MongoDB

### One to One Example
use youtube
db.users.insertOne({
  name: "Tharki",
  verified: "true",
  earnings: 10000
})
db.video.insertOne({
  topic:"Sexual Abuse",
  len: 4,
  creator: ObjectId('66a7e6235d561327fbb04bbf')
})

### Accessing Related Object
var vidUID = db.video.findOne().creator
db.users.findOne({_id: vidUID})

### One to Many Example
db.users.updateMany({}, {$set: {videos: ["Indian Porn", "Desi Porn", "MILFs"]}})

### Other Operations Example
db.comment.updateOne({replies: ["rep1", "rep2", "rep3"]}, {$set: {replies: [{_id: "rep1", text: "xyz 1"}, {_id: "rep2", text: "XYZ 2"}]}})

# Additional Important Commands

## Aggregation Framework
# Perform complex aggregation operations
db.<nameOfCollection>.aggregate(pipeline)

### Example:
db.orders.aggregate([
  { $match: { status: "A" } },
  { $group: { _id: "$cust_id", total: { $sum: "$amount" } } }
])

## Indexing
# Improve query performance by indexing fields
db.<nameOfCollection>.createIndex(keys, options)

### Example:
db.products.createIndex({ sku: 1 })

## Text Search
# Perform full-text search on indexed fields
db.<nameOfCollection>.createIndex({<key>: "text"})

### Example:
db.articles.find({ $text: { $search: "coffee" } })

## Geospatial Queries
# Query based on geographic location
db.<nameOfCollection>.find({
  location: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [ <longitude>, <latitude> ]
      },
      $maxDistance: <distance in meters>
    }
  }
})

## Backup and Restore
# Backup and restore databases and collections
mongodump --db <nameOfDATABASE>
mongorestore --db <nameOfDATABASE> <path_to_backup_folder>

## Transactions (for Replica Sets)
# Perform operations within transactions
session.startTransaction()
db.<collection>.insertOne(document, { session: session })
session.commitTransaction()

## Sharding
# Distribute data across multiple machines to improve performance and capacity
sh.enableSharding("<nameOfDATABASE>")

## Change Streams
# Monitor changes in the database in real-time
const changeStream = db.<nameOfCollection>.watch()

## List Collections
# View all collections in the current database
show collections

