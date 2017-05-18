# Notes
## Terminology
- Database: contains all of the object stores (same as sql)
- Object Store: Similar to tables in sql
- Index: A property of an object store that allows for easy search and retrieval.
- Operation: An interaction with a databas.
- Transaction: A wrapper around an operation that helps keep integrity of a db. If one operation fails, none of the changes are saved and db is unaltered.
- Cursor: method for iterating over a multiple records in a db

## Working with DB
** DB and create Object Store (OS)**
```javascript
var dbPromise = idb.open('test-db2', 1, function(upgradeDb) {
   console.log('making a new object store');
   if (!upgradeDb.objectStoreNames.contains('firstOS')) {
     upgradeDb.createObjectStore('firstOS');
   }
 });
 ```
 Always check if OS is already created --> `!upgradeDb.objectStoreNames.contains('firstOS')`

 ** Primary Keys**

 A key path is a property that always exists and contains a unique value. For example, in the case of a "people" object store we could choose the email address as the key path.

`upgradeDb.createObjectStore('people', {keyPath: 'email'});`

This example creates an object store called "people" and assigns the "email" property as the primary key.

`upgradeDb.createObjectStore('notes', {autoIncrement:true});`

This example creates an object store called "notes" and sets the primary key to be assigned automatically as an auto incrementing number.

`upgradeDb.createObjectStore('logs', {keyPath: 'id', autoIncrement:true});`

This example is similar to the previous example, but this time the auto incrementing value is assigned to a property called "id".
## Working with data
**Creating Data**
All data operations in IndexedDB are carried out inside a transaction. Each operation has this form:

1. Get database object
2. Open transaction on database
3. Open object store on transaction
4. Perform operation on object store

```javascript
dbPromise.then(function(db) { ///step 1
  var tx = db.transaction('store', 'readwrite'); //step 2
  var store = tx.objectStore('store'); //step 3
  var item = {
    name: 'sandwich',
    price: 4.99,
    description: 'A very tasty sandwich',
    created: new Date().getTime()
  };
  store.add(item); //step 4
  return tx.complete;
}).then(function() {
  console.log('added item to the store os!');
});
```

** Reading Data**
```javascript
dbPromise.then(function(db) { //step 1
  var tx = db.transaction('store', 'readonly'); //step 2
  var store = tx.objectStore('store'); //step 3
  return store.get('sandwich'); //step 4
}).then(function(val) {
  console.dir(val);
});
```
**Updating Data**

Similar to creating data except we us the following method
`someObjectStore.put(data, optionalKey);`

**Deleting Data**

Similar to creating data except we us the following method
`someObjectStore.delete(primaryKey);`

## Retrieving Data
**Get all method**

Easiest way that returns all objects in optional range (optionalConstraint) and returns array sorted by primary key.

`someObjectStore.getAll(optionalConstraint);`
```javascript
dbPromise.then(function(db) {
  var tx = db.transaction('store', 'readonly');
  var store = tx.objectStore('store');
  return store.getAll();
}).then(function(items) {
  console.log('Items by name:', items);
});
```
**Cursors**

A cursor selects each object in an object store or index one by one, letting you do something with the data as it is selected.

```javascript
dbPromise.then(function(db) {
  var tx = db.transaction('store', 'readonly');
  var store = tx.objectStore('store');
  return store.openCursor();
}).then(function logItems(cursor) {
  if (!cursor) {return;}
  console.log('Cursored at:', cursor.key);
  for (var field in cursor.value) {
    console.log(cursor.value[field]);
  }
  return cursor.continue().then(logItems);
}).then(function() {
  console.log('Done cursoring');
});
```

**Note: Remember to close the test page. The database version can't be changed while another page is using the database.**
