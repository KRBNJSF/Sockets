# Sockets

https://socket.io/

https://socket.io/docs/v4/server-api/

1) npm init -y
2) npm i express http nodemon socket.io express
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
8)Použití socket scriptu (obr. 10 vložím do index.html)
![image](https://user-images.githubusercontent.com/90755554/148746804-30a30302-b03d-494f-96ec-466e49f3dddd.png)<br>
9)https://cdnjs.com/libraries/socket.io <br>
10)![image](https://user-images.githubusercontent.com/90755554/148747793-075510a9-3d0e-42dc-9eb7-e81e22d8ca94.png) <br>
11)Soubor main.js -> ``const socket = io("http://127.0.0.1:3000");``<br>
12) ![image](https://user-images.githubusercontent.com/90755554/148749802-be03d7ea-f44a-46ae-a4aa-54e4bc5f21b6.png)
![image](https://user-images.githubusercontent.com/90755554/148749277-605e0c2c-1170-4f84-8384-cef5d2b7ff6e.png)
![image](https://user-images.githubusercontent.com/90755554/148749557-442d7bac-59bb-4b24-af47-6c90281173c0.png)
![image](https://user-images.githubusercontent.com/90755554/148749630-aae2a24b-f243-4cba-9c18-0ec50a406161.png)<br>
Do index.html -> 
![image](https://user-images.githubusercontent.com/90755554/149747768-1e8ab672-f2da-41ba-9d70-4cd4a50929a2.png)

Do main.js ->
Přidání nových uživatelů na server
```
const socket = io("http://127.0.0.1:3000");
const users = document.getElementById("users");
const input = document.getElementById("input");
const send = document.getElementById("send");
const chat = document.getElementById("chat");

let userName = "";

const onUserConnect = (user) => {
    userName = user || `User${Math.floor(Math.random()*1000000)}`; //Generating random name
    socket.emit("new user connected", userName); //io.on name
    newUser(userName);
};

const newUser = (user) => {
    if(document.querySelector(`.${user}-userlist`)) return;
    const newUserDiv = `
    <div class="${user}-userlist"> 
    <p>${user}</p>
    </div>
    `;
    users.innerHTML += newUserDiv;
}

window.onload = () => {
    onUserConnect();
}

socket.on("new user connected", (data) => { //index.js - row 19 ([on name])
    console.log(data);
    data.map((user) => newUser(user));
});

socket.on("user disconnected", (user)=> {
    document.querySelector(`.${user}-userlist`).remove();
});
```
<b>:</b>
![image](https://user-images.githubusercontent.com/90755554/149750692-1e3816f4-95b4-4932-8803-8b6c8c896b06.png)<br>
Chat -> main.js
```
//CHAT
send.onclick = () => {
    socket.emit("chat", `${userName}: ${input.value}`);
    input.value = "";
}

socket.on("chat", (data) => {
    chat.innerHTML += `<p>${data}</p>`;
});
```
index.js
```
    socket.on("chat", (data)=> {
        io.emit("chat", data);
    });
});
```
