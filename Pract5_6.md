# Mongo database creation & querying to insert, update & delete

- Installing mongoDB on docker

```bash
 docker run --name mongodb -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=root mongodb/mongodb-community-server:6.0-ubi8
```

- To open mongo shell
```bash
sh-4.4$ mongosh -u root -p
Enter password: ****
Current Mongosh Log ID: 66ccb3c8f0b2d154d55e739b
Connecting to:          mongodb://<credentials>@127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.0
Using MongoDB:          6.0.16
Using Mongosh:          2.3.0
```

- Running commands
```bash
test> show dbs
admin   100.00 KiB
config   12.00 KiB
local    72.00 KiB

test> use symca
switched to db symca

symca> db.createCollection("myCollection");
{ ok: 1 }

symca> db.myCollection.insertOne({
...     name: "John Doe",
...     age: 30,
...     email: "john@example.com"
... });
{
  acknowledged: true,
  insertedId: ObjectId('66ccb43bf0b2d154d55e739c')
}

symca> db.myCollection.find({});
[
  {
    _id: ObjectId('66ccb43bf0b2d154d55e739c'),
    name: 'John Doe',
    age: 30,
    email: 'john@example.com'
  }
]

symca> db.myCollection.updateOne( { name: "John Doe" }, { $set: { age: 31 } } );
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

symca> db.myCollection.find({});
[
  {
    _id: ObjectId('66ccb43bf0b2d154d55e739c'),
    name: 'John Doe',
    age: 31,
    email: 'john@example.com'
  }
]

symca> db.myCollection.deleteOne({ name: "John Doe" });
{ acknowledged: true, deletedCount: 1 }

symca> db.myCollection.find({});
symca>
```
