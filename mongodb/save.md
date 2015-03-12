# Save

After years of deciding between
[`db.collection.insert()`](http://docs.mongodb.org/manual/reference/method/db.collection.insert/)
and
[`db.collection.update()`](http://docs.mongodb.org/manual/reference/method/db.collection.update/) (often with [`upsert`](http://docs.mongodb.org/manual/reference/method/db.collection.update/#upsert-option)),
I was somewhat shocked to discover the much simpler
[`db.collection.save()`](http://docs.mongodb.org/manual/reference/method/db.collection.save/):

```javascript
db.collection.save({_id: 1, name: 'Bob'});
// Will insert "Bob" if no record with ID 1 exists or update if it does.
```
