# EncryptedPasswordMongoDB
## 🔐 Secure User Authentication with Encrypted Passwords using Node.js & MongoDB

This project demonstrates a basic user authentication system using **Node.js**, **Express**, and **MongoDB**, where **passwords are securely stored** in the database using field-level encryption. The frontend is rendered using **EJS templates**, and the UI is styled with custom CSS.

---

## 📚 Features

- User **Signup** and **Login**
- Password encryption using `mongoose-encryption`
- Secure MongoDB database interaction
- Clean and modern UI using plain CSS
- EJS templating engine for dynamic HTML rendering

---

## 📦 Technologies Used

- Node.js
- Express.js
- MongoDB + Mongoose
- EJS (Embedded JavaScript templates)
- Body-parser
- mongoose-encryption (for field-level encryption)
- HTML + CSS

---

## 🧠 Concepts Covered

- **Authentication**: Verifying user credentials (email + password)
- **Authorization**: Allowing access to protected pages only after login
- **Password Encryption**: Securing passwords using a secret key so even if the DB is compromised, the data remains safe
- **EJS Rendering**: Rendering server-side HTML with dynamic content
- **Serving Static Files**: Linking external stylesheets using Express

---

## 🛠️ Project Structure
```
secure-auth/
│
├── app.js # Main server file
├── package.json # Node.js config
├── public/ # Static assets
│ └── styles.css # Styling for all pages
├── views/ # EJS templates
│ ├── home.ejs
│ ├── signup.ejs
│ ├── login.ejs
│ └── access.ejs
└── README.md # This file
```
---

## 🚀 Getting Started

### 1️⃣ Prerequisites

Ensure you have the following installed:

- Node.js and npm
- MongoDB running locally on default port (`mongodb://localhost:27017`)

### 2️⃣ Installation

Clone the repository and install dependencies:

```bash
cd secure-auth
npm install
npm start
```
---
## 🧪 How it Works
### ➕ Signup
User submits email and password.

Password is encrypted using mongoose-encryption.

Data is stored securely in MongoDB.

### 🔑 Login
User submits login credentials.

Encrypted password is automatically decrypted by mongoose-encryption.

If credentials match, user is redirected to a secure dashboard.

---
## 🎨 User Interface
Fully responsive form design

Clean layout with modern buttons and form styling

Easily extendable with Bootstrap, Tailwind, or your own styles

---

## ⚙️ Environment Variables (for production)
Never hard-code secrets in production! Use .env file:
```
ENCRYPTION_SECRET=yourStrongSecretKey
require("dotenv").config();
const secret = process.env.ENCRYPTION_SECRET;
```
---
## ⚠️ Security Notes
mongoose-encryption is educational — in production, use bcrypt for hashing passwords.

Always store secrets securely and use HTTPS in real-world apps.

Add session management (express-session, JWT, etc.) for better authentication control.

---

## 🧼 Future Improvements
Switch to bcrypt for password hashing

Implement session-based login

Add logout and user profile

Use JWT tokens for API-based authentication

Add input validation and error handling

---
## app.js
```
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const encrypt = require("mongoose-encryption");

const app = express();

app.use(express.static("public"));
app.set("view engine", "ejs");
app.use(bodyParser.urlencoded({ extended: true }));

// Connect to MongoDB
mongoose.connect("mongodb://localhost:27017/mydb1");

// Create user schema
const userSchema = new mongoose.Schema({
  email: String,
  password: String,
});

// Secret key for encryption (should be stored in environment variable in production)
const secret = "ThisIsASecretKey";
userSchema.plugin(encrypt, {
  secret: secret,
  encryptedFields: ["password"],
});

// User model
const User = mongoose.model("User", userSchema);

// Routes
app.get("/", (req, res) => res.render("home"));

app.get("/signup", (req, res) => res.render("signup"));

app.get("/login", (req, res) => res.render("login"));

app.post('/signup', async (req, res) => {
    try {
        const newUser = new User({
            email: req.body.username,
            password: req.body.password
        });
        await newUser.save();
        res.render("access");
    } catch (err) {
        console.error(err);
        res.send("Error signing up.");
    }
});

app.post("/login", async (req, res) => {
    const { username, password } = req.body;

    try {
        const foundUser = await User.findOne({ email: username });
        if (foundUser && foundUser.password === password) {
            res.render("access");
        } else {
            res.send("Invalid credentials.");
        }
    } catch (err) {
        console.error(err);
        res.send("Error logging in.");
    }
});

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});

```
---
## home.ejs
```
<link rel="stylesheet" href="/styles.css">
<div class="container">
  <h1>Welcome</h1>
  <div class="nav">
    <a href="/signup">Signup</a>
    <a href="/login">Login</a>
  </div>
</div>

```
---
## login.ejs
```
<link rel="stylesheet" href="/styles.css">
<div class="container">
  <h1>Login</h1>
  <form action="/login" method="POST">
    <label>Email</label>
    <input type="email" name="username" required>

    <label>Password</label>
    <input type="password" name="password" required>

    <button type="submit">Login</button>
  </form>
</div>

```
---
## access.ejs and signup.ejs
```
...so on.
```
---

## ✍️ Author
Made by Hrishikesh

