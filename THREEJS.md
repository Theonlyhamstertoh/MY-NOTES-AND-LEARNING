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
const geometry = new THREE.BufferGeometry();
const count = 500;
const positions = new Float32Array(count * 3);
for (let i = 0; i < count * 3; i++) {
  positions[i] = (Math.random - 0.5) * 10;
}
geometry.setAttribute("position", new THREE.BufferAttribute(positions, 3));

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

## MeshBasicMaterial
```
const torusMaterial = new THREE.MeshBasicMaterial({ color: debugObject.torusColor });

OR

const torusMaterial = new THREE.MeshBasicMaterial();
torusMaterial.map = diamondBlock;
```

You however cannot do this with color. Once instantiated without color property, you have to add a Color class to make it work.
```
const torusMaterial = new THREE.MeshBasicMaterial();
torusMaterial.color = "red". // ERROR

torusMaterial.color = new THREE.Color("red")
```

If you want to have transparency: 
```
material.opacity = 0.5;
material.transparent = true; <----------- remember to do this.
```

## MeshNormalMaterial
Normals are information that contains the direction of the outside of the face. A sphere is pointing to all areas of direction. Us for lighting, reflection, refraction, and so on. So this is what does the animation. Share common properties `wireframe` `transparent` `opacity` and `side`. It shows the direction of the particular vertices of where it is pointing. 
Pink = right
Aqua = left
light green = up
purple = down
lavender = towards screen



![image](https://user-images.githubusercontent.com/75579372/122653436-44860500-d0f9-11eb-8e5c-b6cfb3ae4909.png)

![image](https://user-images.githubusercontent.com/75579372/122653440-4a7be600-d0f9-11eb-9b24-9b80f2367701.png)


Also has `flatShading`. If set true will flatten the faces 

![image](https://user-images.githubusercontent.com/75579372/122653471-82832900-d0f9-11eb-9b92-d49b6d1d358c.png)

## DO YOU FIND IT THAT YOU CAN'T SEE A PLANE'S BACKSIDE???
If so, you need to write in the `material`, `material.side = THREE.DoubleSide`. Try to avoid this because it is more calculations for the GPU. 


## MeshMatCapMaterial
Display color by using normals as a reference to pick the right color on a texture that looks like a sphere and puts it on the shape. You can create a sphere in blender, add light, and take a square render. You can also do it in photoshop. 

![image](https://user-images.githubusercontent.com/75579372/122653740-9891e900-d0fb-11eb-89c2-beff718ea631.png)


## MeshDepthMaterial
* Create depth effect. Can be used for fog. 

![image](https://user-images.githubusercontent.com/75579372/122653833-48675680-d0fc-11eb-8194-a665a496877c.png)

When up close

![image](https://user-images.githubusercontent.com/75579372/122653836-4e5d3780-d0fc-11eb-8196-cb032754c957.png)


## MeshLambertMaterial
Reacts to light! Very basic.

## MeshPhongMaterial
A bit more adequate. Gets rid of the strange line pattern. And actually reflects!

![image](https://user-images.githubusercontent.com/75579372/122653957-23bfae80-d0fd-11eb-978a-3f35b7b4f93e.png)

```
material.shininess = 100;
material.specular = new THREE.Color("red")
```
![image](https://user-images.githubusercontent.com/75579372/122653984-510c5c80-d0fd-11eb-8891-78174c35218d.png)


## MeshToonMaterial
A cartoonish effect. If you add a gradient, you have to remember that the image is blurred. You have to do on the below to make sure you can see the shadings.
```
doorGradient.magFilter = THREE.NearestFilter;
doorGradient.minFilter = THREE.NearestFilter;
doorGradient.generateMipmaps = false;
````


![image](https://user-images.githubusercontent.com/75579372/122654034-78632980-d0fd-11eb-8153-17a5074497cb.png)


## MeshStandardMaterial
* The real serious one. The standard one and very important. You will use this a lot. 
* Uses physicall based rendering principle
* Supports lights with more realistic light algorithms
* Supports roughness and metalness


```
material.aoMap = doorAmbientOcclusion;
material.metalnessMap = doorMetalness;
material.roughnessMap = doorRoughness;
material.displacementMap = doorHeight; <-- the variation in height

```

### aoMap 
* Stands for Ambient Occlusion map. Will add shadows where the texture is dark. 
* Not easy to add. You must provide a second set of UV named coordinates. 

![image](https://user-images.githubusercontent.com/75579372/122657105-478eee80-d115-11eb-9a46-72f57c3d200c.png)

```
plane.geometry.setAttribute(
  "uv2",
  new THREE.BufferAttribute(plane.geometry.attributes.uv.array, 2)
);

material.aoMapIntensity = 10;
material.aoMap = doorAmbient;
```

### Displacement 
* Basically the `height.png`. it creates the feeling that there is a real 3D object. 
`material.displacementMap = doorHeight; <-- the variation in height`

### NormalMap
* Adds detail without doing subdivisions. Look at the below! It's much better than having thousands of vertices. Try to use this when you can. 

![image](https://user-images.githubusercontent.com/75579372/122657863-fafae180-d11b-11eb-89ae-27dca2990cde.png)


## MeshPhysicalMaterial
Same as `MeshStandardMaterial` but has a clear cloat above the surface. Very shiny and shiny. Very specific to the objects however. Use this if you need a clear coat. Otherwise, don't use because ti is more calculations for the GPU. 

## PointsMaterials
* Used to create particles. 

## ShaderMaterial and RawShaderMaterial
Can both be used to create your own material. 

## Environment Map
* An image of what is surrounding the scene and can be used to do reflection and refraction. Can do general lighting. 


![image](https://user-images.githubusercontent.com/75579372/122657925-79578380-d11c-11eb-894f-2f67670326ed.png)


```
const cubeTextureLoader = new THREE.CubeTextureLoader();

const environmentMapTexture = cubeTextureLoader
  .load( [
		'px.png',
		'nx.png',
		'py.png',
		'ny.png',
		'pz.png',
		'nz.png'
	] );
```


# TextBufferGeometry
* Generates text as a geometry. You will need a specific font format called typeface. You have to convert fonts to typeface or use THREEJS Font. 


```
const fontLoader = new THREE.FontLoader();
fontLoader.load("/fonts/helvetiker_regular.typeface.json", (font) => {
  const textGeometry = new THREE.TextBufferGeometry("Hello Three.js", {
    font,
    size: 0.5,
    height: 0.2, // depth of the font
    curveSegments: 4,
    bevelEnabled: true,
    bevelThickness: 0.03,
    bevelSize: 0.02,
    bevelOffset: 0,
    bevelSegments: 4,
  });
  

  
  const textMaterial = new THREE.MeshNormalMaterial();
  const text = new THREE.Mesh(textGeometry, textMaterial);
  scene.add(text);
});

```

1. Create font folder
2. Take the fonts from THREE into the font folder
3. Load them with `new THREE.FontLoader()`

With Fonts, you have to use callback. It's a callback function. You have to remember to optimize the font. The round ones have a ton of triangles and that can slow down performances.

## How to center font
Bounding as information associated with the geometry that tells what space has been taken by that geometry. Can be box or sphere. THREEJS use this to calculate if the object on the screen (called frustum culling). We can use this to recenter geometry. If object is behind camera, they won't rerender the things behind. BY default, THREEJS is using sphere bounding. The `min` property isn't 0 because of the bezel
	
![image](https://user-images.githubusercontent.com/75579372/122689584-9fdaf480-d1d8-11eb-8f96-1cdcd11d737d.png)


```
  textGeometry.computeBoundingBox(); <-- compute the bounding box and move the points 50% back and you subtract the bevelThickness and bevelSize to get 0.
  textGeometry.translate(
    -(textGeometry.boundingBox.max.x - 0.02) * 0.5,
    -(textGeometry.boundingBox.max.y - 0.02) * 0.5,
    -(textGeometry.boundingBox.max.z - 0.03) * 0.5
  );
  ```
  
  ## BUT THERE IS A MUCH SIMPLER WAY.
  Just use `textGeometry.center()`. It's actually the above. 
  
  ```console.time("donuts") .... then later..... console.timeEnd("donuts") will show you the time lapse.```


# LIGHTS !!!!!!!

minimal cost - `ambient` `hemisphere`
moderate - `directionalLight` `pointLight`
high - `spotlight` `rectArealight`

## AmbientLight
* Illuminates all objects in the scene equally. Cannot be used to cast shadows because it doesn't have direction. It is good to simulate light bouncing.

```
const light = new THREE.AmbientLight( 0x404040 ); // soft white light
scene.add( light );
```

![image](https://user-images.githubusercontent.com/75579372/122692654-429c6e80-d1eb-11eb-97b1-32c4c66fa6f6.png)


This is what happens if you don't include the AmbientLight. We don't simulate light bouncing. So include it to simulate it.

![image](https://user-images.githubusercontent.com/75579372/122692801-f1d94580-d1eb-11eb-9022-cee1591069cf.png)

If you now use AmbientLight and the same color as the pointLIght or directional light, you can simulate the color. 

![image](https://user-images.githubusercontent.com/75579372/122692861-277e2e80-d1ec-11eb-8031-32d20b70e561.png)


## DirectionalLight
* Emits light in a specific direction. Behave as though it is infinitely far away and the rays produced are all parallel. Common use case to simulate `daylight` and `sun`. 

![image](https://user-images.githubusercontent.com/75579372/122692944-83e14e00-d1ec-11eb-8436-f41f39972a1b.png)


## HemisphereLight
* A light source positioned directly above scene with color fading from sky color to ground. Used for sky. 
`HemisphereLight( skyColor : Integer, groundColor : Integer, intensity : Float )`

## PointLight
* Light that gets emitted from a single point in all direction. LIke a light bulb.

```
const light = new THREE.PointLight( 0xff0000, 1, 100 );
light.position.set( 50, 50, 50 );
scene.add( light );
```
![image](https://user-images.githubusercontent.com/75579372/122693349-52698200-d1ee-11eb-91c2-7be651a5d0e4.png)

![image](https://user-images.githubusercontent.com/75579372/122693224-cc4d3b80-d1ed-11eb-92f7-fbe3df9cd838.png)
![image](https://user-images.githubusercontent.com/75579372/122693234-d7a06700-d1ed-11eb-9745-094392d6fc53.png)
![image](https://user-images.githubusercontent.com/75579372/122693240-dd964800-d1ed-11eb-80b3-28daf36cd547.png)


`Distance`. A smaller distance means the light won't reach far. A longer distance will make it reach far. Think about the minecraft torch. Set it to `2` for decay for physically accucrate rendering.
![image](https://user-images.githubusercontent.com/75579372/122693684-b2145d00-d1ef-11eb-8e8a-7bd443f57eca.png)



## Spotlight
Light emitted from single point and increase along code. A spotlight. 

```

const spotLight = new THREE.SpotLight( 0xffffff );
spotLight.position.set( 100, 1000, 100 );

spotLight.castShadow = true;

spotLight.shadow.mapSize.width = 1024;
spotLight.shadow.mapSize.height = 1024;

spotLight.shadow.camera.near = 500;
spotLight.shadow.camera.far = 4000;
spotLight.shadow.camera.fov = 30;

scene.add( spotLight );
```

![image](https://user-images.githubusercontent.com/75579372/122692488-3532b480-d1ea-11eb-873a-ccb8994fcd1c.png)

Light can cost a lot in performances. Try to add as few lights as possible. Cost the GPU. 



## RectAreaLight
* Emits light uniformly across the face of rectangular plane. Used to simulate strip LED lights or bright windows! No shadow support. You must have `RectAreaLightUnicofrmsLib` and only `MeshStandardMaterial` and `MeshPhysicalMaterial` supoprts. 

```const width = 10;
const height = 10;
const intensity = 1;
const rectLight = new THREE.RectAreaLight( 0xffffff, intensity,  width, height );
rectLight.position.set( 5, 5, 0 );
rectLight.lookAt( 0, 0, 0 );
scene.add( rectLight )

const rectLightHelper = new THREE.RectAreaLightHelper( rectLight );
rectLight.add( rectLightHelper );
```

You can move the light to `lookAt(...)` to see it


![image](https://user-images.githubusercontent.com/75579372/122692509-5a272780-d1ea-11eb-89ce-30be4c8a3a2b.png)

## Penumbra 
`0.25`
![image](https://user-images.githubusercontent.com/75579372/122694482-513a5400-d1f2-11eb-8a9d-42f8828754da.png)

`0`
![image](https://user-images.githubusercontent.com/75579372/122694496-5bf4e900-d1f2-11eb-9de3-2849a0eda479.png)

## Rotating 
It's not looking at vector3D. It's looking at a Object3D. You will have to change the `spotLight.target.position` and `spotlight.target` to change position. You need to do something thing. 

You have to add `spotlight.target` to the `scene`. 


## IF you want A LOT OF LIGHTS. THINK ABOUT `BAKING`
The idea of `Baking` is to bake the light into the texture. This can be done in Blender. Drawback is you cannot move light anymore and have to load huge textures. If the light moves, then the texture won't be updated. 

![image](https://user-images.githubusercontent.com/75579372/122694852-a0cd4f80-d1f3-11eb-8436-8fb4561461c7.png)

NO THREEJS LIGHT AT ALL!!!!!!!!!!!

![image](https://user-images.githubusercontent.com/75579372/122694866-ab87e480-d1f3-11eb-9217-9ab0bd71efbb.png)

## LIGHT HELPERS
It can be really hard to position light when you can't see it. Helpers will help you 

### HEMI LIGHT HELPER
```
const hemisphereLightHelper = new THREE.HemisphereLightHelper(hemiSphereLight, 0.2);
scene.add(hemisphereLightHelper);
```
Mathces the color! Red on top. Blue on bottom. 

![image](https://user-images.githubusercontent.com/75579372/122695037-2bae4a00-d1f4-11eb-8517-9c2e3fcfc6ca.png)

### DIRECTIONAL LIGHT HELPER
```
const directionalLightHelper = new THREE.DirectionalLightHelper(directionalLight, 0.2);
scene.add(directionalLightHelper);
```
![image](https://user-images.githubusercontent.com/75579372/122695143-6ca65e80-d1f4-11eb-9666-d6387984e724.png)


### POINTLIGHTHELPER
```
const pointLightHelper = new THREE.PointLightHelper(pointLight, 0.2);
scene.add(pointLightHelper);
```

![image](https://user-images.githubusercontent.com/75579372/122695216-a24b4780-d1f4-11eb-82e6-7fdc83fce130.png)

### SPOTLIGHT HELPER
```
const spotLightHelper = new THREE.SpotLightHelper(spotLight);
scene.add(spotLightHelper);


function animate() {  spotLightHelper.update(); } <-- if you move the target, you have to update it on the next frame after moving.
```


### RECTAREALIGHTHELPER
```
import { RectAreaLightHelper } from "three/examples/jsm/helpers/RectAreaLightHelper";

const light = new THREE.RectAreaLight( 0xffffbb, 1.0, 5, 5 );
const helper = new RectAreaLightHelper( light );
light.add( helper ); // helper must be added as a child of the light
```



# Shadows
* The shadow on the objects are called `core shadows`. But what we want are `drop shadows`. The shadows on the plane and not just on the object. 
* When you do one render, it will render for each light supporting shadows to simulate what the light sees as if it was a camera. 
* During lights render, a `MeshDepthMaterial` replaces all meshes materials. 
* Lights are then stored in textures and called `shadow maps`. Then used on every material and to receive shadow and projected on the geometry. 
* If you have 100 cameras, you will have 100 shadow maps. Once you have all the shadow maps, it will take them and use them to color our meshes. It will then generate the shadows. 
* **ONLY** `Directional Light`, `PointLight`, and `Spotlight` supports shadows. 

![image](https://user-images.githubusercontent.com/75579372/122947873-56e98400-d32f-11eb-9bec-c94408895250.png)


## To use Shadows
`renderer.shadowMap.enabled = true;`

You have to manually enable shadow. 

First, enable `castShadow = true` on the lights so they can cast shadows. Then, you need to `castShadow` on the objects of `receiveShadow` on the objects. By default they are false. Remember to also enable `shadowMapping` on the renderer to get it to create shadowMaps.  

![image](https://user-images.githubusercontent.com/75579372/122949601-a5e3e900-d330-11eb-8ff6-dda09cf5d6f8.png)

### Baked Shadow
So, shadows don't look to good and requires a lot of work. If you have mutiple, it just looks off. One solution is to do `Baking Shadows`. But it's not really dynamic. You use textures basically and put it as a map on the plane.

![image](https://user-images.githubusercontent.com/75579372/122961502-4e964680-d339-11eb-8143-5b9b8656a85a.png)


### Alternative to Baked shadow 

## Optimizing Shadows
* The `shadow maps` have their own width and height. To access the shadow map, you do `directionalLight.shadow` 
* By default, the shadow map size is only `512x512`. You can make it better by doing a `power of 2` on for the mapSize to get a higher qualty. This is for the mipmapping. 

For each camera, increase width and height
```
directionalLight.shadow.mapSize.width = 1024 * 4;
directionalLight.shadow.mapSize.height = 1024 * 4;
```

### Controlling near and far on the camera
Make the light shadow map to only fit the scene because it is shooting too far. It will improve precision. 

* A directionLight with parallel rays and coming from everywhere and straight. So that's why it uses 
* To help you debug, get a `cameraHelper` 

What is the difference between a `cameraHelper` and a `lightHelper`? Well, with a lightHelper, all you can see is the direction of where the light is pointing at. But remember, a cameraHelper allows you to see the camera width and height. Since there is a orthographic camera in the `Shadow` property of the `Directional Light`, you can use the `CameraHelper` to position the near and far and allow for greater precision

This is important to help you avoid bugs. Now, position the camera:
```
directionalLight.shadow.camera.near = 1;
directionalLight.shadow.camera.far = 6;
```

### Reducing Amplitude
* By reducing the amplitude, it results in a better shadow generated. When you have a huge camera width and height, you have a really small circle and to produce good details on that small circle is much more difficult. But when you reduce the size of the camera, the circle becomes really big and it can capture the details a whole lot better. `However, if the camera values are too far (such as the `far` is too short), it will crop the shadows. 

![image](https://user-images.githubusercontent.com/75579372/122953215-65d23580-d333-11eb-9e85-d5a5f48bf0c0.png)


To reduce amplitude, do this for `directionalLight orthographic camera`:
```
directionalLight.shadow.camera.left = -2;
directionalLight.shadow.camera.right = 2;
directionalLight.shadow.camera.top = 2;
directionalLight.shadow.camera.bottom = -2;
```


### Blurring
`directionalLight.shadow.radius = 10;`

## Shadow Map Algorithms
* `THREE.BasicShadowMap` = very performant but lousy quality
* `THREE.PCFShadowMap` = less performant but smoother edges (default)
* `THREE.PCFSoftShadowMap` = less performant but even softer edges
* `THREE.VSMShadowMap` = less Performant, more constraints, can have unexpected results. m

To use a different type: `renderer.shadowMap.type = THREE.PCSoftShadowMap`

### `THREE.PCSoftShadowMap`
`radius` does not work with `THREE.PCSoftShadowMap`. 

## Spotlight Shadow
Mixing shadows doesn't look good and currently there is not much to do about it. For a `PerpspectiveCamera`, you change the `FOV` to get higher quality. 
`spotLight.shadow.camera.fov = 30`

## PointLight Shadow
Uses a `PerspectiveCamera` but a `pointLight` points in all direction so you can have shadows in every directions. What THREE.JS will do is create 6 shadowMaps and do 6 renderers in 6 directions. It's like creating a cube env map. The camera is looking downwards because the last step was looking down. So you get a perpsective camera looking down. 

IT's a lot of renderers. This is why you should avoid shadows as necessary. 

**YOU CANNOT CONTROL THE FOV. ONLY CHANGE THE MAPSIZE AND NEAR OR FAR**

## Making a ball bounce up and down in circle
```
sphere.position.x = Math.cos(elapsedTime) * 1.5;
sphere.position.z = Math.sin(elapsedTime) * 1.5;
sphere.position.y = Math.abs(Math.sin(elapsedTime * 3));
```


# Particles
* Can create stars, smoke, rain, dust, fire, etc. 
* Can have thousands and thouands of them with a reasonable frame rate

## To Create a Particle
It's like creating a mesh. a `geometry` and a `pointsmaterial` and a `Points` instead of Mesh

```
const particlesGeometry = new THREE.SphereBufferGeometry(1, 32, 32);
const particlesMaterial = new THREE.PointsMaterial({
	size: 0.2,
	sizeAttenuation: true,
});
```
`size`: size of particle
`sizeAttenuation: true`: creates perspective. if close, big. If far, small.



If you want particles everywhere, do: 
```
const textureLoader = new THREE.TextureLoader();
const particleTexture = textureLoader.load("textures/particles/2.png");
const particlesGeometry = new THREE.BufferGeometry(1, 32, 32);
const particlesMaterial = new THREE.PointsMaterial({
  size: 0.2,
  sizeAttenuation: true,
  color: "red",
  transparent: true,
  alphaMap: particleTexture,
});

const count = 5000;
const positions = new Float32Array(count * 3);
for (let i = 0; i < count * 3; i++) {
  positions[i] = (Math.random() - 0.5) * 10;
}
particlesGeometry.setAttribute("position", new THREE.BufferAttribute(positions, 3));

// Points
const particles = new THREE.Points(particlesGeometry, particlesMaterial);

scene.add(particles);
```
What is really cool about this is how you can use any texture you want on the particles. So just get any image and put them on! The problem is that with particles, there comes the edges. You should use alphaMap instead to get rid of them. THis however still doesn't work too well. That is because particles are drawn in t he same order as they are created and webGL dosen't know which one is in front. There are multiple ways to fix this: 

1. `alphaTest: 0.001`. By default, it is rendering `0` which is black. We don't want it to render. So we put 0.001;
2. `depthTest: false`. This will draw everything even if it is in front of not. THIS WILL `CREATE BUGS`. 
3. `depthWrite: false` Store the greyscale texture of what has been drawn. It will check if it is in front or not. 

`blending: THREE.AdditiveBlending` Add to the color of the one before. You get the feeling of glowing! **can impact performances**
![image](https://user-images.githubusercontent.com/75579372/123181861-09f6d200-d443-11eb-9c5a-63780ed85d2f.png)

## To color the particles
```
const colors = new Float32Array(count * 3);
const positions = new Float32Array(count * 3);
for (let i = 0; i < count * 3; i++) {
  positions[i] = (Math.random() - 0.5) * 10;
  colors[i] = Math.random();
}
particlesGeometry.setAttribute("color", new THREE.BufferAttribute(colors, 3));
particlesMaterial.vertexColors = true;
```
Watch out for any `color`. It will blend. 

## Animating Particles
Because the point inherits from object3D, you can simply animate like any other objects. 

```
for (let i = 0; i < count: i++) {
	const i3 = i * 3;
	i3 === x
	i3[1] === 2
}
```
WHEN A GEOMETRY ATTRIBUTE CHANGE, YOU HAVE TO WRITE `NEEDSUPDATE: TRUE` to do update. 



``` onFinishedChange(generateGalaxy)``` will only run when the dat.gui is stopped. So when you move back and forth, it will work. The problem with this is that it is generating new galaxies but you have to remove them. 

# Disposing
If you want to dispose the previous points or geometries, you should do: 
```
let points = null;
let particleGeometry = null;
let particleMaterials = null;
function generateGalaxy() {
  if (points !== null) {
    particleMaterials.dispose();
    particleGeometry.dispose();
    scene.remove(points);
  }
  ...
}
```


## Mixing Colors
The `lerp(otherColor, how much to lerp (ex. 0.5, perfect mix))` that takes the base colors and interpolate the values with another value to mix them. 
* But watch out! You can't just mix color like `color1.lerp()`. This will overwrite `color1`! Make sure to clone the colors

```
const mixedColor = colorInside.clone();
mixedColor.lerp(colorOutside, radius / particleObject.radius);
```

Galaxy Generator
```
  /**
   * Particles
   */
  particleGeometry = new THREE.BufferGeometry();
  const positions = new Float32Array(particleObject.count * 3);
  const colors = new Float32Array(particleObject.count * 3);

  const colorInside = new THREE.Color(particleObject.insideColor);
  const colorOutside = new THREE.Color(particleObject.outsideColor);

  for (let i = 0; i < particleObject.count; i++) {
    const i3 = i * 3;

    const branchAngle = ((i % particleObject.branch) / particleObject.branch) * Math.PI * 2;
    const radius = Math.random() * particleObject.radius;

    const mixedColor = colorInside.clone();
    mixedColor.lerp(colorOutside, radius / particleObject.radius);

    const spinAngle = radius * particleObject.spin;
    const randomX =
      Math.pow(Math.random(), particleObject.randomnessPower) * (Math.random() < 0.5 ? 1 : -1);
    const randomY =
      Math.pow(Math.random(), particleObject.randomnessPower) * (Math.random() < 0.5 ? 1 : -1);
    const randomZ =
      Math.pow(Math.random(), particleObject.randomnessPower) * (Math.random() < 0.5 ? 1 : -1);

    positions[i3 + 0] = Math.cos(branchAngle + spinAngle) * radius + randomX;
    positions[i3 + 1] = randomY;
    positions[i3 + 2] = Math.sin(branchAngle + spinAngle) * radius + randomZ;

    colors[i3] = mixedColor.r;
    colors[i3 + 1] = mixedColor.g;
    colors[i3 + 2] = mixedColor.b;
  }

  particleGeometry.setAttribute("position", new THREE.BufferAttribute(positions, 3));
  particleGeometry.setAttribute("color", new THREE.BufferAttribute(colors, 3));

  /**
   * Materials
   */
  particleMaterials = new THREE.PointsMaterial({
    size: particleObject.size,
    blending: THREE.AdditiveBlending,
    // sizeAttenuation: true,
    // alphaMap: texture,
    vertexColors: true,
    depthWrite: false,
    // color: particleObject.color,
    // transparent: true,
  });

  points = new THREE.Points(particleGeometry, particleMaterials);

  scene.add(points);
}
generateGalaxy();

/**
 * Dat.GUI
 */
const gui = new dat.GUI();
gui.add(particleObject, "count").min(100).max(1000000).step(100).onFinishChange(generateGalaxy);
gui.add(particleObject, "size").min(0.01).max(1).step(0.01).onFinishChange(generateGalaxy);
gui.add(particleObject, "radius").min(0.01).max(20).step(0.01).onFinishChange(generateGalaxy);
gui.add(particleObject, "branch").min(1).max(10).step(1).onFinishChange(generateGalaxy);
gui.add(particleObject, "spin").min(-5).max(5).step(0.001).onFinishChange(generateGalaxy);
gui.add(particleObject, "randomness").min(0).max(2).step(0.001).onFinishChange(generateGalaxy);
gui
  .add(particleObject, "randomnessPower")
  .min(1)
  .max(10)
  .step(0.001)
  .onFinishChange(generateGalaxy);
gui
  .add(particleObject, "angle")
  .min(0)
  .max(Math.PI * 2)
  .step(Math.PI / 2)
  .onFinishChange(generateGalaxy);
gui.addColor(particleObject, "insideColor").onFinishChange(generateGalaxy);
gui.addColor(particleObject, "outsideColor").onFinishChange(generateGalaxy);
```



How does `const branchAngle = ((i % particleObject.branch) / particleObject.branch) * Math.PI * 2;` work?
* essentially, what you really are doing here is creating a predefined sets of value the angle can be. If branch is `3`, then the only optiosn `i % branch` will get you is `1/3` `0/3` `2/3`. And when you multiply them against `2pi` you will get the corresponding angle. it's basically saying, get me `angle of 120 out of the full circle`. 


How does `const spinAngle = radius * particleObject.spin;` work?
* Remember that the `radius` is actually `Math.random() * radius`. That means if the radius was 20, the value can be anywhere from `0-20`. That radius only defines the highest max value. It is also the radius that determine how far away your point is. `Math.cos(value)` and `Math.sin(value)` will never have a value bigger than `1` or `-1`. The radius vlaue multiplies it to create the distance. That means, if the `radius` is small, the point will be much closer to the origin. If then you do `radius * spin`, it is a very miniscule spin. Something like `2 * 3 = 6`. While, if the radius was `20`. You get `20 * 3 = 60`. See how much spinAngle has increased? This is in proportion to the radius so that's how you get the spin. 
* Secondly, you do `Math.cos(branchAngle + spinAngle)` to offset the current position by that much. That's how it works. 


# RayCaster
* Cast a ray in a specific direction and test what objects intersects with it. 
* Can find out if there is a wall in front of the player. If too close, you can prevent player from moving forward
* Can test if the laser gun hits the enemy
* Test if the cursor is touching anything
* Shoots a line to see if it collides with anything and then provide useful information.
* Can show alert message if the spaceship is heading towards a planet. 


![image](https://user-images.githubusercontent.com/75579372/123692889-337d7800-d80c-11eb-84a9-1fc9fa1c7ef7.png)

# Using Raycaster
* If you want to shoot a ray in a direction, you need to specify the `origin` and `direction` of the array.
* Direction has to be normalized. It's when your vector3 length is `1`. Always normalize

```
const raycaster = new THREE.Raycaster();
const rayOrigin = new THREE.Vector3(-4, 0, 0);
const rayDirection = new THREE.Vector3(10, 0, 0);
rayDirection.normalize(); // reduce the length to 1 but keep the direction.
raycaster.set(rayOrigin, rayDirection);
```

```
const intersect = raycast.intersectObject(object1); // returns a array because one intersect object can be gone through several times. 
const intersect = raycast.intersectObjects([object1, object2, object3]);
```

![image](https://user-images.githubusercontent.com/75579372/123707837-36ce2f00-d81f-11eb-89ae-5b5d4fa64784.png)

`distance` - distance between teh origin of the ray and collision point
`face` what face of the geometry was hit by the ray
`faceIndex` the index of that face
`object` useful. Know what object has been intersect. 
`point` position of the exact collision position
`uv` uv coord of the geometry. 

If your sphere are moving or spinning, you have to test on each other. Can become very heavy. 

### Animate
```
  for (const object of objectsToTest) {
    object.material.color.set("red");
  }
  for (const intersect of intersects) {
    intersect.object.material.color.set("blue");
  }
 ```
### Orienting for hovermode
```
window.addEventListener("mousemove", (e) => {
  mouse.x = (e.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -((e.clientY / window.innerHeight) * 2 - 1);
});

raycast.setFromCamera(mouse, camera);
const objectsToTest = [object1, object2, object3];
const intersects = raycast.intersectObjects(objectsToTest);
```

### mouse enter and mouse out
* Create a witness variable that contain the currently hovered object. 
* If object intersects but there wasn't one before, `mouseenter` occured
* If object intersects but there is a previuos one, `mouseout` occured

```
 if (intersects.length) {
    if (!currentIntersect) {
      console.log("mouseenter");
      currentIntersect = true;
    }
  } else {
    if (currentIntersect) {
      console.log("mouse leave");
      currentIntersect = null;
    }
 ```
 
 ### Click
 work with the mouse enter and mouse out. 
 ```
 window.addEventListener("click", (e) => {
  if (currentIntersect) {
    console.log("clicked on sphere");
  }
});
```

### Finding clicked object
```
window.addEventListener("click", (e) => {
  if (currentIntersect) {
    if (currentIntersect.object === object1) {
      console.log("1");
    } else if (currentIntersect.object === object2) {
      console.log("2");
    } else if (currentIntersect.object === object3) {
      console.log("3");
    }
  }
});
```

# Physics
You can create your own phsyics with some mathematics and solutiosn like `raycaster`. Can use gravity. But if you want realistic physics, **it's better to use a library**. 

## Theory
* We will create a physics world
* And a THREEJS world

So everything first happens in the Physics world and then updates the THREEJS world. You take the coordinates from the Physics world and put it in the THREEJS. 

![image](https://user-images.githubusercontent.com/75579372/123717283-61c17e80-d831-11eb-83fb-2cd90373bff2.png)


## Library
If you can reduce your physics to 2D, then you can use a 2D library. If not, use a 3D library such as cannonjs. S
# Tips
## You Should always optimize 
  Instead of creating 1000 new geometries and materials again and again, you can reuse the geometry. The time to create will go from 231ms to 15ms. When you have the same material, reuse them!

## Start creating the tweaks!
If you do it only at the end, you will feel too overwhelmed to add it. If you do, you can open a lot of opportunity to exploring!


# CANNONJS PHYSICS
```
import CANNON from "cannon"
const world = new CANNON.World()
world.gravity.set(0, -9.82, 0) <-- value for earth
```

In THREEJS, you create `mesh` but in CANNONJS, you create `Body`. 
* Bodies are objects that will fall and collide with other bodies. But before you can create a `Body`, you have to create the shape. Just like how you have to create the `geometry` and `material` before creating the `mesh` in THREEJS.


#### Gaffer on games
> What is the spiral of death? It’s what happens when your physics simulation can’t keep up with the steps it’s asked to take. For example, if your simulation is told: “OK, please simulate X seconds worth of physics” and if it takes Y seconds of real time to do so where Y > X, then it doesn’t take Einstein to realize that over time your simulation falls behind. It’s called the spiral of death because being behind causes your update to simulate more steps to catch up, which causes you to fall further behind, which causes you to simulate more steps…

> So how do we avoid this? In order to ensure a stable update I recommend leaving some headroom. You really need to ensure that it takes significantly less than X seconds of real time to update X seconds worth of physics simulation. If you can do this then your physics engine can “catch up” from any temporary spike by simulating more frames. Alternatively you can clamp at a maximum # of steps per-frame and the simulation will appear to slow down under heavy load. Arguably this is better than spiraling to death, especially if the heavy load is just a temporary spike.

#### Stack overflow
>The reason he does it the more complicated way is to decouple the physics simulation frequency from the rendering frequency. This way, you can render at a variable frame rate (120 FPS, 60 FPS, etc) but still update the game logic and physics at a constant rate.

> One of the benefits of this is that the game logic will behave the same no matter the FPS (many old games get buggy if you run them on modern computers because their physics depends on the FPS). Another benefit is that you can use the integration logic to interpolate some extra frames between the physics simulation steps, which results in a smoother animation.


## A basic sphere 
```
const sphereShape = new CANNON.Sphere(0.5);
const sphereBody = new CANNON.Body({
  mass: 1, // the one with higher mass, will have stronger inertia
  position: new CANNON.Vec3(0, 3, 0), // position of the sphere
  shape: sphereShape, // the shape itself
});
world.addBody(sphereBody); <-- but we still need to update our CANNON.JS world and THREEJS
```

updating
```
world.step(
 a fixed time stamp,
 how much time passed since the last step,
 how much iteration the world can apply to catch up with a potential delay
)
```
becomes...
```
let oldElapseTime = 0;
const tick = () => {
  const elapsedTime = clock.getElapsedTime();
  const deltaTime = elapsedTime - oldElapseTime;
  oldElapseTime = elapsedTime;

world.step(1 / 60, deltaTime, 3);
};
```

Body shapes can have complex shapes by adding more together. 
```
planeBody.addShape(planeShape)
planeBody.addShape(planeShape) // multiple shapes.
```

## Add a simple plane
```
const planeShape = new CANNON.Plane();
const planeBody = new CANNON.Body();
planeBody.mass = 0; // mass will not make it fall.
planeBody.addShape(planeShape);
world.addBody(planeBody);
```
You will find something weird however. Remember, a plane by default is vertical! You need to rotate it with quaternion however. 

```
planeBody.quaternion.setFromAxisAngle(the axis to rotate around, angle);
planeBody.quaternion.setFromAxisAngle(new CANNON.Vec3(-1, 0, 0), Math.PI / 2);
```

## One default material
```
// Materials
// There are just references. Then you associate those with the different bodies. This is just a material but from there we need a contact material
const defaultMaterial = new CANNON.Material("default");

// contact material
// What happens when a concrete material meets a plastic material. Thsi is where you provide: friction (how much does it rub), restitution (how much does it bounce). default for both is 0.3;

// We are saying here that if the defaultMaterial comes in contact with the default material, do this.
const defaultContactMaterial = new CANNON.ContactMaterial(defaultMaterial, defaultMaterial, {
  friction: 0.1,
  restitution: 0.8,
});
```

OR
```
world.defaultContactMaterial = defaultContactMaterial;
```

## ADD FORCE
* ApplyForce - `apply force from a specified point in space. Doesn't have to be on the body surface. Can be like a wind. 
*


