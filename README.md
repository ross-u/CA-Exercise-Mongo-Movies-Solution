# Code Along - MongoDB Movies

 
<br>


## Intro

![img](https://i.imgur.com/NzalR30.png)

When working with any Database, there are a few operations that we will always need to use. These operations are so popular that we have an acronym for it: CRUD stands for create, read, update and delete:

![img](https://i.imgur.com/CRowB2i.png)

## Getting Started

<br>



### Fork and Clone the repo

```bash
git clone https://github.com/ross-u/Code-Along-Mongo-Movies

cd Code-Along-Mongo-Movies
```



<br>



### Import the data via `mongoimport`

Using `mongoimport`, import the JSON data from the `movies.json` file into the collection `movies` in the `video` database.

### Linux

```bash
# Run mongod
sudo service mongod start

# Check if mongod process is running
ps -e | grep 'mongod'

# Import the data from `movies.json` to the `movies` database
mongoimport --db video --collection movies --file movies.json --jsonArray

# Run mongo shell
mongo
```

<br>


### Mac

```bash
# Run mongodb
brew services start mongodb-community

# Check if mongod process is running
ps -e | grep 'mongod'


# Import the data from `movies.json` to the `movies` database
mongoimport --db video --collection movies --file movies.json --jsonArray

# Run mongo shell
mongo
```

<br>


#### ... continued (Mac & Linux)

```js
# List databases
show dbs

# Switch to database `video`
use video

# Show collections
show collections

# List all the documents in the `movies` collection
db.movies.find().pretty()
```

<br>

## Tasks


### 1. Retrieve all the documents  from the `movies` collection:

**<u>Your query</u>**:

```js
 db.movies.find()
```

<br>

 

### 2. Retrieve all the documents  from the  `movies` collection and prettify the result:

**<u>Your query</u>**:

```js
 db.movies.find().pretty()
```

<br>
 

### 3. Retrieve all the documents  from the  `movies` collection where the field `year` is  2000:

**<u>Your query</u>**:

```js
db.movies.find({year:  2000})
```

<br>

 

### 4. Retrieve all the documents from the `movies`  collection where the field `rate` is "8.8"

**<u>Your query</u>**:

```js
db.movies.find({rate:  "8.8"})
```

<br>

 

#### 5. Retrieve the movie with the field `_id` "5cbf03ca570ffc7ef7ac4861"

To specify an **ObjectID**, use the format **ObjectId(’id’)**, 

**<u>Your query</u>**:

```js
db.movies.find( {_id: ObjectId("5cbf03ca570ffc7ef7ac4861")} )
```

 
<br>


### 6.  Retrieve all documents in our collection where the field `year` equals `2000` **AND** the `rate` equals `'8.5'`.

**<u>Your query</u>**:

```js
db.movies.find({ $and: [{year: 2000}, {rate:  "8.5"}]})
```

<br>

 

### 7. Retrieve all documents from the `movies` collection where the field `year` equals `2000` **OR** the `year` equals `2005`.

**<u>Your query</u>**:

```js
db.movies.find({ $or: [{year: 2000}, {year:  2005}]})
```

 
<br>


### 8. Retrieve all documents from the `movies` collection that do not have a rate of “9.0” and limit the result to 10 documents.

**<u>Your query</u>**:

```js
db.movies.find({ rate: { $ne: "9.0"} }).limit(10)
```

<br>


### 9. Retrieve all documents from the `movies` collection where field `director` is `not` "Steven Spielberg" `or` "Quentin Tarantino".  

### Display only `title` and `director` fields for each document.

**<u>Your query</u>**:

```js
  db.movies.find({ 
    $nor: [
      {director:"Steven Spielberg"},
      { director:"Quentin Tarantino"}
    ] 
  }, 
  	{title: 1, director: 1, _id: 0}
  )
```

 
<br>


### 10. Retrieve all documents from the `movies` collection, using the projection return only the fields `title`, `year`, `genre` and exclude field `_id`.

**<u>Your query</u>**:

```js
 db.movies.find({}, {title: 1, year: 1, genre: 1, _id: 0})
```

 
<br>


### 11. Retrieve all documents from the `movies` collection including only `title` field , using and sort them by `name` 

**<u>Your query</u>**:

```js
db.movies.find({}, {title: 1}).sort({title: 1})
```


<br>

 

### 12. Retrieve all documents from the `movies` collection including only `title`  and `director` fields ,  sort them by `name` , skipping first 5 documents

**<u>Your query</u>**:

```js
db.movies.find({}, {title: 1, director: 1}).sort({title: 1}).skip(5)
```

<br>

 

### 13. Retrieve all documents from the `movies` collection where `director` field `equals`  "Robert Zemeckis"

**<u>Your query</u>**:

```js
  db.movies.find({ director: {$eq: "Robert Zemeckis"} } , {director: 1, title: 1})
```


<br>

 

### 14. Retrieve all documents from the `movies` collection where the `rate` field is different than "8.5". Using projection include only `title` and `rate` field, excluding `_id`

**<u>Your query</u>**:

```js
db.movies.find({ rate: {$ne: "8.5"} } , {title: 1, rate: 1, _id: 0})
```

 
<br>


### 15. Retrieve all documents from the `movies` collection where the `year` field is greater than or equal 2015. Using projection include only `title` , `year`and `director` fields.

**<u>Your query</u>**:

```js
  db.movies.find({ year: {$gte: 2015} } , {title: 1, year: 1, director: 1, _id: 0})
```


<br>

 

### 16. Retrieve all documents from the `movies` collection where the `year` field is less than 2000. Using projection include only `title` , `year`and `director` fields.

**<u>Your query</u>**:

```js
db.movies.find({ year: {$lt: 2000} } , {title: 1, year: 1, director: 1, _id: 0})
```


<br>

 

### 17. Retrieve all documents from the `movies` collection created in the years 2000, 2005 and 2010. Sort the result by `year` in ascending order.

**<u>Your query</u>**:

```js
  db.movies.find({ year: {$in: [2000, 2005, 2010] } } , {title: 1, year: 1}).sort({year: 1})
```

 
<br>


### 18. Retrieve all documents from the `movies` collection created in the years 1999 and 2010 and excluding the movie with `title` "Inception"

```js
  db.movies.find({ $and: [{ year: {$in: [1999, 2010] }}, { title: { $ne: "Inception" } }] }, {title: 1, year: 1})
```


<br>



### 19. Delete documents from the `movies` collection created in the year 1999. 

**<u>Your query</u>**:

```js
  db.movies.deleteMany({ year: 1999 })
```


<br>



### 20. Delete document from the `movies` collection with the `_id`  "5cbf03ca570ffc7ef7ac4869".

 **<u>Your query</u>**:

```js
  db.movies.deleteOne({ _id: ObjectId("5cbf03ca570ffc7ef7ac4869") })
```

 
<br>


### 21. Update all documents from the `movies` collection created in the `year` 2017 and add an additional field named `rating`: 

```js
var ratingObj = {
  rating: "TV-G",
  "violence": false,
  "drug_use": false,
  "strong_language": false
}
```

###  

**<u>Your query</u>**:

```js
db.movies.updateMany({ year: 2017 }, {$set: {rating: ratingObj}})
```

 
<br>




### 22. Update the document from the `movies` collection with  `title` "Dunkirk" and set `rating`  to "PG-13" and fields  `violence` and `strong_language` to `true`  : 



**<u>Your query</u>**:

```js
 db.movies.updateMany(
   { title: "Dunkirk" },
   {$set: { 
     "rating.rating": "PG-13",
     "rating.violence": true,
     "rating.strong_language": true
   }})
```

 

<br>



### 23. Delete documents from the `movies` collection using the titles provided in the array:

```js
let moviesToDelete = ["12 Angry Men", "Se7en", "Cidade de Deus", "Braveheart"]
```

###  

**<u>Your query</u>**:

```js
  db.movies.deleteMany(
    { 
      title: { 
      	$in: ["12 Angry Men", "Se7en", "Cidade de Deus", "Braveheart"] 
    	} 
    }
  )
```

