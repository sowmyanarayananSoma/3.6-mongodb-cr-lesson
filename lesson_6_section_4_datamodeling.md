# Section 4: Data Modeling in MongoDB (Expanded Instructor Version)

## 🎯 Learning Objective
By the end of this section, learners will be able to:
- Understand what data modeling means
- Model **one-to-one**, **one-to-many**, and **many-to-many** relationships in MongoDB
- Understand **embedding vs referencing**
- Know when to choose each pattern
- Apply these patterns to real project databases

---

# 1. What is Data Modeling?
Data modeling is the process of **designing how data will be stored** in a database.

In MongoDB, data modeling is more flexible compared to SQL because:
- Documents can have nested structures
- Collections don’t require a strict schema

### Why data modeling matters
A good data model:
- Makes queries faster
- Makes your code simpler
- Reduces the need to join/reference too many documents

---

# 2. Key MongoDB Modeling Approaches
MongoDB supports two primary ways of representing relationships:

## ✔ Embedding (Nested Documents)
Store related data **inside** the document.

## ✔ Referencing (Linking Documents)
Store related data **as separate documents**, connected by IDs.

---

# 3. One-to-One Relationships
A **one-to-one** relationship means:
> One document relates to exactly one other document.

### Example scenarios:
- User ↔ Profile
- Product ↔ Inventory details

## ✔ Option 1: Embed (recommended for 1:1)
```json
{
  "username": "alex123",
  "email": "alex@example.com",
  "profile": {
    "bio": "Hello!",
    "avatar": "img.png"
  }
}
```
**When to use:**
- Data is small
- Data is used together
- Data doesn’t need to be queried independently

## ✔ Option 2: Reference
```json
// Users
{
  "_id": ObjectId("A1"),
  "username": "alex123",
  "profileId": ObjectId("B1")
}

// Profiles
{
  "_id": ObjectId("B1"),
  "bio": "Hello!",
  "avatar": "img.png"
}
```
**When to use:**
- Profile could grow very large
- You need to query profiles separately

---

# 4. One-to-Many Relationships
A **one-to-many** relationship means:
> One document relates to multiple documents.

Example scenarios:
- User → Posts
- Category → Products
- Blog → Comments

## ✔ Option 1: Embed (simple, small sets)
```json
{
  "title": "My Blog Post",
  "content": "Hello world",
  "comments": [
    { "user": "John", "text": "Great post!" },
    { "user": "Lisa", "text": "Thanks for sharing" }
  ]
}
```
**Use this when:**
- Number of children is small
- Comments are always retrieved with the post

## ✔ Option 2: Reference (scalable)
### Parent document
```json
{
  "_id": ObjectId("P1"),
  "title": "My Post",
  "content": "..."
}
```

### Child documents
```json
{
  "_id": ObjectId("C1"),
  "postId": ObjectId("P1"),
  "user": "John",
  "text": "Great post!"
}
```

**Use this when:**
- Children could grow large (e.g., thousands of comments)
- Children need independent querying

---

# 5. Many-to-Many Relationships
A **many-to-many** relationship means:
> Many documents relate to many other documents.

Example scenarios:
- Users ↔ Courses
- Products ↔ Tags

## ✔ Option 1: Reference Lists in Both Collections
### Users
```json
{
  "_id": ObjectId("U1"),
  "name": "Alex",
  "courses": [ObjectId("C1"), ObjectId("C2")]
}
```

### Courses
```json
{
  "_id": ObjectId("C1"),
  "title": "Backend Development",
  "students": [ObjectId("U1"), ObjectId("U4")]
}
```
**Best for:**
- Small–medium relationship sets

## ✔ Option 2: Using a Junction Collection (SQL-like join table)
Example collection: `enrollments`
```json
{
  "userId": ObjectId("U1"),
  "courseId": ObjectId("C1"),
  "createdAt": "2025-01-01"
}
```

Use when:
- The relationship table grows very large
- You need metadata (date enrolled)

---

# 6. Embedding vs Referencing — Quick Rules
| Situation | Preferred Strategy |
|----------|--------------------|
| Small related data | Embed |
| Data read together | Embed |
| Large or growing lists | Reference |
| Data reused in multiple places | Reference |
| Need independent queries | Reference |

Instructor line:
> "MongoDB gives you freedom. Your job is to choose the structure that makes your code easiest and fastest."

---

# 7. Real-Life Modeling Examples
## Example 1: E-commerce
**Product → Reviews (1-to-many)**
- Small reviews → embed
- Thousands of reviews → reference

## Example 2: Social Media
**User → Posts (1-to-many)**
- Posts live in their own collection → reference

**Post → Likes (many-to-many)**
- Likes stored as array of user IDs → embed array

## Example 3: Online Learning Platform
Users ↔ Courses (Many-to-Many)
- Use reference arrays OR junction collection depending on scale

---

# 8. Modeling Exercise (Pencils Down)
Instructor builds a model for a blog app:
- Users
- Posts
- Comments
- Likes

Show embedding vs referencing in each part.

---

# 9. Modeling Exercise (Pencils Up)
Students design a model for one of these:
- Recipe app (recipes, ingredients, reviews)
- Movie database (movies, actors, reviews)
- Fitness app (users, workouts, exercises)
- E-commerce store (users, products, orders)

They must show:
- 1-to-1 example
- 1-to-many example
- Many-to-many example
- Whether embedding or referencing was used

---

# 10. Transition to Homework
Tell students:
> "Next class you will build your database using your own data. Your job now is to bring sample raw data — your model determines your entire backend!"

