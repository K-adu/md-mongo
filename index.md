## indexes

indexes significanty reduces the time it takes to move down the documents in a collection<br>

### HLL explanation of indexing

indexing is the sorting of database attributes/keys so that when performing actions like finding updating and deleting.

indexing is a good approach to reduce time, but if we want to maintain index of all the attributs, it might cost us the insert time<br>

### about mongodb stats

mongoDB provides a so called method to see how it is quering through the documents
```js
db.users.explain().find()
```
the explain method in mongo db helps show details of the time it takes, winning method it choose, rejected methods and all
and is quite helpful

### indexing in mongoDb

we can create index in mongodb with the createIndex() method<br>
the -1 is descending and 1 is the ascending indexing of the document
```js
db.collectionName.createIndex({"key": -1/1})
```
after creating index if we perform certain task we can see the decrease in executionTime.<br>

when we do a indexScan it first gives us the pointer to the document.<br>
then the FETCH stage reach out to the actual collection and gives us the documents.<br>

### What does createIndex() does? (in detail)


we can't really see the index, you can think of the index as a simple list of values + pointers to the original document.<br>

Something like this (for the "age" field):

(29, "address in memory/ collection a1")<br>

(30, "address in memory/ collection a2")<br>

(33, "address in memory/ collection a3")<br>

The documents in the collection would be at the "addresses" a1, a2 and a3. The order does not have to match the order in the index (and most likely, it indeed won't).

The important thing is that the index items are ordered (ascending or descending - depending on how you created the index). createIndex({age: 1}) creates an index with ascending sorting, createIndex({age: -1}) creates one with descending sorting.

MongoDB is now able to quickly find a fitting document when you filter for its age as it has a sorted list. Sorted lists are way quicker to search because you can skip entire ranges (and don't have to look at every single document).

Additionally, sorting (via sort(...)) will also be sped up because you already have a sorted list. Of course this is only true when sorting for the age.

