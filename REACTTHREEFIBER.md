# REACT THREE FIBER

# Importing
```
yarn add three @react-three/fiber
import ... from "@react-three/fiber"
```
# `<Canvas>`
Saves me from creating a `camera`, `resizing`, `and renders scene every frame` and also `scene`. Automatically adds any children into the canvas without having you to write `scene.add(mesh)`

### Esentially does all of this below: 
```
const canvas = document.querySelector("canvas.webgl");

// Scene
const scene = new THREE.Scene();

const renderer = new THREE.WebGLRenderer({
  canvas: canvas,
});
renderer.setSize(sizes.width, sizes.height);

const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100);
scene.add(camera);

window.addEventListener("resize", () => {
  // Update sizes
  sizes.width = window.innerWidth;
  sizes.height = window.innerHeight;

  // Update camera
  camera.aspect = sizes.width / sizes.height;
  camera.updateProjectionMatrix();

  // Update renderer
  renderer.setSize(sizes.width, sizes.height);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
});
```

# ERRORS
`Uncaught SyntaxError: Unexpected token '!'` => Caused by not installing the react fiber and three









