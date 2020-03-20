![alt text ><](https://www.cloudfoundry.org/wp-content/uploads/2017/10/icon_mongodb@2x.jpg)
![image ><](https://is5-ssl.mzstatic.com/image/thumb/Purple123/v4/0f/ff/75/0fff75d6-6bcc-98ad-61ca-b5dcfffc86e3/source/256x256bb.jpg)

<style>

img[alt$=">"] {
  float: right;
}

</style>

# Mid-Term Test/Submission

## Introduction

### *In todays practical test you will be required to create a schema (document with required fields as a model) and a MongoDB implementation for Starbucks. You can model the data as you see fit, however you will have to take the following technical and business requirements into account*

> Notes & instructions:  
Consider using an entity relationship diagram to help develop the schema. Determine the types of relationships between the data such one-to-many and many-to-many relationship or embedded or referenced data.
___

## List of requirements and considerations

> You will have to create a JS or JSON file where you have to "paste" the queries you performed in the file. This will be submitted at the end of the test. The queries marked with `(*)` below are the queries that you have to paste and comment what it does in the above mentioned file as well as paste the respective response that you get from the terminal.

1. Starbucks has a lot of stores in South Africa. The database should hold information about each store  
(eg. Name, Location...)
2. As a company with many stores you have to consider that customers in JHB might also go to a Starbucks in Cape Town.
3. Starbucks need to keep basic information about their customers in their database  
(eg. Name, Email, Mobile Number, Address).
4. The users collection should list all the stores that a customer has visited.  
`(*)`Log what a query would look like to add a new location visited to the customer's document.
5. Starbucks takes orders of existing coffees. You only have to take 5 coffee types into account.
6. Pre-built coffees are at the bottom of the page.
7. Starbucks have a new privacy policy they do not keep mobile numbers in their data any more.  
`(*)` Log how to delete a field form all users.
8. The users document should store the amount of visits that a user has made.  
`(*)` Log how to add a visits field to all customers. `(*)` Log how you would go about incrementing the field every time a user would visit the store.
9. Your database should store the owners of each Starbucks shop as a reference document.
10. Use aggregation to query all stores with their owner information.  
`(*)` Log how you would perform this aggregation and remember to add comments and the terminal output.

## SUBMISSION INSTRUCTIONS

You are required to submit the following:

1. Your Log Sheet
2. Mongo database (`mongodump`)
3. Planning documents (digital or physical)

### MONGODUMP


Step 1:
Create a folder where you would like the database to be saved.
Note: use a new command shell for this.

Step 2:
"cd" into the before mentioned directory then run the following command.

```bash
> mongodump --db <your-db-name>
```

Step 3:
Create the following directory structure for your files:  

| studentName_studentNumber (folder)  
|-- dump  
|-- log.js  
|-- planning-files

Step 4: Submit along with any other physical materials
___
## SOME DATA

```json
{
    "name": "Caffe Latte",
    "description": "Our dark, rich espresso balanced with steamed milk and a light layer of foam. A perfect milk forward warm up.",
    "shop": "Starbucks",
    "prices": {
        "small": 2.95,
        "medium": 3.65,
        "large": 4.15,
        "extra_large": null
    }
},
{
    "name": "Caffe Mocha",
    "description": "We combine our rich, full-bodied espresso with bittersweet mocha sauce and steamed milk, then top it off with sweetened whipped cream. The classic coffee drink to satisfy your sweet tooth.",
    "shop": "Starbucks",
    "prices": {
        "small": 3.45,
        "medium": 4.15,
        "large": 4.65,
        "extra_large": null
    }
},
{
    "name": "White Chocolate Mocha",
    "description": "Our signature espresso meets white chocolate sauce and steamed milk, then is finished off with sweetened whipped cream in this white chocolate delight.",
    "shop": "Starbucks",
    "prices": {
        "small": 3.75,
        "medium": 4.45,
        "large": 4.75,
        "extra_large": null
    }
},
{
    "name": "Freshly Brewed Coffee",
    "description": "Brewing in our stores everyday is Starbucks® Pike Place® Roast. With subtle notes of cocoa and toasted nuts, enjoy a smooth, balanced and rich blend of Latin American coffees made perfectly into one.",
    "shop": "Starbucks",
    "prices": {
        "small": 1.85,
        "medium": 2.10,
        "large": 2.45,
        "extra_large": null
    }
},

```
