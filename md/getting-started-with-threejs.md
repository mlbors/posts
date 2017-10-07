## [Getting started with three.js](https://mlbors.tumblr.com/post/160767437630/getting-started-with-threejs)

In this small post, we are going to see how to simply use _WebGL_ with _three.js_.

_WebGL_ is a _JavaScript API_ for rendering 3D graphics within a compatible web browser. _WebGL_ is based on _OpenGL_ and, through _JavaScript_, uses _OpenGL Shading Language_ (GLSL) that is really similar to _C_ or _C++_. _three.js_ is library that lets you use _WebGL_ easily.

Let’s make a small project to learn this tool. First, we need to download [_three.js_](https://threejs.org/). When it is done, we two create files: _index.html _and _app.js_.

We start by filling the _index.html _file:
    
    
    <!DOCTYPE html>
    <html>
        <head>
    
            <title>ThreeJS - Basic Scene</title>
    
            <style type="text/css">
    
                body {
                    margin: 0;
                    overflow: hidden;
                }
    
                canvas {
                    background: red;
                }
    
            </style>
    
        </head>
    
        <body>
            <canvas id="canvas">
                Your browser does not support HTML 5.
            </canvas>
        </body>
    
        <script src="three.min.js" type="text/javascript"></script>
        <script src="app.js" type="text/javascript"></script>
    
    </html>

_index.html file_

Now let’s focus on the _app.js_ file. First, we tell _three.js_ where to display our scene:
    
    
    var renderer = new THREE._WebGL_Renderer({
        canvas: document.getElementById('canvas'),
        antialiasing: true
    });

Then we set a few things:
    
    
    renderer.setClearColor(0x000000);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

Here we set the background of our scene, the ratio of our device and we resize our output canvas.

We also need a camera:
    
    
    var camera = new THREE.PerspectiveCamera(35, window.innerWidth / window.innerHeight, 0.1, 3000);

Here we set the vertical field of view in degrees of our camera and its aspect ratio. The two lasts arguments tell _three.js_ where to start and where to stop rendering things.

We can now create our scene:
    
    
    var scene = new THREE.Scene();

Unfortunately, we won’t see anything without light! So, let’s create two lights:
    
    
    var ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    
    var pointLight = new THREE.PointLight(0xffffff, 0.5); 
    scene.add(pointLight);

For each light, we set the color and the intensity. We also add them to our scene. An ambient light globally illuminates all objects in the scene and a point light gets emitted from a single point in all directions.

It is time to add some mesh to our scene:
    
    
    var geometry = new THREE.BoxGeometry(100, 100, 100);
    var material = new THREE.MeshLambertMaterial({color: 0xF3FFE2});
    var mesh = new THREE.Mesh(geometry, material);
    mesh.position.set(0, 0, -1000);
    
    scene.add(mesh);

Here, we firstly create a box then some matte material. We then use these two objects to create our mesh. We also set the position of our mesh. We use a negative value for the z position of our mesh because, otherwise, due to the position of the camera (0, 0, 0), this last one would be at the top of our mesh and we wouldn’t see it.

To make our scene more interesting, we are going to create a small function to make our cube rotate:
    
    
    requestAnimationFrame(render);
    
    function render() {
        mesh.rotation.x += 0.05;
        mesh.rotation.y += 0.05;
        renderer.render(scene, camera);
        requestAnimationFrame(render);
    }

If everything is alright, we should see a sweet rotating cube when we open the _index.html _file in a web browser.
    
    
    var renderer = new THREE._WebGL_Renderer({
        canvas: document.getElementById('canvas'),
        antialiasing: true
    });
    
    renderer.setClearColor(0x000000);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    
    var camera = new THREE.PerspectiveCamera(35, window.innerWidth / window.innerHeight, 0.1, 3000);
    
    var scene = new THREE.Scene();
    
    var ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    
    var pointLight = new THREE.PointLight(0xffffff, 0.5); 
    scene.add(pointLight);
    
    var geometry = new THREE.BoxGeometry(100, 100, 100);
    var material = new THREE.MeshLambertMaterial({color: 0xF3FFE2});
    var mesh = new THREE.Mesh(geometry, material);
    mesh.position.set(0, 0, -1000);
    
    scene.add(mesh);
    
    requestAnimationFrame(render);
    
    function render() {
        mesh.rotation.x += 0.05;
        mesh.rotation.y += 0.05;
        renderer.render(scene, camera);
        requestAnimationFrame(render);
    }

_app.js file_
