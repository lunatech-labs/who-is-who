#Content:
    # Who Is Who
    # Deployment
    # Browser interface:
    # REST interface


# Who Is Who:

This web app allow users to have simple list of employees along with their information such as email, location, photo, etc.
It is supposed to be easy to install and use, also, to modify and extend.


# Deployment:
You can fork and clone the app from Git repository on your local machine, then:
sbt, and run.

To be able to run the application you need a database. in conf/application.conf we provide several examples on possible DB configurations, including: in memory DB H2, SQLite, PostgreSQL.
For H2 DB: you can un-comment and use directly.
For SQLite: this is file based DB.
            You need to provide empty .db file in location you specify in the url configuration
For PostgreSQL: this is server based DB.
                You need to install PostgreSQL server, where you have authorized access.
                And need to provide username and password in the application.conf (Please use PostgreSQL guide for details).


# Browser interface:
If deployed on local machine, you can access the app in any browsersing:
http://localhost:9000
Then you can intereact with the app.


# REST interface

GET     /api/persons
GET     /api/person/:email
POST    /api/person/edit

**
Title: Show all persons in the database.
URL: /api/persons
Method: GET
URL Params: NON
Data Params: NON
Success Response:   Code: 200
                    Content: [
                               {
                                 "id": 1,
                                 "name": "Ali Hassan",
                                 "email": "ali.hassan@lunatech.com",
                                 "location": {
                                   "id": 2,
                                   "name": "Paris",
                                   "city": "Montevrain",
                                   "country": "France",
                                   "remarks": "Lunatech main office in France"
                                 },
                                 "photo": "file000541344089.jpg",
                                 "description": "....... "
                               }
                             ]

Error Response: NON (should succeed always unless there is network problem or the app is not up and running)
Sample Call:     curl --request GET  http://localhost:9000/api/persons

**
Title: Get a single person from the database
URL: /api/person/:email
Method: GET
URL Params: REQUIRED: email OPTIONAL: NON
Data Params: NON
Success Response:   Code: 200
                    Content: {"id":3,"name":"Ali Hassan","email":"ali.hassan@gmail.com","location":{"id":2,"name":"Paris","city":"Montevrain","country":"France","remarks":"Lunatech main office in France"},"photo":"file000541344089.jpg","description":"changed from jsonnn"}

Error Response:     404 Not Found.
                    {
                      "Error": "Person Not Found"
                    }
                    This will happen when the request pass email that does not exits in the database.

                    Play default response when you do not provide the requerd parameter: email (error in routing).
Sample Call:        curl --request GET   http://localhost:9000/api/person/ali.hassan@gmail.com

**
Title: Edit specific person in the database
URL: /api/person/edit
Method: POST
URL Params: NON
Data Params:    {
                    "id":16 ,
                    "name":"ali" ,
                    "email":"ali@lunatech.com" ,
                    "photo":"file000626266718.jpg" ,
                    "description":"changed from json"
                }
                For this API call, we only need name, email, and description. Data should be JSON formatted.
                The app will search the database based on the email (not possible to have redendent emails in the DB).
                Then, the app will update the persons name and description as they appear in the call.
Success Response:   Code: 200
                    Content: {"id":3,"name":"Ali Hassan","email":"ali.hassan@gmail.com","location":{"id":2,"name":"Paris","city":"Montevrain","country":"France","remarks":"Lunatech main office in France"},"photo":"file000541344089.jpg","description":"changed from jsonnn"}

Error Response:     400 Bad Request
                    {
                      "status": "KO",
                      "message": "Could not handle your request. Please verify email exists."
                    }
                    This will happen when the request pass email that does not exits in the database.
Sample Call:        curl --request POST  --header "Content-type: application/json"   --data '{"id":16,"name":"Ali Hassan","email":"ali.hassan@lunatech.com","description":"changed from json"}'   http://localhost:9000/api/person/edit
