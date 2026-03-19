# rantadate
ranting giralfriend and boy friends 
rentadate-app/
│── server.js
│── package.json
│── models/
│     └── User.js
│── routes/
│     └── auth.js
│── public/
│     ├── index.html
│     ├── login.html
│     ├── dashboard.html
│     ├── style.css
│   npm init -y
npm install express mongoose bcryptjs jsonwebtoken cors  └── script.jsconst express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();

app.use(express.json());
app.use(cors());
app.use(express.static("public"));

mongoose.connect("mongodb://127.0.0.1:27017/rentadate");

app.use("/api/auth", require("./routes/auth"));

app.listen(3000, () => {
  console.log("Server running on port 3000");
});const mongoose = require("mongoose");

const UserSchema = new mongoose.Schema({
  name: String,
  email: String,
  password: String,
  gender: String,
  role: String, // client or partner
  pricePerHour: Number,
  location: String
});

module.exports = mongoose.model("User" UserSchema
const express = require("express");
const router = express.Router();
const bcrypt = require("bcryptjs");

const User = require("../models/User");

// Register
router.post("/register", async (req, res) => {
  const { name, email, password, role } = req.body;

  const hash = await bcrypt.hash(password, 10);

  const user = new User({
    name,
    email,
    password: hash,
    role
  });

  await user.save();
  res.json({ message: "User Registered" });
});

// Login
router.post("/login", async (req, res) => {
  const user = await User.findOne({ email: req.body.email });

  if (!user) return res.json({ message: "User not found" });

  const isMatch = await bcrypt.compare(req.body.password, user.password);

  if (!isMatch) return res.json({ message: "Wrong password" });

  res.json({ message: "Login Success", user });
});

module.exports = router;
<!DOCTYPE html>
<html>
<head>
  <title>Rent a Date</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Rent a Date 💕</h1>
  <a href="login.html">Login</a>
</body>
</html><!DOCTYPE html>
<html>
<head>
  <title>Dashboard</title>
</head>
<body>
  <h2>Available Partners</h2>
  <div id="users"></div>
</body>
                          </html>body {
  font-family: Arial;
  text-align: center;
  background: #ffe6eb;
}

h1 {
  color: #ff3366;
}
