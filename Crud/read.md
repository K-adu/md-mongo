# READing documents

## findOne and find

findOne finds thee first matching document
<br>
find gives back a cursor
<br>

## comparison operators

'$ne'  not equal 
<br>
'$gt' greater than
<br>
'$lt' less than
<br>
'$lte' less than equal to
<br>
'$in' it will find the docs with only the matching values {$in: [30,40]}
<br>
'$nin' 1-in
<br>
```js
db.users.findOne({rating: {$gt: 10}})
db.users.find({rating: {$gt: 10}})
```

if there is embedded document we use the dot operator to access it
```js
db.users.find({movies.rating: {$gt: 10}})
```
if we have array embedded we need to specify that we are looking for an array
```js
db.users.find([{movies.rating: {$gt: 10}}])
```

## logical operators 

'$and'

```js
db.users.find({
  $and: [
    { age: { $gt: 25 } }
    { country: "USA" }        
  ]
})
```

'$or/nor'
```js
db.users.find({
  $or: [
    { age: { $gt: 40 } },       // age greater than 40
    { country: "Canada" }       // country is Canada
  ]
})
```

'$not'
```js
db.users.find({
  age: { $not: { $gt: 30 } }    // age not greater than 30
})
```

The '$exists' operator allows you to query for documents based on the presence or absence of a field. It takes a boolean value (true or false) as its value.


```js
// Find documents where the "address" field exists
db.users.find({ address: { $exists: true } })

// Find documents where the "phone" field does not exist
db.users.find({ phone: { $exists: false } })

```


'$type'
```js
// Find documents where the "age" field is of type "number" (32-bit or 64-bit integer)
db.users.find({ age: { $type: ['int', 'long'] } })

// Find documents where the "email" field is of type "string"
db.users.find({ email: { $type: 'string' } })

// Find documents where the "status" field is of type "boolean"
db.users.find({ status: { $type: 'bool' } })

```

## evaluation operators

'$regex'

we can use reqular expression inside mongodb
```js
// Find documents where the "name" field starts with "J"
db.users.find({ name: { $regex: /^J/ } })

// Find documents where the "email" field contains the word "example"
db.users.find({ email: { $regex: /example/ } })

// Find documents where the "description" field matches a case-insensitive regular expression pattern
db.products.find({ description: { $regex: /pattern/i } })

```
'$expr'

The '$expr' operator in MongoDB allows you to use aggregation expressions within the query language. It enables you to perform more complex comparisons and conditions that are not directly supported by the regular query operators.
```js
// Find documents where the "price" field is greater than the "discountedPrice" field
db.products.find({
  $expr: { $gt: ["$price", "$discountedPrice"] }
})

```


## Quering through array

if we have a title embedded inside hobbies and we have different tiles inside tile like (sports and frequency) and we cant to get only the title sports

we can use
```js
db.users.find({"hobbies.title": "Sports"})
```


'$size'

```js
db.users.find({hobbies: {$size:3}})

// output: hobbies with array size of 3
```


sorting out the result using findone
-1 descending
1 ascending


```js
db.movies.find().sort({"primarySroting": -1,"secondarySorting": 1})
```


there is limit() and skip() methods too

## using projection to shape our result

the second argument is to define how values are projected
<br>
the values set to 1 are projected
<br>
we need to put to remove id from projection

```js
db.movies.find({},{name:1, genres:1, runtime:1, rating:1, _id: 0}).pretty()
```

### the $slice operator
suppose we have a object
```js
{
  "_id": 1,
  "name": "John",
  "scores": [85, 90, 95, 80, 70]
}
```
the slice operator
```js
db.users.find(
  { "_id": 1 },
  { "scores": { "$slice": 3 } }
)

```
output
```js
{
  "_id": 1,
  "scores": [85, 90, 95]
}

```