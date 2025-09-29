---
title: "Graphics Engine"
excerpt: "Custom 3D engine with math library, mesh loading, lighting and deferred shading <br/><img src='/images/Graphics_Engine/Graphics_Engine_FrontPage.gif' width='500' height='300'>"
collection: portfolio
---

This project is about building a custom 3D graphics engine in C++ with a pre-existing framework that included OpenGL and a basic pipeline rendering and empty window.

## Project Structure

The project was developed iteratively, each major feature is a seperate C++ project this makes testing independent features a lot easier (For assignment grading purposes).

---

### Math Library

One of the core  requirements were to implement a custom math library instead of relying on GLM.
Custom `vec3`, `vec4` and `mat4` classes handle all vector and matrix calculations used in the engine.

---

### Mesh Resource
Supports basic 2D geometry as well as 3D geometry with textures.

<div style="display: flex; flex-wrap: wrap; gap: 20px; justify-content: center; align-items: flex-start;">

  <div style="text-align: center; flex: 0 1 250px;">
    <img src="/images/Graphics_Engine/Rendering_Mesh.png" width="250" height="150" style="object-fit: contain;" />
    <div>2D Mesh</div>
  </div>

  <div style="display: flex; gap: 10px; flex: 1 1 auto; justify-content: center;">
    <div style="text-align: center;">
      <video width="300" height="170" autoplay muted loop playsinline>
        <source src="/files/Graphics_Engine/Brick_Texture.mp4" type="video/mp4">
      </video>
      <div>Brick Cube</div>
    </div>
    <div style="text-align: center;">
      <video width="300" height="170" autoplay muted loop playsinline>
        <source src="/files/Graphics_Engine/Forest_Texture.mp4" type="video/mp4">
      </video>
      <div>Forest Cube</div>
    </div>
  </div>

</div>

---

### Model Loading
Model loading without external libraries 

<div style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div style="text-align: center;">
    <video width="300" height="170" autoplay muted loop playsinline>
      <source src="/files/Graphics_Engine/gltf.mp4" type="video/mp4">
    </video>
    <div>GLTF</div>
  </div>
  <div style="text-align: center;">
    <video width="300" height="170" autoplay muted loop playsinline>
      <source src="/files/Graphics_Engine/obj.mp4" type="video/mp4">
    </video>
    <div>OBJ</div>
  </div>
</div>

---

### Lighting
Custom lighting system 

<div style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div style="text-align: center;">
    <video width="300" height="170" autoplay muted loop playsinline>
      <source src="/files/Graphics_Engine/PointLight.mp4" type="video/mp4">
    </video>
    <div>Point Light</div>
  </div>
  <div style="text-align: center;">
    <video width="300" height="170" autoplay muted loop playsinline>
      <source src="/files/Graphics_Engine/DirectionalLight.mp4" type="video/mp4">
    </video>
    <div>Directional Light (Sun)</div>
  </div>
</div>

---

### Normal Maps
Normal map support to improve surface details

<div style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div style="text-align: center;">
    <video width="300" height="170" autoplay muted loop playsinline>
      <source src="/files/Graphics_Engine/NormalMapHelm.mp4" type="video/mp4">
    </video>
    <div>Damaged Helm</div>
  </div>
  <div style="text-align: center;">
    <video width="300" height="170" autoplay muted loop playsinline>
      <source src="/files/Graphics_Engine/NormalMap.mp4" type="video/mp4">
    </video>
    <div>Normal Tangent Mirror Test</div>
  </div>
</div>


---

### Deferred Shading

Deferred shading pipeline to render scenes with thousands of point lights.  

Visualizing the G-buffer textures and the final render, along with a short demo:

<div style="display: flex; gap: 20px; justify-content: center; align-items: flex-start;">

  <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">

    <div style="text-align: center;">
      <img src="/images/Graphics_Engine/albedo.png" width="200" />
      <div>Albedo</div>
    </div>

    <div style="text-align: center;">
      <img src="/images/Graphics_Engine/normal.png" width="200" />
      <div>Normals</div>
    </div>

    <div style="text-align: center;">
      <img src="/images/Graphics_Engine/positions.png" width="200" />
      <div>Positions</div>
    </div>

    <div style="text-align: center;">
      <img src="/images/Graphics_Engine/final.png" width="200" />
      <div>Final Render</div>
    </div>

  </div>

  <div style="text-align: center;">
    <video width="400" autoplay muted loop>
      <source src="/files/Graphics_Engine/Deferred_Shading.mp4" type="video/mp4">
    </video>
    <div>Deferred Shading Demo</div>
  </div>

</div>


