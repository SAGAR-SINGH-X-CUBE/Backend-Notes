# Day 2 – Mongoose Data Modelling (Todo App & E-Commerce)

## Introduction

**Data Modelling** means designing how data will be structured inside a database.

When using **MongoDB with Mongoose**, we define the structure using **Schemas** and interact with data using **Models**.

Basic Flow:

```
Application → Schema → Model → MongoDB Collection
```

Key things defined in a Schema:

- Fields
- Data Types
- Validation
- Relationships
- Default values

---

# 1. Todo App Data Models

## User Model

Each user can create multiple todos.

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
{
  username: {
    type: String,
    required: true,
    unique: true
  },

  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true
  },

  password: {
    type: String,
    required: true
  }

},
{ timestamps: true }
);

export const User = mongoose.model("User", userSchema);
```

---

## Todo Model

Each Todo belongs to a User and can contain multiple SubTodos.

```javascript
import mongoose from "mongoose";

const todoSchema = new mongoose.Schema(
{
  content: {
    type: String,
    required: true
  },

  complete: {
    type: Boolean,
    default: false
  },

  createdBy: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User"
  },

  subTodos: [
    {
      type: mongoose.Schema.Types.ObjectId,
      ref: "SubTodo"
    }
  ]

},
{ timestamps: true }
);

export const Todo = mongoose.model("Todo", todoSchema);
```

---

## SubTodo Model

SubTodos represent smaller tasks inside a main todo.

```javascript
import mongoose from "mongoose";

const subTodoSchema = new mongoose.Schema(
{
  content: {
    type: String,
    required: true
  },

  complete: {
    type: Boolean,
    default: false
  },

  createdBy: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User"
  }

},
{ timestamps: true }
);

export const SubTodo = mongoose.model("SubTodo", subTodoSchema);
```

---

# 2. E-Commerce Data Models

A typical **E-Commerce backend** contains:

- Users
- Products
- Categories
- Orders
- Order Items

---

# Category Model

Products are grouped into categories.

```javascript
import mongoose from "mongoose";

const categorySchema = new mongoose.Schema(
{
  name: {
    type: String,
    required: true
  }
},
{ timestamps: true }
);

export const Category = mongoose.model("Category", categorySchema);
```

---

# Product Model

Each product belongs to a category and has an owner (seller).

```javascript
import mongoose from "mongoose";

const productSchema = new mongoose.Schema(
{
  name: {
    type: String,
    required: true
  },

  description: {
    type: String,
    required: true
  },

  productImage: {
    type: String
  },

  price: {
    type: Number,
    required: true,
    default: 0
  },

  stock: {
    type: Number,
    default: 0
  },

  category: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Category",
    required: true
  },

  owner: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User"
  }

},
{ timestamps: true }
);

export const Product = mongoose.model("Product", productSchema);
```

---

# Order Item Schema

Each order can contain multiple products.

```javascript
import mongoose from "mongoose";

const orderItemSchema = new mongoose.Schema(
{
  productId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Product"
  },

  quantity: {
    type: Number,
    required: true
  }

},
{ timestamps: true }
);
```

---

# Order Model

Represents a customer purchase.

```javascript
import mongoose from "mongoose";

const orderItemSchema = new mongoose.Schema(
{
  productId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Product"
  },

  quantity: {
    type: Number,
    required: true
  }

},
{ timestamps: true }
);

const orderSchema = new mongoose.Schema(
{
  orderPrice: {
    type: Number,
    required: true
  },

  customer: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User"
  },

  orderItems: [orderItemSchema],

  address: {
    type: String,
    required: true
  },

  status: {
    type: String,
    enum: ["PENDING", "CANCELLED", "DELIVERED"],
    default: "PENDING"
  }

},
{ timestamps: true }
);

export const Order = mongoose.model("Order", orderSchema);
```

---

# Mongoose Concepts Used

## Schema

Defines the structure of documents.

```
const schema = new mongoose.Schema({...})
```

---

## Model

Model allows interaction with a MongoDB collection.

```
const User = mongoose.model("User", userSchema)
```

---

## ObjectId Reference

Used to create relationships between collections.

Example:

```
ref: "User"
```

Used in:

- Product → Owner
- Order → Customer
- Todo → User

---

## Embedded Documents

Sometimes we store documents inside other documents.

Example:

```
orderItems: [orderItemSchema]
```

This embeds order items directly inside the order document.

---

## Timestamps

```
{ timestamps: true }
```

Automatically adds:

```
createdAt
updatedAt
```

---

# Example Data Relationship

```
User
 ├── Todos
 │      └── SubTodos
 │
 └── Orders
        └── OrderItems
              └── Products
                     └── Category
```

---

# Key Learning Points

- Mongoose is an **ODM (Object Data Modeling) library for MongoDB**
- Schemas define **data structure and validation**
- Models allow **CRUD operations**
- `ObjectId + ref` creates **relationships**
- Embedded documents reduce extra queries

---

# Technologies Used

- Node.js
- MongoDB
- Mongoose
