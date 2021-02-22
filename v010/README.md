# Documentation for Renderspice V010
***
## Setup
Renderspice is a 2d Game Engine built for the HTML canvas, and to have a canvas that fills the screen, we need to setup our code. We will first need to create a new HTML project, in this we will need to create 3 files, `index.html`, `style.css`, and `script.js`. This is the code that needs to go into each

***
##### index.html
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>Renderspice Game</title>
    <link href="style.css" rel="stylesheet" type="text/css" />
  </head>
  <body>
    <!-- Canvas that Renderspice will use -->
    <canvas id="canvas"></canvas>

    <!-- Renderspice code -->
    <script src="https://renderspice.repl.co/2d/v010/v010.js"></script>
    
    <!-- Code we will write -->
    <script src="script.js"></script>
  </body>
</html>
```
***
##### style.css
```css
/* Remove borders and scroll bars */
body {
    margin: 0;
    overflow: hidden;
}

/* Fill entire screen */
#canvas {
    width: 100vw;
    height: 100vh;
}
```
***
##### script.js
```js
// Create Renderspice object on element with id canvas
const rs = new Renderspice('canvas');
```
# Simple Objects
In Renderspice there are preset objects which can be stretched, rotated, and translated to create almost any shape.
***
##### rs.box(x, y, w, h)
```js
// Creates box with center (x, y)
let a = rs.box(1, 4, 2, 4);
```
##### rs.triangle(x, y, w, h)
```js
// Creates isoceles triangle with center (x, y)
let b = rs.triangle(5, -2, 3, 3);
```
##### rs.circle(x, y, r, t)
```js
// Creates circle with r radius and t tesselations
let c = rs.circle(-3, 0, 1.5, 20);
```
# Complex Objects
In some cases, preset objects are not the best method to create a polygon. For these scenarios, it would be easiest to create a polygon by mapping all of the points in that polygon.
### rs.polygon(x, y, points)
```js
// Creates polygon at (x, y) with specific points
let z = rs.polygon(0, -4, [
    { x: -1, y: 1 },
    { x: 1, y: 1 },
    { x: 2, y: 0 },
    { x: 1, y: -1 },
    { x: -1, y: -1 }
]);
```
# Camera
The Renderspice camera is a simple but useful tool that captures the scene. It has an x, y, width, height, and background.
```js
rs.camera.x = 0;
rs.camera.y = 0;
rs.camera.w = 50;
rs.camera.h = 50 / rs.aspectRatio;
rs.camera.background = "skyblue";
```
# Rendering the Screen
To render the screen a single time, all that needs to be called is `rs.render()`, but that only renders a single frame. To loop the rendering process, you can call `rs.renderLoop()`. There are also 2 other settings that could be applied.
```js
rs.render(); // Renders single frame
rs.renderLoop(); // Renders frame at around 60 fps
```
***
### Preprocessing
In some games and applications, code might need to be ran right before any calculations take place, such as taking user input or running game logic. This would require a function to be called right before processing. To do this, set the variable `rs.preProcessing` to a function in your code.
```js
rs.preProcessing = () => {
    ('starting frame processing');
}
```
***
### Prerender
Just as some code might need to be ran before processing, there is some code that needs to be ran before a render and after processing, such as camera movement or collision resolutions. To call a function pre-render, set the variable `rs.preRender` to a function in your code.
***
```js
rs.preRender = () => {
    console.log('frame processed');
    console.log('started frame rendering');
}
```
# User Input
In almost any game or application, user input will need to be taken to move the player or change something in the scene. Renderspice makes it easy to take input from the user.
***
### Keyboard Events
To capture keyboard events, Renderspice has `rs.keys`. To detect whether a key is being pressed, we can take the Unicode value of the key.
```js
// If pressing the 'a' key
let aa = rs.keys[65].down; // true
```
Number keys do not correspond to their key value,
```js
// If pressing the '3' key 
let bb = rs.keys[51].down; // true
let cc = rs.keys[3].down; // false
``` 
We can also setup events for when a key is pressed or lifted,
```js
// When the space bar is pressed
rs.keys[32].onPress = () => {
    player.velocity.y += 10;
}

// When shift key is lifted
rs.keys[16].onPress = () => {
    player.gravity += 10;
}
```
***
### Mouse Events
Arguably the most important user input method, the mouse has a lot of input values that could be attained. Renderspice takes the main few, the position on the page, the position in Renderspice space, and whether it is being clicked.
```js
let alpha = rs.mouse.down; // if being clicked
let beta = rs.mouse.space.x; // x position in space
let gamma = rs.mouse.screen.y; // y position on screen
```
# Object Settings
Objects would be pretty boring by themselves, however there are many settings applicable to objects that can make a Renderspice scene faster, better looking, or easier to create.
***
### Object ID
Every Renderspice object has a unique ID that helps to decrease memory usage and prevent circular objects from appearing.
```js
let object = rs.box(10, -34, 4, 1);
let id = object.id;
let testObject = rs.getObject(id);

console.log(object === testObject); // true
```
***
### Basic Values
Renderspice objects have many basic values that have to do with the rendering process such as location, color, and other simple values.
##### object.x
Position of center of object on the x-axis.
```js
bottle.x = 3;
```
##### object.y
Position of center of object on the y-axis.
```js
watermelon.y = 2;
```
##### object.velocity
Speed of object in certain direction. Measured in units per second.
```js
ping.velocity.x = 5; // Velocity on the x-axis
pong.velocity.y = 10; // Velocity on the y-axis
```
##### object.gravity
Amount of velocity object loses on the y-axis. Measured in units per second per second.
```js
stalactite.gravity = 30; // Falls at a rate of 30 units/s/s
```
##### object.fill
Fill object with color.
```js
donut.fill = true;
```
##### object.fillColor
Color that object will be filled with, if `object.fill` is set to `true`.
```js
player.fillColor = "blue";
grass.fillColor = "#0F0";
water.fillColor = "#33AA88E0";
```
##### object.line
Outline object
```js
coin.line = true;
```
##### object.lineColor
Color object should be outlined with, granted `object.line` is `true`.
```js
cup.lineColor = "#050505";
```
##### object.lineWidth
Width of the object outline, as long as `object.line` is set to `true`;
```js
box.lineWidth = 5;
```
##### object.lifespan
Length of time that object should exist for. Measured in seconds.
```js
powerup.lifespan = 5;
```
##### object.timeAlive
Read only. Number of seconds that object has existed for.
```js
if(lava.timeAlive > 2) lava.fillColor = "#d60"
if(lava.timeAlive > 5) lava.fillColor = "#222";
```
##### object.rotation
Read only. Amount in radians that object has rotated since inception.
```js
chain.x = gear.rotation;
```
##### object.scaleValue
Read only. Value on axes that object has been scaled by since inception.
```js
icecube.x += water.scaleValue.x / 2;
icecube.y += cup.scaleValue.x / 3;
```
##### object.points
Read only. List of points object contains.
```js
if(hexagon.points.length != 6) console.log("what");
```
##### object.objectType
Read only. Type of object (ui or polygon).
```js
if(picture.objectType == 'ui') player.velocity.y = 0;
```
***
### Collisions
Renderspice has its own polygonal collision detection and resolution system that can be toggled and edited for different objects. `object.collisionBody` is the value in an object that contains all of the settings and functions for a collision body.

##### object.collisionBody.detect
Check for collisions that involve object. If set to false, Renderspice will not check whether object is colliding or not.
```js
object.collisionBody.detect = true;
```
##### object.collisionBody.resolve
Resolve collision involving object, by halting colliding velocity and moving object out of collision zone. If set to false, object may detect collisions, but will not change velocity or position.
```js
object.collisionBody.resolve = false;
```
##### object.collisionBody.include
List of object IDs to detect collisions with. If empty, include all objects.
```js
grenade.collisionBody.include = [player.id, ground.id];
```
##### object.collisionBody.exclude
List of object IDs to exclude from collision detection.
```js
plane.collisionBody.exclude = [cloud.id, spike.id];
```
##### object.collisionBody.colliding
Read only. Is object colliding?
```js
if(player.collisionBody.colliding) {
    allowJump = true;
}
```
##### object.collisionBody.colliders
Read only. List of object IDs colliding with object
```js
for(let id of object.collisionBody.colliders) {
    rs.getObject(id).fillColor = "blue";
}
```
##### object.collisionBody.method
Method of collision detection. If set to circle, radius used will be maximum possible collision distance. If set to polygon, collisions will be detected by intersecting lines. Defaults to polygon
```js
shuriken.collisionBody.method = "polygon";
bullet.collisionBody.method = "circle";
```
##### object.collisionBody.range
Read only. Maximum possible distance from object where a collision is possible, used to optimize rendering speed.
```js
console.log(frisbee.collisionBody.range);
```
##### object.collisionBody.onCollide
Function called on collision with object, passes collider as argument.
```js
crate.collisionBody.onCollide = (obj) => {
    if(obj.tag === "jumpPad") crate.velocity.y *= -0.8;
}
```
# Object Methods
Objects also have functions that can be called to alter the points and behavior of the object.
***
### Transformations
##### object.scale(x, y)
Scale object on x and y
```js
boat.scale(1, 2); // Scale Y by 2
cookie.scale(5); // Scale X and Y by 5
squid.scale(-1, 1); // Flip horizontally
```
##### object.setScale(x, y)
Set scale from original
```js
if(egg.scaleValue.y != 3) oval.setScale(2, 3);
```
##### object.rotate(theta)
Rotate theta radians counterclockwise
```js
flashlight.rotate(1); // Rotate 1 radian counterclockwise
marker.rotate(Math.PI*2); // Make full rotation
```
##### object.setRotation(theta)
Set rotation from original
```js
lantern.setRotation(0); // Reset rotation back to original
fence.setRotation(1); // Set rotation to 1 radian from original
```
***
### Miscellaneous
##### object.duplicate()
Create new object with the same values as object, excluding object ID.
```js
let a = house.duplicate();
console.log(a.x == house.x && a.y == house.y); // true
console.log(a.id == house.id); // false
```
##### object.destroy()
Delete object from scene
```js
bridge.destroy();
```
# Tags
Tags are an important part of any game engine, they allow for groups of objects that move together, allowing for less code and easier usage.

To create a tag, just call the `rs.tag('tagName')` function and give a tag name.
```js
let object1 = rs.box(0, 0, 1, 1);
let object2 = rs.triangle(1, 0, 1, 1);

let player = rs.tag('player');
player.add(object1, object2);
```
***
### Tag Settings
Tags have most of the same variables and methods as objects, and will apply those settings to all objects in the tag.
```js
/* Set and read */
tag.x = 3; // X Position
tag.y = 6; // Y Position

tag.velocity.x = 4; // Velocity on x-axis
tag.velocity.y = 2; // Velocity on y-axis

tag.lifespan = 5; // Number of seconds that object should exist for

/* Read only */
tag.objects; // List of objects in tag
tag.name; // Name of tag
tag.rotation; // Rotation from original
tag.timeAlive; // Number of seconds object has existed for
```
***
### Tag Methods
##### tag.add(...objects)
Add objects to tag
```js
let o1 = rs.box();
let o2 = rs.circle();
tag.add(o1, o2);
```
##### tag.remove(...objects)
Remove objects from tag
```js
console.log(tag.objects.length); // 5
tag.remove(o1, o2, o3);
console.log(tag.objects.length); // 2
```
##### tag.merge(...tags)
Merge other tags with tag
```js
console.log(tag.objects.length); // 5
tag.merge(tag1, tag2, tag3, tag4);
console.log(tag.objects.length); // 17
```
##### tag.duplicate()
Duplicate tag and objects
```js
let tag = rs.tag('enemies');
let newTag = tag.duplicate();
console.log(newTag.name); // enemiesCopy
```
##### tag.destroy()
Delete tag but keep objects
```js
tag.add(o1);
tag.destroy();
console.log(o1 == undefined); // false
```
***
# Text
Even with the best objects, any decent game engine will need to have a way for text to be displayed to show information to the user. For this, renderspice has `rs.text()`. This takes in 5 parameters: string, x, y, font, and size (px).
```js
let gameOverText = rs.text("Game Over", player.x, player.y + 5, "Segoe UI", 16);
```
Renderspice center aligns the text, meaning that the center of the string is exactly at the x and y. It also normalizes the font size against the canvas scale so that it matches what the user would normally see in a paragraph or header tag.
***
# Renderspice Variables
Even though Renderspice has a lot of content through rendering, input, objects, and more, there are some essentials that can increase your application's efficiency and lower time creating.
***
### Timing
##### rs.frame
How many frames have been rendered so far. This can be used for getting a certain timing or interval.
```js
// Every 20th frame this will be true
if(rs.frame % 20 == 0) {
    console.log('20 frames have passed, adding new enemy');
    makeEnemy();
}
```
##### rs.deltaTime
Amount of time passed since previous frame. Helps getting movements between frames consistent.
```js
let currentFPS = 1 / rs.deltaTime;
console.log(`Game running at ${currentFPS}fps`);

player.y += object.velocity.y * rs.deltaTime;
```
##### rs.totalTime
Length of time passed since page load.
```js
console.log(`${rs.totalTime} seconds passed since start`);
```
##### rs.lastFrame
Unix timestamp in milliseconds when previous frame was rendered.
```js
console.log(`Last frame rendered at ${rs.lastFrame}`);
```
### Other
##### rs.aspectRatio
Ratio between width and height of window size. Can be used for normalizing scaling.
```js
rs.camera.h = rs.camera.w / rs.aspectRatio;
```
##### rs.canvas
HTML element that Renderspice draws onto.
```js
rs.canvas.innerHTML = "lol";
```
##### rs.ctx
Canvas context that Renderspice uses to draw onto the canvas.
```js
rs.ctx.fillRect(0, 0, 4, 4);
```
