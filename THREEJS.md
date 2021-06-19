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

If You don't add camera to scene, it is automatically added for you.

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

## All About Cameras

### Camera
Abstract base class for camera. Not intended to be used directly. It's a class that should be inherited when you build a new camera with `PerspectiveCamera` or `OrthographicCamera`. All cameras below inherit the Camera class.

### ArrayCamera
An array of cameras to efficiently render a scene. Very important when creating VR scenes. This is useful when you have multiplayer game and playing with split screen. Renders scene from multiple cameras on specific areas of the render. 

![image](https://user-images.githubusercontent.com/75579372/122254799-5447e400-ce82-11eb-9259-728d5bb3748c.png)

### CubeCamera
A cubeCamera is 6 cameras. Used often to create more realistic reflections on objects like sphere, cube, etc. One forward, back, left, right, to, and bottom. Create a render of the surrounding. Things like 360 maps, reflection, shadows, and so on.

![image](https://user-images.githubusercontent.com/75579372/122256201-afc6a180-ce83-11eb-81b0-b12b8de9f72b.png)

### Orthographic Camera
Renders the scene without perspective. Useful for 2D games. If you go further, the characters stays the same size.Object in front of you will have same **SIZE** despite how far or close. Look at the image below, a orthographic camera is more like a rectangle compared to the Perspective camera looking like a cone. 
`const camera = new THREE.OrthographicCamera(left, right, top, bottom, near?, far?)`

![image](https://user-images.githubusercontent.com/75579372/122259231-d0442b00-ce86-11eb-8e26-bf688cd81436.png)


To keep the camera aspect ratio: `const camera = new THREE.OrthographicCamera(-1 * aspectRatio, 1 * aspectRatio, 1, -1, 0.1, 100);`

### Perspective Camera
Most common for rendering 3D scenes and micmic human eyes.
Decide the Vertical FOV in the beginning. 
`new THREE.PerpsectiveCamera(Vertical FOV in degrees, near, far)`
DO NOT put for `near` or `far` something like: `0.0000001` and `999999999999999`. This is a bug called z-fighting when the GPU doesn't know what object is in front or behind.

![image](https://user-images.githubusercontent.com/75579372/122259262-d803cf80-ce86-11eb-828f-e2d59f2772be.png)

### SteroCamera
Render the scene through two renders to micmic the eye. Can create depth effect, red and blue glasses, etc. Imagine VR. One eye will see one render camera. Another eye will see another render camera.

## Fullscreen mode
```
body { overflow: hidden }
canvas {
  display: block;
  outline: none; <-- other browser can have blue outline
}
```
Remember to update the camera as well:
```
window.addEventListener("resize", (e) => {
  //update renderer
  renderer.setSize(window.innerWidth, window.innerHeight);
  // update camera
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  //update pixel ratio in case go to new screen
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
});
```

### Blurry or stair like cube?
You might see this when your screen has a pixel ratio **greater than 1**. Pixel Ratio corresponds to how many physical pixels you have on the screen for one pixel unit. For years, all screens had a pixel ratio of 1. Now, companies like Apple starts building screens with a pixel ratio of 2. This means 4 times more pixels are renderer.  Highest pixel ratios are usually on the weakest device - mobiles. It's marketing. You don't need more than 2-3. 

![image](https://user-images.githubusercontent.com/75579372/122425227-2fb44080-cf44-11eb-81f1-263196feede0.png)

### ALWAYS HAVE THIS
Use `Window.devicePixelRatio` to check your pixelRatio. And `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2)`. Math.MIN will pick the minimum of the two. If you have a pixelRatio of 1, it will pick 1. If 1.25, it will pick 1.25. But if you have more than 2, it will stay 2. This is how you limit the pixel sizing.


### Listen for double click fullscreen
```
window.addEventListener("dblclick", (e) => {
  const fullscreenElement = document.fullscreenElement || document.webkitFullscreenElement;
  if (!fullscreenElement) {
    if (renderer.domElement.requestFullscreen()) {
      renderer.domElement.requestFullscreen();
    } else if (renderer.domElement.webkitRequestFullscreen()) {
      renderer.domElement.webkitRequestFullscreen();
    }
  } else {
    if (renderer.domElement.exitFullscreen) {
      document.exitFullscreen();
    } else if (renderer.domElement.webkitExitFullscreen) {
      renderer.domElement.webkitExitFullscreen();
    }
    document.exitFullscreen();
  }
});
```

## Geometry
What exactly is a geometry in THREEJS? They are composed of vertices (point coordinates in 3D space) and faces (triangles that join those vertices). Can also be used for meshes and particles. Can store more data than positions (UV Coordinates, normals, colors, anything).

![image](https://user-images.githubusercontent.com/75579372/122444264-37301580-cf55-11eb-9c35-d5822b6aa273.png)


All built in geometries inherit from `Geometry` class. 

### Different Types of Geometries
#### BoxGeometry

![image](https://user-images.githubusercontent.com/75579372/122445849-d275ba80-cf56-11eb-9ed8-6eaf6c610a85.png)

#### CircleGeometry
![image](https://user-images.githubusercontent.com/75579372/122445943-f0dbb600-cf56-11eb-99e2-4a32a63187e9.png)

#### ConeGeometry
![image](https://user-images.githubusercontent.com/75579372/122445960-f6390080-cf56-11eb-8571-708f294aade4.png)

#### CylinderGeometry
![image](https://user-images.githubusercontent.com/75579372/122445972-fc2ee180-cf56-11eb-9b2a-92bae0733681.png)

#### DodecahedronGeometry (12 flat faces)
![image](https://user-images.githubusercontent.com/75579372/122445992-018c2c00-cf57-11eb-9f41-39b7a47f7e8a.png)

#### EdgesGeometry
To help see edges of a geometry
#### ExtrudeGeometry
![image](https://user-images.githubusercontent.com/75579372/122446143-2ed8da00-cf57-11eb-8c1a-ad42ce3324ec.png)

#### IcosahedronGeometry (20 sided face)
![image](https://user-images.githubusercontent.com/75579372/122446174-35675180-cf57-11eb-9f05-9ef1d3102b8b.png)

#### LatheGeometry
![image](https://user-images.githubusercontent.com/75579372/122446229-46b05e00-cf57-11eb-8068-9103f704cdb2.png)

#### OctahedronGeometry
![image](https://user-images.githubusercontent.com/75579372/122446257-4fa12f80-cf57-11eb-92cf-632f9a1d4854.png)

#### ParametricGeometry
![image](https://user-images.githubusercontent.com/75579372/122446270-54fe7a00-cf57-11eb-885d-2d92fe613bda.png)

#### PlaneGeometry
![image](https://user-images.githubusercontent.com/75579372/122446290-5af45b00-cf57-11eb-81ae-8f3251036beb.png)

#### PolyhedronGeometry
A polyhedron is a solid in 3D space with flat faces. Takes array of vertices and project them onto a sphere and devide them up to desire level. Used by Dodecahedron, Isosahedron, octahedron, tetrahedron.

#### RingGeometry
![image](https://user-images.githubusercontent.com/75579372/122446502-97c05200-cf57-11eb-8e49-e2b73a92b838.png)

#### ShapeGeometry
Creating on sided shapes from path shapes. Like using `moveTo` and `bezierCurveTo`. 

![image](https://user-images.githubusercontent.com/75579372/122446542-a444aa80-cf57-11eb-920f-eb42af7484dd.png)

#### SphereGeometry
![image](https://user-images.githubusercontent.com/75579372/122446674-c8a08700-cf57-11eb-95ff-32c65ac4bb5c.png)

#### TetrahedronGeometry
![image](https://user-images.githubusercontent.com/75579372/122446709-d2c28580-cf57-11eb-9066-572ece0c7ffb.png)

#### TextGeometry
![image](https://user-images.githubusercontent.com/75579372/122446811-ee2d9080-cf57-11eb-9978-9d79d2549914.png)

`TextGeometry(text, parameters)`

#### TorusGeometry
![image](https://user-images.githubusercontent.com/75579372/122446880-01406080-cf58-11eb-9d18-4bb668aa8390.png)

#### TorusKnotGeometry
![image](https://user-images.githubusercontent.com/75579372/122446929-10271300-cf58-11eb-8b1a-0d22da5e766a.png)


#### TubeGeometry
![image](https://user-images.githubusercontent.com/75579372/122446978-1b7a3e80-cf58-11eb-9d79-fb8f98e305df.png)


#### WireframeGeometry
![image](https://user-images.githubusercontent.com/75579372/122447038-2a60f100-cf58-11eb-9754-5c8cbbfe21ad.png)

### Why do you need more triangles?
![image](https://user-images.githubusercontent.com/75579372/122448292-782a2900-cf59-11eb-8a6a-0f8dbc10f213.png)

You need more if you want to do complex design such as a mountain. With more triangles, you can create a irregular terrain.

## To Create a Geometry without any built in: 
```
const geometry = new THREE.Geometry();

// creates the vertices
const vertex1 = new THREE.Vector3(0, 0, 0);
const vertex2 = new THREE.Vector3(0, 3, 0);
const vertex3 = new THREE.Vector3(1, 0, 0);
geometry.vertices.push(vertex1);
geometry.vertices.push(vertex2);
geometry.vertices.push(vertex3);

// create face. You put 0, 1, 2 because they are the index inside the vertices array.
const face = new THREE.Face3(0, 1, 2);
geometry.faces.push(face);
```
## BUFFER GEOMETRIES
Are much more efficient and optimized but less developer-friendly. You have to use them for performance reasoning. You will have to write a lot more code. When they are simple geometries, just use them! Get into the of using them. 

It will be a bit harder when creating your own buffered geometries
Geometry share common vertices and you can specify a bunch of vertices that can be reused by the GPU.
### Float32Array
A typed array. Can only store floats. Have a fixed length. Easier to handle for the computer. 

## Creating a Debug Panel
This is very important because it takes too long to change the value by yourself. It doesn't just serve for you to makign it easy but also for the client. It's very important and must not neglect. What you need a Debug UI. Without it, you won't be able to find the perfect values! 

Libraries you can use are
```
dat.GUI
control-panel
Guify
Oui
```

### dat.GUI
![image](https://user-images.githubusercontent.com/75579372/122578543-d104ca80-d008-11eb-8ea2-b8827797ae3d.png)

![image](https://user-images.githubusercontent.com/75579372/122588355-d6b3dd80-d013-11eb-941f-01c50e07496f.png)

```
import * as dat from "dat.gui"
const gui = new dat.GUI();
```

You can have folders and go deeper. To add a property, `gui.add(object, property you want to tweak)`. It only accepts an object to tweak. By doing this, you only however get numbers. To have more controls, add paramters. `gui.add(mesh.position, min, max, step)`

![image](https://user-images.githubusercontent.com/75579372/122588946-87ba7800-d014-11eb-8e4f-e4f25a1e0733.png)

Another way: 
`gui.add(torusKnot.position, "x").min(-3).max(3).step(0.01);` This is called chaining. 

Doing this definitely makes life easier. No longer do you have to manage a lot of different objects and instead delegate it to the GUI.

Press `H` or `gui.hide()` to hide. `new dat.GUI({closed: true})` to close on default.  `new dat.GUI({width: 400})` to increase default width.
#### Checkbox
`gui.add(torusKnot, "visible")` It will automatically detect boolean values. 

#### Color
```
const parameters = {
  color: 0xff0000,
};
```

`gui.add(parameters, "color").onChange(() => mesh.material.set(paramters.color))`


![image](https://user-images.githubusercontent.com/75579372/122591610-09f86b80-d018-11eb-96ca-835f0602318a.png)

#### Functions
A function has to be stored in an object.


# Textures
Textures are based on images that cover surface of geometries. 

### Color (Albedo)
* Most simple one
* Applied on the geometry

![image](https://user-images.githubusercontent.com/75579372/122622679-4e066300-d04e-11eb-96e3-fe7d3dedf416.png)


### Alpha
* Grayscale image
* white visible
* black not visible
* grey half visible

### Height
* Create elevation. 
* grayscale image
* If white, it goes up. If down, it goes down. If perfect white and black, it won't move.
* Move the vertices to create the height difference.
* Need a lot of subdivisions

![image](https://user-images.githubusercontent.com/75579372/122622702-65455080-d04e-11eb-986e-438eadc8bc91.png)

![image](https://user-images.githubusercontent.com/75579372/122622745-8e65e100-d04e-11eb-80eb-0b0af51ef82a.png)


### Normal
* Add details on lighting
* Doesn't need subdivisions
* Lure the light of the face orientation.
* Make it look like it has height but doesn't.
* Better performances than adding a height texture with a lot of subdivisions
* Really cool.

![image](https://user-images.githubusercontent.com/75579372/122622748-9291fe80-d04e-11eb-976a-4c20bf1db305.png)


### Ambient
* Grayscale mage
* add fake shadows in crevices
* Not physically accurate
* Helps to create contrats and see details

![image](https://user-images.githubusercontent.com/75579372/122622504-c6b8ef80-d04d-11eb-8716-b492eb39c47c.png)

### Metalness
* Grayscale image
* White is metallic
* Black is non-metallic
* Mostly for reflection.
* Can see what is behind

![image](https://user-images.githubusercontent.com/75579372/122622531-de907380-d04d-11eb-827a-ffed9765d0c9.png)

### Roughness
* Grayscale image
* Works in duo with metalness
* White is rough. Black is smooth.
* For light dissipation. Carpet is really rough which means it's really white. Think of a mirror. It is very smooth so it will be completely black. This is so the light can bounce. 

# Texture Follows PBR (Physically Based Rendering)
* Metalness and roughness really follow this
* Many companies are using it
* Becoming the standard for realistic rendering
* Get really realistic results
* Follow real life. Light bouncing.

![image](https://user-images.githubusercontent.com/75579372/122622544-e9e39f00-d04d-11eb-995a-7f8bfd9911f3.png)

## Using texture image
```
const image = new Image();
const texture = new THREE.Texture(image)
image.onload = () => texture.needsUpdate = true
image.src = "/textures/door/color.jpg"
```

Another way is to use `TextureLoader` 
```
// one texture loader can load multiple textures.
const textureLoader = new THREE.TextureLoader();
const texture = textureLoader.load("/textures/door/color.jpg"); <- has three optional parameters: load, progress, error. All have to be functions. 
```

## LoadingManagers
* Mutualize the entire events and know all the progress in the loadings. You can then create a loading progress bar. Use inside the loader. 
```
const loadingManager = new THREE.LoadingManager();
loadingManager.onStart = () => console.log("loading started");
loadingManager.onProgress = () => console.log("progress");
loadingManager.onLoad = () => console.log("loading finished");

const textureLoader = new THREE.TextureLoader(loadingManager);
```

## What is UV UnWrapping
There is a problem with using different shapes. If you switch from box to a circle, the image will be stretched like this: 

![image](https://user-images.githubusercontent.com/75579372/122624255-20bcb380-d054-11eb-8436-004d509511d8.png)

UV Unwrapping is like unwraping an origami or a candy wrap to make it flat. It's the same id square. 

![image](https://user-images.githubusercontent.com/75579372/122624286-5bbee700-d054-11eb-81cc-0414098af3c2.png)

![image](https://user-images.githubusercontent.com/75579372/122624304-742f0180-d054-11eb-8b76-2fe1b60789da.png)

UV Coordinates are generated by THREEJS that decided where the UV coords are. But if you make your own geometry using Blender, you'll have to specify the UV coordinates.  



### Wrapping
```
colorTexture.repeat.x = 4; <-- think of this as grid-template-rows
colorTexture.repeat.y = 5; <-- think of this as grid-template-columns
colorTexture.wrapS = THREE.MirroredRepeatWrapping;
colorTexture.wrapT = THREE.MirroredRepeatWrapping;
```

ClampToEdgeWrapping is the default. The last pixel of the texture stretches to the edge of the mesh.

### Offset
Set the offset from the beginning position.

![image](https://user-images.githubusercontent.com/75579372/122626306-9e85bc80-d05e-11eb-86ef-aab6975d97f4.png)


### Rotate
```texture.rotation = Math.PI / 3```
![image](https://user-images.githubusercontent.com/75579372/122626341-e0aefe00-d05e-11eb-9546-5c64da075253.png)

The problem with this is that it is rotating at the bottom corner. If you want to do a rotation in the middle, you'll have to move the point. 

![image](https://user-images.githubusercontent.com/75579372/122626409-4f8c5700-d05f-11eb-9c51-539afd720206.png)

To rotate center: do 
```
colorTexture.center.x = 0.5;
colorTexture.center.y = 0.5;
```

## Mipmapping
Technique of creating half a smaller version of a texture again and again until we get `1x1` texture. They are sent to the GPU and chosen for the appriopriate situation. 

![image](https://user-images.githubusercontent.com/75579372/122649223-bbaf9f00-d0e1-11eb-8a3c-bc3a7a561a4b.png)

![image](https://user-images.githubusercontent.com/75579372/122649250-d3872300-d0e1-11eb-85c7-5a0961999dc1.png)


### Minificiation Filter
Happens when the pixel of texture are smaller than the pixel of the render. Can change the minification filter of texture. If you use NearestFilter on mipmapping, you don't need it. You should then deactivate them `colorTexture.generateMipMaps = false`
 
 (From THREEJS DOC)
**NearestFilter** returns the value of the texture element that is nearest (in Manhattan distance) to the specified texture coordinates.

**LinearFilter** is the default and returns the weighted average of the four texture elements that are closest to the specified texture coordinates, and can include items wrapped or repeated from other parts of a texture, depending on the values of wrapS and wrapT, and on the exact mapping.

**NearestMipmapNearestFilter** chooses the mipmap that most closely matches the size of the pixel being textured and uses the NearestFilter criterion (the texel nearest to the center of the pixel) to produce a texture value.

**NearestMipmapLinearFilter** chooses the two mipmaps that most closely match the size of the pixel being textured and uses the NearestFilter criterion to produce a texture value from each mipmap. The final texture value is a weighted average of those two values.

**LinearMipmapNearestFilter** chooses the mipmap that most closely matches the size of the pixel being textured and uses the LinearFilter criterion (a weighted average of the four texels that are closest to the center of the pixel) to produce a texture value.

**LinearMipmapLinearFilter** is the default and chooses the two mipmaps that most closely match the size of the pixel being textured and uses the LinearFilter criterion to produce a texture value from each mipmap. The final texture value is a weighted average of those two values.

### Magnification Filter
The exact opposite of the minificiation filter. Happens when the pixel of the texture are bigger than the pixel of the render. in Other words, texture is too small for the surface to cover it. It looks blurry when stretched. The user will probably won't notice it is being stretched. A solution however if you want to make it not blurry is to use `THREE.NearestFilter` Below is the images that change after using the filter. Using this is better performant friendly. 

![image](https://user-images.githubusercontent.com/75579372/122649648-9a4fb280-d0e3-11eb-8aa9-e47147d41330.png)
![image](https://user-images.githubusercontent.com/75579372/122649638-90c64a80-d0e3-11eb-9502-68c95a1e4aed.png)


### Think of THREE THINGS in Textures
* The Weight (keep it as light as possible by com
  * JPG => lossy compression but lighter
  * PNG => lossless compression but heavier
* The Size or resolution
  * Each pixel will be stored to the GPU regardless of weight. GPU has storage limitation. Even worse when mipmapping. Try to reduce size. 
  * Always use texture of power of `2`. `512x512` `1024x1024` `512x2048`. Width and height don't have to be same. Just have to be the power of 2. 
* The data
  * Texture supports transparency but you cannot have transparency in `jpg`. Better use `png` if you want to comine color and alpha, use `png`. 
  * Data is stored exactly in the file. Usually in `png` 

# Material
* Algorithms are written in programs called shaders
* 















