# Creating 3D Characters in Three.js - Codrops
![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/10/Threejs_tutorial.jpg)

From our sponsor: [Ship fast and never break a thing with Shortcut (formerly Clubhouse.io).](https://srv.buysellads.com/ads/long/x/TH4TNQS3TTTTTT6TFWEN4TTTTTTTR3JTK6TTTTTTE4R45YVTTTTTTESLPVSNKSJ7537HLRSWP36FP2QYVQCI4WZ727CT)

[Three.js](https://threejs.org/) is a JavaScript library for drawing in 3D with WebGL. It enables us to add 3D objects to a scene, and manipulate things like position and lighting. If you’re a developer used to working with the DOM and styling elements with CSS, Three.js and WebGL can seem like a whole new world, and perhaps a little intimidating! This article is for developers who are comfortable with JavaScript but relatively new to Three.js. Our goal is to walk through building something simple but effective with Three.js — a 3D animated figure — to get a handle on the basic principles, and demonstrate that a little knowledge can take you a long way!

## Setting the scene

In web development we’re accustomed to styling DOM elements, which we can inspect and debug in our browser developer tools. In WebGL, everything is rendered in a single `<canvas>` element. Much like a video, everything is simply pixels changing color, so there’s nothing to inspect. If you inspected a webpage rendered entirely with WebGL, all you would see is a `<canvas>` element. We can use libraries like Three.js to draw on the canvas with JavaScript.

### Basic principles

First we’re going to set up the scene. If you’re already comfortable with this you can skip over this part and jump straight to the section where we start creating our 3D character.

We can think of our Three.js scene as a 3D space in which we can place a camera, and an object for it to look at.

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/09/threejs-01.png)

We can picture our scene as a giant cube, with objects placed at the center. In actual fact, it extends infinitely, but there is a limit to how much we can see.

First of all we need to create the scene. In our HTML we just need a `<canvas>` element:

Now we can create the scene with a camera, and render it on our canvas in Three.js:

For brevity, we won’t go into the precise details of everything we’re doing here. The documentation has much more detail about [creating a scene](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene) and the various camera attributes. However, the first thing we’ll do is move the position of our camera. By default, anything we add to the scene is going to be placed at co-ordinates (0, 0, 0) — that is, if we imagine the scene itself as a cube, our camera will be placed right in the center. Let’s place our camera a little further out, so that our camera can look at any objects placed in the center of the scene.

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/09/threejs-02a.png)

Moving the camera away from the center allows us to see the objects placed in the center of the scene.

We can do this by setting the _z_ position of the camera:

We won’t see anything yet, as we haven’t added any objects to the scene. Let’s add a cube to the scene, which will form the basis of our figure.

## 3D shapes

Objects in Three.js are known as _meshes_. In order to create a mesh, we need two things: a _geometry_ and a _material_. Geometries are 3D shapes. Three.js has a selection of geometries to choose from, which can be manipulated in different ways. For the purpose of this tutorial — to see what interesting scenes we can make with just some basic principles — we’re going to limit ourselves to only two geometries: cubes and spheres.

Let’s add a cube to our scene. First we’ll define the geometry and material. Using Three.js [BoxGeometry](https://threejs.org/docs/index.html?q=box#api/en/geometries/BoxGeometry), we pass in parameters for the x, y and z dimensions.

For the material we’ll choose [MeshLambertMaterial](https://threejs.org/docs/index.html#api/en/materials/MeshLambertMaterial), which reacts to light and shade but is more performant than some other materials.

Then we create the mesh by combining the geometry and material, and add it to the scene:

Unfortunately we still won’t see anything! That’s because the material we’re using depends on light in order to be seen. Let’s add a [directional light](https://threejs.org/docs/index.html#api/en/lights/DirectionalLight), which will shine down from above. We’ll pass in two arguments: `0xffffff` for the color (white), and the intensity, which we’ll set to 1.

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/09/threejs-03.png)

By default, the light points down from above

If you’ve followed all the steps so far, you still won’t see anything! That’s because the light is pointing directly down at our cube, so the front face is in shadow. If we move the z position of the light towards the camera and off-center, we should now see our cube.

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/09/threejs-04.png)

Moving the light gives us a better view

We can alternatively set the position on the x, y and z axis simultaneously by calling `set()`:

We’re looking at our cube straight on, so only one face can be seen. If we give it a little bit of rotation, we can see the other faces. To rotate an object, we need to give it a rotation angle in \[radians](). I don’t know about you, but I don’t find radians very easy to visualize, so I prefer to use a JS function to convert from degrees:

We can also add some ambient light (light that comes from all directions) with a color tint, which softens the effect slightly end ensures the face of the cube turned away from the light isn’t completely hidden in shadow:

Now that we have our basic scene set up, we can start to create our 3D character. To help you get started I’ve created a [boilerplate](https://codepen.io/michellebarker/pen/gORxyMB) which includes all the set-up work we’ve just been through, so that you can jump straight to the next part if you wish.

## Creating a class

The first thing we’ll do is create a class for our figure. This will make it easy to add any number of figures to our scene by instantiating the class. We’ll give it some default parameters, which we’ll use later on to position our character in the scene.

### Groups

In our class constructor, let’s create a Three.js group and add it to our scene. Creating a group allows us to manipulate several geometries as one. We’re going to add the different elements of our figure (head, body, arms, etc.) to this group. Then we can position, scale or rotate the figure anywhere in our scene without having to concern ourselves with individually positioning those parts individually every time.

## Creating the body parts

Next let’s write a function to render the body of our figure. It’ll be much the same as the way we created a cube earlier, except, we’ll make it a little taller by increasing the size on the _y_ axis. (While we’re at it, we can remove the lines of code where we created the cube earlier, to start with a clear scene.) We already have the material defined in our codebase, and don’t need to define it within the class itself.

Instead of adding the body to the scene, we instead add it to the group we created.

We’ll also write a class method to initialize the figure. So far it will call only the `createBody()` method, but we’ll add others shortly. (This and all subsequent methods will be written inside our class declaration, unless otherwise specified.)

## Adding the figure to the scene

At this point we’ll want to render our figure in our scene, to check that everything’s working. We can do that by instantiating the class.

Next we’ll write a similar method to create the head of our character. We’ll make this a cube, slightly larger than the width of the body. We’ll also need to adjust the position so it’s just above the body, and call the function in our `init()` method:

You should now see a narrower cuboid (the body) rendered below the first cube (the head).

## Adding the arms

Now we’re going to give our character some arms. Here’s where things get slightly more complex. We’ll add another method to our class called `createArms()`. Again, we’ll define a geometry and a mesh. The arms will be long, thin cuboids, so we’ll pass in our desired dimensions for these.

As we need two arms, we’ll create them in a `for` loop.

We don’t need to create the geometry in the for loop, as it will be the same for each arm.

Don’t forget to call the function in our `init()` method:

We’ll also need to position each arm either side of the body. I find it helpful here to create a variable `m` (for multiplier). This helps us position the left arm in the opposite direction on the _x_ axis, with minimal code. (We’ll also use it rotate the arms in a moment too.)

Additionally, we can rotate the arms in our `for` loop, so they stick out at a more natural angle (as natural as a cube person can be!):

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/09/threejs-06.png)

If our figure is placed in the center, the arm on the left will be positioned at the negative equivalent of the x-axis position of the arm on the right

### Pivoting

When we rotate the arms you might notice that they rotate from a point of origin in the center. It can be hard to see with a static demo, but try moving the slider in this example.

We can see that the arms don’t move naturally, at an angle from the shoulder, but instead the entire arm rotates from the center. In CSS we would simply set the `transform-origin`. Three.js doesn’t have this option, so we need to do things slightly differently.

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/09/threejs-05.png)

The figure on the right has arms that rotate from the top, for a more natural effect

Our steps are as follows for each arm:

1.  Create a new Three.js group.
2.  Position the group at the “shoulder” of our figure (or the point from which we want to rotate).
3.  Create a new mesh for the arm and position it relative to the group.
4.  Rotate the group (instead of the arm).

Let’s update our `createArms()` function to follow these steps. First we’ll create the group for each arm, add the arm mesh to the group, and position the group roughly where we want it:

To assist us with visualizing this, we can add one of Three.js’s built-in helpers to our figure. This creates a wireframe showing the bounding box of an object. It’s useful to help us position the arm, and once we’re done we can remove it.

To set the transform origin to the top of the arm rather than the center, we then need to move the arm (within the group) downwards by half of its height. Let’s create a variable for height, which we can use when creating the geometry:

Then we can rotate the arm group.

In this demo, we can see that the arms are (correctly) being rotated from the top, for a more realistic effect. (The yellow is the bounding box.)

### Eyes

Next we’re going to give our figure some eyes, for which we’ll use the [Sphere geometry](https://threejs.org/docs/index.html?q=sphere#api/en/geometries/SphereGeometry) in Three.js. We’ll need to pass in three parameters: the radius of the sphere, and the number of segments for the width and height respectively (defaults shown here).

As our eyes are going to be quite small, we can probably get away with fewer segments, which is better for performance (fewer calculations needed).

Let’s create a new group for the eyes. This is optional, but it helps keep things neat. If we need to reposition the eyes later on, we only need to reposition the group, rather than both eyes individually. Once again, let’s create the eyes in a `for` loop and add them to the group. As we want the eyes to be a different color from the body, we can define a new material:

We _could_ add the eye group directly to the figure. However, if we decide we want to move the head later on, it would be better if the eyes moved with it, rather than being positioned entirely independently! For that, we need to modify our `createHead()` method to create another group, comprising both the main cube of the head, and the eyes:

In the `createEyes()` method we then need to add the eye group to the head group, and position them to our liking. We’ll need to position them forwards on the _z_ axis, so they’re not hidden inside the cube of the head:

### Legs

Lastly, let’s give our figure some legs. We can create these in much the same way as the eyes. As they should be positioned relative to the body, we can create a new group for the body in the same way that we did with the head, then add the legs to it:

## Positioning in the scene

If we go back to our constructor, we can position our figure group according to the parameters:

Now, passing in different parameters enables us to position it accordingly. For example, we can give it a bit of rotation, and adjust its x and y position:

Alternatively, if we want to center the figure within the scene, we can use the Three.js `Box3` function, which computes the bounding box of the figure group. This line will center the figure horizontally and vertically:

## Making it generative

At the moment our figure is all one color, which doesn’t look particularly interesting. We can add a bit more color, and take the extra step of making it generative, so we get a new color combination every time we refresh the page! To do this we’re going to use a function to randomly generate a number between a minimum and a maximum. This is one I’ve borrowed from [George Francis](https://georgefrancis.dev/), which allows us to specify whether we want an integer or a floating point value (default is an integer).

Let’s define some variables for the head and body in our class constructor. Using the `random()` function, we’ll generate a value for each one between 0 and 360:

I like to use [HSL](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/hsl()) when manipulating colors, as it gives us a fine degree of control over the hue, saturation and lightness. We’re going to define the material for the head and body, generating different colors for each by using template literals to pass the random hue values to the `hsl` color function. Here I’m adjusting the saturation and lightness values, so the body will be a vibrant color (high saturation) while the head will be more muted:

Our generated hues range from 0 to 360, a full cycle of the color wheel. If we want to narrow the range (for a limited color palette), we could select a lower range between the minimum and maximum. For example, a range between 0 and 60 would select hues in the red, orange and yellow end of the spectrum, excluding greens, blues and purples.

We could similarly generate values for the lightness and saturation if we choose to.

Now we just need to replace any reference to `material` with `this.headMaterial` or `this.bodyMaterial` to apply our generative colors. I’ve chosen to use the head hue for the head, arms and legs.

We could use generative parameters for much more than just the colors. In this demo I’m generating random values for the size of the head and body, the length of the arms and legs, and the size and position of the eyes.

## Animation

Part of the fun of working with 3D is having our objects move in a three-dimensional space and behave like objects in the real world. We can add a bit of animation to our 3D figure using the [Greensock](https://greensock.com/) animation library (GSAP).

GSAP is more commonly used to animate elements in the DOM. As we’re not animating DOM elements in this case, it requires a different approach. GSAP doesn’t require an element to animate — it can animate JavaScript objects. As one post in the GSAP forum puts it, GSAP is just “changing numbers really fast”.

We’ll let GSAP do the work of changing the parameters of our figure, then re-render our figure on each frame. To do this, we can use GSAP’s `ticker` method, which uses `requestAnimationFrame`. First, let’s animate the `ry` value (our figure’s rotation on the y axis). We’ll set it to repeat infinitely, and the duration to 20 seconds:

We won’t see any change just yet, as we aren’t re-rendering our scene. Let’s now trigger a re-render on every frame:

Now we should see the figure rotating on its y axis in the center of the scene. Let’s give him a little bounce action too, by moving him up and down and rotating the arms. First of all we’ll set his starting position on the y axis to be a little further down, so he’s not bouncing off screen. We’ll set `yoyo: true` on our tween, so that the animation repeats in reverse (so our figure will bounce up and down):

As we need to update a few things, let’s create a method called `bounce()` on our `Figure` class, which will handle the animation. We can use it to update the values for the rotation and position, then call it within our ticker, to keep things neat:

To make the arms move, we need to do a _little_ more work. In our class constructor, let’s define a variable for the arms, which will be an empty array:

In our `createArms()` method, in addition to our code, we’ll push each arm group to the array:

Now we can add the arm rotation to our `bounce()` method, ensuring we rotate them in opposite directions:

Now we should see our little figure bouncing, as if on a trampoline!

## Wrapping up

There’s much, much more to Three.js, but we’ve seen that it doesn’t take too much to get started building something fun with just the basic building blocks, and sometimes limitation breeds creativity! If you’re interested in exploring further, I recommend the following resources.

### Resources

-   [Three.js Journey](https://threejs-journey.xyz/) video course by Bruno Simon (includes transcripts and starter projects every lesson)
-   [Three.js Fundamentals](https://threejsfundamentals.org/threejs/lessons/threejs-fundamentals.html)
-   [Three.js documentation](https://threejs.org/)
-   [A Generative SVG Starter Kit](https://georgefrancis.dev/writing/a-generative-svg-starter-kit/) by George Francis — not Three.js-related, but covers some of the principles of generative art, and some handy utility functions
-   [Greensock documentation](https://greensock.com/) 
    [https://tympanus.net/codrops/2021/10/04/creating-3d-characters-in-three-js/](https://tympanus.net/codrops/2021/10/04/creating-3d-characters-in-three-js/) 
    [https://tympanus.net/codrops/2021/10/04/creating-3d-characters-in-three-js/](https://tympanus.net/codrops/2021/10/04/creating-3d-characters-in-three-js/)
