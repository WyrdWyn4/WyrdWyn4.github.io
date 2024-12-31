---
title: TripTailor Github Commit Logs
description:
date: 2024-11-25 00:00:00 +0800
categories: [Projects, TripTailor]
tags: [triptailor, software design, microservices, golang, react.js, postgresql, gin, docker, jwt authorization]
pin: false
math: true
mermaid: true
image: assets/img/covers/trip-tailor/triptailor-commits-cover.png
author: 
permalink: /triptailor/commits
hidden: false
---

Below is a list of all the commits I made for the TripTailor project. The commits are listed in chronological order, with the most recent commit at the end. The commits are grouped by the feature or task that they were related to, in a PR (Pull Request).

Each commit includes a brief description of the changes made and the Jira ticket associated with the task.

---
## Database Implementation
> #### Jira Tickets
 - `Database`: [Connect Database to backend (TT-13)](https://ece6400triptailor.atlassian.net/browse/TT-13)
 - `Database`: [Create User Table (TT-30)](https://ece6400triptailor.atlassian.net/browse/TT-30)

> ### Database
   - Added new package **_Models_**
      - `database.go` is our base implementation for database commands. Basic CRUD operations added in.
      - `user-db.go` is a 1st level implementation for our user table. Uses functions from `database.go` for user-specialized operations.
   - Added new package **_Testing_**
      - `testing-db` is a basic testing unit. I don't know how to make the CLI work with Docker, so I just did everything manually in the `main.go` file to test operations.
   - Added Microservice db folders

> ### Main Program
   - Updated `main.go`
      - Added UserHandler, which can be accessed at http://localhost:8080/users, for initial database review.
      - Added _Database Initialisation_ and _User Table creation_.
   - Updated `go.sum`
      - Added required dependencies for github.com/lib/pq.
   - Updated `go.mod`
      - Added required dependencies for github.com/lib/pq.

> ### Docker
   - Dockerfile
      - Changed `go-container` to `backend-container`
      - Added _COPY_ commands for Docker files.
      - Changed _RUN_ command to directly go to the program start point (`main.go`).
   - Docker Composer
      - Removed Version, as it is an obsolete requirement for composer files.
      - Changed `go-container` to `backend-container`.
   
> ### Notes
1. After some review, I decided not to use `GORM`, instead sticking with `database/sql`. The main benefit that comes from `GORM` is that we can run dynamic queries. However, it comes with way more added complexity, which I think will be difficult for implementation for our team. Poking around in forums online, most of the _Go community_ is against ORMs.
2. I added in _Database Initialization Functions_, _Table Functions_, and _Row Functions_. I couldn't think of any other functions, which is why the team needs to go through these and think what other base functions we can add.
3. Testing has been a big concern. Someone with expertise needs to direct the team on setting up a testing environment.
We also need to have a discussion on error handling critical functions.
In the Dockerfile, I tried to _COPY_ all the files, but it was giving me errors, so I decided to just write up _COPY_ instructions for all folders. Someone else with more docker experience should probably look into that, and try to simplify the Copypaste.
It would be convenient if we can abstract as much as possible by having a very complex implementation for database.go, and then pull in that code to have easier implementations for other microservices, like for example, `user-db` pulling in the `AddRows()` function to implement the `AddUser()` function. However, we need to sit down and see if we can optimise the base functions in `database.go` any further, because every database operation in every microservice will depend on this.
While building in docker, I noticed that a lot of times, the database container would start either after the backend-container, or start in between, which caused the program to crash, as it couldn't implement a database connection. One short solution that I implemented was adding a 3 second delay, which is a justified cause for termination. We need to investigate synchronisation if we have time.

---
## User Table Implementation
> #### Jira Tickets
 - `Database`: [Update User Table with Abdul's columns (TT-48)](https://ece6400triptailor.atlassian.net/browse/TT-48)

> ### Database
   - Updated `user-db.go`
      - `user-db.go` now supports additional columns for sign-up features, including:
         - UserID
         - Username
         - Email
         - Password
         - Date of Birth
      - All functions are now based on a username primary key.
   - Updated `testing-db.go`
      - `testing-db.go` contains code which was used for testing.
      - Re-wrote the entire _test file_ for the new code.

> ### Main Program
   - Updated `main.go`
      - Added DeleteTable() function to clear pre-existing data.
      - This is something Miguel and I were discussing, and we will change this later.

> ### Docker
   - Dockerfile
      - Added _COPY_ commands for auth-service, profile-service, and db/tests.


---
## Database Implementation
> #### Jira Tickets
 - `Database`: [Create Database Tables (TT-50)](https://ece6400triptailor.atlassian.net/browse/TT-50)
 - `Database`: [Add Profile info to Database (TT-57)](https://ece6400triptailor.atlassian.net/browse/TT-57)

> ### Dockerfile
 - Removed COPY `event-service` in the docker file, as event-service folders have been removed, but the docker file wasn't updated. This caused the docker file to not be built.

> ### Main.go
 - Instead of calling individual functions for Deleting and Creating tables, I created one function to delete all tables, and create all tables, and this is now called in `main.go`.

> ### Database.go
Added the following functions and implementations:
 - #### CreateAllTables()
    - This function calls the create functions for each table.
 - #### DeleteAllTables()
     - This function drops all tables.
 - #### IntsToStrings() and StringsToInts()
     - These functions convert to and fro between Ints and Strings
 - #### UpdateAttribute()
     - Instead of creating individual functions for each and every attribute update, I created a general helper function to allow us to do the same task.
     - Through the use of interfaces, different types can be passed into this function.
 - #### AddAttributeArray()
     - In most tables, we are storing IDs as arrays to represent one-to-many or many-to-many relationships. This is a helper function that allows us to Add values to the existing arrays of data.
     - Again, through the use of interfaces, different types can be passed into this function. However, there are issues with reading data other than Strings, which is why I haven't held any other array-type implementations.
 - #### RemoveAttributeArray()
     - Just like the above, this function removes IDs from arrays, rather than add them.

> ### User-db.go
 - #### Column Changes
     - Added Profile columns to the User table _(Name, Country, Languages, Tags)_
     - Added Front-linking array columns to the User table _(Boards and Posts)_
 - #### Function Changes
     - Updated `CreateUserTable()` function
     - `AddUser()` now takes a struct as an argument rather than column values
     - ~`UpdateUser()`~ is now outdated and has been replaced with individual update functions for each column, which call upon `UpdateAttribute()`, `AddAttributeArray()`, and `RemoveAttributeArray()` respectively.
     - User has a One-to-many relationship with Boards. If a Board gets deleted, it will be removed from the User's list of boards, and this logic has been handled in the code.
     - User has a One to Many relationship with Posts. If a Post gets deleted, it will be removed from the User's list of boards, and this logic has been handled in the code.
     - If the User is deleted, all Posts and Boards will also be deleted.

> ### Board-db.go
 - #### Column Changes
     - Added columns _(BoardId, Name, CreationDate, Description, Username, Posts, Tags)_
 - #### Functionality Changes
     - Created Functionality for CRUD Operations.
     - Board has a Many to Many relationship with Posts _(One board can contain many Posts, and the same Post can be on many boards)_

> ### Post-db.go
 - #### Column Changes
     - Added columns _(PostId, ItineraryId, Title, ImageLink, Description, CreationDate, Username, Tags )_
 - #### Functionality Changes
     - Created Functionality for CRUD Operations.
     - Post has a Many to Many relationship with Board _(One board can contain many Posts, and the same Post can be on many boards)_. If a Post is deleted, it will be removed from all Boards, and this logic has been handled in the code.
     - Post has a One to One relationship with Itinerary. In fact, it would be considered a subset. Every Post has an itinerary, but not every Itinerary has a post. If an Itinerary gets deleted, the post also gets deleted, and this logic has been handled in the code.

> ### Itinerary-db.go
 - #### Column Changes
     - Added columns _(ItineraryId, Name, Country, Languages, Tags, Events, PostId, Username, CreationDate, LastUpdate )_
 - #### Functionality Changes
     - Created Functionality for CRUD Operations.
     - Itinerary has a One to One relationship with Post. In fact, it would be considered a subset. Every Post has an itinerary, but not every Itinerary has a post. If an Itinerary gets deleted, the post also gets deleted, and this logic has been handled in the code.
     - Event has a Many to Many relationship with Itineraries. An Itinerary may include multiple events, whereas an event could be a part of multiple itineraries.

> ### Event-db.go
 - #### Column Changes
     - Added columns _(EventId, Name, Price, Location, Description, StartDate, EndDate, ItineraryIds, PhotoLinks)_
 - #### Functionality Changes
     - Created Functionality for CRUD Operations.
     - Event has a Many to Many relationship with Itineraries. An Itinerary may include multiple events, whereas an event could be a part of multiple itineraries.

> ### Authentication Service
 - I refactored [@AbdulTur's](https://github.com/AbdulTur) code to work with the updated database tables.

> ### Notes
 - Since our application use-case is small, I have added linking columns for relationships to avoid complexities with Join tables. Basically, each table will have a tracker linking column for its related table.
   - For example, a Board might contain several Posts, so it will contain a list of those Posts. This is called Frontlinking. 
   - Next, a Post might be contained in several Boards, so it will contain a list of those Boards. This is called Backlinking.
   - Whenever we have any updates to either a post or board, we would need to use these linking columns to apply those changes in the related tables as well.
 - The issue with linking columns is that we wish to store an array of integers (primary key Ids). However, while we can store an array of Integers in PostgreSQL, the scanner has issues reading those integers out. For this reason, we will store the numbers as strings, and when we pull them out, we will convert them back into integers. To help with this, we have the IntsToStrings() and StringsToInts() functions.
 - We need to solve the concurrency issue with the backend starting before the database, as our docker files are getting bigger and bigger, this issue is getting worse. I have to rerun the `docker-compose up --build` command twice almost 90% of the time.
 - While writing out the logic for these, I was referring to our _6400 Assignment 1 - UML Diagram_. However, some things just didn't make sense. For example:
   - The user has direct relationships with posts and boards but not with itineraries. Isn't the source of the itineraries the user himself? How is the user going to create these itineraries?
     - What is the source of Itineraries and Events? Are events just going to be added by the program itself?
     - If the user is the source of all of these things, then we not only need a new UML Diagram, but we also need to refactor our tables for added linking columns and our functionalities for linked column updates.
   - How much effect does the source table have on the drain table?
     - If a user is deleted, do we delete all of their boards and posts?
     - If an itinerary is deleted, do we delete the post as well?
   - What happens to itineraries that are not posted? What's the point of having them in the app? You can not save them to boards (According to our UML Diagram, only a post can be saved to a board)
 - I added Recursive Logic for the Add functions.
   - Basically, if you add a Post to the list of posts in a Board, it should also add the Board to a list of boards in the post.
   - However, if you call the AddPost function in the AddBoard function, and call the AddBoard function in the AddPost function, it leads to infinite recursion.
   - To solve this issue, I added an additional argument, which is recursion = false, to stop that.
   - Whenever we're using any AddPost, AddBoard, etc. functions, we need to set recursion = true so that both tables get updated.
 - Philosophical question, but what the hell is a post?


---
## ConnString Database Function Updates
> #### Jira Tickets
 - `Database`: [Add Connection String Argument (TT-66)](https://ece6400triptailor.atlassian.net/browse/TT-66)

> ### Database
 - Added `DB *sql.DB`, meaning that now you need to pass in the database connection to run any function.

> ### database.go
 - I spoke with [@noahjrc](https://github.com/noahjrc) today, and he mentioned using some outdated functions, which was my mistake.
 - In order to avoid confusion, I removed them. Among them were UpdateRow, AddRow, and others.

> ### Events and Itineraries
 - When you add an Event, it should also add that Event to all Itineraries associated with that Event.
 - Same thing, but replace Event with Itineraries, and vice versa.


---
## Github Code Scanner
> #### Jira Tickets
 - `Database`: [Github Code Scanner (TT-67)](https://ece6400triptailor.atlassian.net/browse/TT-67)

> ### Code Scanner
 - This is a Github Code Scanner
 - It runs at 10 AM, everyday, generating code review reports


---
## Image Storage Implementation 
> #### Jira Tickets
 - `Database`: [Implement Image Storage (TT-65)](https://ece6400triptailor.atlassian.net/browse/TT-65)

> ### DBModels
 - Removed duplicate code in DBModels
 - The main-service/internal/db/models will be used to store the database code, and everyone can copy from here.

> ### User-db.go
 - Added Profile and Cover images for user
 - Added logic to update and remove these images

> ### Post-db.go
 - Added Post Images
 - Added functionality to add or remove these images

> ### Event-db.go
 - Added Event Images
 - Added functionality to add or remove these images

> ### Database.go
 - Updated CreateAllTables and DeleteAllTables functions to add Images table

> ### Images-db.go
 - Created a new table for Images
 - Current columns are:
    - `ImageId`: Numeric unique serial Id
    - `ImageData`: Image Data stored as a string of 'Bytea' type
    - `Metadata`: Tells how the image is being used {profile, cover, event, post}
 - Added the following functions:
    - `InitializeImageTable`: Initializes the base Profile photo and base Cover photo with Ids 1 and 2
    - `CreateImageTable`: Creates the Image Table
    - `AddImage`, `GetImage`, `RemoveImage`: CRUD with Images
    - `AddImageMetaData`, `RemoveImageMetaData`: CRUD with Metadata
    - `ImageToByte`: Can convert image at `imagepath` to storable byte
    - `WebImageToByte`: Can convert image at `imageurl` to storable byte
    - `ByteToImage`: Can convert bytes to image, and store at `outputPath`
 - Added Image Handler
    - Images can now be seen at http://localhost:8080/images/{id}

> ### Notes
 - Base Profile photo is stored at http://localhost:8080/images/1
 - Base Cover photo is stored at http://localhost:8080/images/2
 - Run the code and open http://localhost:8080/images/4 : )

---
## Database ReLogic
> #### Jira Tickets
 - `Database`: [Database Logic Revision (TT-71)](https://ece6400triptailor.atlassian.net/browse/TT-71)
 - `Database`: [Update Tables (TT-81)](https://ece6400triptailor.atlassian.net/browse/TT-81)

> ### Recursive Logic Functions
 - Recursive Logic functions were used for many-to-many relationship table updates (If A updates, B must also update)
 - Fixed functionality _(Accidentally used conditional statement at the wrong place)_

> ### Auth Service
 - Updated User struct
 - Renamed `user-repository.go` to `user-db.go` to ensure naming consistency throughout code.

> ### Main Service
 - In addition to Recursive Logic Functions, added `UpdateName` function, from Profile Service

> ### Profile Service
 - Updated Tables

> ### Search Service
 - Updated Tables
 - Renamed `itinerary.go` to `itinterary-db.go` to ensure naming consistency throughout code.
 - Renamed `event.go` to `events-db.go` to ensure naming consistency throughout code.

> ### Column Changes
 - Removed many-to-many relationship between `Itineraries` and `Events`
 - Updated `Itinerary`, `Post` and `Event` Columns
    - `Posts`:
          - Added Likes and Comments
          - Removed Title, Description, PostImages, Tags
    - `Itineraries`:
          - Added Title, Description, Price
          - Removed CreationDate and LastUpdate
    - `Events`:
          - Renamed Location to Address
          - Renamed StartDate to StartTime
          - Renamed EndDate to EndTime
          - Renamed Price to Cost, and made it a floating point value.

> ### Comments Table
 - Added `Comments` table

> ### Notes
 - Ran all the services:
    - Main, Auth and Search services, work fine
    - No idea how to test Profile service _(I guess it is working, I created an account?)_
    - Please see below
 
> ![Service Run Image](https://github.com/user-attachments/assets/47727239-db27-467b-a812-14ea31f5eee0)

---
## Add Functions Optimization
> #### Jira Tickets
  - `Database`: [Optimize Add Functions (TT-83)](https://ece6400triptailor.atlassian.net/browse/TT-83)

> ### post-db.go
 - Updated the `AddPost()` function so that it automatically adds the `Post` to the `User's` list of posts as well.
 - Now you no longer need to call the `AddUserPost()` function
 - Post now returns (postID, err)

> ### board-db.go
 - Updated the `AddBoard()` function so that it automatically adds the `Board` to the `User's` list of posts as well.
 - Now you no longer need to call the `AddUserBoard()` function
 - Board now returns (boardID, err)

---
## My Travels Page Implementation
> #### Jira Tickets
 - `Frontend`: [Display itineraries in My itineraries (TT-76)](https://ece6400triptailor.atlassian.net/browse/TT-76)
 - `Frontend`: [Profile Editing Menu (TT-79)](https://ece6400triptailor.atlassian.net/browse/TT-79)
 - `Frontend`: [Create UI for boards/saved itineraries (TT-89)](https://ece6400triptailor.atlassian.net/browse/TT-89)
 - `Frontend`: [Move add image button from itinerary to individual event (TT-92)](https://ece6400triptailor.atlassian.net/browse/TT-92)
 - `Database`: [Add followers and following to db (TT-82)](https://ece6400triptailor.atlassian.net/browse/TT-82)
 - `Saves`: [Save Service (TT-90)](https://ece6400triptailor.atlassian.net/browse/TT-90)

> #### Docker Changes
 - `docker-compose.yml`
   - Added Save-Service Container
   - Renamed `intinerary-service` to `itinerary-service`
 - `itinerary-service/Dockerfile`
   - Changed `Expose 8083` to `EXPOSE 8083`
 - `save-service/Dockerfile`
   - Setup Save-Service Dockerfile, for Port 8086
 - `save-service/wait-for-it.sh`
   - Ctrl C'd [@MiguelPond's](https://github.com/MiguelPond) wait-for-it function

> #### Database Changes
 - `user-db.go`
   - Added Followers and Following.
   - Fixed `RemoveUser()` function.
   - Added Add/Remove functions for `Followers `and `Following `columns.
 - `board-db.go`
   - Fixed `RemoveBoard()` function.
 - `post-db.go`
   - Added `UpdatePostItinerary()` function, in order to sync `Posts` and `Itineraries`.
 - `itinerary-db.go`
   - Removed `Name` Column.
   - Fixed `GetItinerary()` function.
   - In the `AddItinerary()` function, added a function call to `UpdatePostItinerary()`, in order to sync `Posts` and `Itineraries`.
   - Added Handling for `Nil` Languages, Tags and Events, making them an empty array

> #### Backend Changes
 - `Save-Service`
   - Added standard files, like main.go, etc. for the container.
   - Added `routes.go`
      - `r.GET("/boards")` -> gets Boards, given UserId.
      - `r.GET("/posts")` -> gets Posts, given BoardId.
      - `r.GET("/itineraries")` -> gets Itinerary, given PostId.
      - `r.GET("/events")` -> gets Events, given ItineraryId.
      - `r.DELETE("/boards/:boardId/posts/:postId")` -> Removes Post from Board.
      - `r.DELETE("/boards/:boardId")` -> Deletes Board (If we have an implementation where you can save a board, we would have to make a deep copy of the board, or we would have to change this implementation).
 - `Search-Service`
   - Removed `Name` from `itinerary_repository.go` and `itinerary_repository_test.go`.
   - Updated test cases in `itinerary_repository_test.go` with removed `Name` column.
   - Updated `itineraries.sql` to replace `name` with `title`.
   - Updated `test.go` to replace `Name` with `Title`.

> #### Frontend Changes
 - `App.js`
   - Added MyBoards and BoardPosts imports and routes.
 - `boardAPI.js`
   - Created boardAPI.js.
 - `Navbar.js`
   - Replaced itinerary.name with itinerary.title.
 - `AccountSettings.js`
   - Added routing to each button on the account settings page.
 - `BoardPosts.js`
   - Created Frontend for the Board Posts Page.
   - This displays all Posts for a specific Board, and displays them.
   - Within the code, data for all Posts, Itineraries, and Events, in the order of relationship to the Board, are pulled.
   - The Post will display one of the Event images, otherwise, it'll default to a Minecraft Image
   - Posts can also be deleted from Boards, by hovering over and pressing a red $${\color{red}X}$$ on the top-right.
   - Just to clarify, this only removes Posts from Board, but does not delete them. The posts still belong to the user.
 - `CreateItinerary.js`
   - Moved the Add Image icon from the itinerary section to each individual event.
 - `EditUserProfile.js`
   - Created Frontend for Edit User Profile page, where users can update their personal info including:
      - Name
      - Preferred Tags
      - Country of Residence
      - Spoken Languages
   - **_(Can someone please verify and likely change the integration to backend in this file :))_**
 - `InitialUserProfile.js`
   - Added a few countries
 - `SearchResults.js`
   - Removed unused code
 - `UserBoards.js`
   - Created Frontend for the My Boards page.
   - This fetches all Boards for the user, and displays them.
   - Clicking a Board will take you to the BoardPosts.js page.
   - Boards can also be deleted, by hovering over and pressing the white X on the top-right.
   - At the moment, Boards have random Minecraft Images for display. (I have a bunch of functions pulling Board, Post, Itinerary, and Event data, and I used this in BoardPosts.js to display Event Images for related Posts. Perhaps someone can look into either reusing that code, or they could standardize it into one function, and call it everywhere.)
 - `UserDashboard.js`
   - This page is an overhead page for both "My Boards" and "My Itineraries", called "My Travels". It allows users to select either tab to visit that page.
 - `UserItineraries.js`
   - This is the My Itineraries page. It displays the user's itineraries, similar to the My Boards page. **_(idk if the get itineraries endpoint actually exists yet. If someone could verify this functionality that would be great)_**
 - `UserLogin.js`
   - Routes logged in users to home page


---
## Database Array + Itinerary Cost Fix
> #### Jira Tickets
 - `Database`: [Cost not being calculated for Itineraries (TT-98)](https://ece6400triptailor.atlassian.net/browse/TT-98)
 - `Database`: [Fix Arrays (TT-99)](https://ece6400triptailor.atlassian.net/browse/TT-99)

> #### Database Changes
 - `user-db.go`
   - Added Null check for all arrays (`Followers`, `Following`, `Languages`, and `Tags`), in Add and Get functions
 - `board-db.go`
   - Added Null check for all arrays (`Posts`, `Tags`), in Add and Get functions
   - Added Missing Loop for Adding Posts passed into `AddBoard()`
 - `post-db.go`
   - Added Null check for all arrays (`Board`, `Comments`), in Add and Get functions
   - Added Missing Loop for Adding Boards passed into `AddPost()`
 - `itinerary-db.go`
   - Added `UpdateItineraryPrice()` in `search-service` itinerary-db file
   - Fixed `itinerary-service` itinerary-db file to match others
 - `event-db.go`
   - Added Null check for all arrays (`Images`), in Add and Get functions
   - Called `UpdateItineraryPrice()` Function in `AddEvent()`
 - `images-db.go`
   - Added Null check for all arrays (`Metadata`), in Add and Get functions

---
## Itinerary Image Display Fix
> #### Jira Tickets
 - `Frontend`: [Display Images from DB (TT-100)](https://ece6400triptailor.atlassian.net/browse/TT-100)

> ### UserItineraries.js
 - Added `fetchevents()` function, which reuses `save-service` event fetching.
 - Replaced `Itineraries` with `itineraryEvents`, a 2D array which stores all Itineraries and Events.
 - Fixed Display code to show images

> ### Pack Data
 - Added additional data to pack, for user `Herobrine`

---
## Save Service Endpoint Implementation 
> #### Jira Tickets
 - `Frontend + Backend`: [Add Endpoints for Save-service (TT-109)](https://ece6400triptailor.atlassian.net/browse/TT-109)

> ### Save Service
 - #### Routes.go
    - Added `\addboardpost`, `\addboard`, and `\editboard` endpoints.
 - #### AddBoard.go
    - `AddBoard.go` takes the username and boardname, and creates a new board.
 - #### AddBoardPosts.go
    - `AddBoardPosts` takes BoardId and PostId, and attaches the post to the board.
 - #### EditBoard.go
    - `EditBoard` takes the boardId, board name, and board description, and updates them.
 - #### ItineraryGrid.js
    - Added `/addboardpost` endpoint implementation
    - Added `/addboard` endpoint implementation
        - [@allymreid](https://github.com/allymreid), I implemented it in a way that when the user clicks "Create", it also automatically saves the itinerary to the board.
 - #### BoardPosts.js
    - Added delete `/post` endpoint implementation
    - Added `/editboard` endpoint implementation
        - Fixed UI for editing the board.

> ### Pack Data
 - Fixed EventId mismatches
 - Reset all Itinerary Prices to 0.00