# BLENDER

# Areas
The parts of the screen. 

### 3D Viewport
![image](https://user-images.githubusercontent.com/75579372/123138111-8967af80-d409-11eb-9bb4-04d9fe0d2efb.png)

### Timeline
![image](https://user-images.githubusercontent.com/75579372/123138226-a308f700-d409-11eb-8c91-9ed4c330d5c2.png)

### Outliner (the layers)
![image](https://user-images.githubusercontent.com/75579372/123138252-a8664180-d409-11eb-863d-13e22091a18d.png)

### Properties
![image](https://user-images.githubusercontent.com/75579372/123138282-b320d680-d409-11eb-90fe-9ac8ca5e28d3.png)

## To Split
* Go to the corner and drag. 
* To remove a split, drag to the nearby screen and ti will combine

# Tons of shortcut
* Can be extremely useful to master
* Always an alternative through the interface.
* If you want to be productive, you have to learn them.
* Shortcuts are *area* sensitive. If your `mouse` is in the 3D viewport, it will do shortcuts only for the 3D viewport. *Make sure they are in the right area* 
* Mode sensitive

## Truck (up and down left and right) By holding `shift` + `wheel` 
![image](https://user-images.githubusercontent.com/75579372/123140201-de0c2a00-d40b-11eb-8608-de800abe779f.png)

#### Forward and backward without viewport limit`ctrl` + `shift`+ `wheel`
#### Dolly (forward and backward) rotate wheel
#### Fly Mode : `shift + f` 
#### orthographic camera: `numpad 5`
#### `numpad 1` -y (press ctrl to go opposite direction)
#### `numpad 3` x (press ctrl to go opposite direction)
#### `numpad 7` z (press ctrl to go opposite direction)
#### `numpad 0` to get camera view
#### `shift + c` to go back to home
#### `numpad .` to view a specific object
#### `numpad + /` to hide all other objects and only see the specific one
#### To unselect, you have to do it only on the active one.
#### `a` will select a
#### `aa` unselect everything
#### `shift + b` to zoom in on the selected
#### `b` to select using rectangle
#### `c` to select by circle. Press `shift + c` to unselect. rotate wheel for bigger or smaller circle. 
#### `f9` for changing the mesh.
#### `x` to delete
#### `h` to hide a object
#### `alt + h` to show everything that has been hidden
#### `shift + h` to hide all objects that you didn't select.
#### `t` to hide or unhide menu
#### `g` to translate on cursor. right click to cancel. left click to place.
#### `r` to rotate
#### `s` to scale. 
#### Press (x, y, z) during the translate, rotate, or scale mode, to limit it to only one side
#### `Shift z` to exclude the specific side
The lighter color is the active one. 
#### `Tab` to switch from object to edit
#### `ctrl + tab` 
![image](https://user-images.githubusercontent.com/75579372/123148120-b4a3cc00-d414-11eb-9ef7-3ac746677ef7.png)
#### `f12` to render camera
#### `f3` to search
#### `z` to open shading mode
![image](https://user-images.githubusercontent.com/75579372/123148909-925e7e00-d415-11eb-8451-876be1f578c6.png)




# mode
Default mode is `object mode`. You play with object. To change mode, press  `ctrl + tab`

## Edit mode
Similar to object mode but allow you to do transformation on the **vertices, the edges, and faces**
`1`, `2`, `3` => vertices, sides, and face. Can then use `g, r , s` on them.

## Shading Mode
Shading is how you see objects in the `3D viewport`. The default if `solid` and we can see default material. 

If everything is dark, that means you need to add a light.


# Properties
Show render properties, environmetnal properties, and active object properties. 

![image](https://user-images.githubusercontent.com/75579372/123149504-321c0c00-d416-11eb-8437-a75831921cb7.png)

# Render engine
## Eevee
* Use GPU 
* Very performant
* Limitations like realism, light bounces, reflection, and fraction

## Workbench
* Legacy render engine
* Not used a lot
* performant
* not very realistic.

## Cycles
* A ray tracing engine
* Super realistic 
* handles light bounce
* but not very performant


## When joining
Make sure you have about the same modifier or else you can have problems

## What is raytracing?
When you use raytracing, blender will render one pixel and shoot rays in every direction and test what colors to collide. This is why you can see colors. 

![image](https://user-images.githubusercontent.com/75579372/123854335-70607200-d8d3-11eb-8e39-a2ad6684240e.png)

The idea of baking is to save a very beautiful render and place them as a geometry on the scene. So the texture will come with the shading and very beautiful with real time. No lights! No shading! that's really cool. Performant great. 
![image](https://user-images.githubusercontent.com/75579372/123854695-e4027f00-d8d3-11eb-98f1-09860806a4ce.png)

* Create the scene in 3D software
* Optimize all the objects
* UV unwrap everything
* bake the render in texture
* export scene and texture
* import everything in threejs and apply texture. 



# Donut Tutorial
`alt + z` - open x-ray mode which will allow you to select everything in highlight else you won't be able to select things behind it. 
`p` - press to convert the selected into a object. Else the things you created in edit mode still belongs to the one object. 
`ctrl + i` -invert selection
`ctrl + alt + numpad 0` - get camera snap to position
Is your object vertices going through another object? Worry no more! We got the ability to stick to the faces! ![image](https://user-images.githubusercontent.com/75579372/124002823-102d0700-d98b-11eb-84e8-799ff5f345fc.png)

## Pro tips
* always scale on edit mode so it won't effect the UV unwrapping. Always keep cube scale to 1. 
* Always have a reference image or model so you know how it will look

`blend1` is a backup file. 
