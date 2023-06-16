# Geospatial Queries

It is fun so stay Tuned



so we have seen storing datas like texts numbers and all, now we are gonna see how to store location data and query to those data

## Adding GeoJSON Data
lets insert a first latitude and longitude
```js
db.place.insertOne({
    name: "A random place in the universe where human beings live",
    location: {
        type: "Point", // note this is provided my mongodb not user Defined
        coordinates: [$longitude, $latitude]
    }
})
```
now we saw how to create GeoJson data now lets see how we can query those data

## Quering GeoJSON Data

<b>'$near'</b> is the operator provided by mongodb to work with geoSpatial data
<b>'$geometry'</b> 

```js
db.places.find({location: {$near: {$geometry: {type:"point",coordinates: [lat,lon]}}}})
```
but if we run this query this will throw an error saying requires GeoSpatial Index

### Creating Geospatial Index

now when creating index we can use the createIndex keyword similary like before but we need to specify how we gonna index it<br>

```js
db.places.createIndex({location: "2dsphere"})
```
now the above find command will work and now we want to find the place in our db using the places near it
so now what we can do is use the keywords like max distance and min distance
```js
{
   <location field>: {
     $near: {
       $geometry: {
          type: "Point" ,
          coordinates: [ <longitude> , <latitude> ]
       },
       $maxDistance: <distance in meters>,
       $minDistance: <distance in meters>
     }
   }
}
```
