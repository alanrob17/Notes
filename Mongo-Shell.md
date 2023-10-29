## MongoDB

[Documentation for MongoDB Node.js driver](https://mongodb.github.io/node-mongodb-native/ "Documentation for MongoDB Node.js driver")

[MongoDB manual](https://docs.mongodb.com/manual/ "MongoDB manual").

### MongoDB executables

In the **bin** folder.

**mongod.exe** Is the server and should be running from startup as a service.

If you need to restart the server.

```bash
    mongod --config mongod.cfg
```

``mongod.cfg`` has the server setup configuration including where install our databases.

**mongo.exe** Is how we connect to MongoDB it is our Mongo shell.

### Mongo shell

Mongo has a command shell and it is useful to have a quick look at your databases. I have added some of the main commands down below.

To see the databases.

```bash
    show dbs;
```

> admin                  0.000GB  
> classes                0.000GB  
> config                 0.000GB  
> devcamper              0.000GB  
> files                  0.015GB  
> local                  0.000GB  
> meanhotel              0.000GB  
> record-db              0.004GB  
> task-manager           0.000GB  
> task-manager-api       0.000GB  
> task-manager-api-test  0.000GB  
> test                   0.000GB  
> wikiDB                 0.000GB  

### Importing sample data

Copy the json sample data into your folder.

``exit`` to exit out of mongo shell. Run the commands.

```bash
    mongoimport --db cooker examples.json
    mongoimport --db cooker recipes.json
    mongoimport --db cooker users.json
```

When you run ``show dbs;`` this time you will see your *cooker* database.

### Documents

Are field-value pairs stored in JSON like BSON (binary JSON for reducing size).

To change to a database.

```bash
    use cooker;
```

To create a document in mongo.

```bash
    doc = { "title":"Tacos", "desc":"Yummy tacos", "cook_time":20 };
```

We need somewhere to store the document.

```bash
    db.tacos.insertOne(doc);
```

This will insert one document into the **tacos**  collection. If the **tacos** collection doesn't exist MongoDB will create it.

To search for the documents in a collection.

```bash
    db.tacos.find();
```

will show you.

> { "_id" : ObjectId("5f4af4016779fb2cba44625d"), "title" : "Tacos", "desc" : "Yummy tacos", "cook_time" : 20 }

Notice that there is a new field, ``_id``. The ``_id`` field is required in every document in MongoDB. every ``_id`` is unique in MongoDB and contains an encoded datetime field set to when the document was created which can be used for sorting.

We can also format the previous command.

```bash
     db.tacos.find().pretty();
```

> {  
>         "_id" : ObjectId("5f4af4016779fb2cba44625d"),  
>         "title" : "Tacos",  
>         "desc" : "Yummy tacos",  
>         "cook_time" : 20  
> }  

Create another document.

```bash
    doc2 = { "title":"Dos Tacos", "desc":"Even more yummy tacos", "cook_time":20 };
```

Insert it.

```bash
    db.tacos.insertOne(doc2);
```

Run the ``find()`` command again.

> {  
>         "_id" : ObjectId("5f4af4016779fb2cba44625d"),  
>         "title" : "Tacos",  
>         "desc" : "Yummy tacos",  
>         "cook_time" : 20  
> }  
> {  
>         "_id" : ObjectId("5f4af82fcd9cfb1ae53706be"),  
>         "title" : "Dos Tacos",  
>         "desc" : "Even more yummy tacos",  
>         "cook_time" : 20  
> }  

Now we can refine our search.

```bash
	db.tacos.find({title : 'Tacos'}).pretty();
```

> {  
>         "_id" : ObjectId("5f4af4016779fb2cba44625d"),  
>         "title" : "Tacos",  
>         "desc" : "Yummy tacos",  
>         "cook_time" : 20  
> }  

### What can we store in a Mongo document?

We can store a number of different types in MongoDB. The following code stores an array for the directions.

```bash
    {
      "title": "Apple Pie",
      "directions": [
        "Roll the pie crust",
        "Make the filling",
        "Bake"
      ]
    }
```

``directions`` is literally a JavaScript style array.

Look at the *ingredients.json* file that expands on the previous example.

```bash
    {
      "title": "Apple Pie",
      "directions": [
        "Roll the pie crust",
        "Make the filling",
        "Bake"
      ],
      "ingredients": [
         {
           "amount": {
             "quantity": 2,
               "unit": null
             },
            "name": "pie crusts"
        }, {
           "amount": {
             "quantity": 1,
               "unit": "tbsp"
            },
            "name": "cinnamon"
        }
      ]
    }
```

This time ingredients is an array of objects. It also has objects within objects.

We can get more complex than this. The following is the first recipe in recipes.json.

```bash
    {
    	"_id": {
    		"$oid": "5e6fd805fa98021236426a24"
    	},
    	"title": "Chicken Soft Tacos",
    	"calories_per_serving": 205,
    	"cook_time": 19,
    	"desc": "Mexican soft tacos",
    	"directions": [
    		"Put seasoning on chicken breasts",
    		"Grill until done",
    		"Chop chicken into peices",
    		"Put in totillas"
    	],
    	"ingredients": [
    		{
    			"name": "chicken breast",
    			"quantity": {
    				"amount": 1,
    				"unit": "lbs"
    			}
    		},
    		{
    			"name": "taco seasoning",
    			"quantity": {
    				"amount": 2,
    				"unit": "oz"
    			}
    		},
    		{
    			"name": "small flour totillas",
    			"quantity": {
    				"amount": 12,
    				"unit": "oz"
    			}
    		}
    	],
    	"likes": [
    		261,
    		1,
    		415
    	],
    	"likes_count": 3,
    	"prep_time": 10,
    	"rating": [
    		4,
    		4,
    		4,
    		4,
    		2,
    		5,
    		3
    	],
    	"rating_avg": 3.71,
    	"servings": 5,
    	"tags": [
    		"mexican",
    		"quick",
    		"easy",
    		"chicken"
    	],
    	"type": "Dinner"
    }
```

### Databases and Collections

Creating documents with nested data can quickly become unmanageable. This can slow down processing.

This is where **collections** come in. In collections we can keep related documents together making it easier to query the data. This leads to better performance.

For example we can search for all documents in a collection and only show particular fields.

```bash
    db.recipes.find({}, {"title": 1});
```

> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos" }  
> { "_id" : ObjectId("5e877cba20a4f574c0aa56da"), "title" : "Pancakes" }  
> { "_id" : ObjectId("5e87856d07beb474c074c5ca"), "title" : "Brown Sugar Meatloaf" }  
> { "_id" : ObjectId("5e878f5220a4f574c0aa56db"), "title" : "Maple Smoked Salmon" }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }  
> { "_id" : ObjectId("5edf1d313260aab97ea0d589"), "title" : "Zucchini Brownies" }  
> { "_id" : ObjectId("5edf1cd43260aab97ea0d588"), "title" : "Apple Pie" }  

``{}`` tells us to find all documents. ``{"title": 1}`` means only show the title field and the ``1`` means include the field in the output.

To show the current database.

```bash
    db.getName();
```

> cooker

To show all collections in a particular database.

```bash
    show collections;
```

In our *cooker* database we have the following collections.

> examples  
> recipes  
> tacos  

To create a new empty collection.

```bash
    db.my_new_stuff.insertOne({});
```

**Note:** I can't use a dash **-** in a collection name.

You can also create sub documents.

```bash
    db.my_new.stuff.insertOne({});
```

``show collections`` will return

> examples  
> my_new.stuff  
> my_new_stuff  
> recipes  
> tacos  

Sub documents don't store your data any differently but it can be helpful for query purposes.

You can also create **capped** collections. This is where we tell MongoDB that we only want to create collections of a particular size.

In these collections when we insert a new document and the collection is full it will automatically remove the oldest document and then create the new one.

### Search Record DB

```bash
    use record-db
```

Show artists.

```bash
    db.artists.find({}, {"name": 1});
```

Returns

> { "_id" : "5f3defa4163278d1ac061c64", "name" : "Mick Abrahams" }  
> { "_id" : "5f3defa4163278d1ac061c65", "name" : "Geoff Achison" }  
> { "_id" : "5f3defa4163278d1ac061c66", "name" : "William Ackerman" }  
> { "_id" : "5f3defa4163278d1ac061c67", "name" : "Ryan Adams" }  
> { "_id" : "5f3defa4163278d1ac061c68", "name" : "Duane Allman" }  
> { "_id" : "5f3defa4163278d1ac061c69", "name" : "Greg Allman" }  
> { "_id" : "5f3defa4163278d1ac061c6a", "name" : "The Allman Brothers Band" }  
> ...  

To search for an artist's records.

```bash
    db.records.find({"artist": ObjectId("5f3defa4163278d1ac061c9a")}, {"name": 1});
```

> { "_id" : ObjectId("5f49c30db8661243744d7c96"), "name" : "Michael Brecker" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c97"), "name" : "Don't Try This At Home" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c98"), "name" : "The Cost Of Living" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c99"), "name" : "Now You See It...(Now You Don't)" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9a"), "name" : "Tales From The Hudson" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9b"), "name" : "Two Blocks From The Edge" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9c"), "name" : "Time Is Of The Essence" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9d"), "name" : "Nearness Of You" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9e"), "name" : "Directions In Music" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9f"), "name" : "Pligrimage" }  

Note that in the search by ``_id`` I have to wrap ``_id`` in the ``ObjectId()`` to be able to search correctly. It won't search on a string value.

### Sort, limit and skip

When you do a query on a database, MongoDB sends back a ``cursor``. The ``cursor`` doesn't run straight away. It waits for the data to be requested. This happens automatically unless you manually iterate on the cursor. It is important to be aware of this even though you don't see it in operation that often.

What this allows you to do is run other commands after the ``find()`` command and this is called **chaining**.

#### count()

```bash
	db.records.find({"artist": ObjectId("5f3defa4163278d1ac061c9a")}, {"name": 1}).count();
```

> 10

#### limit()

Limit the number of documents sent back.

```bash
	db.records.find({"artist": ObjectId("5f3defa4163278d1ac061c9a")}, {"name": 1}).limit(5);
```

#### sort()

```bash
	db.records.find({"artist": ObjectId("5f3defa4163278d1ac061c9a")}, {"name": 1}).sort({ "recorded": 1 });
```

Will sort on the *recorded* field in an ascending direction.

> { "_id" : ObjectId("5f49c30db8661243744d7c96"), "name" : "Michael Brecker" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c97"), "name" : "Don't Try This At Home" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c98"), "name" : "The Cost Of Living" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c99"), "name" : "Now You See It...(Now You Don't)" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9a"), "name" : "Tales From The Hudson" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9b"), "name" : "Two Blocks From The Edge" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9c"), "name" : "Time Is Of The Essence" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9d"), "name" : "Nearness Of You" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9e"), "name" : "Directions In Music" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9f"), "name" : "Pilgrimage" }  

To sort descending.

```bash
	db.records.find({"artist": ObjectId("5f3defa4163278d1ac061c9a")}, {"name": 1}).sort({ "recorded": -1 });
```

> { "_id" : ObjectId("5f49c30db8661243744d7c9f"), "name" : "Pilgrimage" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9e"), "name" : "Directions In Music" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9d"), "name" : "Nearness Of You" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9c"), "name" : "Time Is Of The Essence" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9b"), "name" : "Two Blocks From The Edge" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c9a"), "name" : "Tales From The Hudson" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c99"), "name" : "Now You See It...(Now You Don't)" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c98"), "name" : "The Cost Of Living" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c97"), "name" : "Don't Try This At Home" }  
> { "_id" : ObjectId("5f49c30db8661243744d7c96"), "name" : "Michael Brecker" }  

Show two fields.

```bash
	db.records.find({"artist": ObjectId("5f3defa4163278d1ac061c9a")}, { "name": 1, "recorded": 1}).sort({ "recorded": -1 });
```

### Working with operators and arrays

#### $gt, $lt, $gte or $lte

Find the number of records recorded greater than or equal to 2000.

```bash
	db.records.find({ "recorded": { $gte: 2000 }}, { "title": 1 }).count();
```

> 358

#### $or

```bash
	db.records.find({ $or: [{ "recorded": 1974}, {"recorded": 1975 }]}, { "name": 1, "recorded": 1 });
```

> { "_id" : ObjectId("5f49c30db8661243744d7c16"), "name" : "Stacked Deck", "recorded" : 1975 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c24"), "name" : "Big Red Rock", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c25"), "name" : "Big Red Rock [Remastered]", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c37"), "name" : "Northern Lights, Southern Cross", "recorded" : 1975 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c51"), "name" : "Blow By Blow", "recorded" : 1975 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c52"), "name" : "Blow By Blow", "recorded" : 1975 }  
> ...  

Add a ``sort()`` to that query.

```bash
	db.records.find({ $or: [{ "recorded": 1974}, {"recorded": 1975 }]}, { "name": 1, "recorded": 1 }).sort({ "recorded": 1 });
```

> { "_id" : ObjectId("5f49c30db8661243744d7c24"), "name" : "Big Red Rock", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c25"), "name" : "Big Red Rock [Remastered]", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c6a"), "name" : "Fields Of November", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7c95"), "name" : "Sor & Giulani", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7ca0"), "name" : "Jim Brewer", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7ca9"), "name" : "Late For The Sky", "recorded" : 1974 }  
> { "_id" : ObjectId("5f49c30db8661243744d7caa"), "name" : "Late For The Sky", "recorded" : 1974 }  
> ...  

### Filtering on tags in the recipe collection

In the *recipe* collection find recipes with the tag **easy**.

```bash
	db.recipes.find({ "tags": "easy" }, { "title": 1, "tags": 1 });
```

> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos", "tags" : [ "mexican", "quick", > "easy", "chicken" ] }  
> { "_id" : ObjectId("5e87856d07beb474c074c5ca"), "title" : "Brown Sugar Meatloaf", "tags" : [ "groud beef", > "family meal", "easy" ] }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos", "tags" : [ "mexican", "quick", "easy", > "ground beef" ] }  
> { "_id" : ObjectId("5edf1d313260aab97ea0d589"), "title" : "Zucchini Brownies", "tags" : [ "sweets", "easy" ] }  

What if we wanted to search for tags with *quick* **and** *easy*?

```bash
	db.recipes.find({ "tags": { $all: ["easy", "quick"] }}, { "title": 1, "tags": 1 });
```

> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos", "tags" : [ "mexican", "quick", "easy", "chicken" ] }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos", "tags" : [ "mexican", "quick", "easy", "ground beef" ] }  

Searching for tags with *quick* **or** *easy*?

```bash
	db.recipes.find({ "tags": { $in: ["easy", "quick"] }}, { "title": 1, "tags": 1 });
```

> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos", "tags" : [ "mexican", "quick", "easy", "chicken" ] }  
> { "_id" : ObjectId("5e87856d07beb474c074c5ca"), "title" : "Brown Sugar Meatloaf", "tags" : [ "groud beef", "family meal", "easy" ] }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos", "tags" : [ "mexican", "quick", "easy", "ground beef" ] }  
> { "_id" : ObjectId("5edf1d313260aab97ea0d589"), "title" : "Zucchini Brownies", "tags" : [ "sweets", "easy" ] }  

If we want to reach into objects of an array we use dot **.** notation.

```bash
	db.recipes.find({ "ingredients.name": "egg" }, { "title": 1 });
```

> { "_id" : ObjectId("5e877cba20a4f574c0aa56da"), "title" : "Pancakes" }  
> { "_id" : ObjectId("5edf1d313260aab97ea0d589"), "title" : "Zucchini Brownies" }  

### Updating documents

To search for a document ``_id``.

```bash
	db.examples.find({ "_id": ObjectId("5ee69d943260aab97ea0d58d") });
```

> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Pizza" }

Change the title.

db.examples.find({}, { "title": 1 });

> { "_id" : ObjectId("5ee69e393260aab97ea0d58e"), "title" : "Delete me" }  
> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Pizza" }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }  

```bash
	db.examples.update({ "title": "Pizza" }, { $set: { "title": "Thin crust pizza" }});
```

> WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> { "_id" : ObjectId("5ee69e393260aab97ea0d58e"), "title" : "Delete me" }  
> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Thin crust pizza" }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }  

Add another field in the document.

```bash
	db.examples.updateOne({ "title": "Thin crust pizza" }, { $set: { "vegan": false }});
```

Find that document.

```bash
	db.examples.find({ "title": "Thin crust pizza" });
```

> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Thin crust pizza", "vegan" : false }

To remove the new field use ``$unset``.

```bash
	db.examples.updateOne({ "title": "Thin crust pizza" }, { $unset: { "vegan": 1 }});
```

> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Thin crust pizza" }

#### Incrementing a field in a document

Increment the ``likes_count`` field in the Tacos recipe.

> ],  
>         "likes_count" : 2,  
>         "prep_time" : 10,  
>         "rating" : [  

```bash
	db.examples.updateOne({ "title": "Tacos" }, { $inc: { "likes_count": 1 }});
```

>],  
>        "likes_count" : 3,  
>        "prep_time" : 10,  
>        "rating" : [  

**{ $inc: { "likes_count": 1 }}** means increment by 1.

**{ $inc: { "likes_count": -1 }}** means increment by -1, in other words decrement by 1.

### Working with arrays

Add an item to the **likes** array with ``$push``. This adds an item to the end of the array.

 > ],  
 >        "likes" : [  
 >                1,  
 >                415  
 >        ],  

```bash
	db.examples.updateOne({ "title": "Tacos" }, { $push: { "likes": 60 }});
```

> ],  
>         "likes" : [  
>                 1,  
>                 415,  
>                 60  
>         ],  

To remove an item from the array we use ``$pull``.

```bash
	db.examples.updateOne({ "title": "Tacos" }, { $pull: { "likes": 60 }});
```

> ],  
>         "likes" : [  
>                 1,  
>                 415  
>         ],  

### Deleting a document

```bash
	db.examples.find({}, { "title": 1  });
```

Results:

> { "_id" : ObjectId("5ee69e393260aab97ea0d58e"), "title" : "Delete me" }  
> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Thin crust pizza" }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }  

We want to delete the document with the title "Delete me".

You can use ``.delete()`` or to be safe and delete only one document, ``.deleteOne()``.

```bash
	db.examples.deleteOne({ "_id" : ObjectId("5ee69e393260aab97ea0d58e") });
```

> { "_id" : ObjectId("5ee69d943260aab97ea0d58d"), "title" : "Thin crust pizza" }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }  

#### Challenge No. 1

**Recipes**

* Top rated recipes.
* Sort by most popular first.
* Limited to 4 results.

```bash
	db.recipes.find({}, {"title": 1, "rating_avg": 1 }).sort({ "rating_avg": -1 }).limit(4);
```

> { "_id" : ObjectId("5edf1cd43260aab97ea0d588"), "title" : "Apple Pie", "rating_avg" : 4.8 }  
> { "_id" : ObjectId("5e877cba20a4f574c0aa56da"), "title" : "Pancakes", "rating_avg" : 3.88 }  
> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos", "rating_avg" : 3.71 }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos", "rating_avg" : 3.5 }  

#### Challenge No. 2

* Bring back recipes tagged with "Mexican".
* Sort by most Popular first.
* Limited to 4 documents.

```bash
	db.recipes.find({ "tags": "mexican" }, {"title": 1, "rating_avg": 1 }).sort({ "rating_avg": -1 }).limit(4);
```

> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos", "rating_avg" : 3.71 }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos", "rating_avg" : 3.5 }  

#### Challenge No. 3

* Bring back all recipes liked by user id 1.
* Sorted by title.
* No limit.

```bash
	db.recipes.find({ "likes": 1 }, {"title": 1 }).sort({ "title": 1 });
```

or

```bash
	db.recipes.find({ "likes": { $all: [1] }}, {"title": 1 }).sort({ "title": 1 });
```

> { "_id" : ObjectId("5edf1cd43260aab97ea0d588"), "title" : "Apple Pie" }  
> { "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos" }  
> { "_id" : ObjectId("5e878f5220a4f574c0aa56db"), "title" : "Maple Smoked Salmon" }  
> { "_id" : ObjectId("5e877cba20a4f574c0aa56da"), "title" : "Pancakes" }  
> { "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }  

If we want to see recipes liked by user id's 1 and 35.

```bash
	db.recipes.find({ "likes": { $all: [1, 35] }}, {"title": 1 }).sort({ "title": 1 });
```

Only brings back.

> { "_id" : ObjectId("5e877cba20a4f574c0aa56da"), "title" : "Pancakes" }

This is the only recipe liked by both user id's 1 & 35.

```bash
	db.recipes.find({ "likes": { $in: [1, 35] }}, {"title": 1 }).sort({ "title": 1 });
```

This returns all recipes liked by user id's 1 & 35.

{ "_id" : ObjectId("5edf1cd43260aab97ea0d588"), "title" : "Apple Pie" }
{ "_id" : ObjectId("5e6fd805fa98021236426a24"), "title" : "Chicken Soft Tacos" }
{ "_id" : ObjectId("5e878f5220a4f574c0aa56db"), "title" : "Maple Smoked Salmon" }
{ "_id" : ObjectId("5e877cba20a4f574c0aa56da"), "title" : "Pancakes" }
{ "_id" : ObjectId("5e5e9c470d33e9e8e3891b35"), "title" : "Tacos" }

### Data and Schema modeling

> Data that is accessed together should be stored together &mdash; MongoDB

This means that you shouldn't have a user's collection and an address collection. You should think of a way to join both collections into 1. if you are going to access a user you are more than likely going to want their address details at the same time.

You should think differently about 1 to 1 or 1 to many relationships in databases.

In my Record DB I could add an artist's records into an array within the artist document but seeing as some artists have a large number of records it might be more efficient to store the records in a different collection. This means that when I search for an artist that has a large number of records I am not bringing back all of their records each time I query that artist.

### Basic indexing

Having a basic understanding of indexes can make a big difference when querying your data.

When you have an index it will stop you from querying every document in your collection and only search for a single document. We have seen how this can dramatically increase the speed of your query.

Do this search on artists in record-db.

```bash
	db.artists.find({ "lastname": "Dylan" }, {"name": 1 });
```

returns,

> { "_id" : "5f3defa4163278d1ac061cfb", "name" : "Bob Dylan" }

Now check the execution of the query.

```bash
	db.artists.find({ "lastname": "Dylan" }, {"name": 1 }).explain("executionStats");
```

It brings back a large amount of information but we are only interested in,

> "executionStats" : {  
>                 "executionSuccess" : true,  
>                 "nReturned" : 1,  
>                 "executionTimeMillis" : 1,  
>                 "totalKeysExamined" : 0,  
>                 "totalDocsExamined" : 616,  

We search through 616 documents in our collection. Imagine if we had thousands of documents in our collection?

We can create an index with the following.

```bash
	db.artists.createIndex({ "name": 1 });
```

This time search for an artist's name.

```bash
	db.artists.find({ "name": "Bob Dylan" }, {"name": 1 }).explain("executionStats");
```

> "executionStats" : {  
>                 "executionSuccess" : true,  
>                 "nReturned" : 3,  
>                 "executionTimeMillis" : 0,  
>                 "totalKeysExamined" : 3,  
>                 "totalDocsExamined" : 3,  

The number of documents searched has been reduced to 3 because we built an index for the **name** field.

This shows you the benefits of indexing.

Use the ``db.artists.`` and tab twice. This will show you all of the commands you can run on the artists collection.

You will see.

```bash
	db.artists.getIndex();
```

and this will show you a list of all your indexes, in our case,

> [  
>     {  
>             "v" : 2,  
>             "key" : {  
>                     "_id" : 1  
>             },  
>             "name" : "_id_",  
>             "ns" : "record-db.artists"  
>     },  
>     {  
>             "v" : 2,  
>             "key" : {  
>                     "name" : 1  
>             },  
>             "name" : "name_1",  
>             "ns" : "record-db.artists"  
>     }  
> ]  

In this case we have the default index on ``_id`` and we also have one on ``name``.

If we want to drop and index we can run.

```bash
	db.artists.dropIndex({ "name_1" });
```

### Coding with MongoDB using Node.js

Install Node.js in your project.

```bash
	npm install mongodb --save
```

I have installed the examples in **D:\WebDev\Node\examples\node-mongo-db-coding**

### Backups

The simplest of backup methods is to go to your database folder and stop people from writing into the files.

In mongo shell.

```bash
	db.fsyncLock();
```

You get the message.

> {  
>         "info" : "now locked against writes, use db.fsyncUnlock() to unlock",  
>         "lockCount" : NumberLong(1),  
>         "seeAlso" : "http://dochub.mongodb.org/core/fsynccommand",  
>         "ok" : 1  
> }  

Now unlock.

```bash
	db.fsyncUnlock();
```

returns the message.

> { "info" : "fsyncUnlock completed", "lockCount" : NumberLong(0), "ok" : 1 }

**Note: ** I got a *file being used* message so I had to skip backing that file up. I won't use this method of backing up again. Instead I'll shut the server down.

### Stopping the MongoDB server

Open Powershell as administrator

```bash
	net stop MongoDB
```

Now you can safely do backups. In production you would do this out of hours.

To restart the MongoDB server you would issue a **net start MongoDB**. I did this but it didn't start up properly. I ended up restarting my PC.

#### mongodump

**mongodump** will dump the contents of all databases into a dump folder in your current folder.

There are a number of other options with mongodump, see the help.

```bash
	mongodump --help
```

You can backup a particular database with mongodump.

**mongorestore** will restore the dump folder or the particular database you dumped.
