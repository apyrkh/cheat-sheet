# Mongo DB

## Installation

https://www.mongodb.com/download-center

**On Windows:**
```
d:\>wmic os get osarchitecture
OSArchitecture
64-bit

d:\>unzip mongodb-win32-x86_64-2008plus-2.6.3.zip
d:\>cd mongodb-win32-x86_64-2008plus-2.6.3\

d:\mongodb-win32-x86_64-2008plus-2.6.3>mkdir data\db
d:\mongodb-win32-x86_64-2008plus-2.6.3>cd bin

d:\mongodb-win32-x86_64-2008plus-2.6.3\bin>mongod -dbpath ..\data\db
```

## Mongo Shell

**Hotkeys:**
```
help keys

ctrl+a / HOME
ctrl+f / END
ctrl+k delete to the end
alt + backspase
```

**Start shell:**
```
# by default connection is mongodb://127.0.0.1:27017
mongo
mongo "<PATH>/<DB>" --username <username>
mongo "<PATH>/<DB>" --username <username> --password <password>
```

**Base:**
```
show dbs
use newdb
show collections
```

Global object `db` is a link to the current DB.

### Insert

```
doc = {city: "Minsk", population: 1837000}
db.populdations.insert( doc )
db.populdations.find()
{_id: ObjectId(“9a1b2c3”),
  city: “Minsk”,
  population: 1837000
}
```

### Read

```
db.collection.find(selector, fields)
db.collection.findOne(selector, fields)
db.сollection.count(selector)
```

#### Selectors

```
db.collection.find({field: {$gt: value}})
db.collection.find({field: {$lt: value}})
db.collection.find({field: {$regex: regex}})

db.collection.find({
  $or: [
    condition_1,
    condition_2,
    …
    condition_n
]
})

db.collection.find({
  $and: [
    condition_1,
    condition_2,
    …
    condition_n
]
})

```

**$exists/$type**
```
db.collection.find{field: {$exists: true/false}}
db.collection.find{field: {$type: number}}
```
where field types are:

|  Type  | Number |
|-------:|-------:|
| Double |    1   |
| String |    2   |
| Object |    3   |
| Array  |    4   |


Search in arrays:
```
db.collection.find({array_field: {$in, [1,2,3]}})
db.collection.find({array_field: {$all, [1,2,3]}})
```

Search in object:
```
db.people.find({“object.property”: value})
```

#### Fields:
```
db.collection.find(selector, {field: true/false, _id: false})
```

#### Cursor
```
cursor = db.collection.find()
while (cursor.hasNext()) {
  printjson(cursor.next())
}
```

```
db.collection.find().sort(order)
db.collection.find().skip(count)
db.collection.find().limit(count)
db.collection.find().sort(order).skip(count).limit(count)
db.collection.find().sort({_id: 1/-1}).skip(5).limit(2)
```

### Update

```
db.collection.update(selector, doc, options)
```

**Update:**
```
db.collection.update(selector, {$inc: {field: value}})
db.collection.update(selector, {$unset: {field: <anything, e.g. true>}})
db.collection.update(selector, {$set: {field: value}})
```
**Update arrays:**
```
db.collection.update(selector, {$set: {“field.index”: 1}})

db.collection.update(selector, {$push: {array_field: value}})
db.collection.update(selector, {$pop: {array_field: value}}) //value 1 or -1
db.collection.update(selector, {$pull: {array_field: value}})

db.collection.update(selector, {$pushAll: {array_field: value}})// [1,2,3]
db.collection.update(selector, {$addToSet: {array_field: value}})
```

#### Options
**Insert/Update:**
```
db.collection.update(selector, {$set: {field: value}}, {upsert: true})
```

**Multiple update:**
```
db.collection.update(selector, {$set: {field: value}, {multi: true})
```

### Remove

```
db.collection.remove(selector)
db.collection.drop()
```

### Aggregation Framework

```
db.collection.aggregation([
    {stage1: options1},
    {stage2: options2},
    ...
    {stageN: optionsN},
])
```

TODO...

### Javascript in Mongo DB
```
var colors = ["red","green","blue"];
for(var i=0; i<10; i++){
  db.points.insert({
    "coordinates":{
      "x": Math.floor(Math.random()*100),
      "y": Math.floor(Math.random()*100)},
    "color": colors[ i % 3 ]
  })
}
```

## Mongo Import
```
mongoimport -d<database> -c<collection> < <path_to_file.json>
```
