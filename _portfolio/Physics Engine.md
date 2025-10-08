---
title: "Physics Engine"
excerpt: "Custom physics engine with ray picking, dynamics and collision response <br/><img src='/images/Physics_Engine/Physics_Engine_FrontPage.gif' style='width:500px !important; height:300px !important; object-fit:cover;'>" 
collection: portfolio
---

This project is about building a custom physics engine in C++ with a pre-existing graphics framework that included <a href="https://github.com/ocornut/imgui" target="_blank">ImGui</a> for debugging.

## Project Structure

The project was developed iteratively, each major feature is a separate C++ project this makes testing independent features a lot easier.

---

### Ray Picking & Intersection

Cast rays from screen-space to world-space.
Includes visual debug info and hit detection.

- **Ray Properties:**  
  - `origin` — starting point of the ray (camera position)  
  - `direction` — normalized vector pointing from the camera to the clicked screen position  
  - `IntersectionData` — struct containing intersection information, e.g.:  
    - `vec3 IntersectionPoint` — intersection point in world space 
    - `enum type { Model, Quad }` — intersected object type

- **Debug Info:**  
  - `Length` — length of the ray  
  - `LifeTime` — duration the ray is exists (seconds)  
  - `Ray_Force` — amount of force applied to the object at the intersection point (N)  
  - Drawing toggles: ray / hit_point

<div style="display: flex; gap: 20px; justify-content: center; flex-wrap: wrap;">

  <div style="text-align: center;">
    <video width="300" autoplay muted loop controls>
      <source src="/files/Physics_Engine/ray_models.mp4" type="video/mp4">
       Your browser does not support the video tag.
    </video>
    <div>Raycasting</div>
  </div>

  <div style="text-align: center;">
    <video width="300" autoplay muted loop controls>
      <source src="/files/Physics_Engine/ray_planes.mp4" type="video/mp4">
       Your browser does not support the video tag.
    </video>
    <div>Raycasting against Quads(Planes)</div>
  </div>

</div>

---

###  Unconstrained Dynamics  

Ability to apply arbitrary forces to objects in the scene based on the point it was clicked.
- **Dynamic Variables:**  
  - `linearVelocity` — rate of change of position
  - `angularVelocity` — rate of change of orientation around center of mass
  - `linearAcceleration` — rate of change of linearVelocity with respect to time
  - `InverseInertiaTensorWorld` — inverse inertia tensor transformed to world space (used to calculate how forces generate angular acceleration based on shape and orientation, common shapes have exact [formulas](https://en.wikipedia.org/wiki/List_of_moments_of_inertia))


<div style="text-align: center;">
  <video width="500" autoplay muted loop controls>
    <source src="/files/Physics_Engine/Unconstrained_demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Unconstrained Dynamics Demo</div>
</div>

---

####  Linear Motion  

Handles change in position.
- `linearVelocity += linearAcceleration * dt`  
- `position += linearVelocity * dt`  
- Integrates motion using [Semi-implicit Euler](https://gafferongames.com/post/integration_basics/) 

<div style="text-align: center;">
  <video width="500" autoplay muted loop controls>
    <source src="/files/Physics_Engine/Linear_demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Only Linear Motion</div>
</div>
---

####  Angular Motion  

Handles change in rotation.
- `angularVelocity += I⁻¹ * (r × F)` — applies torque  
- Quaternion updated per frame: `rotation += 0.5 * (angularVelocityQuat * rotation) * dt`  

<div style="text-align: center;">
  <video width="500" autoplay muted loop controls>
    <source src="/files/Physics_Engine/Angular_demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Only Angular Motion</div>
</div>

---

####  Directional force   

Controllable Directional force (gravity)
  - `directedForceDirection = (0, -1, 0)` — direction (downwards in this case)
  - `directedForceMagnitude = 0.9806f` — acceleration (gravity in this case)
- Updates acceleration:  
  `linearAcceleration = directedForceDirection * directedForceMagnitude`  

<div style="text-align: center;">
  <video width="500" autoplay muted loop controls>
    <source src="/files/Physics_Engine/Directional_demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Only Directional Force</div>
</div>

---

### Collision Detection & Response  

Two collision detection methods, one for coarse collisions and one for detailed collisions. [Impulse-based](https://en.wikipedia.org/wiki/Collision_response#Impulse-based_contact_model) response is applied to each object based on results from the detailed collision.

---

#### Broadphase  

Used to identify potential overlapping objects using [AABB](https://developer.mozilla.org/en-US/docs/Games/Techniques/3D_collision_detection) (Axis-Aligned Bounding Box) by checking if the AABB's overlapp first before using a more computationally expensive method.

<div style="text-align: center;">
  <video width="500" autoplay muted loop controls>
    <source src="/files/Physics_Engine/AABB_demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>AABB Overlap Detection</div>
</div>

---

#### Narrowphase  

Runs more computationally expensive collision detection for candidate pairs from broadphase. If true -> returns collision info for response.  

- **Collision Methods:**  
  - [`GJK (Gilbert–Johnson–Keerthi)`](https://dyn4j.org/2010/04/gjk-gilbert-johnson-keerthi/#gjk-support)- detects if convex shapes intersect  
  - [`EPA (Expanding Polytope Algorithm)`](https://dyn4j.org/2010/05/epa-expanding-polytope-algorithm/) - computes penetration depth and collision normal  
  - `CollisionInfo` — returned by EPA if true:  
    - `ContactPointA` / `ContactPointB` — points on each object  
    - `Normal` — collision normal (B – A)  
    - `Depth` — penetration depth  
  - `Impulse Resolution:` - resolves collision based on relative velocities between two objects and data from CollisionInfo


<div style="text-align: center;">
  <video width="500" autoplay muted loop controls>
    <source src="/files/Physics_Engine/Collision_response.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div>Collision Response</div>
</div>


---

### Final Demo

<div style="text-align: center;">
  <video width="600" autoplay muted loop controls>
    <source src="/files/Physics_Engine/Final_demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</div>