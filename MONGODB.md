# MongoDB
* They are split into collections. And there are many documents in each collections. It stores `key` and `value`. 
* each model maps to a **collection of documents**. Each collection has multiple documents. The document may then have a collection and so on. 
### Schema
Models are defined as schemas. Schemas is what allow you to create the documents data along with validation requirements or default values. You then would compile them with `mongoose.model()` method.

```
const Schema = mongoose.Schema;

// Imagine schemas as the typescript `interface`. It is basically a structure of what each schema shape will look like. For example, we want a `string` for the `title` key. We want a `number` for the `age` key. And so on. Creating a schemas is done by using the `new` keyword. 
const newSchema = new Schema({
  fruit: String,
  food: String,
})

// If you then want to use your schema definition, you will need to convert the `schema` into a model. By default, it will automatically generate a `_id`. You can overwrite it yourself but remember mongoose will only accept code that has a _id. 
// You are creating a collection when you create a model. The first argument is the database collection name and the second is the schema, or the structure of the documents you want to be using. 
const newModel = mongoose.model("newModel", newSchema)
```

### Schema Types
```
var schema = new Schema(
{
  name: String,
  binary: Buffer,
  living: Boolean,
  updated: { type: Date, default: Date.now() },
  age: { type: Number, min: 18, max: 65, required: true },
  mixed: Schema.Types.Mixed, <-- accept any value here
  _someId: Schema.Types.ObjectId,
  array: [],
  ofString: [String], // You can also have an array of each of the other types too.
  nested: { stuff: { type: String, lowercase: true, trim: true } }
})
```

```
type: String ===> when find a nested property named type, it will think it needs to be defined a SchemaType
```

### SCHEMA OPTIONS
```
String -----------------------------------------------
lowercase: boolean, whether to always call .toLowerCase() on the value
uppercase: boolean, whether to always call .toUpperCase() on the value
trim: boolean, whether to always call .trim() on the value
match: RegExp, creates a validator that checks if the value matches the given regular expression
enum: Array, creates a validator that checks if the value is in the given array.
minLength: Number, creates a validator that checks if the value length is not less than the given number
maxLength: Number, creates a validator that checks if the value length is not greater than the given number
populate: Object, sets default populate options

Number --------------------------------------------------
min: Number, creates a validator that checks if the value is greater than or equal to the given minimum.
max: Number, creates a validator that checks if the value is less than or equal to the given maximum.
enum: Array, creates a validator that checks if the value is strictly equal to one of the values in the given array.
populate: Object, sets default populate options

Date ----------------------------------------------------
min: Date
max: Date

```
At the end of a schema, there is a option object as the second argument. This is where you can add things like `timestamp` which will add a timestamp property to your object. You can add minimize which will remove any empty objects. Simply add these validations as the second object argument.
### Models
Models built on top of the schema, using that data structure to interact with the database. It is not the collection but it will make a reference to the collection name. The Model is essentially how you would use CRUD features. Save, delete, update, remove. 


**Watch out that mongoose pluralize your collection names! It will change it! If you want to disable it, use `mongoose.pluralize(null)`**

Once Mongoose save to the database, mongoose sent you back the actual document saved in the collection. 

**If collection doesn't exist, it will create one for you.**

### Getting a document
If you want to get a document from a collection, you will have to use the model and write `Blog.find().then(....).catch(...)`. Remember that they are async and should be treated asyncly. Finds all of the documents inside the collection. 

### Virtual
* A virtual is a property not stored in MongoDB. 

### Sorting a document
`Blog.find().sort()` can be used to sort. By writing for exmample {createdAt : -1}, it will sort by the newest. 

### Getting a document by ID
Use `Blog.findById(id)`. It is async and Mongoose will automatically convert the string you put in into a `ObjectId` because in Mongoose, the id is stored as ObjectID. 

### Virtuals
Imagine virtuals as additional properties you create to enhance a a existing object. For example, a user property has both `first` and `last` name but what if you want to combine the two together? You can store a `fullname` property into the MongoDB but feels a bit redudant wouldn't it? What if you can create a property that combine the two first and last property together and not save it into the database so that the database have only what is necessary data? This is where Virtuals comes to play. It doesn't actually get stored into the database but when you use your schema, you will have that extra property `fullname` to use. 

* There are two ways you can use virtuals: `get` and `set` methods. A get method only allow you to read the data while a `set` will allow you to override the previous data. For example, if you add the below: 
```
userSchema.virtual('fullname').set(function (name) {  
  var split = name.split(' ');
  this.first = split[0];
  this.last = split[1];
});
```
And you write `user1.fullname = "weibo zhang"`, you will overwrite the database with that new data. 

To use it as get, do 
```
userSchema.virtual('fullname').get(function() {  
    return this.first + ' ' + this.last;
});

console.log(user1.fullname)
```

One problem is that a virtual field cannot be used in looking up things in the document. Because they are not actually saved in the database, that field technially does not exist. 

### String Patterns
* `ab?d` - it means the preceding letter can have 0 or 1 occurence of it. So you can have it or not. 
* `ab+d` - have one or more of the preceding letter before it
* `ab*cd` - can have any letter within `ab_____________________________cd` as long as the endpoints are still the same
* `()` - Grouping match on a set of characters to perform another operation on, e.g. '/ab(cd)?e' will perform a ?-match on the group `(cd)`—it will match abe and abcde.

### Populate
If you write `    user: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },`, You can be able to reference another collection and use that data. That will keep things in a much more organized place. 

### Async Module
You can use the native Promises but the Async module solves some of the hassles you will be facing with Promises. Some useful methods are 
`async.parallel()` - run any that should be run concurrently
`async.series()` - make sure the async event runs in a series one after another
`async.waterfall()` - every async event must run in series and use the previous async return value

### Async.parallel (from MDN)
The first argument to async.parallel() is a collection of the asynchronous functions to run (an array, object or other iterable). Each function is passed a callback(err, result) which it must call on completion with an error err (which can be null) and an optional results value.

The optional second argument to  async.parallel() is a callback that will be run when all the functions in the first argument have completed. The callback is invoked with an error argument and a result collection that contains the results of the individual asynchronous operations. The result collection is of the same type as the first argument (i.e. if you pass an array of asynchronous functions, the final callback will be invoked with an array of results). If any of the parallel functions reports an error the callback is invoked early (with the error value).
```
async.parallel({
  one: function(callback) { ... },
  two: function(callback) { ... },
  ...
  something_else: function(callback) { ... }
  },
  // optional callback
  function(err, results) {
    // 'results' is now equal to: {one: 1, two: 2, ..., something_else: some_value}
  }
);
```

### Async.series
The method async.series() is used to run multiple asynchronous operations in sequence, when subsequent functions do not depend on the output of earlier functions. It is essentially declared and behaves in the same way as async.parallel().

**If the order really is important, then you should pass an array instead of an object, as shown below.**
```
async.series([
  function(callback) {
    // do some stuff ...
    callback(null, 'one');
  },
  function(callback) {
    // do some more stuff ...
    callback(null, 'two');
  }
 ],
  // optional callback
  function(err, results) {
  // results is now equal to ['one', 'two']
  }
);
```

### Async.Waterfall
The method async.waterfall() is used to run multiple asynchronous operations in sequence when each operation is dependent on the result of the previous operation.

The callback invoked by each asynchronous function contains null for the first argument and results in subsequent arguments. Each function in the series takes the results arguments of the previous callback as the first parameters, and then a callback function. When all operations are complete, a final callback is invoked with the result of the last operation. The way this works is more clear when you consider the code fragment below (this example is from the async documentation):
```
async.waterfall([
  function(callback) {
    callback(null, 'one', 'two');
  },
  function(arg1, arg2, callback) {
    // arg1 now equals 'one' and arg2 now equals 'two'
    callback(null, 'three');
  },
  function(arg1, callback) {
    // arg1 now equals 'three'
    callback(null, 'done');
  }
], function (err, result) {
  // result now equals 'done'
}
);```
