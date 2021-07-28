# Drei
Drei is the useful helpers for the library `React-three-fiber`

## HTML 
* You can actually put the HTML tag inside the `<mesh>`! What that benefits you is that the origin point now resolves around the `<mesh>` point. In that way, where ever you move the mesh, the HTML tag will be relative to the position of the mesh. Imagine `position: relative` basically. Otherwise, if you put the HTMl inside of the `Canvas`, and you move the mesh, it wouldn't be relative to the mesh but to the canvas. In that way, moving things around becomes very difficult as you have to consider moving both the HTML and the mesh to stay in position. If you put the HTML inside, these problems go away. 

* `occlude` - Turns on detection when something is in front of it or blocking and if true, hide away or whatever custom styles you do. If not turn on, then the HTML tag will always be visible 
* `onOcclude` - this is the callback property that runs with the `occlude`. It will return `true` or `false` depending if there are blocks in front. Use this if you wish to have your customized styles. 
* `transform` - Will give it matrix, making it face basically in one direction instead of following you around. By default, it will point towards the z axis. you can rotate it around to look at different areas. 
* `position` - to change current position
* `rotation` - to rotate it around
* `distanceFactor` - See it as a scaling method for you. If you get closer, it would scale using the distanceFactor value you set. It will make things bigger and smaller depending on your liking. In `HTML`, this is a easier way to scale things proportionally than to individually add width and height or fontSize to each. 
* `fullscreen` - covers the entire screen and stop you from using the canvas or making action in it. Can be useful to display GUI and so on. Aligns itself to the left top corner. 


![image](https://user-images.githubusercontent.com/75579372/127249755-ec8e6c6a-3eeb-45d9-a157-48ae1909a328.png)

##
