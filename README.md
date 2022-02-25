# FiveM MongoDB wrapper
## Description
This resource is a simple MongoDB wrapper for [FiveM](https://fivem.net/). It's running on top of [MongoDB Node Driver](https://mongodb.github.io/node-mongodb-native/).

## Installation

1. Clone this repository to `resources/mongodb` folder.
2. Copy `mongodb/database.cfg` to your server root directory.
3. Add the following lines to your server config:
```
exec "database.cfg"
start mongodb
```
4. Change `mongodb_url` and `mongodb_database` in `database.cfg`.
5. Run `npm install` in `resources/mongodb` directory.

## Usage

Every callback accepts `success<boolean>` as its first argument. If `success` is `false`, second argument contains error message.

Example (Lua):
```lua
exports.mongodb:findOne({ collection = "users", query = { _id = id } }, function (success, result)
    if not success then
        print("Error message: "..tostring(result))
        return
    end

    print("User name is "..tostring(result[1].name))
end)
```

#Aggregate
```lua
    -- how to use 
exports.mongodb:aggregate({ 
	collection = "users",
	pipeline = {
		[1] = { ["$match"] = { identifier = "steam:xxxxxxxxxxxxxxx" } },
		[2] = { ["$project"] = { playerName =  1  } } ,
		[3] = { ["$limit"] = 1 }
	}
}, function (success,result)
	if not success then
		print("Error message: "..tostring(result))
	else
	  for _, user in pairs(result) do
		for key, value in pairs(user) do
			print(key, " : " , value)
		end
		print("-------")
	  end
	end
end)
```

#Find
```lua
exports.mongodb:find({
    collection = "phone_message",
    query = {
        transmitter = "",
        receiver = ""
    },
    limit = 100,
    sort = {
        time = -1
    }
}, function (success, result)
    if not success then
        return
    end

    print("\n** 100 message lastest")
    for i, document in ipairs(result) do
        for k, v in pairs(document) do
            print("* "..tostring(k).." = \""..tostring(v).."\"")
        end
    end
end)
```



#CreateIndex
```lua
AddEventHandler("onDatabaseConnect", function (databaseName)
	exports.mongodb:createIndex({
		collectionName = "esx_status",
		keys = { ["identifier"] = 1},
		options= { ["unique"] = true , ["name"] = "identifier" }
	})
end)
```


## exports.mongodb.isConnected
* Returns boolean

Returns true if database connection is established.

## exports.mongodb.insert(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.documents<Object>` - an array of documents to insert
* `params.options<Object>` - optional settings object. See [collection.insertMany in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#insertMany)
* `callback(success<boolean>, insertedCount<number>, insertedIds<Array>)` - callback (optional)
Inserts an array of documents into MongoDB.

## exports.mongodb.insertOne(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.document<Object>` - document object
* `params.options<Object>` - optional settings object. See [collection.insertMany in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#insertMany)
* `callback(success<boolean>, insertedCount<number>, insertedIds<Array>)` - callback (optional)

Inserts a single document into MongoDB.

## exports.mongodb.find(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.options<Object>` - optional settings object. See [collection.find in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#find)
* `params.limit<number>` - limit documents count
* `callback(success<boolean>, documents<Array>)` - callback (optional)

Performs a find query.

## exports.mongodb.findOne(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.options<Object>` - optional settings object. See [collection.find in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#find)
* `callback(success<boolean>, documents<Array>)` - callback (optional)

Performns a find query with `limit = 1`.

## exports.mongodb.update(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.update<Object>` - update query object
* `params.options<Object>` - optional settings object. See [collection.updateMany in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#updateMany)
* `callback(success<boolean>, updatedCount<number>)` - callback (optional)

Update multiple documents on MongoDB.

## exports.mongodb.updateOne(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.update<Object>` - update query object
* `params.options<Object>` - optional settings object. See [collection.updateMany in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#updateMany)
* `callback(success<boolean>, updatedCount<number>)` - callback (optional)

Update a single document on MongoDB.

## exports.mongodb.count(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.options<Object>` - optional settings object. See [collection.countDocuments in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#countDocuments)
* `callback(success<boolean>, count<number>)` - callback (optional)

Gets the number of documents matching the filter.

## exports.mongodb.delete(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.options<Object>` - optional settings object. See [collection.deleteMany in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#deleteMany)
* `callback(success<boolean>, deletedCount<number>)` - callback (optional)

Delete multiple documents on MongoDB.

## exports.mongodb.deleteOne(params, callback);
* `params<Object>` - params object
* `params.collection<string>` - collection name
* `params.query<Object>` - filter query object
* `params.options<Object>` - optional settings object. See [collection.deleteMany in docs](http://mongodb.github.io/node-mongodb-native/3.1/api/Collection.html#deleteOne)
* `callback(success<boolean>, deletedCount<number>)` - callback (optional)

Delete a document on MongoDB.
