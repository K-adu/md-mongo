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