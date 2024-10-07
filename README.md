# Database Interacation in Web Applications

This demonstrates the cconnection of MySQL database and Node.js to create a simple API

## Requirements
- [Node.js](https://nodejs.org/) installed
-  MySQL installed and running
-  A code editor, like [Visual Studio Code](https://code.visualstudio.com/download)

## Setup
1. Clone the repository
2. Initialize the node.js environment
   ```
   npm init -y
   ```
3. Install the necessary dependancies
   ```
   npm install express mysql2 dotenv nodemon
   ```
4. Create a ``` server.js ``` and ```.env``` files
5. Basic ```server.js``` setup
   <br>
   
   ```js
   const express = require('express')
   const app = express()

   
   // Question 1 goes here


   // Question 2 goes here


   // Question 3 goes here


   // Question 4 goes here

   

   // listen to the server
   const PORT = 3000
   app.listen(PORT, () => {
     console.log(`server is runnig on http://localhost:${PORT}`)
   })
   ```
<br><br>

## Run the server
   ```
   nodemon server.js
   ```
<br><br>

## Setup the ```.env``` file
```.env
DB_USERNAME=root
DB_HOST=localhost
DB_PASSWORD=your_password
DB_NAME=hospital_db
```

<br><br>

## Configure the database connection and test the connection
Configure the ```server.js``` file to access the credentials in the ```.env``` to use them in the database connection

<br>

## 1. Retrieve all patients
Create a ```GET``` endpoint that retrieves all patients and displays their:
- ```patient_id```
- ```first_name```
- ```last_name```
- ```date_of_birth```

<br>

## 2. Retrieve all providers
Create a ```GET``` endpoint that displays all providers with their:
- ```first_name```
- ```last_name```
- ```provider_specialty```

<br>

## 3. Filter patients by First Name
Create a ```GET``` endpoint that retrieves all patients by their first name

<br>

## 4. Retrieve all providers by their specialty
Create a ```GET``` endpoint that retrieves all providers by their specialty

<br>


-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Answer back-up:

1. // Import required packages
   
const express = require('express');
const dotenv = require('dotenv');
const mysql = require('mysql2');  // Use mysql2 instead of mysql

// Load environment variables from .env file

dotenv.config();

// Create an instance of express

const app = express();

// Set the port from the environment variables or default to 3000

const PORT = process.env.PORT || 3000;

// Database connection configuration

const db = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME,
    port: process.env.DB_PORT
});

// Connect to the database

db.connect((err) => {
    if (err) {
        console.error('Database connection failed: ' + err.stack);
        return;
    }
    console.log('Connected to the database');
});

// Define the GET endpoint to retrieve all patients

app.get('/patients', (req, res) => {
    // SQL query to select required fields
    const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients';

    // Execute the query using mysql2
    
    db.query(query, (err, results) => {
        if (err) {
            // Send error response if something goes wrong
            return res.status(500).json({ error: err.message });
        }

        // Send the results as JSON
        
        res.json(results);
    });
});

// Define a basic route (optional)

app.get('/', (req, res) => {
    res.send('Hello, welcome to your Express server!');
});

// Start the server
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});


-------------------------------------------------------------------------------------------------------------

Update 1:

// Import required packages

const express = require('express');
const dotenv = require('dotenv');
const mysql = require('mysql2');  // Using mysql2 for the database connection

// Load environment variables from .env file

dotenv.config();

// Create an instance of express

const app = express();

// Set the port from the environment variables or default to 3000

const PORT = process.env.PORT || 3000;

// Database connection configuration

const db = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME,
    port: process.env.DB_PORT
});

// Connect to the database

db.connect((err) => {
    if (err) {
        console.error('Database connection failed: ' + err.stack);
        return;
    }
    console.log('Connected to the database');
});

// Define the GET endpoint to retrieve all patients (from previous example)

app.get('/patients', (req, res) => {
    const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients';
    db.query(query, (err, results) => {
        if (err) {
            return res.status(500).json({ error: err.message });
        }
        res.json(results);
    });
});

// Define the GET endpoint to retrieve all providers

app.get('/providers', (req, res) => {
    // SQL query to select the required fields from the providers table
    const query = 'SELECT first_name, last_name, provider_specialty FROM providers';

    // Execute the query using mysql2
    
    db.query(query, (err, results) => {
        if (err) {
            // Send error response if something goes wrong
            return res.status(500).json({ error: err.message });
        }

        // Send the results as JSON
        
        res.json(results);
    });
});

// Define a basic route (optional)

app.get('/', (req, res) => {
    res.send('Hello, welcome to your Express server!');
});

// Start the server

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

----------------------------------------------------------------------------------------------------------------

Update 2:

// Import required packages

const express = require('express');
const dotenv = require('dotenv');
const mysql = require('mysql2');  // Using mysql2 for the database connection

// Load environment variables from .env file

dotenv.config();

// Create an instance of express

const app = express();

// Set the port from the environment variables or default to 3000

const PORT = process.env.PORT || 3000;

// Database connection configuration

const db = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME,
    port: process.env.DB_PORT
});

// Connect to the database

db.connect((err) => {
    if (err) {
        console.error('Database connection failed: ' + err.stack);
        return;
    }
    console.log('Connected to the database');
});

// Define the GET endpoint to retrieve patients by their first name

app.get('/patients/search', (req, res) => {
    // Get the 'first_name' query parameter from the request
    const { first_name } = req.query;

    // If no first_name is provided, return a bad request response
    
    if (!first_name) {
        return res.status(400).json({ error: "Please provide a first_name query parameter" });
    }

    // SQL query to select patients with the matching first name
    
    const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients WHERE first_name = ?';

    // Execute the query using mysql2, passing the first_name as a parameter
    
    db.query(query, [first_name], (err, results) => {
        if (err) {
            // Send error response if something goes wrong
            return res.status(500).json({ error: err.message });
        }

        // If no patients are found, return a 404 response
        if (results.length === 0) {
            return res.status(404).json({ message: `No patients found with the first name: ${first_name}` });
        }

        // Send the results as JSON
        
        res.json(results);
    });
});

// Define a basic route (optional)

app.get('/', (req, res) => {
    res.send('Hello, welcome to your Express server!');
});

// Start the server

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

-------------------------------------------------------------------------------------------------------------

Update3:

// Import required packages

const express = require('express');
const dotenv = require('dotenv');
const mysql = require('mysql2');  // Using mysql2 for the database connection

// Load environment variables from .env file

dotenv.config();

// Create an instance of express

const app = express();

// Set the port from the environment variables or default to 3000

const PORT = process.env.PORT || 3000;

// Database connection configuration

const db = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME,
    port: process.env.DB_PORT
});

// Connect to the database

db.connect((err) => {
    if (err) {
        console.error('Database connection failed: ' + err.stack);
        return;
    }
    console.log('Connected to the database');
});

// Define the GET endpoint to retrieve providers by their specialty column

app.get('/providers/specialty', (req, res) => {
    // Get the 'specialty' query parameter from the request
    const { specialty } = req.query;

    // If no specialty is provided, return a bad request response
    
    if (!specialty) {
        return res.status(400).json({ error: "Please provide a specialty query parameter" });
    }

    // SQL query to select providers with the matching specialty column
    
    const query = 'SELECT provider_id, first_name, last_name, provider_specialty FROM providers WHERE provider_specialty = ?';

    // Execute the query using mysql2, passing the specialty as a parameter
    
    db.query(query, [specialty], (err, results) => {
        if (err) {
            // Send error response if something goes wrong
            return res.status(500).json({ error: err.message });
        }

        // If no providers are found, return a 404 response
        if (results.length === 0) {
            return res.status(404).json({ message: `No providers found with the specialty: ${specialty}` });
        }

        // Send the results as JSON
        res.json(results);
    });
});

// Define a basic route (optional)

app.get('/', (req, res) => {
    res.send('Hello, welcome to your Express server!');
});

// Start the server

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

## NOTE: Do not fork this repository
