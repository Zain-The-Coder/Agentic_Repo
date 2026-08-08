# 🚀 How to Connect PostgreSQL

This guide helps you set up a clean, reliable PostgreSQL connection in your Node.js application. Follow these steps to ensure a smooth, bug-free integration.

## 📋 Prerequisites

Before you start, make sure you have the following installed in your project:

- pg
- dotenv

You can install them via terminal:

```bash
npm install pg dotenv
```

## 🛠️ Implementation Steps

### 1. Set Up Your Environment Variable

Ensure your `.env` file is in the root directory of your project. This is critical for security and configuration.

Open your `.env` file and add the following variable:

```
DATABASE_URL=postgresql://<username>:<password>@<host>:5432/your_database_name
```

**Important:** Do not hardcode your credentials directly into your files. Always use the `DATABASE_URL` variable as shown above.

### 2. Create the Database Directory Structure

For better project organization, follow this structure:

- Create a folder named `src` in your project root.
- Inside `src`, create a folder named `db`.
- Inside `db`, create a file named `db.js`.

**Resulting Structure:**

```
your-project/
├── src/
│   └── db/
│       └── db.js
├── .env
└── package.json
```

### 3. Add the Connection Code

Copy and paste the following code into `src/db/db.js`. This script uses a connection pool and includes built-in error handling so you don't have to debug connection drops manually.

```js
const { Pool } = require('pg');

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: process.env.NODE_ENV === 'production' ? { rejectUnauthorized: false } : false,
});

const connectDB = async () => {
  try {
    const client = await pool.connect();
    console.log(`✅ PostgreSQL Connected successfully to: ${client.connectionParameters.host}`);
    client.release();
  } catch (error) {
    console.error(`❌ PostgreSQL Connection Error: ${error.message}`);
    process.exit(1); // Exit process with failure
  }
};

module.exports = { pool, connectDB };
```

### 4. Initialize in Your Main Server File

Finally, call the `connectDB` function in your main server file (e.g., `server.js` or `index.js`):

```js
require('dotenv').config();
const { connectDB } = require('./src/db/db');

// Connect to Database
connectDB();

// ... rest of your server setup
```

### 5. Running a Query (Example)

Once connected, you can use the exported `pool` to run queries anywhere in your app:

```js
const { pool } = require('./src/db/db');

const getUsers = async () => {
  const result = await pool.query('SELECT * FROM users');
  return result.rows;
};
```

If you follow these steps precisely, your PostgreSQL connection will be up and running with zero debugging required.
