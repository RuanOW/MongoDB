# Embedded and reference documents

Sometimes we want to create complex data systems between documents in our database.

In JavaScript, we can do this by making an object value within another object. For example:

```JSON
{
  "someKey": {
    "extraField": "extraData"
  }
}
```

However, in a typical relational database, we can do this by linking two tables together, and joining them during a SELECT.

So, which method does mongo use? Well, it offers both. These two methods are called embedded documents and reference documents, respectively. 

We are going to create a new collection in our vehicles database called manufacturers. This collection stores details about the different car manufacturers, such as how to contact them if something goes wrong with a vehicle.

```JSON
> use vehicles
> db.manufacturers.insert({"name": "Ford"});
> db.manufacturers.insert({"name": "Toyota"});
```

## Embedded Documents

Each of these manufacturers will hold an embedded document filled with contact numbers. Each contact number is keyed to a particular country.

```javascript
> db.manufacturers.update(
    {"name": "Toyota"},
    {"$set": {
      "contactNumbers" : {
        "southAfrica" : "0800 022 600"
        }
      }
    }
  );
```

We can also use set to change and add fields to the embedded document. To add the number for the Botswana office:

```javascript
> db.manufacturers.update(
    {"name": "Toyota"},
    {"$set": {
      "contactNumbers.botswana": "+27 11 799 1640"
      }
    }
  );
```

And, lets update ford too:

```javascript
> db.manufacturers.update(
    {"name": "Ford"},
    {"$set": {"contactNumbers.southAfrica": "012 951 9363"}});
```

As you can see, embedded documents act just like regular documents and can add structured subsections of information to a document. Of course, we could have just added the contact numbers directly as individual fields in the manufacturer documents, but the embedded documents allow is to create a more understandable semantic structure.

## Reference Documents

We also store the manufacturer in the cars database in the “make” field. We may need to get the contact details of the manufacturer while viewing a particular car. However, it doesn’t make sense to store the contact details as embedded objects in the cars too, because:

a. It uses up a lot of space by duplicating data.

b. If the contact number of a manufacturer changes, we’ll need to update it for ALL the cars created by that manufacturer.

We’re going to update the cars collection to reference their manufacturer by the unique _id field. First, we are going to rename make to make_id in our cars object to indicate this field references a foreign id.

```javascript
> db.cars.update(
  {},
  {"$rename": {"make": "make_id"}},
  {"multi": true}
  );
```

Now, we need to update the make_id to use the _id of the manufacturer instead of the name. The value of ObjectId can be found using db.manufacturers.find()

```javascript
> db.cars.update(
{"make_id": "Toyota"},
{"$set": {"make_id" : ObjectId("abc")}},
{"multi": true}
  );
```

```javascript
> db.cars.update(
{"make_id": "Ford"},
{"$set": {"make_id" : ObjectId("def")}},
{"multi": true}
  );
```

Now that our cars reference the manufacturers, we need to use a lookup to merge the data together.

```javascript
db.cars.aggregate([
   {
     $lookup:
       {
         from: "manufacturers",
         localField: "make_id",
         foreignField: "_id",
         as: "manufacturer"
       }
  }
]);
```

And with that, we have saved the contact details for manufacturer only once, but can view the appropriate numbers for each car using a lookup aggregation with a reference document. We’ll take a closer look at other aggregations in weeks 5 and 6.
