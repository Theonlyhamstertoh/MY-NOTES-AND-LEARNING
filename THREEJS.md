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

### `camera.lookAt(object)`
This will point the camera at the object. But do it after setting position or else you would only see it's original one instead of the changed.
## Renderer
* Do a render of your scene and see through from your camera point of view. Then, it will show onto the canvas.
* THREEJS will use WebGL to draw render inside the canvas.
* Rendering is like taking picture. If you take picture of your friend and then tell your friend to move, the picture won't show friend moving. So put before renderer.

## BoxGeometry
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
* Position <-- a vector3. 
* Scale
* Rotation
* Quaternion 

Example: 
```
  mesh.rotation.y = 1;  <--- 1 can be anything. You decide if it is feet, yards, miles, and so on. Do what makes you easy.
  mesh.rotation.x = 1;
  mesh.position.length() === finds length from orgin to current position. 
```
## Vector3
A 3d vector and has more than just x y and z. 
All properties will be compiled into matrices. But what is Matrices? 

#### Find distance from one point to another
`mesh.position.distanceTo(camera.position)` 
`mesh.position.distanceTo(new Vector(1, 1, 1)`

####   `position.normalize()`
`mesh.position.normalize();` will bring the mesh object towards the camera so that the length becomes only 1. If you had the cube somewhere 10,000 miles away, you can use `mesh.position.normalize();` to bring the mesh only 1 miles away.

#### `position.set(x, y, z)`
The shorthand for writing 
```
  .position.x = 4;
  .position.y = 4;
  .position.z = 4;
```

### Matrices
Just a rectangle array of numbers. All the numbers are entries in the matrices. When you say it's 2x3 matriv, they are telling you there is two rows and three columns. A 1x1 matrix is just one number. [3, 7, 17] is a 1x3 matrix.
* They are a way to represent information. However, really valuable in computer graphics because they can represent if an object is there or how intense the colors are.

### Axes Helper
Positioning objects is really hard. But if you use the Axes helper, it will show the X Y Z line for you! The X axis is red. The Y axis is green. The Z axis is blue.
![image](https://user-images.githubusercontent.com/75579372/122085333-77f22800-cdb7-11eb-95f4-bf4d87060fe4.png)

To write one:
```
const axesHelper = new THREE.AxesHelper( 5 );
scene.add( axesHelper );
```

### Gimbal Lock
There is a problem with using rotation and it is that two different axes may be rotating in the same direction. For example, take a look at the image below:

![image](https://user-images.githubusercontent.com/75579372/122096838-d4f3db00-cdc3-11eb-9e66-2a2fb694631e.png)
![image](https://user-images.githubusercontent.com/75579372/122096921-ee952280-cdc3-11eb-9d2b-274376546292.png)

and look at what happens when we rotate the blue.
now the green and blue will change in the same direction.

Use `mesh.rotation.reoder("yxz")` so that you change the order

### Quaternion
One solution to avoiding the gimbal lock is to use quaternion.


### Group
Imagine you have a really complex object. A house with walls, bed, windows, roof, frames, piano, stairs, etc and you decided that you want to make the house a bit bigger or move them a little to the left. You will have to literally move every single object this way and it would takes hours! The secret sauce to solving this is to group everything together. Make it a habit to group objects when you should. Now, if you group all the objects as a House object, you can scale, translate, rotate, as a entire object!

To group a object, write 
```
const group = new THREE.Group();
group.add(cube1);
group.add(cube2);
group.position.x = 1  <-- now all items will be moved as one!
```
You can also shorten the way you create mesh items. Put everything in one!


## Animation
Animation is like a stop motion. You move a object and take a picture. Move the object a bit more and take a picture. So you need to take 60 pictures per second to feel smooth. You have to make sure that your animation maintain the high framerate per second. If too slow, you will see the object skipping across. 

**`RequestAnimationFrame()`**
Purpose is **not** to do **ANIMATION**. The purpose is to called the same function again on the next frame. 

#### Problem
A problem that occurs is that if you have a faster framerate per second, you will make the animation go fast. If slow FPS, the cube will move slower. 
To solve this issue, we will need to use `DeltaTime`. `DeltaTime` is the time that is the difference from the new time to previous time. 

Let us say you have a computer that has consistent 60FPS per second and renders a new frame every 10miliseconds. Now, imagine you have another computer that renders at 30FPS now with every new frame with a gap of 20 ms. By the end of a minute, both the slow computer and the fast one will look smooth nontheless. The 10 miliseconds and the 20 are what is called DeltaTime.`30x20 = 600` and `60x10=600` are the same value. 

Let's say you are doing animation like this `cube.rotation.x = 1 * DeltaTime` 

On a fast computer, it would look like, 
`cube.rotation.x = 1 * 10.`

On a slow computer, it would look like
`cube.rotation.x = 1 * 20`

From comparing the two above, you can see that on the slower computer, the rotation is essentially doubled to be twice as fast than the fast computer. This is what keeps the animation in sync no matter how good of a computer you have. The fast computer has 60 frames while the slow has 30. The fast will render 60 times compared to the slow rendering only 30 times. The slow makes up the framerate gap by doubling the rotation speed. 1 frame in slow === 2 frames in fast. And that ratio works out to look smooth throughout the minute. 


#### Alternative Way than DeltaTime
You can use ThreeJS prebuilt `clock.getElapsedTime()` that will start at 0 when initalize. It will then update with that. 
To get a full revolution per second, you will need to write `clock.geteLapsedTime() * Math.PI * 2` 

Don't recommend using `getDelta()`. It may work in one project and don't in another. 

### Use a library if you want to do advance animations. 

Simple GSAP
```
gsap.to(mesh.position, { duration: 1, delay: 1, x: 5, y: 2 });
gsap.to(mesh.position, { duration: 1, delay: 3, x: 0, y: 0 });
const loop = () => {
  requestAnimationFrame(loop);
  renderer.render(scene, camera);
};
loop();
```











