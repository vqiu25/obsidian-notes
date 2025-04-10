# MongoDB Fundamentals

* Each MongoDB database can have an arbitrary number of **~={green}collections=~**
	* Each **~={green}collection=~** can have any number of **~={red}documents=~**
		* Each **~={red}document=~** can be represented as **~={blue}JSON=~**
			* Each **~={red}document=~** has a field called **~={yellow}id=~** (akin to primary key), which is an identifier that's guaranteed to be unique amongst all documents in its collection
			* Documents may have any **~={yellow}number of other fields=~** (akin to  columns)

## MongoDB Types

The following are valid datatypes for a field in a MongoDB document:
* **~={purple}Commonly used=~**: Binary, Boolean, Date, Decimal128, Double, Int32, Int64, ObjectId, String, Timestamp
* **~={purple}Less Common=~**: Code, MaxKey, MinKey, BSONRegexp, Symbol
* In addition, fields can be null, undefined, or other objects (essentially sub-documents)

# Mongoose

> [!Quote] Quick Notes
> JavaScript library used to interact with MongoDB databases

How to Install:

```js
npm install --save mongoose
```

How to Import:

```js
import mongoose from 'mongoose';
```

## Defining a Schema in JS

* Use the Schema constructor from Mongoose to define the structure of your documents.
* **~={red}Schema=~**: A schema defines the shape and content of documents in a MongoDB collection. (Create a new Schema which defines what a User looks like in our system)
* **~={red}Model=~**: Create the **~={green}User=~** class. The name given as a string will also determine the name of the MongoDB collection used (it will use the plural form of the given word, in all lower case - e.g. **~={green}users=~** in this case).
* **~={red}Document=~**: Create an individual User instance

```js
import mongoose from 'mongoose';
const Schema = mongoose.Schema;

const userSchema = new Schema({
  fieldName: FieldType,
});

// Create Model
const User = mongoose.model('User', userSchema);
// Create Document
const user = new User();
```

---
### Example Fields

**~={purple}Basic Fields=~**

* We can mark fields as unique or required

```js
username: { type: String, unique: true }, // Unique field
firstName: {type: String required: true}, // Standard string
dateOfBirth: Date,                        // Date type
```

**~={purple}Embedded Documents (Nested Objects)=~**

```js
address: {
  number: String,
  street: String,
  city: String,
  postcode: Number
}
```

**~={purple}Array of Objects (No limit on amount)=~**

```js
creditCards: [
  { lastFourDigits: String, encryptedInfo: String }
]
```

**~={purple}Array of Objects=~**

```js
registeredPets: [{ type: Schema.Types.ObjectId, ref: 'Pet' }]
```

## Time Stamps

```js
const userSchema = new Schema({...}, { timestamps: {} });
```

## Mongoose Custom Functions

🔹 ~={purple}**Instance Methods**=~
* Used to define methods that can be called on a **document instance**.
* These have access to this, referring to the specific document.

✅ ~={green}**Use Case**=~:
* When you want to define behaviours related to a single document.

**🧾 ~={red}Example=~:**

```js
userSchema.methods.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
};

const user = new User({ firstName: 'John', lastName: 'Doe' });
console.log(user.getFullName()); // John Doe
```

---

🔹 ~={purple}**Static Methods**=~
* Used to define methods that can be called on the **Model** itself.
* These do **not** access individual documents with this, but instead the whole model.

**✅ ~={green}Use Case=~:**
* When you need functionality like custom queries or operations on the collection level.

🧾 **~={red}Example=~**:

```js
userSchema.statics.findByUsername = function (username) {
  return this.findOne({ username });
};

const user = await User.findByUsername('john123');
```

---

**🔹 ~={purple}Virtuals=~**
* Virtual properties are **not stored in MongoDB** but computed dynamically.
* Useful for derived fields like fullName, age, etc.
* Use `.get()` for read-only virtuals or .set() for writeable ones.

**✅ ~={green}Use Case=~:**
* Adding calculated fields to your models without storing them in the DB.

**🧾 ~={red}Example=~:**

```js
userSchema.virtual('fullName').get(function () {
  return `${this.firstName} ${this.lastName}`;
});

const user = new User({ firstName: 'Jane', lastName: 'Smith' });
console.log(user.fullName); // Jane Smith
```

**🔄 With Setters:**

```js
userSchema.virtual('fullName')
  .get(function () {
    return `${this.firstName} ${this.lastName}`;
  })
  .set(function (v) {
    const parts = v.split(' ');
    this.firstName = parts[0];
    this.lastName = parts[1];
  });

const user = new User();
user.fullName = 'Alice Johnson';
console.log(user.firstName); // Alice
```

---

**📝 ~={purple}Summary Table=~**

|**Feature**|**Applied On**|**Use Case**|**Access** this**?**|
|---|---|---|---|
|Instance Method|Document|Behavior for one record|✅ Yes|
|Static Method|Model|Collection-wide operations|❌ No (uses model context)|
|Virtual|Document|Derived/calculated fields|✅ Yes|

## Schema Validation
### Checking for Validation

![[Pasted image 20250405212601.png]]

### Custom Validation Logic

![[Pasted image 20250405212637.png]]

![[Pasted image 20250405212711.png]]

# Mongoose Queries

## Working with Documents

📘 **~={purple}Defining a Model=~**

* Once you’ve created a **schema** using Mongoose (which defines the structure of your documents), you can create a **model** from it using:
* This model acts as a **class** for interacting with the documents of that type in the MongoDB collection.

```js
const ModelName = mongoose.model('ModelName', schema);
```

🆕 **~={purple}Creating Documents (Instances)=~**

* You can create a new document instance using the model:

```js
const doc = new ModelName();
```

* You can either:
	* Set fields one by one (doc.fieldName = value), or
	* Pass an object with initial values to the constructor:

```js
const doc = new ModelName({ field1: value1, field2: value2 });
```

Mongoose automatically assigns an _id to each document unless configured otherwise.

🔗 **~={purple}Connecting to MongoDB=~**

* Before performing operations, connect to the database:
* Common option: { useNewUrlParser: true } to avoid deprecation warnings.

```js
await mongoose.connect('mongodb://<host>:<port>/<database>', options);
```

💾 **~={purple}Saving a Document=~**

* Once a document instance is created, use `.save()` to persist it to the database:

```js
await doc.save();
```

❌ **~={purple}Deleting a Document=~**

* To remove a document matching certain criteria:
* This deletes the **first document** that matches the criteria.

```js
await ModelName.deleteOne({ key: value });
```

* **~={red}Mongoose operations like .connect(), .save(), and .deleteOne() return promises.**
* **Use await inside async functions or handle them with .then()/.catch().=~**

## Querying in Mongoose

✅ **~={purple}Common Query Methods=~**

Mongoose provides **~={green}static=~** methods on model classes (created via mongoose.model) to query documents in the database:

* `findById(id)` – Retrieves a single document by its _id.
* `findOne(criteria)` – Retrieves the first document that matches the provided criteria.
* `find(criteria)` – Retrieves all documents matching the given criteria.

**~={red}These methods are async, so they return objects that can be awaited, although they are not Promises directly (but behave like them).=~**

```js
// Find the user whose _id is 5ea8aede135c347ed00028be 
const user = await User.findById('5ea8aede135c347ed00028be'); 
// Find all users 
const allUsers = await User.find();
```


**📑** **~={purple}Query Criteria and Operators=~**

You can define complex queries using MongoDB query operators:

* $gt, $lt, $lte, $gte – Greater/Less than (or equal)
* $in – Matches any value in an array
* $not, $size, regex (/pattern/) – For negations, array size, and pattern matching

**~={red}Examples=~**

![[Pasted image 20250405220132.png]]

🌱 **~={purple}Populating References Between Collections=~**

* Population is Mongoose’s way of automatically replacing object IDs with actual documents from the referenced collection.
* This is useful when fields in a document reference another document (e.g., User references Pet).

1. Define Schema

```js
// Pet schema
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const petSchema = new Schema({
  name: String,
  species: String,
  owner: { type: Schema.Types.ObjectId, ref: 'User' } // references User
});

const userSchema = new Schema({
  username: String,
  email: String,
  registeredPets: [{ type: Schema.Types.ObjectId, ref: 'Pet' }] // array of Pet references
});

const Pet = mongoose.model('Pet', petSchema);
const User = mongoose.model('User', userSchema);
```

2. Create and Save Documents

```js
// Create a user
const user = new User({ username: 'anhydrous', email: 'a@example.com' });
await user.save();

// Create pets that reference the user
const pet1 = new Pet({ name: 'Fluffy', species: 'Cat', owner: user._id });
const pet2 = new Pet({ name: 'Rex', species: 'Dog', owner: user._id });
await pet1.save();
await pet2.save();

// Add pet references to user and save
user.registeredPets.push(pet1._id, pet2._id);
await user.save();
```

3. Populate the References

🔍 Option A: Populate When Querying

```js
const result = await User.findOne({ username: 'anhydrous' })
                         .populate('registeredPets');

console.log(result.registeredPets);
// Logs an array of Pet objects instead of ObjectIds
```

🔁 Option B: Populate After Query

```js
const pets = await Pet.find({ species: 'Dog' });
await Pet.populate(pets, { path: 'owner' });

console.log(pets[0].owner.username);
// Logs the username of the owner of the pet
```

Which would turn something like this:

```js
"registeredPets": ["609c1df2e123a95fd8c123ab"]
```

Into:

```js
"registeredPets": [
  { "_id": "609c1df2e123a95fd8c123ab", "name": "Fluffy", "species": "Cat", ... }
]
```