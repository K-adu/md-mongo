## Aggregation Framework


### Aggregation vs find
Aggregation: Aggregation refers to the process of combining multiple data records and calculating a summary or statistical value from them. Aggregation functions are used to perform this task, such as sum, average, count, max, min, etc. Aggregation is often used to analyze and summarize large datasets to gain insights and make data-driven decisions. For example, you might aggregate sales data to calculate the total revenue for a specific time period.
<br>

Find: "Find" typically refers to a query operation used to retrieve specific data records from a database or collection that match certain criteria. The "find" operation is commonly associated with NoSQL databases, such as MongoDB. It allows you to search for documents based on specified conditions or filters. For example, you might use the "find" operation to retrieve all customers who have made a purchase in the last month.
<br>


taking a look at the first aggregation query<br>

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

