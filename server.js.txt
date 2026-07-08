require('dotenv').config();
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();
const server = http.createServer(app);
const io = new Server(server, { cors: { origin: "*" } });

app.use(cors());
app.use(express.json());

// MongoDB (yahan apna URL dalein)
mongoose.connect(process.env.MONGO_URI || 'mongodb://localhost:27017/game');

let timeLeft = 60;
setInterval(() => {
    timeLeft--;
    if (timeLeft <= 0) timeLeft = 60;
    io.emit('timer', timeLeft);
}, 1000);

const PORT = process.env.PORT || 5000;
server.listen(PORT, () => console.log('Server running on port ' + PORT));

