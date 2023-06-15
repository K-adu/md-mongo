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

