## CREATE operations in mongo db



### default

the ordered method is used when we want to specify the order

the true will order the list and will fail on the point where the ordered gets and error.
False will continue
```js
db.hobbies.insertMany([{doc1},{doc2}],{ordered: true/false})
```




