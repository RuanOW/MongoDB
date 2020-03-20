# Aggregations

Today we’ll be looking at aggregations: A powerful way to find, sort, filter, and analyse data.

Aggregation is a great way to do reports and audits on data. You can think of aggregations as a powerful extension to the find() command. In fact, an empty aggregation has the same functionality as calling find()

```javascript
> db.cars.aggregate([]);
```

Let’s start with something simple. We want to count the number of cars created by each manufacturer. First, let’s group the cars by manufacturer.

```javascript
> db.cars.aggregate([
  {"$group": {"_id": "$make_id"}}
  ]);
```

This should give you back something like this:

```javascript
{ "_id" : ObjectId("5bb711f387150ee2a6298f32") }
{ "_id" : ObjectId("5bb711fb87150ee2a6298f33") }
```

While this tells us that we have two different manufacturers, we can’t see the count of each group. To do this, we’ll need to add a new field to each group called total. Total’s value should increase by 1 for each car of the associated manufacturer found in the collection.

```javascript
> db.cars.aggregate([
    {"$group": {"_id": "$make_id", "total": {"$sum": 1}}}
  ])
```

Great! We’re getting totals now, but the output is still quite hard to read. We should grab the name of each manufacturer from the manufacturers table to improve the readability of the output.

We have already seen how we can do a $lookup aggregation to link 2 tables together, but how to we perform both the $group and $lookup aggregation. Well, each aggregation command is actually a pipeline of aggregation. You may have noticed that the parameter of aggregation is an array. We can use this array to pass through multiple aggregations which are performed in order, with the output of each of those aggregations becoming the input for the next aggregation in the pipeline. 

Let’s update the command we have to far to pipe the groups to a lookup that will find the names associated with each manufacturer.

```json
> db.cars.aggregate([
    {"$group": {"_id": "$make_id", "total": {"$sum": 1}}},
    {"$lookup": {
      "from": "manufacturers",
      "localField": "_id",
      "foreignField": "_id",
      "as": "make"
    }}
  ]);
```

Now we can see the name of each manufacturer, but the output is still a little difficult to read due to all the unnecessary info included. Fortunately, there is a projection aggregation. Let’s add this as the third aggregation step in our pipeline.

```javascript
> db.cars.aggregate([
  {"$group": {"_id": "$make_id", "total": {"$sum": 1}}},
  {"$lookup": {
    "from": "manufacturers",
    "localField": "_id",
    "foreignField": "_id",
    "as": "make"
  }},
  {"$project": {
    "_id": false,
    "make._id": false,
    "make.contactNumbers": false
  }}]);
```

We have a customer who is interested in buying a car with a sunroof and wants to see all the cars with the lowest kilometrage first. 

We’ll need at least 2 steps in this pipeline, one to find only the cars with sun roofs and one to order the results. We could do these steps in either order and get the same result. However, it is far more efficient to filter first, then sort, since that means we’ll have to process fewer documents in the sort step.

Filtering is done with the $match aggregation.

```javascript
> db.cars.aggregate([{"$match": {"options": "Sun Roof"}}])
```

Ok, now we can sort the results. We can specify that a sort is ascending by supplying the value 1 or descending by supplying the value -1. In this case, we’ll want ascending order.

```javascript
> db.cars.aggregate([
    {"$match": {"options": "Sun Roof"}},
    {"$sort": {"kilometrage": 1}}
  ])
```

Once again, that’s more output than we need. Let’s add a final projection to see only the id, model, kilometrage, and price.

We used $sum earlier to count cars to determine the size of a group. The real power of $sum, however, is totalling particular values.

For example, to determine the total number of kilometres driven by all our vehicles:

```javascript
> db.cars.aggregate([
    {"$group": {
      "_id": null,
      "totalKilometres": {
        "$sum": "$kilometrage"
        }
      }
    }
  ])
```

Note that we specify null for the group id so that we end up with a single group with a single total.

In addition to $sum, we also have $min, $max, and $average to do calculations. To get the lowest price:

```json
{ "_id" : null, "lowestPrice" : 130000 }
```