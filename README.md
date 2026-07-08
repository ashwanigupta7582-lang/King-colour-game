{
  "name": "color-game",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.0",
    "socket.io": "^4.6.0",
    "cors": "^2.8.5",
    "dotenv": "^16.0.0"
  }
}
require('dotenv').config();
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();
const server = http.createServer(app);
const io = new Server(server, { cors: { origin: "*" } });

app.use(express.json());
app.use(cors());

// MongoDB connection (Yahan apna URL dalen)
mongoose.connect('YOUR_MONGODB_URL_HERE');

// Game Engine
let timeLeft = 60;
setInterval(() => {
    timeLeft--;
    if (timeLeft <= 0) timeLeft = 60;
    io.emit('timer', timeLeft);
}, 1000);

server.listen(5000, () => console.log('Server running on port 5000'));
import React, { useState, useEffect } from 'react';
import io from 'socket.io-client';

const socket = io('http://localhost:5000');

function App() {
  const [timeLeft, setTimeLeft] = useState(60);

  useEffect(() => {
    socket.on('timer', (data) => setTimeLeft(data));
  }, []);

  return (
    <div style={{ textAlign: 'center', marginTop: '50px' }}>
      <h1>Color Prediction Game</h1>
      <h2>Timer: {timeLeft}s</h2>
    </div>
  );
}
export default App;
