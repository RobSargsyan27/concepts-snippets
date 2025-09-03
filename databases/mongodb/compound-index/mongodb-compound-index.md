# MongoDB: Compound Indexes

## Key Points

- The order of the indexed fields impacts the effectiveness of a compound index. Compound indexes contain references to documents according to the order of the fields in the index. To create efficient compound indexes, follow the ESR (Equality, Sort, Range) rule.  We have many schemas, where we can benefit from this rule. This can be considered as a vital enhancement.
- Index prefixes are the beginning subsets of indexed fields. Compound indexes support queries on all fields included in the index prefix.
    - For example, `{ "item": 1, "location": 1, "stock": 1 }` can replace
     `{ "item": 1, "location": 1 }` and `{ "item": 1 }` but can not replace 
    `{"location": 1, "stock": 1 }`.

<aside>
💡

If you have a collection that has both a compound index and an index on its prefix (for example, `{ a: 1, b: 1 }` and `{ a: 1 }`), if neither index has a [sparse](https://www.mongodb.com/docs/manual/core/index-sparse/#std-label-index-type-sparse) or [unique](https://www.mongodb.com/docs/manual/core/index-unique/#std-label-index-type-unique) constraint, you can remove the index on the prefix (`{ a: 1 }`). MongoDB uses the compound index in all of the situations that it would have used the prefix index.

</aside>

- There is no `index: true`  option for mongoDB indexes. Here is the [list](https://www.mongodb.com/docs/manual/core/indexes/index-properties/#std-label-index-properties) of all available MongoDB index properties.

## The ESR Rule

ESR stands for equality, sort and range. The ESR rule is used to arrange index keys in compound index in order to define a more efficient index.

### Equality

The equality refers to the principe that all fields which are later going to be used for exact match should be placed at the beginning of the index. If there are multiple exact matched index fields, their order depends on the later usage of ****index prefix. 

### Sort

Before understanding sort principle there are two important definitions that one should know:

- Non-blocking Sorting: This refers to the sorting when MongoDB driver doesn’t load the result set into the memory to execute sorting but rather directly uses the index to retrieve documents in the sorted order.
- Blocking Sorting: This refers to the sorting when MongoDB driver loads the result set into the memory and executes sorting before returning the documents. This can happen even if sorting fields are a subset of a compound index but their sorting order is different. For example, the `price`  field is sorted in ascending order but the query is looking for descending order. In this case index will be used to load documents into the memory and then execute sorting before returning the documents.

The sort refers to the principle that all fields by which later the result set is going to be sorted should be defined right after exact match index fields. This ensures that in some cases non-blocking sorting can be done and secondly, the compound index can be used in full scale. If there are more than one sort field, then their order and sorting order should be defined according to the queries that are run on the collection. While defining the order, keep in mind the index prefix and non-blocking sorting concepts. 

### Range

The range refers to the principle that the fields that are being used in query with rage operators such as `$gt, $gte, $lte, $lt, $ne, $nin, $in`  should be placed at the very end, after sort fields. This ensures that the compound index can be used both for querying and sorting the result set. If the order of sort and range field is mixed or switched the MongoDB driver will have to do a blocking sorting before returning the result set. But when the sort fields come before the range fields, MongoDB driver uses non-blocking sorting because the fields have already been sorted and can be directly returned in the right order. For example, think about the differences between these two indexes: `{price: 1, item: 1}` and `{item: 1, price: 1}` and how these indexes used with following query: `db.collection.find({ price: { $gt: 50 }}).sort({item: 1})` .

## Compound Index Sort Order

MongoDB driver will use the compound index for the sorting operation if the sorting fields exists in the index and they are defined in the same order as defined in sorting operation. 

The sorting order (i.e. `1 || -1`) which is used in the compound index is also important. The index will be used either if it exactly matches with sorting operation’s fields’ sorting order or if it is complete opposite of it. This is also true for the index prefix. 

<aside>
⚙️

Compound index ⇒ `db.leaderboard.createIndex( { score: -1, username: 1 } )` 
`db.leaderboard.find().sort( { score: -1, username: 1 } )`  ⇒ works ✅
`db.leaderboard.find().sort( { score: 1, username: -1 } )` ⇒ works ✅
`db.leaderboard.find().sort( { score: -1, username: -1 } )` ⇒ doesn’t work ❌
`db.leaderboard.find().sort( { score: 1, username: 1 } )` ⇒ doesn’t work ❌

</aside>