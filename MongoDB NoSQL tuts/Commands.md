# MongoDB Commands with Examples

```shell
# General Commands

// MongoDB stores data as JSON (Java Script Object Notation) objects, which are stored as BSON (Binary JSON) objects in the database, with the engine being used as WiredTiger at the MONGODB backend

// Allows us to do CRUD (Create, Read, Update, Delete) operations on the database
// MongoDB is schema-less, which means that we can insert any type of data into the database, without having to define the schema of the data beforehand
// MongoDB is a NoSQL database, which means that it does not use SQL (Structured Query Language) to interact with the database, but uses its own query language to interact with the database
// Triple As of MongoDB: Application, Analytics and Administration


// If not in MongoDB Compass, then open the terminal and type mongosh to enter the MongoDB shell
mongosh (Enter)


// to show the databases
show dbs 



// switching to a particular database/creating a new database
use <nameOfDATABASE>




// CREATE


SYNTAX
db.<nameOfDATABASE>.insertOne(data, options)
db.<nameOfDATABASE>.insertMany(data, options)


// inserting a single object into the switched database
db.insertOne({
    "name": "Hitesh",
	"email": "hitesh@hiteshchoudhary.com",
	"contact": "9999999999",
	"courseCount": 4,
	"isVerified": true
    }
)
also works with
db.<nameOfDATABASE>.insertOne({})
Ex:
db.testbooks.insertOne({
  "title": "ABC1",
  "author": "XYZ1",
  "pages": 600,
  "genre": [
    "Fantasy",
    "Drama"
  ],
  "rating": 8
})


// Inserting many objects into the database
db.<nameOfDATABASE>.insertMany([])
Ex:
db.insertMany([
    {
        "name": "Hitesh",
        "email": "asjnsa@gmail.com",
        }
        ])
db.authors.insertMany([{name: "Author1", age: 20, awards: ["Manbooker", "Hare raam"]}, {name:"Author2", age:21}])

Also works with
db.<nameOfDATABASE>.insertMany([])

// Each time an insertion is done.. an "_id" string is generated for each object, which is unique for each object
// If you want to insert your own "_id" string, then you can do so by adding "_id" key in the object




// READ


SYNTAX
db.<nameOfDATABASE>.find(filter, options)
db.<nameOfDATABASE>.findOne(filter, options)

// To view all the inserted objects
db.<nameOfDATABASE>.find()

// To view a particular object in the database
db.<nameOfDATABASE>.findOne({<key>: <value>})
Ex:
db.testbooks.find({title: "ABC1"}).pretty()

// To view all the inserted objects in a formatted way
db.<nameOfDATABASE>.find().pretty()
// The above command gives only some JSON data
Type "it" to view the next set of JSON data

// forEach() is used to view all the objects in the database

// find() is not actually used in production, as it is not efficient
// so we use db.<collectionName>.find().forEach( (<varName> ) => { printjson(<varName>) } )
Ex:
db.students.find().forEach( (student) => { printjson(student) } )

// For viewing only a particular key in the objects
// db.<collectionName>.find().forEach( (<varName> ) => { printjson(<varName>.<key>) } )
Ex:
db.students.find().forEach((student) => (printjson(student.email)))

// Querying an array
db.<nameOfDATABASE>.find({<key>: [<value1>, <value2>]})
Ex:
db.users.find({videos: ["magic", "comedy"]})

//Querying a field in an array
db.<nameOfDATABASE>.find({<key>: <value1 in array>})
Ex:
db.users.find({videos: "Indian Porn"})

// Greater than filter {$gt: <value>}
db.<nameOfDATABASE>.find({<key>: {$gt: <value>}})
Ex:
db.testbooks.find({rating: {$gt: 8}}).pretty()

// Less than filter {$lt: <value>}
db.<nameOfDATABASE>.find({<key>: {$lt: <value>}})
Ex:
db.testbooks.find({rating: {$lt: 9}}).pretty()
// Less than or equal to filter {$lte: <value>}
// Greater than or equal to filter {$gte: <value>}
Ex:
db.oldstudents.find({"thumbnails.small": {$gte : 10}})
// Not equal to filter {$ne: <value>}
// In filter {$in: [<value1>, <value2>]}
// Not in filter {$nin: [<value1>, <value2>]}
// And filter {$and: [{<key1>: <value1>}, {<key2>: <value2>}]}
// Or filter {$or: [{<key1>: <value1>}, {<key2>: <value2>}]}
// Not filter {$not: {<key>: <value>}}
// Exists filter {$exists: <boolean>}
// Type filter {$type: <type>}
// Modulus filter {$mod: [<divisor>, <remainder>]}
// Regex filter {$regex: /pattern/}
// Text filter {$text: {$search: <string>}}
// Where filter {$where: <js code>}
// Nor filter {$nor: [{<key1>: <value1>}, {<key2>: <value2>}]}
// Size filter {$size: <number>}
// All filter {$all: [<value1>, <value2>]}
Ex:
db.users.find({videos: {$all : ["Indian Porn", "Desi Porn"]}})

// UPDATE


SYNTAX
db.<nameOfDATABASE>.updateOne(filter, update, options)
db.<nameOfDATABASE>.updateMany(filter, update, options)
db.<nameOfDATABASE>.replaceOne(filter, replacement, options)

// To update a particular object in the database
db.<nameOfDATABASE>.updateOne({<key>: <value>}, {<key>: <value>})
Ex:
db.testbooks.updateOne({title: "ABC1", rating: 8}, {$set: {title: "Objectifying Homies", author: "Master Faggot"}})

// To update all the objects in the database
db.<nameOfDATABASE>.updateMany({}, {<key>: <value>})
Ex:
db.testbooks.updateMany({} ,{$set: {fbloggedin: "yes"}})

// To unset/delete the fields
Ex:
db.students.updateMany({}, {$unset: {thumbnails: ""}})

// To update a field with arrays/objects
db.<nameOfDATABASE>.updateMany({}, {$set: {<fieldname>: { <keys>: values } }}
Ex:
db.oldstudents.updateMany({}, {$set: { thumbnails: { small: 50, medium: 100, large: 200 } }})

// To increment values (to decrement use # -no )
db.<nameOfDATABASE>.updateMany({}, {$inc: {<fieldname>: #value_to_incre } }}
Ex:
db.oldstudents.updateMany({}, {$inc: {courseCount: 2}})

// To remove a field or a value
Ex:
db.oldstudents.updateOne({_id: ObjectId('66a7998c1e4387f6028ca827')}, {$pull :{lastlogin: "Sat"}})

// To push a value into an array/ create a new  field and then push
Ex:
db.oldstudents.updateOne({_id: ObjectId('66a7998c1e4387f6028ca827')}, {$push :{lastlogin: "Sat"}})

// To push multiple values into a new/old field
Ex:
db.oldstudents.updateOne({_id: ObjectId('66a7998c1e4387f6028ca827')}, {$push :{weekoff: {$each: ["Sat", "Sun"]}}})




// DELETE

SYNTAX
db.<nameOfDATABASE>.deleteOne(filter, options)
db.<nameOfDATABASE>.deleteMany(filter, options)

// To delete all the objects in the database
db.<nameOfDATABASE>.deleteMany({})
Ex:
db.testbooks.deleteMany({}, {marked: "deleted"})

// To delete a particular object in the database
db.<nameOfDATABASE>.deleteOne({<key>: <value>})
Ex:
db.testbooks.deleteOne({
title: 
"adbbjabdjabd"})

// To unset/delete the fields
Ex:
db.students.updateMany({}, {$unset: {thumbnails: ""}})

// Deleting the whole database
show dbs
use <nameOfDATABASE>
db.dropDatabase()

// Deleting a particular collection
db.<nameOfCollection>.drop()



// Saving Bandwidth of querying in MongoDB

Use of find() is cost expensive because it dumps all the information from the database collection without the meta information
basically, causing burden on the bandwidth of MongoDB, so it reduce this we can either store it in an array or gives the specific information from the database
Ex:
// Instead of db.students.find().forEach((student) => (printjson(student.email)))

// A faster and efficient way to do this is
db.students.find({}, {email: 1})
Outputs:
{
  _id: ObjectId('66a78ac21e4387f6028ca813'),
  email: 'theodore@example.com'
}

// Selective Parameter querying
db.students.find({}, {email: 1, _id: 0, name: 1})
Outputs:
{
  name: 'Mark',
  email: 'mark@example.com'
}

// Converting it to Array
db.students.find({}, {<key>: 1, _id: 0}).toArray()
Ex:
db.students.find({}, {email: 1, _id: 0, name: 1}).toArray()
Outputs:
[
  { name: 'Hitesh', email: 'hitesh@hiteshchoudhary.com' },
  { name: 'Mark', email: 'mark@example.com' },
  .......
  { name: 'Alexis', email: 'alexis@example.com' },
  { name: 'Ford', email: 'ford@example.com' }
]

// To find the objects of objects with a particular key
db.oldstudents.find({"thumbnails.small": 50})

// To update the objects of objects with a particular key
db.oldstudents.updateOne({name: 'Hitesh'}, {$set: {"thumbnails.large": 500}})

// Updates an array of values to a particular key fieldname
db.oldstudents.updateOne({}, {$set: {lastlogin: ["Mon", "Tue", "Wed","Thurs", "Fri","Sat"]}})

// To find the objects of array with a particular key field
db.oldstudents.findOne({name: "Mark"}).lastlogin
[ 'Mon', 'Tue', 'Wed', 'Thurs', 'Fri', 'Sat' ]



//MongoDB Schema (Structure/Arrangement of the data)

MySQL has a rigid schema(tables) and MongoDB has a flexible(not completely rigid) schema(JSON)

// Database Modelling

It is about how we structure our data in the database
1. Predefined Database (Login info, etc)
2. Generated/Dynamic Database (User generated data, etc)
3. Where will we use the data (Application, Analytics, Administration)
4. How much filters/querying will be done on the data (Indexing)
5. How many queries can be fired on the data (requests made by the user)
6. How often will the data be updated/changed (CRUD operations)


// Relations in MongoDB

1. One to One
2. One to Many
3. Many to Many


// One to One

use youtube

Ex:
db.users.insertOne({
  name: "Tharki",
  verified: "true",
  earnings: 10000
})

The above user has ObjectID: 66a7e6235d561327fbb04bbf

db.video.insertOne({
  topic:"Sexual Abuse",
  len: 4,
  creator: ObjectId('66a7e6235d561327fbb04bbf')
})
So now videos is linked to the user with the ObjectID: 66a7e6235d561327fbb04bbf,so it leads to the user metadata 

// Accessing the Object _id
db.video.findOne().creator
ObjectId('66a7e6235d561327fbb04bbf')

// Store the Object _id in a variable
var vidUID = db.video.findOne().creator

// To find the user with the ObjectID
db.users.findOne({_id: vidUID})

The above shows 1-1 relation between the user and the video


// One to Many

Suppose the user has multiple videos, then we can use the One to Many relation
db.users.updateMany({}, {$set: {videos: ["Indian Porn", "Desi Porn", "MILFs"]}})
This could be of help

db.comment.updateOne({replies:  [
    "rep1",
    "rep2",
    "rep3"
  ]}, {$set: {replies: [{_id: "rep1", text: "xyz 1"}, {_id: "rep2", text: "XYZ 2"}]}})

// Other operations

// Sorting
db.<nameOfCollection>.find().sort(<key>: 1/-1)
1 - ascending -1 - descending
Ex:
db.students.find().sort({name: 1})

// Count 
db.<nameOfCollection>.find().count()
Ex:
db.students.find().count()

// Limit
db.<nameOfCollection>.find().limit(#Num)
Ex:
db.students.find().limit(4)

// Can be used in serial also
Ex:
db.students.find().sort({name: 1}).limit(4)

// INDEXES

// Basically MongoDB has to scan through the entire document to find a particular field value as in the case of find({key: value}), This can be found using
db.<nameOfDATABASE>.find({<key>: <value>}).explain('executionStats')
" totalDocsExamined: #X" this tells u the no of JSON's scanned

Ex:
db.oldstudents.find({isVerified: true}).explain('executionStats')
Output: Here, it is totalDocsExamined: 2, which means that 2 JSON's were scanned to find the value
{
  explainVersion: '1',
  queryPlanner: {
    namespace: 'test1.oldstudents',
    indexFilterSet: false,
    parsedQuery: {
      isVerified: {
        '$eq': true
      }
    },
    queryHash: 'CF3BDA84',
    planCacheKey: 'CF3BDA84',
    maxIndexedOrSolutionsReached: false,
    maxIndexedAndSolutionsReached: false,
    maxScansToExplodeReached: false,
    winningPlan: {
      stage: 'COLLSCAN',
      filter: {
        isVerified: {
          '$eq': true
        }
      },
      direction: 'forward'
    },
    rejectedPlans: []
  },
  executionStats: {
    executionSuccess: true,
    nReturned: 1,
    executionTimeMillis: 0,
    totalKeysExamined: 0,
    totalDocsExamined: 2,
    executionStages: {
      stage: 'COLLSCAN',
      filter: {
        isVerified: {
          '$eq': true
        }
      },
      nReturned: 1,
      executionTimeMillisEstimate: 0,
      works: 3,
      advanced: 1,
      needTime: 1,
      needYield: 0,
      saveState: 0,
      restoreState: 0,
      isEOF: 1,
      direction: 'forward',
      docsExamined: 2
    }
  },
  command: {
    find: 'oldstudents',
    filter: {
      isVerified: true
    },
    '$db': 'test1'
  },
  serverInfo: {
    host: 'DESKTOP-5GABVHC',
    port: 27017,
    version: '7.0.12',
    gitVersion: 'b6513ce0781db6818e24619e8a461eae90bc94fc'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted'
  },
  ok: 1
}

// So a better alternative is to create an index of the field, so that the scanning is reduced, and the value is found faster
db.<nameOfDATABASE>.createIndex({<key>: 1/-1})
Ex:
db.students.createIndex({courseCount: 5})

// The below code gets u the list of indexes
db.<nameOfCollection>.getIndexes()
Ex:
db.students.getIndexes()

// To drop an index
db.<nameOfCollection>.dropIndex({<key>: 1/-1})

// To drop all the indexes
db.<nameOfCollection>.dropIndexes()

// To find the index of the field
db.<nameOfCollection>.find({<key>: <value>}).explain('executionStats')
Ex:
db.students.find({courseCount: 5}).explain('executionStats')
Output: Here the totalDocsExamined: 3, which means that 3 JSON's were scanned to find the value
{
  explainVersion: '1',
  queryPlanner: {
    namespace: 'test1.students',
    indexFilterSet: false,
    parsedQuery: {
      courseCount: {
        '$eq': 5
      }
    },
    queryHash: 'FD45C2E3',
    planCacheKey: '61C944F7',
    maxIndexedOrSolutionsReached: false,
    maxIndexedAndSolutionsReached: false,
    maxScansToExplodeReached: false,
    winningPlan: {
      stage: 'FETCH',
      inputStage: {
        stage: 'IXSCAN',
        keyPattern: {
          courseCount: 5
        },
        indexName: 'courseCount_5',
        isMultiKey: false,
        multiKeyPaths: {
          courseCount: []
        },
        isUnique: false,
        isSparse: false,
        isPartial: false,
        indexVersion: 2,
        direction: 'forward',
        indexBounds: {
          courseCount: [
            '[5, 5]'
          ]
        }
      }
    },
    rejectedPlans: []
  },
  executionStats: {
    executionSuccess: true,
    nReturned: 3,
    executionTimeMillis: 15,
    totalKeysExamined: 3,
    totalDocsExamined: 3,
    executionStages: {
      stage: 'FETCH',
      nReturned: 3,
      executionTimeMillisEstimate: 10,
      works: 4,
      advanced: 3,
      needTime: 0,
      needYield: 0,
      saveState: 0,
      restoreState: 0,
      isEOF: 1,
      docsExamined: 3,
      alreadyHasObj: 0,
      inputStage: {
        stage: 'IXSCAN',
        nReturned: 3,
        executionTimeMillisEstimate: 10,
        works: 4,
        advanced: 3,
        needTime: 0,
        needYield: 0,
        saveState: 0,
        restoreState: 0,
        isEOF: 1,
        keyPattern: {
          courseCount: 5
        },
        indexName: 'courseCount_5',
        isMultiKey: false,
        multiKeyPaths: {
          courseCount: []
        },
        isUnique: false,
        isSparse: false,
        isPartial: false,
        indexVersion: 2,
        direction: 'forward',
        indexBounds: {
          courseCount: [
            '[5, 5]'
          ]
        },
        keysExamined: 3,
        seeks: 1,
        dupsTested: 0,
        dupsDropped: 0
      }
    }
  },
  command: {
    find: 'students',
    filter: {
      courseCount: 5
    },
    '$db': 'test1'
  },
  serverInfo: {
    host: 'DESKTOP-5GABVHC',
    port: 27017,
    version: '7.0.12',
    gitVersion: 'b6513ce0781db6818e24619e8a461eae90bc94fc'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted'
  },
  ok: 1
}


