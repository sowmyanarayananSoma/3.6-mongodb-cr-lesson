# MongoDB Practice Commands for Class
### Comprehensive Instructor + Student Exercise Set

This practice sheet contains **all the commands students should try in class** using the MongoDB shell (`mongosh`).

It covers:
- Creating databases & collections
- Insert operations
- Read operations
- Update operations
- Delete operations
- Complex nested data
- One‑to‑many modeling
- Many‑to‑many modeling

Use this as a live coding + hands-on worksheet.

---

# 1. Create a New Database
```js
use catalogDB
```

---

# 2. Create Basic Collections
```js
db.createCollection("books")
db.createCollection("authors")
db.createCollection("students")
db.createCollection("courses")
db.createCollection("enrollments")
```

---

# 3. Insert Simple Documents
## Books
```js
db.books.insertMany([
  { title: "Clean Code", price: 40, category: "Programming" },
  { title: "Atomic Habits", price: 20, category: "Productivity" },
  { title: "Deep Work", price: 25, category: "Productivity" }
])
```

## Authors
```js
db.authors.insertMany([
  { name: "Robert Martin", country: "USA" },
  { name: "James Clear", country: "Canada" },
  { name: "Cal Newport", country: "Iceland" }
])
```

---

# 4. Basic CRUD Queries
## Read All
```js
db.books.find()
```

## Filter by Field
```js
db.books.find({ category: "Productivity" })
```

## Projection (select fields)
```js
db.books.find({}, { title: 1, price: 1, _id: 0 })
```

## Update One
```js
db.books.updateOne(
  { title: "Atomic Habits" },
  { $set: { price: 22 } }
)
```

## Delete One
```js
db.books.deleteOne({ title: "Deep Work" })
```

---

# 5. Insert Complex Data (Nested / Embedded)
## Example: One Author → Many Books (Embedding)
```js
db.authors.insertOne({
  name: "J. R. R. Tolkien",
  nationality: "UK",
  books: [
    { title: "The Hobbit", year: 1937 },
    { title: "The Fellowship of the Ring", year: 1954 },
    { title: "The Two Towers", year: 1954 }
  ]
})
```

---

# 6. One-to-Many Using Referencing
### One-to-Many Referencing (WITHOUT JavaScript `const`)

### Step 1: Insert Author
```js
db.authors.insertOne({ name: "Malcolm Gladwell" })
```
Check the inserted ID:
```js
db.authors.find({ name: "Malcolm Gladwell" })
```
Copy the `_id` you see in the result.

### Step 2: Insert Books Referencing That Author
```js
db.books.insertMany([
  { title: "Outliers", authorId: ObjectId("PASTE_ID_HERE") },
  { title: "Blink", authorId: ObjectId("PASTE_ID_HERE") },
  { title: "The Tipping Point", authorId: ObjectId("PASTE_ID_HERE") }
])
```

### Step 3: Query Books by Author
```js
db.books.find({ authorId: ObjectId("PASTE_ID_HERE") })
```
```js
db.books.find({ authorId: authorId })
```

---

# 7. Many-to-Many Example (Students ↔ Courses)
## Many-to-Many Example (Students ↔ Courses)

### Step 1: Insert Courses
```js
db.courses.insertOne({ name: "Backend Development", duration: 10 })
db.courses.insertOne({ name: "Frontend Development", duration: 12 })
```
Find their IDs:
```js
db.courses.find()
```
Copy each `_id`.

### Step 2: Insert Students
```js
db.students.insertOne({ name: "Alex", age: 21 })
db.students.insertOne({ name: "Maria", age: 23 })
```
Find their IDs:
```js
db.students.find()
```

### Step 3: Insert Enrollment Records (junction collection)
```js
db.enrollments.insertMany([
  { studentId: ObjectId("STUDENT1_ID"), courseId: ObjectId("COURSE1_ID"), enrolledAt: new Date() },
  { studentId: ObjectId("STUDENT1_ID"), courseId: ObjectId("COURSE2_ID"), enrolledAt: new Date() },
  { studentId: ObjectId("STUDENT2_ID"), courseId: ObjectId("COURSE1_ID"), enrolledAt: new Date() }
])
```

### Step 4: Query: All Courses for One Student
```js
db.enrollments.find({ studentId: ObjectId("STUDENT1_ID") })
```

### Step 5: Query: All Students in One Course
```js
db.enrollments.find({ courseId: ObjectId("COURSE1_ID") })
```
```js
db.enrollments.find({ courseId: courseA })
```

---

# 8. Array Query Commands
## Match array element
```js
db.books.find({ tags: "classic" })
```

## Match all
```js
db.books.find({ tags: { $all: ["classic", "fiction"] } })
```

---

# 9. Update Operators
## Simple Update Examples (Beginner Friendly)

### Update ONE field
```js
db.users.updateOne(
  { name: "Alex" },
  { $set: { age: 25 } }
)
```

### Update MULTIPLE fields
```js
db.users.updateOne(
  { name: "Alex" },
  { $set: { age: 26, city: "Toronto" } }
)
```

### Increase a number using `$inc`
```js
db.products.updateOne(
  { name: "Laptop" },
  { $inc: { price: 50 } }
)
```

### Add to an array using `$push`
```js
db.users.updateOne(
  { name: "Maria" },
  { $push: { hobbies: "cooking" } }
)
```

### Remove from an array using `$pull`
```js
db.users.updateOne(
  { name: "Maria" },
  { $pull: { hobbies: "gaming" } }
)
```

### Simplest update
```js
db.items.updateOne({ id: 1 }, { $set: { name: "New Name" } })
```


## $inc
```js
db.students.updateOne({ name: "Alex" }, { $inc: { age: 1 } })
```

## $push
```js
db.students.updateOne({ name: "Maria" }, { $push: { skills: "JavaScript" } })
```

## $pull
```js
db.students.updateOne({ name: "Maria" }, { $pull: { skills: "JavaScript" } })
```

---

# 10. Delete Operations
## Delete Many
```js
db.books.deleteMany({ category: "Productivity" })
```

## Drop an entire collection
```js
db.books.drop()
```

---

# 11. Projection / Display Commands
Projection allows you to display **only one column**, **multiple columns**, or **hide specific fields**.

## Display ONE field only
```js
db.books.find({}, { title: 1, _id: 0 })
```
This will show only the `title` column.

## Display MULTIPLE fields
```js
db.books.find({}, { title: 1, price: 1, category: 1, _id: 0 })
```

## Hide specific fields
```js
db.books.find({}, { _id: 0, price: 0 })
```
(Hides `_id` and `price`, shows everything else.)

## Display nested fields (dot notation)
```js
db.authors.find({}, { "books.title": 1, _id: 0 })
```

## Display selected fields + filter
```js
db.books.find({ category: "Programming" }, { title: 1, price: 1, _id: 0 })
```

---

# 12. Sorting, Limiting, Skipping
```js
db.books.find().sort({ price: -1 })
db.books.find().limit(2)
db.books.find().skip(2)
```

---

# 13. Challenge Exercises for Students
1. Insert a collection named `movies` with complex nested data.
2. Create one-to-many relations: Director → Movies.
3. Create many-to-many relations: Actors ↔ Movies using a junction table.
4. Update an embedded field.
5. Delete based on multiple conditions.
6. Query nested fields using dot notation.
7. Add tags array to documents and query using `$all`.

---

Use the above commands to run a **full interactive MongoDB practice session**. All commands run directly in **mongosh** and work cross‑platform.

