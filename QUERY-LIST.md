
![](https://i.pinimg.com/originals/e0/af/1e/e0af1eebcbbb75d9e8f2753145cda239.jpg)

# QUERY LIST FOR MONGODB

When you are done setting up the database run the following queries. If your database does not allow for any of the following queries/requests to successfully retrieve the data you are allowed to alter the structure of your documents and collections. *THE MOST IMPORTANT PART IS THAT YOU KEEP YOUR LOG OF ALL QUERIES UP TO DATE*

_IMPORTANT:_  For the sections to follow, please number your LOGS in your LOGS File according to the numbering below.

## Get or put the following information into your Mongo Database

### Instructions

#### Section A

1. Add a user, Peter Parker, with a login count of 3.
1. Set the age of Peter Parker to 25
1. Set Peter Parker's address to:  
city: New York  
suburb: Queens  
street: 20 Ingram Street  
1. Add a user, Tony Stark, with login count of 5.
1. Set the age of Tony to 53
1. Set Tony's address  
city: Malibu  
Suburb: N/A  
Street: 10880 Malibu Point  
1. Add user, Thanos, logins of 300
1. Set age of Thanos to 1000
1. Thanos's location is unknown
1. Each item/product should contain a field that indicates whether it is up for sale or not.
1. Find all the items in the items/products collection
1. Add the following items for the user, assume te price to be the reserve price of an item in your auction:
    1. Peter Parker wants to sell his mask with a starting value of 1200
    1. Tony Stark wants to sell his _Palladium Mini-Arc Reactor Mark I_ for 45000
    1. Tony Stark also wants to add _Mark V_ suit to his items but he is not sure that he wants to sell it yet. (Items should have a field that states whether an item is up for auction or not)
    1. Thanos wants to sell the _Time Stone_ for 120000
1. Place the following bids in your database for Peter's Mask:
    1. First Bid for Peter Parker's mask:
        * Tony Stark placed the bid
        * at a value of 1300
    1. Second Bid for Peter's mask
        * Thanos Placed the bid
        * at a value of 1400
1. Place the following bid for Tony's _Palladium Mini-Arc Reactor Mark I_:
    1. First bid for Tony's reactor
        * Peter Parker Placed the bid
        * at a value of 5000
    1. Second bid for Tony's reactor
        * Thanos placed the bid
        * at a value of 5500
    1. The third bid for Tony's reactor
        * _Was placed by any other person in your database_
        * _at a price that you choose_
1. Thanos's Time Stone is up next for auction, place any 3 bid from any 3 users on your database.

## End of day queries

### Section B

We have reached the end of the auction day, but we still have to get some information from our database in order to up date the company books. Answer the following question with queries and aggregations that you perform on your database.

1. The names and descriptions, but not ids, of all items for sale (use a projection)
1. All the items that have at least one bid with an amount of
at least 5000, but less than 15.
1. All items with the data of their associated sellers (use an
aggregation)
1. The average amount of all bids made for the item named
“_Palladium Mini-Arc Reactor Mark I_”.
1. The total price of all items that are for sale and the total
price of all items that are not for sale. (use groups) 

#  The End 💥

![](https://www.cheatsheet.com/wp-content/uploads/2019/02/avengers-endgame-logo.jpg)
