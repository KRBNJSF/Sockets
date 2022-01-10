# Sockets

https://socket.io/

1) npm init -y
2) npm i express http nodemon socket.io
3) file index.js
4) scripts { "start": "nodemon index.js" } 
5) Do index.js souboru -> 
```
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

const activeUsers = new Set();

io.on("connection", (socket) => {
    console.log("New connection");

    socket.on("new user connected", (data) => {
        console.log(`${data} connected`); //Printing out connected data -> (user)
        socket.data.user = data;          //Get users name
        activeUsers.add(data);            //Adding user to active users
        io.emit("new user connected", [...activeUsers]); //Converting activeUser to an Array
    });

    socket.on("disconnect", () => {                      //No data were sent
        console.log(`${socket.data.user} disconnected`); //Printing out logged out data -> (user)
        activeUsers.delete(socket.data.user);            //Deleting data (user) from activeUsers
        io.emit("user disconnected", socket.data.user);  //Sending data to others that someone has disconnected
    });

});

server.listen(PORT, () => console.log(`Server is running on ${PORT}`));

```
6)Spuštění serveru v node.js -> npm start, vypnutí serveru: Ctrl+c<br>
7)Tvorba složky clients a vklad do hlavní složky, 8 -> Základní struktura složek<br>
8)
![image](https://user-images.githubusercontent.com/90755554/148746804-30a30302-b03d-494f-96ec-466e49f3dddd.png)<br>
9)https://cdnjs.com/libraries/socket.io <br>
10)![image](https://user-images.githubusercontent.com/90755554/148747793-075510a9-3d0e-42dc-9eb7-e81e22d8ca94.png) <br>


