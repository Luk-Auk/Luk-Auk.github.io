---
title: "ENet Multiplayer in C++"
excerpt: "Adding multiplayer support to an OpenGL game <br/><img src='/images/ENet_Multiplayer/ENet_FrontPage.gif' style='width:500px !important; height:300px !important; object-fit:cover;'>" 
collection: portfolio
---

This project is about adding multiplayer support to a pre-existing work in progress game (fancy way of saying the game had missing core gameplay elements that would need to be added 😅). The game is written in C++ using [OpenGL](https://en.wikipedia.org/wiki/OpenGL) and networking is handled using [ENet](http://enet.bespin.org/) with [Flatbuffers](https://flatbuffers.dev/).

## Project Structure  

The project follows a [Client-Server](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) architecture, where the Server has authority over Clients. 
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

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/Input.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Sending Input</div>
</div>

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

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/GameState.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Receiving Gamestage packet when connected</div>
</div>
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

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/Spawn.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Receiving SpawnPlayer packet when someone connects</div>
</div>

---

### ❌👤 DespawnPlayer  
Packet containing the player_id that has disconnected
- Gets sent to everyone connected.
- Data included:
  - `player_id`

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/Despawn.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Receiving DespawnPlayer packet when someone disconnects</div>
</div>

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



<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/Update.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Receiving UpdatePlayer packet every tick </div>
</div>

---

### 🌀👤 TeleportPlayer  
Teleports a player to a specific position (used when colliding with objects or getting hit by a laser).  
- Gets sent to everyone connected.
- Data included:
  - `new_position`  
  - `new_orientation`  
  - `player_id` (which player to update)

*Velocity and acceleration are just set to 0*

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/Teleport.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Receiving TeleportPlayer packet upon collision </div>
</div>

---

### ⚡ SpawnLaser  
Spawns a laser in a given direction with a defined lifespan.  
- Gets sent to all clients.  
- Data included:  
  - `position` 
  - `direction`
  - `life_time`
  - `lazer_id` 

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/ENet_Multiplayer/Spawn_Lazer.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Receiving SpawnLaser </div>
</div>

---

### ❌⚡ DespawnLaser 
Removes a laser from the game using its unique ID.  
- Gets sent to all clients.  
- Data included:  
  - `lazer_id` 

---

### 📶 Dead Reckoning 
A technique called [Dead Reckoning](https://www.researchgate.net/publication/293809946_Believable_Dead_Reckoning_for_Networked_Games) is used to reduce the effects of latency by ensuring game state remains responsive and fluid from player's perspective.

The video below showcases gameplay under **400ms packet delay** and **10% packet loss** network conditions using [clumsy](https://jagt.github.io/clumsy/index.html).

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
      <source src="/files/ENet_Multiplayer/Deadreckoning.mp4" type="video/mp4">
      Your browser does not support the video tag.
  </video>
</div>

---

*The full protocol document: [.pdf](/files/ENet_Multiplayer/Protocol_v1.4.pdf "Click to view the protocol document")* 
