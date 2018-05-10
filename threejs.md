
in three.js

when you use the stl loader

that it will load stl file and add it to scene

depends on stl file type,

case bin stl file:

it will turns into bufferGeometry

case ascii stl file:

it will turns into geometry

everything works fine

_BUT_

if you get a bin stl file
and you want to add it as a geometry

yep 

you can use three.js native tool

 ```let newgeometry = new THREE.Geometry().fromBufferGeometry(oldbuffergeometry)```
 
sad things happen when you add newgeometry to scene

it will raise error for can't find x and on and on and on
```
Uncaught TypeError: Cannot read property 'x' of undefined
    at setMeshBuffers (three.js:19733)
    at updateObject (three.js:21675)
    at projectObject (three.js:21163)
    at projectObject (three.js:21200)
    at THREE.WebGLRenderer.render (three.js:21035)
```
the soltuion is:

jsut void the faceVertexUvs[0] in newgeometry

```
newgeometry.faceVertexUvs[0] = []
```
and it works
