---
title: "Strategic AI"
excerpt: "Finite state machine managing workers, resources, and world exploration <br/><img src='/images/Strategic_AI/Strategic_AI_FrontPage.gif' width='500' height='300'>"
collection: portfolio
---

---
This project is about creating a strategic AI by implementing a <a href="https://en.wikipedia.org/wiki/Finite-state_machine" target="_blank"> finite state-machine</a> that makes dynamic decisions. The AI handles world exploration, resource gathering and transportation, building and production.
Developed using <a href="https://angelscript.hazelight.se/" target="_blank">Haze light unreal engine v5.1.1</a> without relying on built-in AI modules.

## Project Structure  

The project is divided into two major components:  

# Managers  

Handle global systems such as world loading, simulation speed, resources, and other background processes.  

### 🌎 World Manager  
The World Manager loads and manages the map from a [.txt](/files/Strategic_AI/MapAI.txt "Click to view the map file") file, where each character represents terrain or world features.

**Tile Legend:**  
- 🟥 **S** = Start (surrounding 8 tiles walkable)  
- 🟩 **T** = Trees (harvestable resource)  
- ⬛ **B** = Mountain (unwalkable)  
- 🟫 **G** = Swamp (0.5x walking speed)  
- 🟦 **V** = Water (unwalkable)  
- ⬜ **M** = Ground (Starts as fog of war -> 🟪 explored)

*The World Manager also maintains fog of war and resource availability.*


### ⚙️ Game Manager

Controls core game simulation parameters:

- ⏱️ **Simulation Speed**  - How many "ticks" (game state updates) per second
- 🧑‍🤝‍🧑 **Starting Units**    - How many worker units you start with (50 default)
- ⚒️ **Upgrade Costs** – Manage how long the upgrade takes and how many resources needed for the upgrade.

<div style="display: flex; align-items: flex-start; gap: 10px;">
  <div style="text-align: center;">
    <img src="/images/Strategic_AI/Slow_Simulation.gif" width="300" />
    <div>1x Simulation Speed</div>
  </div>
  <div style="text-align: center;">
    <img src="/images/Strategic_AI/Fast_Simulation.gif" width="300" />
    <div>25x Simulation Speed</div>
  </div>
</div>

---

# 🧠 GameAI

Controls all the agents (workers, wanderers, builders, coal mill workers) and makes strategic decisions through a finite state machine.


- 📊 **Unit Management** – Creates different types of units depending on circumstances, are there enough explorer units? No? -> creates more.
- 🌲 **Resource Gathering** – Have the explorers discovered trees? Yes -> sends workers to gather.  
- 🎯 **Dynamic Strategy** – Makes decisions based on resources discovered and goals.  
- 🔨 **Building** – Do we have enough wood to start building? Yes -> builds.  

<div style="text-align: center;">
  <video width="600" controls>
    <source src="/files/Strategic_AI/Strategic_AI_Demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>60x Simulation Speed demo</div>
</div>

## 🛠️ Technical Details  

 - 🌐 **Map Size** - 100×100 tile grid with movement costs and terrain types.  
 - 🧭 **Pathfinding** - <a href="https://en.wikipedia.org/wiki/A*_search_algorithm" target="_blank">A* Path finding algorithm</a> to navigate between tiles.  
 - 📝 **Editable Parameters** - Production times, resource values and terrain travel times can be changed.

---

<a href="https://drive.google.com/file/d/19BX4hkLCzrVc2WmQ9S_dqxClKyHmWsRs/view?usp=drive_link" target="_blank">[Project download]</a>
