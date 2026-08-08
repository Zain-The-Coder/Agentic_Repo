🚀 How to Connect MongoDB

This guide helps you set up a clean, reliable MongoDB connection in your Node.js application. Follow these steps to ensure a smooth, bug-free integration.

📋 Prerequisites

Before you start, make sure you have the following installed in your project:

mongoose

dotenv

You can install them via terminal:

npm install mongoose dotenv

🛠️ Implementation Steps

1. Set Up Your Environment Variable

Ensure your .env file is in the root directory of your project. This is critical for security and configuration.

Open your .env file and add the following variable:

MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/your_database_name

Important: Do not hardcode your credentials directly into your files. Always use the MONGO_URI variable as shown above.

2. Create the Database Directory Structure

For better project organization, follow this structure:

Create a folder named src in your project root.

Inside src, create a folder named db.

Inside db, create a file named db.js.

Resulting Structure:

your-project/
├── src/
│   └── db/
│       └── db.js
├── .env
└── package.json

3. Add the Connection Code

Copy and paste the following code into src/db/db.js. This script includes built-in error handling so you don't have to debug connection drops manually.

const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });

    console.log(`✅ MongoDB Connected successfully to: ${conn.connection.host}`);
  } catch (error) {
    console.error(`❌ MongoDB Connection Error: ${error.message}`);
    process.exit(1); // Exit process with failure
  }
};

module.exports = connectDB;

4. Initialize in Your Main Server File

Finally, call the connectDB function in your main server file (e.g., server.js or index.js):

require('dotenv').config();
const connectDB = require('./src/db/db');

// Connect to Database
connectDB();

// ... rest of your server setup

If you follow these steps precisely, your MongoDB connection will be up and running with zero debugging required.