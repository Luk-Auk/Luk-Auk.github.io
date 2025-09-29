---
title: "ENet Multiplayer in C++"
excerpt: "Adding multiplayer support to an OpenGL game <br/><img src='/images/ENet_Multiplayer/ENet_FrontPage.gif' width='500' height='300'>"
collection: portfolio
---

This project is about adding multiplayer support to a pre-existing work in progress game (fancy way of saying the game had missing core gameplay elements that would need to be added 😅). The game is written in C++ using <a href="https://en.wikipedia.org/wiki/OpenGL" target="_blank"> OpenGL</a> and networking is handled using <a href="http://enet.bespin.org/" target="_blank"> ENet</a> with <a href="https://flatbuffers.dev/" target="_blank"> Flatbuffers</a>.

## Project Structure  

The project follows a <a href="https://en.wikipedia.org/wiki/OpenGL" target="_blank"> Client-Server architecture</a>, where the Server has authority over Clients. 
Basically there are **two** independent projects: the **Server** is a barebones version of the game that runs the simulation while the **Client** only sends its input.

<div style="display: flex; align-items: flex-start; gap: 10px;">
  <div style="text-align: center;">
    <img src="/images/ENet_Multiplayer/Server_Project.png" height="500" style="object-fit: contain;" />
    <div>Server Project</div>
  </div>
  <div style="text-align: center;">
    <img src="/images/ENet_Multiplayer/Client_Project.png" height="500" style="object-fit: contain;" />
    <div>Client Project</div>
  </div>
</div>


## Requests & Responses

Clients only send their input while the server is in-charge of processing them and updating player position and orientation.



<video width="600" autoplay muted loop>
<source src="/files/ENet_Multiplayer/Input.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

### 🎮 Gamestate

When a new player joins, they receive the most up-to-date game state packet:

- **👥 Players**  
  Connected players and their data:  
  - `position`  
  - `orientation`  
  - `velocity`  
  - `acceleration`  
  - `player_id`

- **⚡ Lazers**  
  Each lazer contains:  
  - `direction`  
  - `lifespan`

<video width="600" controls autoplay muted loop>
<source src="/files/ENet_Multiplayer/GameState.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

---

### 👤 SpawnPlayer

Instead of sending **Gamestate** packet to already connected players they receive a **SpawnPlayer** packet when someone new joins.

- This packet only contains data of the newly connected player.  
- Data included:  
  - `position`  
  - `orientation`  
  - `velocity`  
  - `acceleration`  
  - `player_id`

<video width="600" autoplay muted loop>
<source src="/files/ENet_Multiplayer/Spawn.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

---

### ❌👤 DespawnPlayer  
Packet containing the player_id that has disconnected
- Gets sent to everyone connected.
- Data included:
  - `player_id`

<video width="600" autoplay muted loop>
<source src="/files/ENet_Multiplayer/Despawn.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

---

### 🔄👤 UpdatePlayer  
The most frequently sent packet containing new other player positions/orientation in real-time. 
- Gets sent to everyone connected.
- Data included:
  - `new_position`  
  - `new_orientation`  
  - `new_velocity`  
  - `new_acceleration`  
  - `player_id` (which player to update)

*This data is used for Dead Reckoning*

<video width="600" autoplay muted loop>
<source src="/files/ENet_Multiplayer/Update.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

---

### 🌀👤 TeleportPlayer  
Teleports a player to a specific position (used when colliding with objects or getting hit by a laser).  
- Gets sent to everyone connected.
- Data included:
  - `new_position`  
  - `new_orientation`  
  - `player_id` (which player to update)

*Velocity and acceleration are just set to 0*

<video width="600" autoplay muted loop>
<source src="/files/ENet_Multiplayer/Teleport.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

---

### ⚡ SpawnLaser  
Spawns a laser in a given direction with a defined lifespan.  
- Gets sent to all clients.  
- Data included:  
  - `position` 
  - `direction`
  - `life_time`
  - `lazer_id` 

<video width="600" autoplay muted loop>
<source src="/files/ENet_Multiplayer/Spawn_Lazer.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

---

### ❌⚡ DespawnLaser 
Removes a laser from the game using its unique ID.  
- Gets sent to all clients.  
- Data included:  
  - `lazer_id` 

---

### 📶 Dead Reckoning 
A technique called <a href="https://www.researchgate.net/publication/293809946_Believable_Dead_Reckoning_for_Networked_Games" target="_blank"> Dead Reckoning</a> is used to reduce the effects of latency by ensuring game state remains responsive and fluid from player's perspective.

The video below showcases gameplay under **400ms packet delay** and **10% packet loss** network conditions using <a href="https://jagt.github.io/clumsy/index.html" target="_blank"> clumsy</a>.

<video width="600" controls>
    <source src="/files/ENet_Multiplayer/Deadreckoning.mp4" type="video/mp4">
    Your browser does not support the video tag.
</video>

---

*The full protocol document: [.pdf](/files/ENet_Multiplayer/Protocol_v1.4.pdf "Click to view the protocol document")* 

<!-- Source code available upon request (I think they still use this as a course assignment) -->