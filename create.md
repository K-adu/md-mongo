## CREATE operations in mongo db



### Ordered

the ordered method is used when we want to specify the order

the true will order the list and will fail on the point where the ordered gets and error.
False will continue
```js
db.hobbies.insertMany([{doc1},{doc2}],{ordered: true/false})
```

### Write Concern

```js
db.persons.insertOne({name: "manish"}, {writeConcern:{w: 0}})
```
we send the request and immediately return, we dont wait for the response of the request.
in this it is not stored in the memory and has no chance to generate an object id
it will give ack as false, as we dont know whether it succeeded or not
The default is w: 1 
 
#### the journal
```js
db.persons.insertOne({name: "manish"}, {writeConcern:{w: 1, j:true}})
```
the default is false
it is a bit slow and secure cause we wait for it to finish writing in the journal 






