# Updating documents
Today we will continue with the cars collection of the vehicles database. Let’s add some more cars and then start updating. Use the insert command from last week to add the following:
```JSON
{
   "make" : "Ford",
    "model" : "Focus",
    "year" : "2015",
    "colour" : "silver",
    "plate" : "TRM 778 GP",
    "mileage" : 51352,
    "automaticTransmission" : false,
    "options" : [ "ABS", "Airbags", "Electric Windows", "Radio", "Sun Roof" ]
},
{
    "make" : "Ford",
    "model" : "Figo",
    "year" : "2012",
    "colour" : "silver",
    "plate" : "LGS 433 GP",
    "mileage" : 93613,
    "automaticTransmission" : false,
    "options" : [ "ABS", "Airbags", "Electric Windows", "Radio" ]
},
{
    "make" : "Toyota",
    "model" : "Land Cruiser",
    "year" : "2012",
    "colour" : "white",
    "plate" : "QWT 971 GP",
    "mileage" : 154000,
    "automaticTransmission" : true,
    "options" : [ "ABS", "Airbags", "Electric Windows", "Radio", "Sat Nav", "Sun Roof" ]
}
```

## Removing Documents
Just as we finish adding in the new vehicles, we get the news that the 2012 Ford Fiesta with plate XYZ 123 GP has been sold! Let’s remove it from the database.

```JSON
> db.cars.remove({"plate" : "XYZ 123 GP"})
```

The remove statement uses the argument to find the document to be deleted and removes it from the collection. To ensure that this worked, run `db.cars.find()`. Make sure to supply an argument… Forgetting to do so will delete everything in the collection!
## Update Document
Oops… We just realised that the Ford Focus we added earlier is actually the Focus Ecoboost model. Let’s update this in the database.

```JSON
> db.cars.update({
   "plate" : "TRM 778 GP"
    },
    {
        "$set" : {
            "model": "Focus Ecoboost"
        }
  })
```

The update command takes two parameters: First, a selector for the document that needs to be updated. Second, a $set operator which updates the model with a new value.

### Using the $set

Note that we need to use the $set to ensure we only overwrite a single field. Consider what would happen if we did the following instead:

```JSON
> db.cars.update(
    {
       { "plate" : "TRM 778 GP"
    },
    {
        "model" : "Focus Ecoboost"
    }) } )
```



This will replace the entire document with just `{ "model" : "Focus Ecoboost" }`

This is an easy mistake to make! Remember to always use $set to overwrite fields!

We want to start counting how many test drives it takes to sell each car. Let’s add a testDrive field to each car to store this count. The first parameter can be {} to match all the cars in the collection.

```JSON
> db.cars.update({}, {"$set": {"testDrives":
``

This only adds a count to the first car! By default, update only updates the first document it matches. To match ALL the documents it matches, we have to add a multi option as a third parameter and set its value to true.

```JSON
> db.cars.update( {}, {"$set": {"testDrives": 0}}, {"multi": true} )
```

### Using the $inc operator

Someone just came in to test drive the Toyota Land Cruiser. We can’t see the plate number while the potential buyer is out driving the car but we can update by specifying the make and model since we know there is only Toyota Land cruiser for sale. We will use the $inc operator instead of $set to increment the testDrive value by one instead of setting it to a particular value.

```JSON
> db.cars.update( {"make" : "Toyota", "model" : "Land Cruiser"}, {"$inc": {"testDrives": 1}} )
```

## Removing field from all documents

It turns out the colour of the car isn’t that important after all as the dealer can re-spray the vehicle any colour the buyer wants. Let’s remove the colour field from all the cars.

```JSON
> db.cars.update( {}, {"$unset": {"colour": ""}}, {"multi": true} )
```

The value supplied to colour isn’t actually used in the $unset but it is good practice to supply an empty string.

## Renaming fields

The mileage field is becoming a problem for the data entry staff, some of who are confused by the fact we are actually measuring kilometers. We can update the names of fields using the $rename operator.

```JSON
db.cars.update( {}, {"$rename": {"mileage": "kilometrage"}}, {"multi": true} )
```

User testing on the website has shown that most of our users don’t understand the Sat Nav option because they refer to that system as GPS instead. We need to change Sat Nav whenever it appears in an options array.

First, let’s check if we have any cars with a Sat Nav option. We can simply use find() and supply the "Sat Nav" value for options. Mongo will know to search the array to check if it contains the supplied element.

```JSON
db.cars.find({"options": "Sat Nav"})
```

We can’t use the $set operation as we have been doing so far since that will replace the entire options array with a single value. Instead, we just want to replace a single element in the array: For this, we use a $ placeholder.

```JSON
> db.cars.update(
    {"options": "Sat Nav"},
    {"$set": {"options.$" : "GPS"}},
    {"multi": true}
)
```

We just learned that we can now offer Full Service Plans for all our Toyota models as an extra option. We can append new elements to an array with the $push operator.

```JSON
db.cars.update( {"make": "Toyota"}, {"$push": {"options": "Full Service Plan"}}, {"multi": true} )
```

Note that our Toyota Corolla, which previously didn’t even have an options field, now does and its value is an array with a single element.

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUwNTI4MjAzNl19
-->