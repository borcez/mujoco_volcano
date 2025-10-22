<p align="center">
  <a href="https://zalo.github.io/mujoco_wasm/"><img src="./examples/MuJoCoWasmLogo.png" href></a>
</p>
<p align="left">
  <a href="https://github.com/zalo/mujoco_wasm/deployments/activity_log?environment=github-pages">
      <img src="https://img.shields.io/github/deployments/zalo/mujoco_wasm/github-pages?label=Github%20Pages%20Deployment" title="Github Pages Deployment"></a>
  <!--<a href="https://github.com/zalo/mujoco_wasm/deployments/activity_log?environment=Production">
      <img src="https://img.shields.io/github/deployments/zalo/mujoco_wasm/Production?label=Vercel%20Deployment" title="Vercel Deployment"></a> -->
  <!--<a href="https://lgtm.com/projects/g/zalo/mujoco_wasm/context:javascript">
      <img alt="Language grade: JavaScript" src="https://img.shields.io/lgtm/grade/javascript/g/zalo/mujoco_wasm.svg?logo=lgtm&logoWidth=18"/></a> -->
  <a href="https://github.com/zalo/mujoco_wasm/commits/main">
      <img src="https://img.shields.io/github/last-commit/zalo/mujoco_wasm" title="Last Commit Date"></a>
  <a href="https://github.com/zalo/mujoco_wasm/blob/main/LICENSE">
      <img src="https://img.shields.io/badge/license-MIT-brightgreen" title="License: MIT"></a>
</p>

## The Power of MuJoCo in your Browser.

Load and Run MuJoCo 2.3.1 Models using JavaScript and WebAssembly.

This repo is a fork of @stillonearth 's starter repository, adding tons of functionality and a comprehensive example scene.

### [See the Live Demo Here](https://zalo.github.io/mujoco_wasm/)

### [See a more Advanced Example Here](https://kzakka.com/robopianist/)

## 🚁 Quick Start - Skydio X2 Drone Viewer

This repository has been configured to show the **Skydio X2 Drone** simulation.

### Starting the Viewer

**Option 1: Python HTTP Server (Recommended)**
```bash
cd /home/borcez/roba/mujoco_volcano
python3 -m http.server 8100
```
Then open your browser and navigate to: `http://localhost:8100`

**Option 2: Using npx**
```bash
cd /home/borcez/roba/mujoco_volcano
npx http-server -p 8100
```

**Option 3: Using Node.js http-server**
```bash
cd /home/borcez/roba/mujoco_volcano
npm install -g http-server
http-server -p 8100
```

### What's Included

The `examples/scenes/` directory now contains only:
- **skydio_x2.xml** - Quadcopter drone with 4 independently controlled rotors
- **favicon.obj** & **favicon.png** - ROBA logo 3D assets for the drone body

### Controls (in browser)
- **Left Mouse**: Rotate camera
- **Right Mouse**: Pan camera
- **Mouse Wheel**: Zoom in/out
- **Space**: Pause/Resume simulation

## Building

**1. Install emscripten**

**2. Autogenerate the bindings by running src/parse_mjxmacro.py**

**3. Build the mujoco_wasm Binary**

On Linux, use:
```bash
mkdir build
cd build
emcmake cmake ..
make
```

On Windows, run `build_windows.bat`.

An older version of Emscripten (<3.1.56) may be necessary.

*4. (Optional) Update MuJoCo libs*

Build MuJoCo libs with wasm target and place to lib. Currently v2.3.1 included.

## JavaScript API

```javascript
import load_mujoco from "./mujoco_wasm.js";

// Load the MuJoCo Module
const mujoco = await load_mujoco();

// Set up Emscripten's Virtual File System
mujoco.FS.mkdir('/working');
mujoco.FS.mount(mujoco.MEMFS, { root: '.' }, '/working');
mujoco.FS.writeFile("/working/humanoid.xml", await (await fetch("./examples/scenes/humanoid.xml")).text());

// Load in the state from XML
let model       = new mujoco.Model("/working/humanoid.xml");
let state       = new mujoco.State(model);
let simulation  = new mujoco.Simulation(model, state);
```

Typescript definitions are available.

## Work In Progress Disclaimer

So far, most mjModel and mjData state variables and functions (that do not require custom structs) are exposed.

At some point, I'd like to de-opinionate the binding and make it match the original MuJoCo API better.
