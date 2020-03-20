# Notes 1

## What is Mongo DB

Mongo DB (Mongo Database) is an open source database. It's name is derived from "Humongous database".

You have likely worked with some database before, such as a MySQL or Oracle database. These databases are **relational**, **structured** data that use SQL to manipulate records in tables.

Example of an **SQL Table**

| ID | first_name | last_name | age  |
|----|------------|-----------|------|
| 1  | John       | Doe       | 27   |
| 2  | Jane       | Doe       | 24   |
| 3  | Paul       | Turner    | 34   |

MongoDB is a little different. Mongo is a **document-centric**, **semi-structured** database that uses **BSON** to manipulate documents in collections.MongoDB Documents are structured in a similar way to JSON data. The following code snippet show what data for describing a car would look like.

```JSON
{
  "make": "Ford",
  "model": "Ranger",
  "year": "2020",
  "colour": "white",
  "plate": "CAR 456 GP",
  "mileage": 16900,
  "automaticTransmission": false,
  "option": [
    "ABS",
    "Airbags",
    "Electric Windows",
    "Radio"
  ]
}
```

## Installation

The Windows and MAC install works a bit differently. Please follow instructions below for the most up to date installation instructions.

For this class you will only have to install the community edition of MongoDB

[Installation instrunctions](https://docs.mongodb.com/guides/server/install/)

## Getting Started

Assuming you have a successful installation of MongoDB the following instructions will work for you. If you do not have a successful MongoDB installation please contact your lecturer.

### Starting the MongoDB Server

Before we can start interacting with the MongoDB Database we first need to start the MongoDB server. You can start the server by running the following command.

```bash
$ mongod
```

The MongoDB Should start. If it does not give you any errors or exit codes at the end of the lines show on the screen, you can proceed by opening a new tab in your terminal.

If you would like a custom path for your data you can run th following command:

```bash
$ mongod --dbpath="/route/to/your/file/data/db"
```

To interact with MongoDB you need to start a Mongo Shell Session by running the following command. _Note that the is no 'd' at the end of mongo_

```bash
$ mongo
```

You will know the session has started when your prompt changes from **$** to **>**

## Creating and using a database

Let’s create a database that is going to be used by a car dealership. This will be the in class project. The mongo command to switch to a database will also create a new database if one with that name does not already exist.

```bash
> use vehicles
```

To check that we the database has been created, we can show all the databases hosted by our server. Note that some are created by default for us.

```bash
> show dbs
```

To check that we have switched the current database to vehicles, run the following:

```bash
> db
```

Let’s add a document to our database. A document represents a single item: A single car. Documents belong to a collection of related objects: In this case, cars.

```JSON
> db.cars.insert({
  "make":"Ford",
  "model": "Fiesta",
  "year": "2010",
  "colour": "blue",
  "plate": "XYZ 123 GP"
})
```

The shell should respond that 1 new document has been inserted. Does the data format look familiar? It should, since it is JSON. Well, actually, its BSON, which is an extension of JSON that supports more native types, including a special type for unique id values, a type for dates and times as well as a few others. So far, however, all of the values are strings.

We can view all the documents in a collection using find().

```bash
> db.cars.find()
```

You will see that MongoDB has generated a unique _id field for us. This is required for all documents.

Let’s add a second document.

```JSON
> db.cars.insert({
  "make":"Toyota",
  "model": "Corolla",
  "year": "2004",
  "colour": "red",
  "plate": "ABX 789 GP"
})
```

Once again, let’s view all documents in the collection. When working with multiple documents, it is usually easier to read the output using `pretty()`.

```bash
> db.cars.find().pretty()
```

We can also specify an object argument to find to return only documents with a specific set of characteristics. For example, to find only cars that are manufactured by Toyota, run the following:

```JSON
> db.cars.find({"make":"Toyota"}).pretty()
```

Let’s add a third document.

```JSON
> db.cars.insert({
  "make": "Ford",
  "model": "Ranger",
  "year": "2020",
  "colour": "white",
  "plate": "CAR 456 GP",
  "mileage": 16900,
  "automaticTransmission": false,
  "option": [
    "ABS",
    "Airbags",
    "Electric Windows",
    "Radio"
  ]
})
```

Note that this document has fields that are not shared by the other documents in the collection. This is what we mean by semi- structured. Also, you will see that we are using some new types for our values. The mileage is a number (in this case, an integer), automaticTransmission is a boolean, and options is an array. We should now be able to extract both the Ford vehicles while excluding the Toyota as follows:

```JSON
> db.cars.find({"make": "Ford"}).pretty()
```

To exit the mongo shell and shut down your server, `CTRL+C` in each terminal tab.


<!--stackedit_data:
eyJoaXN0b3J5IjpbNTQ2MzYxMDk3XX0=
-->