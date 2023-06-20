## capped collection

Capped collections are fixed-size collections that support high-throughput operations that insert and retrieve documents based on insertion order. <br>


## creating a capped collection
You must create capped collections explicitly using the db.createCollection() method, which is a 
mongosh.helper for the create command. When creating a capped collection you must specify the maximum size of the collection in bytes, which MongoDB will pre-allocate for the collection. The size of the capped collection includes a small amount of space for internal overhead.

```js
db.createCollection( "log", { capped: true, size: 100000 } )
//you may also specify a maximum number of documents for the collection using the max field as in the following document:

db.createCollection("log", { capped : true, size : 5242880, max : 5000 } )

```
## quering a capped collection

If you perform a find() on a capped collection with no ordering specified, MongoDB guarantees that the ordering of results is the same as the insertion order.

To retrieve documents in reverse insertion order, issue find() along with the sort() method with the $natural parameter set to -1, as shown in the following 

example:

```js
db.cappedCollection.find().sort( { $natural: -1 } )


//to check if a collection is capped
db.collection.isCapped()

//convert a collection to capped
// we can use the convertToCapped
db.runCommand({"convertToCapped": "mycoll", size: 100000});
```

