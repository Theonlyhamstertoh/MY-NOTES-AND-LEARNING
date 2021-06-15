## To use THREE
```
npm i three
import * as THREE from "three";
```

## Scene
Consider what a movie scene is. You have your actors, lights, buildings, right? Same in JS but with objects and shading. Scene is a necessity to have to see. 

## Objects
Can create objects from
* Primitive geometries
* Imported models from like blender
* Particles
* Lights
* ETC

## Mesh
Class representing triangular polygon mesh. It's a combination of geometry (shape) and material (the color of shape)

## Camera
You can have multiple cameras just like a movie set but you really only have one camera being moved around in Javascript. You can also have different types of cameras! 
`
const camera = new THREE.PerspectiveCamera(verticalFOV , width / height, near, far )
scene.add(camera)
` 
If you have a high FOV, you will see a lot more but also have distortion. What the near and far means is that if the object exceeds or falls below the values, it will not render it.

## Renderer
* Do a render of your scene and see through from your camera point of view. Then, it will show onto the canvas.
* THREEJS will use WebGL to draw render inside the canvas.
### BoxGeometry
All of these are optional. By default, it is all pointed towards 1. 
```
THREE.BoxGeometry(
  width: float, * meaning main ones
  height: float, *
  depth (z): Float, *
  widthSegments: Integer, 
  heightSegments: Integer, 
  depthSegment: Integer);
```

To create a geometry, write `const geometry = new THREE.BoxGeometry(1, 1, 1);`

### Creating a simple rotating cube
```
// Scene
const scene = new THREE.Scene();

// Simply a geometry
const geometry = new THREE.BoxGeometry(1, 1, 1);
// MeshBasicMaterial takes an object which can be {color: ...} to style it.
const material = new THREE.MeshBasicMaterial({ color: 0xffccee });
//put together the geometry and the material to form a object!
const cube = new THREE.Mesh(geometry, material);
// add the red cube to the scene.
scene.add(cube);

const sizes = {
  width: 800,
  height: 600,
};
const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height);
camera.position.z = 5;
scene.add(camera);

// renderer
const canvas = document.querySelector(".webgl");
const renderer = new THREE.WebGLRenderer({
  canvas: canvas,
});
// set size of the canvas
renderer.setSize(sizes.width, sizes.height);

// to show the CUBE
renderer.render(scene, camera);
// if you see black, you are inside the object! Colors only exist on the outside but not on the inside.

function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  cube.rotation.z += 0.01;
}
animate();
```


## TRANSFORMING
Any object that comes from **Object3D** has the following properties: 
* Position
* Scale
* Rotation
* Quaternion 

All properties will be compiled into matrices. But what is Matrices?

### Matrices
Just a rectangle array of numbers. All the numbers are entries in the matrices. When you say it's 2x3 matriv, they are telling you there is two rows and three columns. A 1x1 matrix is just one number. [3, 7, 17] is a 1x3 matrix.
* They are a way to represent information. However, really valuable in computer graphics because they can represent if an object is there or how intense the colors are.


