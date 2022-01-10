# Sockets

https://socket.io/

1) npm init -y
2) npm i express http nodemon socket.io
3) file index.js
4) scripts { "start": "nodemon index.js" } 
5) Do index.js souboru -> 
const express = require("express");
const http = require("http");
const { Server } = require("socket.io")
const app = express();
const server = http.createServer(app);
const io = new Server(server, {
   cors: {
       origin: "*"
   } 
});
const PORT = 3000;

server.listen(PORT, () => consloe.log(`Server is running on ${PORT}`));
