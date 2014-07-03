#Importing a CSV file into mongoDB

This example illustrates how to use the mongoDB to import data in a CSV format from a local directory into mongoDB. This examples also covers other important components in the studio including the datamapper and Scopes such as the Message Enricher and Foreach.

###Assumption
This document describes the details of the example within the context of Anypoint™ Studio, Mule ESB’s graphical user interface (GUI). This document assumes that you are familiar with Mule ESB and the [Anypoint Studio interface](http://www.mulesoft.org/documentation/display/current/Anypoint+Studio+Essentials).

###Example Use Case
In this example we transform a sample CSV file containing sales data into a Map with a key value pair. We use the Datamapper transformer to do so. This Map is basically a collection called customers_Copy. We now use the mongoDB connector embedded in a Message Enricher scope to check if such a collection exists in the database. The message enricher basically enriches the message from #[payload] to #[flowVars['existsCollection']]. This is then used by the choice router to decide whether to route to a mongoDB connector that creates a collection or just use the default option. The last mongoDB connector embedded in the For Each scope, saves the object from the map, iteratively for each of the elements in the collection.

###Set Up and Run the Example


1  **Download** [mongoDB](http://www.mongodb.org/downloads) and [install](http://docs.mongodb.org/manual/installation/) it. If you are are running a linux based OS, Homebrew is an easy way to install mongoDB.

2  **Run mongoDB**             
   
   *  Make sure that you have created */data/db* in your filesystem per installation instructions
   * Now open two instances of the command terminal. In the first window start the mongoDB server by navigating to *mongodb install path/bin* and then typing in *mongod*. You should get the following message when your server is up and running:
   
        2014-07-01T16:15:25.282-0700 [initandlisten] waiting for connections on port 27017

   * In the second window run mongoDB by navigating to *mongodb install path/bin* and typing in *mongo*. You should get the following message when you are connected.
         
         MongoDB shell version: 2.6.3
         connecting to: test


3  Stay in the second command terminal window and use the following commands to **create a database** and a user called mule who is granted permission to access the database:  
	     
	use customers
    db.createUser
    (
     {
	user: "mule",
	pwd: "mule",
	roles:
	  [
       {
    	role: "userAdmin",
    	db: "admin"
       }
      ]
     }
    )


4  **Open** this example application in Studio

5 **Create a folder** called *test* in your Desktop and then copy input.csv from *src.main.resources* under *src/main/app* into the test folder

6 Navigate to common.properties under *src.main.resources* and **edit path.to.dir** to the path of the test folder as follows:
  
    /users/username/desktop/test

7 **Run** the example project as a mule application

8 **Go to the mongDB console** (the second command terminal window) and type:
	
	db.customers.find()
	
The output then shows the inserted objects as shown below.


    { "_id" : ObjectId("53a81ad81bd1ae1d20030922"), "firstname" : "John", "surname"
    : "Doe", "phone" : "12003246879", "state" : "12/10/80" }
    { "_id" : ObjectId("53a81ad81bd1ae1d20030923"), "firstname" : "Mary", "surname"
    : "Jane", "phone" : "9879842316", "state" : "03/03/76" }

### Go Further
* Read more about the mongoDB connector [here](http://www.mulesoft.org/documentation/display/current/Message+Enricher)
* Read more about the message enricher scope [here](http://www.mulesoft.org/documentation/display/current/Message+Enricher)
* Read more about the For Each connector [here](http://www.mulesoft.org/documentation/display/current/Foreach)
