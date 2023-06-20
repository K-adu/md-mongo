## Aggregation Framework


### Aggregation vs find
Aggregation: Aggregation refers to the process of combining multiple data records and calculating a summary or statistical value from them. Aggregation functions are used to perform this task, such as sum, average, count, max, min, etc. Aggregation is often used to analyze and summarize large datasets to gain insights and make data-driven decisions. For example, you might aggregate sales data to calculate the total revenue for a specific time period.
<br>

Find: "Find" typically refers to a query operation used to retrieve specific data records from a database or collection that match certain criteria. The "find" operation is commonly associated with NoSQL databases, such as MongoDB. It allows you to search for documents based on specified conditions or filters. For example, you might use the "find" operation to retrieve all customers who have made a purchase in the last month.
<br>


taking a look at the first aggregation query<br>


### $lookup

the lookup operator in mongodb is used to perform a left outer join between two collections.

```js
db.orders.aggregate([{
  $lookup:{
    from: "customers",
    localField:"customers",
    foreignField: "_id",
    as: "customer",
  }
}])
```




### $match
In the context of the aggregation framework in MongoDB, the '$match' operator is used as the initial stage in an aggregation pipeline to filter and select documents that meet specific criteria. It allows you to narrow down the set of documents that will be processed in subsequent stages of the aggregation pipeline.<br>
The '$match' operator takes a query expression as its argument and filters the documents based on the specified conditions. The query expression can contain various operators and comparisons to match specific fields or values within the documents.
<br>
just like the find method the aggrigate method returns a cursor

```js
db.persion.aggregate([{
    $match: {
        gender: "female"
    }
}])
```

### $group

the 'group' stage allows us to group the data according to certain fields.
let's say we want to group the data by cerain field

```js
db.persons.aggregate{[{
        $match:{
            gender: "female"
    }
    }{
        $group: { _id: { state: "$location.state"}, totalPersons: {$sum: 1}}
    }{
        sort: {totalPersons: -1}
    }
    ]}
```
'$lookup' with pipeline
```js
db.orders.aggregate([
  {
    $lookup: {
      from: "products",
      let: { productId: "$productId" },
      pipeline: [
        {
          $match: {
            $expr: { $eq: ["$_id", "$$productId"] }
          }
        }
      ],
      as: "product"
    }
  }
]);

```
some more example of aggregation
```js
db.orders.aggregate( [
   // Stage 1: Filter pizza order documents by date range
   {
      $match:
      {
         "date": { $gte: new ISODate( "2020-01-30" ), $lt: new ISODate( "2022-01-30" ) }
      }
   },
   // Stage 2: Group remaining documents by date and calculate results
   {
      $group:
      {
         _id: { $dateToString: { format: "%Y-%m-%d", date: "$date" } },
         totalOrderValue: { $sum: { $multiply: [ "$price", "$quantity" ] } },
         averageOrderQuantity: { $avg: "$quantity" }
      }
   },
   // Stage 3: Sort documents by totalOrderValue in descending order
   {
      $sort: { totalOrderValue: -1 }
   }
 ] )

```
## The `$match` stage:

- Filters the pizza order documents to those in a date range specified using `$gte` and `$lt`.
- Passes the remaining documents to the `$group` stage.

## The `$group` stage:

- Groups the documents by date using `$dateToString`.
- For each group, calculates:
  - Total order value using `$sum` and `$multiply`.
  - Average order quantity using `$avg`.
- Passes the grouped documents to the `$sort` stage.

## The `$sort` stage:

- Sorts the documents by the total order value for each group in descending order (`-1`).
- Returns the sorted documents.


example output:
```js
[
   { _id: '2022-01-12', totalOrderValue: 790, averageOrderQuantity: 30 },
   { _id: '2021-03-13', totalOrderValue: 770, averageOrderQuantity: 15 },
   { _id: '2021-03-17', totalOrderValue: 630, averageOrderQuantity: 30 },
   { _id: '2021-01-13', totalOrderValue: 350, averageOrderQuantity: 10 }
]
```

on the above code the sorting doesnot have access to the result retyurned by the match operator.
this is how the aggregration pipeline works, filtering from layer to layer and performing operations


## $project
the '$project' operator is used to reshape documents by specifying which fields to include or exclude, creating new fields, renaming existing fields, and manipulating field values. It allows you to control the output of the aggregation pipeline.

```js
db.collection.aggregate([
  { $project: { field1: 1, field2: 1 } }
])

// This example will include only the fields "field1" and "field2" in the output documents, discarding all other fields.

db.collection.aggregate([
  { $project: { field1: 0, field2: 0 } }
])
//This example will exclude the fields "field1" and "field2" from the output documents, while including all other fields.
```

creating a new field with the value of the old field
```js
db.collection.aggregate([
  { $project: { newField: "$existingField", field2: 1 } }
])
```
there are many other operators but no need to keep in mind. they are available in the docs, just know how it works


## turning the location into geoJSON object

