# MongoDB Aggregation Pipeline
MongoDB's Aggregation Framework is a powerful tool that allows you to perform operations on the data stored in your collections. It is used to process data records and return computed results. The aggregation pipeline is a series of stages where each stage transforms the document. The output of one stage becomes the input to the next.

* **Aggregation:** A way of processing a large number of documents in a collection by passing them through different stages.
* **Pipeline:** A series of stages where each stage performs a specific operation on the input document and passes the result to the next stage.
* **Stages:** The individual operations that can be chained together to form a pipeline.

## Common Aggregation Stages

1. **$match:** Filters documents based on a condition.
2. **$group:** Groups documents by a specified field and can perform operations on the grouped data.
3. **$sort:** Sorts the documents.
4. **$project:** Shapes the documents by including, excluding, or adding new fields.
5. **$limit:** Limits the number of documents passed to the next stage.
6. **$unwind:** Deconstructs an array field from the input documents to output a document for each element.

In MongoDB Atlas, you can run aggregation pipelines directly in the Aggregation tab of a collection.

In the MongoDB Shell `(mongosh)`, you can run the aggregation pipeline using the db.collection.aggregate() method. Here's an example:

```

db.users.aggregate([
  { $match: { isActive: true } },
  { $count: "ActiveUser" }
])

```

## MongoDB Aggregation Pipeline Examples

The following MongoDB aggregation pipeline examples are based on data retrieved from Hitesh Chaudary's GitHub Gist. You can visit the [Gist link](https://gist.github.com/hiteshchoudhary/) to access the data and push it to your MongoDB database.

For a better understanding, watch Hitesh Chaudary's MongoDB Aggregation Playlist.


### 1. How Many Users are Active?

```
[
  {
    $match: {
      isActive: true
    }
  },
  {
    $count: "ActiveUser"
  }
]
```
**Output:**

```
{
  "ActiveUser": 516
}
```
This query counts the number of users who are active by matching documents where `isActive` is `true` and then counting the results.


### 2. What is the Average Age of All Users?

* Average age of total collection:

```
[
  {
    $group: {
      _id: null,
      AverageAge: {
        $avg: "$age"
      }
    }
  }
]
```

**Output:**

```
{
  "_id": null,
  "AverageAge": 29.835
}
```
his query calculates the average age of all users by grouping all documents together and then taking the average of the `age` field.


* Average age based on gender:

```
[
  {
    $group: {
      _id: "$gender",
      AverageAge: {
        $avg: "$age"
      }
    }
  }
]
```
**Output:**

```
{
  "_id": "male",
  "AverageAge": 29.851926977687626
}
{
  "_id": "female",
  "AverageAge": 29.81854043392505
}
```
This query calculates the average age for each gender by grouping users by their `gender` and then taking the average age within each group.

### 3. List the Most Common Favorite Fruit Among the Users
```
[
  {
    $group: {
      _id: "$favoriteFruit",
      count: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      count: -1
    }
  },
  {
    $limit: 1
  }
]
```

**Output:**

```
{
  "count": 339,
  "_id": "banana"
}
```
This query finds the most common favorite fruit by grouping users by their favoriteFruit, counting the occurrences of each fruit, and then sorting to find the highest count.

### 4. Find the Number of Males and Females in the Collection

```
[
  {
    $group: {
      _id: "$gender",
      GenderCount: {
        $sum: 1
      }
    }
  }
]
```
**Output:**

```
{
  "_id": "male",
  "GenderCount": 493
}
{
  "GenderCount": 507,
  "_id": "female"
}
```
This query groups users by their gender and counts the number of users in each group.

### 5. Find the User Count Per Country

```
[
  {
    $group: {
      _id: "$company.location.country",
      perUser: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      perUser: -1
    }
  }
]
```
**Output:**

```
{
  "_id": "Germany",
  "perUser": 261
}
{
  "_id": "USA",
  "perUser": 255
}
{
  "_id": "France",
  "perUser": 245
}
{
  "_id": "Italy",
  "perUser": 239
}
```
 This query groups users by the country they are located in and counts how many users belong to each country, then sorts the results by the count.



### 6. List All the Unique Eye Colors Present in the Collection

```

[
  {
    $group: {
      _id: "$eyeColor"
    }
  }
]
```
**Output:**

```

{
  "_id": "blue"
}
{
  "_id": "brown"
}
{
  "_id": "green"
}
```

This query lists all unique eye colors by grouping users by their `eyeColor`.


#### 7. Find the Average Number of Tags Per User

**Approach 1:** Using `$unwind`

*Whenever you deal with an array in MongoDB aggregation pipeline, use the $unwind operator to deconstruct the array. This operator creates duplicates of the documents, with each duplicate containing one element from the array, but all with the same _id as the original document.*

```
[
  {
    $unwind: "$tags"
  },
  {
    $group: {
      _id: "$_id",
      noOfTags: {
        $sum: 1
      }
    }
  },
  {
    $group: {
      _id: null,
      AverageTagsPerUser: {
        $avg: "$noOfTags"
      }
    }
  }
]
```

**Output:**

```
{
  "_id": null,
  "AverageTagsPerUser": 3.556
}
```

**Steps:**

1. Unwind the `tags` array: This creates a separate document for each tag within the array.
2. Group by `_id` to count the number of tags per user.
3. Group using `_id:` `null` to calculate the average number of tags per user.

**Approach 2: Using `$addFields`**

This approach adds a new field to the document using $addFields and then calculates the size of the tags array.

```
[
  {
    $addFields: {
      noOfTags: {
        $size: {
          $ifNull: ["$tags", []]
        }
      }
    }
  },
  {
    $group: {
      _id: null,
      AverageTagsPerUser: {
        $avg: "$noOfTags"
      }
    }
  }
]
```
**Output:**

```
{
  "_id": null,
  "AverageTagsPerUser": 3.556
}
```

This method also returns the same output. Here, we add a new field to the document using `$addFields` and then access the `size` property to calculate the average number of `tags` per user.


### 8. How many users have 'enim' as one of their tags 

```
[
  {
    $match: {
      tags:"enim"
    }
  },
  {
    $count: 'Total No of enim Tags'
  }
]
```
**Output:**

```
{
  "Total No of enim Tags": 62
}
```
This query filters users who have "enim" in their tags array and then counts the results.

### 9. What are the names and age of the users who are inactive and a tag "valit" in their document

```
[
  {
    $match: {
      isActive:false , tags:"velit"
    }
  },
  {
    $project: {
      name:1,
      age:1
    }
  }
  
]
```
In here we filter the document based on tags and isActive status , then further filtered out the document details using project stage


### 10. Find the last three registered users name and favorite fruit

```

[
  {
    $sort: {
      registered: -1
    }
  }
  ,
  {
    $limit: 3
  },
  {
    $project: {
      name:1,
      registered:1,
      favoriteFruit:1
    }
  }

]

```
**Output:**

```

{
  "_id": {
    "$oid": "66c72f43dd088db781effec5"
  },
  "name": "Stephenson Griffith",
  "registered": "2018-04-14T03:16:20+0000",
  "favoriteFruit": "apple"
}
{
  "_id": {
    "$oid": "66c72f43dd088db781effdb6"
  },
  "name": "Sonja Galloway",
  "registered": "2018-04-11T12:52:12+0000",
  "favoriteFruit": "strawberry"
}
{
  "_id": {
    "$oid": "66c72f43dd088db781efffd7"
  },
  "name": "Mcpherson Christensen",
  "registered": "2018-04-11T07:18:42+0000",
  "favoriteFruit": "strawberry"
}
```
This query sorts users by their registration date in descending order, limits the results to the last three users, and then projects their name and favoriteFruit.


### 11. Categorize the user by their favorite fruit
```
[
  {
    $group: {
      _id: "$favoriteFruit",
      users:{
      $push:"$name"
      }
    }
  }
]
```
**Output:**
```
{_id:"strawberry",
users:Array (323) // array with users name with Fav fruit as strawberry
}
,
{_id:"banana",
users:Array (339) // array with users name with Fav fruit as banana
}
,
{_id:"apple",
users:Array (338) // array with users name with Fav fruit as apple
}
```
This query groups users by their favoriteFruit and creates an array of user names for each fruit category.

### 12. Find the users count with "ad" as second property in the tags array

  ```
  [
  {
    $match: {
      "tags.1":"ad"
    }
  },
  {
    $count: 'Users with ad as Second Tag'
  }
]
```
**Output:**

```
{
  "Users with ad as Second Tag": 12
}
```

This query filters users whose second tag in the tags array is "ad" and counts the number of such users

### 13. find users who have both 'enim and 'id' in tags


```
[
  {
  $match:{
    tags:{
      $all:["id","enim"]
    }
  }
}
]
```

**Output:**

```
User Documents with tag name 'enim' and 'id

```
This query filters users who have both "enim" and "id" in their tags array.

### 14. List all companies located in Usa with their corresponding user count

```

[
  {
  $match:{
    "company.location.country":"USA"
  }
}
  ,{
    $group:{
      _id:"$company.title",
      userCount:{
        $sum:1
      }
    }
  }
]
```
**Output:**

```
{
  "_id": "BESTO",
  "userCount": 1
},
{
  "_id": "DANJA",
  "userCount": 1
},
{
  "_id": "KONGLE",
  "userCount": 1
}
.......etc

```
This query filters users based on their company's location being in the USA, groups the results by the company title, and counts the number of users for each company.

