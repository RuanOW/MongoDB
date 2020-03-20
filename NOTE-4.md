# Operators

So far, our find and update operations have been working with exact values. Today, we’ll use operators, which give us the flexibility to find and update ranges of values instead.

We have a customer who is interested in vehicles that have done less than 60 000 kilometres to avoid extra maintenance.

We can use the “less than” operator to find these vehicles.

```json
> db.cars.find({"kilometrage": {"$lt": 60000}}).pretty()
```

Another customer has just walked in. He is a bit of a germaphobe and wants to know which cars have been test driven so he can keep his distance. We have two ways to test for this condition.

1. We could check that the testDrive value is at least 1, with the “greater than or equal to” operator:

```javascript
> db.cars.find({"testDrives": {"$gte": 1}}).pretty();
```

2. Since we know that test drive counts can’t be negative, we could simply check that testDrives has any value other than 0 with the “not equal” operator.

```json
> db.cars.find({"testDrives": {"$ne": 0}}).pretty();
```

We can combine these checks together for more complex queries. If we want to find all models after 2012 but before 2018, we can run:

```json
> db.cars.find({"year": {"$gt": "2012", "$lt": "2018"}}).pretty();
```

Note that this works even through the years are stores as strings. This is because “less than” means “earlier in the alphabet” for strings, and for 4 digit years and all dates in the ISO format (yyyy-mm-dd), alphabetical order is equivalent to chronological order.

We just received some prices for our Toyotas. Let’s update the database.

```json
> db.cars.update({"plate": "ABC 789 GP"}, {"$set": {"price": 158000}});
> db.cars.update({"plate": "QWT 971 GP"}, {"$set": {"price": 200000}});
```

Now that we have these prices, we want to apply a discount for the Toyota models: 10%

We do this by using the $mul operator to multiply the price value by 0.9 (to replace the price by 90% of the current price)

```javascript
> db.cars.update({"make_id": ObjectId("5bb711fb87150ee2a6298f33")}, {"$mul": {"price": 0.9}}, {"multi": true});
```

However, we never want a discounted price to be less than 130 000. Update the price of all these vehicles to either 130 000 or the current price, whichever is greater.

```javascript
> db.cars.update({"make_id": ObjectId("5bb711fb87150ee2a6298f33")}, {"$max": {"price": 130000}}, {"multi": true});
```

Projections are queries that can expand or contract the amount of information we see in each document in a result. We have already used one type of projection, the lookup. Let’s see how we can use projections to get back only the data we need. This can be useful for a number of reasons:

1. Speed. The less data we return from the database, the faster the query. Useful if we expect millions of records to be returned.
2. Legibility. If we only get back information we are interested in, the documents become easier for humans to read.
3. Security. If we want to expose a user’s name in an API, we probably don’t want to expose their password too!

We want to find all the vehicles that are 2018 models, but we are not interested in the options. By excluding the options, we make the results easier to read.

Simple projections can be done by adding a second parameter to the find() command and listing all the properties to be excluded.

```javascript
> db.cars.find({"year": "2018"}, {"options": false}).pretty()
```

If we had a customer who wants back only critical information about automatic vehicles because he can’t drive a manual, we could run the following:

```javascript
> db.cars.find({"automaticTransmission": true}, {"plate": true, "model": true}).pretty()
```

Note that, when we specify true for one or more fields, all fields not listed are assumed false and vice-versa. A single projection object must list all fields with true values or list them all with false values.

The only exception here is _id, which is always assumed true. We can specify _id as false, even when everything else is true. Modifying the command above to exclude the _id we have:

```javascript
> db.cars.find({"automaticTransmission": true}, {"plate": true, "model": true, "_id": false}).pretty()
```
