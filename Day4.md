# Day 4 – MongoDB Connection & Environment Variables

## 1. Why Environment Variables Are Used

In backend development, sensitive information should **not be written directly in the source code**.

Examples:
- Database URI
- API Keys
- Secret Tokens
- Port numbers

Instead, we store them inside a **`.env` file** and load them using the **dotenv package**.

Example `.env` file:

```env
PORT=8000
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net
```

These values can be accessed in Node.js using:

```javascript
process.env.PORT
process.env.MONGODB_URI
```

This improves **security, flexibility, and maintainability**.

---

# 2. Installing dotenv

To load environment variables from the `.env` file:

```bash
npm install dotenv
```

dotenv reads the `.env` file and loads variables into **process.env**.

---

# 3. Project Structure

A clean backend structure helps scalability.

```
project-root
│
├── src
│   ├── db
│   │   └── index.js
│   │
│   ├── constants.js
│   │
│   └── index.js
│
├── .env
├── package.json
└── node_modules
```

Explanation:

- `db/` → database connection logic  
- `constants.js` → stores constants like database name  
- `index.js` → main entry point  
- `.env` → environment variables  

---

# 4. Creating Database Name Constant

`constants.js`

```javascript
export const DB_NAME = "backendDB";
```

Separating constants improves **code readability and maintainability**.

---

# 5. Professional MongoDB Connection

`src/db/index.js`

```javascript
import mongoose from "mongoose";
import { DB_NAME } from "../constants.js";

const connectDB = async () => {
  try {
    const connectionInstance = await mongoose.connect(
      `${process.env.MONGODB_URI}/${DB_NAME}`
    );

    console.log("MongoDb Connected:", connectionInstance.connection.host);
  } catch (error) {
    console.log("Something went wrong:", error);
    process.exit(1);
  }
};

export default connectDB;
```

Explanation:

`mongoose.connect()` connects the Node.js application to MongoDB.

Connection string used:

```
${process.env.MONGODB_URI}/${DB_NAME}
```

Example final connection string:

```
mongodb+srv://username:password@cluster.mongodb.net/backendDB
```

Important parts:

- `process.env.MONGODB_URI` → loaded from `.env`
- `DB_NAME` → imported constant
- `connectionInstance.connection.host` → prints the MongoDB host
- `process.exit(1)` → stops the application if connection fails

---

# 6. Loading Environment Variables and Starting Database

`src/index.js`

```javascript
import dotenv from "dotenv";
import connectDB from "./db/index.js";

dotenv.config();

connectDB();
```

Explanation:

`dotenv.config()` loads environment variables from `.env` into `process.env`.

Then `connectDB()` establishes the database connection.

---

# 7. Application Execution Flow

When the backend starts:

1. Application starts
2. dotenv loads `.env`
3. connectDB() is called
4. Mongoose connects to MongoDB
5. Success message appears

Example terminal output:

```
MongoDb Connected: ac-xxxx-shard-00-01.mongodb.net
```

---

# 8. Why This Is a Professional Approach

Advantages:

- Database logic separated from main server
- Clean folder structure
- Secure environment variables
- Proper error handling
- Easier to scale large projects

This structure is widely used in **MERN stack backend development**.

---

# 9. Security Best Practice

`.env` file should **never be pushed to GitHub**.

Add this to `.gitignore`:

```
.env
```

This prevents **database credentials and secrets from being exposed**.

---

# Summary

Today we learned:

- Importance of environment variables
- Installing and using dotenv
- Professional Node.js backend structure
- MongoDB connection using Mongoose
- Separating database logic from server
- Protecting secrets using `.env`
