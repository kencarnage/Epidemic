# Epidemic Simulation
This project explores different ways a virus can spread and lets the user influence how events unfold in a city populated by blobs. It was build using [ThreeJS](https://threejs.org/)

# Implementation
## Spread algorithm

A day in the simulation is measured by the blobs’ round trip from home to work. Each day, every infected blob has a chance to pass the disease to a random number of other blobs they come into contact with—whether those blobs are already infected or not. I designed it this way to better reflect how diseases can spread in real life.

# Generating paths
The algorithm gives each blob two random houses. Since the city’s layout is pretty simple, this setup works well for the simulation.The graph paths are created based on the buildings’ district and their positions within each district.Using complex algorithms like Dijkstra’s would have made things unnecessarily complicated.For example, a 5×5 district city with 4 buildings in each district would require a graph with 84 nodes and 94 edges!

# Loading GLTF objects, applying textures
I used free 3D objects and converted them into GLTF 2.0 objects Then imported them into ThreeJS with:
```javascript
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

// Basic scene setup
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 5;

const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Add light
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(5, 10, 7.5);
scene.add(light);

// Load a GLTF model (replace with your path)
const loader = new GLTFLoader();
loader.load(
  '/path/to/yourModel.glb',
  (gltf) => {
    const model = gltf.scene;
    scene.add(model);
    console.log("Model loaded into Three.js scene successfully");
  },
  undefined,
  (error) => console.error('Error loading model:', error)
);

// Animation loop
function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
}

animate();

