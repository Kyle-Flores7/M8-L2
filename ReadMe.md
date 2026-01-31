# Module 8 – Lab 2: MongoDB Database

## Overview
This lab implements a MongoDB database for a general Blogging application.
The database design is based on the logical model created in Lab 1.

## Database Design

The MongoDB database uses the following collections:

- users
- posts
- comments
- likes

The collections use references to relate data:
- Posts reference users via authorId
- Comments reference both users and posts
- Likes reference both users and posts

This structure mirrors the logical model from Lab 1.

---

## Terminal Commands to Recreate the Database

The following commands can be run in the MongoDB shell (`mongosh`) to recreate
the database and insert sample data.

### 1. Start MongoDB Shell

bash
mongosh

### 2. Create and use Database

use blogging_app

### 3. Create Collections

db.createCollection("users")
db.createCollection("posts")
db.createCollection("comments")
db.createCollection("likes")

### 4. Insert Sample Users

db.users.insertMany([
  {
    name: "Alice",
    email: "alice@example.com",
    password_hash: "hashed_password_1",
    created_at: new Date()
  },
  {
    name: "Bob",
    email: "bob@example.com",
    password_hash: "hashed_password_2",
    created_at: new Date()
  }
])

### 5. Insert Sample Posts

const alice = db.users.findOne({ email: "alice@example.com" })

db.posts.insertMany([
  {
    authorId: alice._id,
    title: "My First Blog Post",
    description: "This is my first post using MongoDB.",
    image_url: "https://example.com/image1.jpg",
    created_at: new Date()
  }
])

### 6. Insert Sample Comments

const post = db.posts.findOne({ title: "My First Blog Post" })
const bob = db.users.findOne({ email: "bob@example.com" })

db.comments.insertMany([
  {
    postId: post._id,
    userId: bob._id,
    content: "Great post!",
    created_at: new Date()
  },
  {
    postId: post._id,
    userId: alice._id,
    content: "Thanks for reading!",
    created_at: new Date()
  }
])

### 7. Insert Sample likes 

db.likes.insertMany([
  {
    postId: post._id,
    userId: bob._id,
    created_at: new Date()
  },
  {
    postId: post._id,
    userId: alice._id,
    created_at: new Date()
  }
])

### 8. Verify Data

db.users.find()
db.posts.find()
db.comments.find()
db.likes.find()

### Summary

This lab demonstrates the creation of a MongoDB database for a blogging
application. The database follows the logical model from Lab 1 and includes
collections for users, posts, comments, and likes. Sample documents were
inserted into each collection, and relationships are represented using
document references.