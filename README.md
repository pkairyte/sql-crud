# FIRST PART
### Tables and import

Creating a table with all the restaurants and their information:

CREATE TABLE restaurants(
id INTEGER, 
Restaurant TEXT, 
Category TEXT, 
Price TEXT, 
Neighborhood TEXT, 
Open_Time TEXT, 
Close_Time TEXT, 
Average_Rating INTEGER, 
Good_for_kids TEXT);

Creating a table with reviews of the restaurants:

CREATE TABLE reviews (
id INTEGER,
Client_Name TEXT,
Review TEXT
);

Importing data:

.mode csv
.separator ","
.headers on

.import data\restaurants.csv restaurants
DELETE FROM restaurants WHERE rowid IN (SELECT rowid FROM restaurants LIMIT 1);

.import data\reviews.csv reviews
DELETE FROM reviews WHERE rowid IN (SELECT rowid FROM reviews LIMIT 1);

### SQL Queries
1. SELECT * FROM restaurants WHERE Price = "Cheap" AND Neighborhood = "Bronx";
2. SELECT * FROM restaurants WHERE Category = "Mexican" AND Average_Rating>=3 ORDER BY Average_Rating DESC; 
3. SELECT * FROM restaurants WHERE strftime('%H:%M', 'now') >= Open_Time AND strftime('%H:%M', 'now') <= Close_Time;
4. SELECT restaurants.id, restaurants.Restaurant, reviews.Review FROM restaurants INNER JOIN reviews ON restaurants.id=reviews.id;
   UPDATE reviews SET review = "Terrible experience, found a hair in my food and the server refused to bring me a new plate" WHERE id=846;
5. DELETE FROM restaurants where good_for_kids = "False";
6. SELECT Neighborhood, count(Restaurant) FROM restaurants GROUP BY Neighborhood; 

# SECOND PART

Creating a users table:

CREATE TABLE users (
id INTEGER PRIMARY KEY,
Username TEXT,
User_Password TEXT,
Email TEXT
);

Creating a posts table:

CREATE TABLE posts (
from_id INTEGER,
to_id INTEGER,
Messages TEXT,
Sent_Time TEXT,
Visibility_Messages INT,
Stories TEXT,
Posted_Time TEXT,
Visibility_Stories INT
);

.mode csv
.separator ","
.headers on

.import data\users.csv users
.import data\posts.csv posts
DELETE FROM posts WHERE rowid IN (SELECT rowid FROM posts LIMIT 1);

### SQL Queries
1. INSERT INTO users (id,Username,User_Password,Email) VALUES (1001,"renom","q2w3e4r5","renom@gmail.com");
2. INSERT INTO posts (from_id,to_id,Messages,Sent_Time) VALUES (1001,97,"Hey, can you help me with SQLite set up?","2022-03-02 23:23:23");
3. INSERT INTO posts (from_id,Stories,Posted_Time) VALUES (1002,"What is going ooooon?!","2022-03-03 01:17:47");
4. SELECT * FROM posts WHERE Visibility_Messages=1 ORDER BY Sent_Time DESC LIMIT 10;
   SELECT * FROM posts WHERE Visibility_Stories=1 ORDER BY Posted_Time DESC LIMIT 10;
5. SELECT * FROM posts WHERE from_id=946 AND to_id=891 AND Visibility_Message=1 ORDER BY Sent_Time DESC LIMIT 10;
6. UPDATE posts SET Visibility_Stories=0 WHERE (SELECT ROUND((JULIANDAY('now', '-5 hours') - JULIANDAY(Posted_Time)) * 24) > 24);
7. SELECT from_id, Stories, Posted_Time, Visibility_Stories FROM posts WHERE Visibility_Stories=0 ORDER BY Posted_Time DESC LIMIT 10;
   UPDATE posts SET Visibility_Messages=0 WHERE (SELECT ROUND((JULIANDAY('now', '-5 hours') - JULIANDAY(Sent_Time)) * 24) > 24);
   SELECT from_id, Messages, Sent_Time, Visibility_Messages FROM posts WHERE Visibility_Messages=0 ORDER BY Sent_Time DESC LIMIT 10;
8. SELECT posts.from_id, users.Username, COUNT(*) FROM posts INNER JOIN users ON posts.from_id=users.id GROUP BY posts.from_id;
9. SELECT posts.Messages, users.Email, users.Username FROM posts INNER JOIN users ON posts.from_id=users.id WHERE (SELECT ROUND((JULIANDAY('now', '-5 hours') - JULIANDAY(Sent_Time)) * 24) <= 24);
10. SELECT users.Email FROM users LEFT JOIN posts ON users.id=posts.from_id WHERE posts.Stories IS NULL; 
   
(For excercises using JULIANDAY added '-5 hours', because of my laptops time zone)