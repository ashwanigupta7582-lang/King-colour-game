const express = require('express');
const mongoose = require('mongoose');
const app = express();

// Render ke liye zaroori: dynamic port
const PORT = process.env.PORT || 10000;

// MongoDB connection
// Yahan process.env.MONGO_URI ka use kiya gaya hai
const dbURI = process.env.MONGO_URI;

mongoose.connect(dbURI)
  .then(() => console.log("MongoDB Connected Successfully"))
  .catch(err => console.log("Database Connection Error:", err));

app.get('/', (req, res) => {
    res.send('Game Server is Running!');
});

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
