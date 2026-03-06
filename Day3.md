# Day 3 – Project Setup, Git Ignore, Nodemon & Prettier

## 1. Git does not push empty folders

Git does **not track empty folders**.  
If a folder has no files inside it, Git will ignore it automatically.

### Solution

Create a placeholder file inside the folder.

```
.gitkeep
```

Example:

```
uploads/
   └── .gitkeep
```

This allows Git to track the folder.

---

# 2. `.gitignore`

The `.gitignore` file tells Git **which files or folders should not be pushed to GitHub**.

Common things we ignore:

- `node_modules`
- `.env`
- build folders
- logs
- OS files

### How to generate `.gitignore`

Developers usually use:

```
https://www.toptal.com/developers/gitignore
```

Select:

```
Node
VisualStudioCode
macOS (if needed)
```

Example `.gitignore`

```
node_modules
.env
dist
.vscode
```

---

# 3. `.env` File

The `.env` file stores **environment variables**.

These are **sensitive values** that should not be pushed to GitHub.

Examples:

- Database URL
- API Keys
- JWT secrets
- Port numbers

Example `.env`

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/mydatabase
JWT_SECRET=mysecretkey
```

Usage in Node.js:

```javascript
import dotenv from "dotenv"

dotenv.config()

const PORT = process.env.PORT
```

---

# 4. Project Folder Structure

A clean backend project usually follows this structure.

```
project-name
│
├── public
│
├── src
│   ├── controllers
│   ├── db
│   ├── middlewares
│   ├── models
│   ├── routes
│   ├── utils
│   │
│   ├── app.js
│   └── index.js
|   |__constansts.js
│
├── .env
├── .gitignore
├── package.json
```

### Folder Purpose

**controllers**  
Contains business logic.

**db**  
Database connection setup.

**middlewares**  
Custom middleware functions.

**models**  
Database schemas (Mongoose models).

**routes**  
API route definitions.

**utils**  
Reusable helper functions.

**app.js**  
Express app configuration.

**index.js**  
Entry point of the application.

---

# 5. Dev Dependencies

Development dependencies are packages used **only during development**, not in production.

Install using:

```
npm install -D package-name
```

Example:

```
npm install -D nodemon prettier
```

---

# 6. Nodemon

**Nodemon** automatically restarts the server when code changes.

Without nodemon:

```
node src/index.js
```

You must restart the server manually after every change.

With nodemon:

```
nodemon src/index.js
```

The server restarts automatically.

---

# 7. Add Nodemon Script

Inside `package.json`

```json
"scripts": {
  "dev": "nodemon src/index.js"
}
```

Run the server using:

```
npm run dev
```

This runs:

```
nodemon src/index.js
```

---

# 8. Prettier

**Prettier** is a code formatter that automatically formats code.

Benefits:

- Consistent code style
- Cleaner code
- Better team collaboration

Install:

```
npm install -D prettier
```

---

# 9. `.prettierrc`

This file stores **Prettier configuration settings**.

Example:

```json
{
  "singleQuote": false,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

Explanation:

| Option | Meaning |
|------|------|
| singleQuote | Use double quotes |
| tabWidth | 2 spaces indentation |
| trailingComma | Adds commas where valid in ES5 |

---

# 10. `.prettierignore`

This file tells Prettier **which files should not be formatted**.

Example:

```
# Environment files
.env
.env.*

# Dependencies
node_modules

# Build output
dist

# Editor settings
.vscode
```

---

# 11. Example `index.js`

This file starts the server.

```javascript
import express from "express"

const app = express()

app.get("/", (req, res) => {
  res.send("Server is running")
})

const PORT = 5000

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

---

# Key Learning Points

- Git cannot track empty folders → use `.gitkeep`
- `.gitignore` prevents unnecessary files from being pushed
- `.env` stores sensitive configuration
- `nodemon` auto-restarts server during development
- `prettier` formats code automatically
- Use proper **backend folder structure** for scalable projects
