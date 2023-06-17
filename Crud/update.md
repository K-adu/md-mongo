## Update


'$set' to set the values to the updated one<br>
'$inc' is used to increment value by 1<br>


### min operator
he '$min' operator in MongoDB is used to update a field's value in a document with the specified value if the specified value is less than the current value of the field<br>
similarly for max<br>
and mul operator will multiply the value by provided value

suppose we have document 
```js
{
  "_id": 1,
  "name": "Example Product",
  "price": 50
}

```
applying min operator to it
```js
{
  "_id": 1,
  "name": "Example Product",
  "price": 50
}

```
the update will be is:
```js
{
  "_id": 1,
  "name": "Example Product",
  "price": 40
}
```

### getting rid of fields
```js
db.users.updateOne(
  { "_id": 1 },
  { "$unset": { "email": "", "age": "" } }
)

```

### rename
```js
{
  "_id": 1,
  "fname": "John",
  "lname": "Doe",
  "email": "john.doe@example.com"
}

```
the rename operator
```js
db.users.updateOne(
  { "_id": 1 },
  { "$rename": { "fname": "firstName", "lname": "lastName" } }
)
```

after renamed
```js
{
  "_id": 1,
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com"
}

```

## upsert()


In MongoDB, the upsert() function in the mongosh shell is used to perform an update operation on a document. If the specified document is not found, the upsert() function creates a new document with the specified update criteria.

```js
db.collection.update(
  <query>,
  <update>,
  {
    upsert: true
  }
)

```below is the pratical example for upsert()

```js
db.users.update(
  { "email": "john@example.com" },
  { "$set": { "name": "John Doe" } },
  { upsert: true }
)

```


### updating matched array elements
```js
{
  "_id": 1,
  "name": "John",
  "scores": [85, 90, 95, 80, 70]
}
```
Let's say we want to update a specific score in the scores array. For example, we want to increment the second score by 10. We can use the positional operator $ in combination with the $inc operator in an update operation:

```js
db.users.updateOne(
  { "_id": 1, "scores": 90 },
  { "$inc": { "scores.$": 10 } }
)

```

```js
{
  "_id": 1,
  "name": "John",
  "scores": [85, 100, 95, 80, 70]
}

```

### Adding element to an array
lets say we want to attach a new hobby to the old hobby without updating
'$push'

```js
db.users.updateOne{$push:{hobbies: {title: "sports"}}}
```

'$each'
we can use the each operator to add many objects 
```js
db.users.updateOne({key: value},{$push: {key: {$each: [{doc1},{docN}]}}})
```
'$sort'
we can use it as a sibling to the each operator to sort 


## Addtoset operator

we can push dublicate command with the '$push' operator but with '$addToSet' we cannot add a dublicate value to the set