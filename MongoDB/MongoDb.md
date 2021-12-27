# INDEXES

# LIST

db.collection.getIndexes()

## DELETE

db.collection.dropIndex()

### example
db.pets.dropIndex( "catIdx" )
db.pets.dropIndex( { "cat" : -1 } )