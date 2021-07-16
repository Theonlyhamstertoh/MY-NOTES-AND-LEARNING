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
If you want to get a document from a collection, you will have to use the model and write `Blog.find().then(....).catch(...)`. Remember that they are async and should be treated asyncly. 
