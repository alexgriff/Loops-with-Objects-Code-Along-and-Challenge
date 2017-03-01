# Loops with Objects
## Code-Along: Parting the Sea of Bubbles

Let's start by doing something crazy like adding 300 bubbles to the canvas. To do this we will use two loops. In `setup` we'll create a bunch bubble objects and `push` them into the `bubbles` array. In `draw` we'll loop over that `bubbles` array and draw an `ellipse` for each bubble.

Paste in this code and then we'll go over it and clean it up:

```javascript
var bubbles = [];
var bubbleSize = 40;

function setup() {
  createCanvas(600,400);

  for (var i = 0; i < 300; i++) {
    bubbles.push({x: random(0, width), y: random(0, height)})
  }
}

function draw() {
  background(0);
  fill(255);
  for(var i = 0; i < bubbles.length; i++) {
    ellipse(bubbles[i].x, bubbles[i].y, bubbleSize, bubbleSize);

    bubbles[i].x += random(-2, 2);
    bubbles[i].y += random(-2, 2);
  }
}
```

You should see something like this:

![300 bubbles](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/300-bubbles.gif)

### The `for` loop in `setup`

```javascript
for (var i = 0; i < 300; i++) {
  bubbles.push({x: random(0, width), y: random(0, height)})
}
```

A few things to take note of here.

The `bubbles` array doesn't yet have anything in it, so notice that the second part of our `for` loop is `i < 300`. This means "count up to the number `300`". We can't say something like `bubbles.length` because right now `bubbles.length` is `0`!

What exactly is the *one* argument we pass to `push`? There's a lot of text in there, but in fact there is only one thing between the opening `(` and closing `)`.

It's an object!

Since this code is inside of a loop, it will happen many times. We could even declare a variable inside the loop and each time the loop happens the it's like the variable is declared all over again.

I think making that object we pass to `push` a variable will make the code a lot easier to follow.

```javascript
for (var i = 0; i < 300; i++) {
  var bubble = {
    x: random(0, width),
    y: random(0, height)
  }

  bubbles.push(bubble)
}
```

Great, we have an array, or collection, of bubble**s** and each thing inside it is a single bubble. Makes sense to me.

### The `for` loop in `draw`

**This is probably the trickiest concept in the whole exercise**

```javascript
for(var i = 0; i < bubbles.length; i++) {
  ellipse(bubbles[i].x, bubbles[i].y, bubbleSize, bubbleSize);

  bubbles[i].x += random(-2, 2);
  bubbles[i].y += random(-2, 2);
}
```
Having just gone over how we pushed items into the `bubbles` array, we should be able to answer the question, "what is each item of the `bubbles` array?"

If your answer was "An object", you are correct! Each element of the array is a whole object (that has certain properties).

So for the first loop `bubbles[i]` is the first object, next time `bubbles[i]` is the second object, and then the third, and so on. Thats why we're able to treat `bubbles[i]` like an object and access it's properties by writing `bubbles[i].x` or `bubbles[i].y`.  

I don't like having `bubbles[i]` all over the code though, it takes my brain some time to decipher what that means and I think we can be more explicit.  Let's store the value of `bubbles[i]` in a variable called something like `bubble`, or `current`, or `currentBubble` indicating it is the bubble in the loop we are currently dealing with. Replace each `bubbles[i]` with the variable name.

```javascript
for(var i = 0; i < bubbles.length; i++) {
  var currentBubble = bubbles[i];

  ellipse(currentBubble.x, currentBubble.y, bubbleSize, bubbleSize);

  currentBubble.x += random(-2, 2);
  currentBubble.y += random(-2, 2);
}
```

## Changing Each Bubble

The loop above is really cool because it gives us a chance to look at each bubble individually as we draw it to the canvas.

I want to check if the mouse is near the bubble (i.e. the *distance* between the mouse coordinates and bubble center is less than some small value, let's say 50px) and if so, color it purple. Give this a shot on your own before referring to the code below. The conditional you write should be *in* the loop and use the `currentBubble` variable. Here's the end result.

![bubble highlight](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/bubble-highlight.gif)

```javascript
// your for loop in the draw function
// should look similar to this.
for(var i = 0; i < bubbles.length; i++) {
  var currentBubble = bubbles[i];

  var mouseDist = dist(mouseX, mouseY, currentBubble.x, currentBubble.y);

  if (mouseDist <= 50) {
    fill(200, 100, 200);
  } else {
    fill(255);
  }


  ellipse(currentBubble.x, currentBubble.y, bubbleSize, bubbleSize);

  currentBubble.x += random(-2, 2);
  currentBubble.y += random(-2, 2);
}
```

## Parting the Bubbles

Now that we've shown we can use loops to make changes to each bubble let's see if we can make the bubbles move away from the mouse so they clear a path for it like this:

![bubble part](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/bubble-part.gif)

Try it on your own before looking at the finished code below. You may need to use multiple conditionals.

```javascript
for(var i = 0; i < bubbles.length; i++) {
  var currentBubble = bubbles[i];

  var mouseDist = dist(mouseX, mouseY, currentBubble.x, currentBubble.y);


  if (mouseDist <= 50) {
    // bubble is near mouse

    if (currentBubble.x > mouseX) {
    // bubble is to Right of mouse, make it move further right
      currentBubble.x += 2;
    } else {
      // bubble is to Left of mouse, make it move further left
      currentBubble.x -= 2;
    }

    if (currentBubble.y > mouseY) {
    // bubble is Below of mouse, make it move further down
      currentBubble.y += 2;
    } else {
    // bubble is Above mouse, make it move further up
      currentBubble.y -= 2;
    }


  } else {
    // otherwise bubble is NOT near mouse, so normal bubble random movement
    currentBubble.x += random(-2, 2);
    currentBubble.y += random(-2, 2);
  }


  ellipse(currentBubble.x, currentBubble.y, bubbleSize, bubbleSize);

}
```

Now that you understand how to loop over an array of objects you can apply this knowledge in a few challenges

## Forcefield Challenge

Here's a rocket flying through space moments before entering an asteroid field. As soon it hits an asteroid it will explode.

![explosion](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/explosion.gif)

Luckily, the ship comes equipped with a forcefield (activated by clicking the mouse) that will deflect all asteroids.

![forcefield](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/forcefield.gif)

Except, currently, the `applyForcefield` function doesn't work :(

Your task is to write code in the `applyForcefield` function that loops over the array called `asteroids`.

`asteroids` is an *array of objects* just like `bubbles`. Also like the `bubbles` array, each object has an `x` and `y` property. Additionally each object has an `xSpeed` and `ySpeed` property. All together it looks something like this

```
asteroids = [
{x: ..., y: ..., xSpeed: ..., ySpeed: ...},
{x: ..., y: ..., xSpeed: ..., ySpeed: ...},
...]
```

Loop through `asteroids` and if any individual asteroid is within the forcefield (use the variable `forcefieldSize`), flip the direction of it's `xSpeed` and `ySpeed`.

Here's all the code to paste into a new project, remember you only need to add to the function `applyForcefield`.

```javascript
var asteroids = [];
var asteroidSize = 40;
var stars = [];
var distance, rocketX, rocketY;
var shipWidth = 15;
var shipHeight = 60;
var forcefieldUp = false;
var forcefieldSize = 150;
var exploded = false;


function setup() {
  createCanvas(800,600);
  initStars();
  initAsteroids();
  rocketX = width/2;
  rocketY = height/2;
}

function draw() {
  background(95);

  drawStars();

  if (!exploded) {
    drawShip();
  } else {
    drawExplosion();
  }

  if (frameCount > 100) {
    drawAsteroids();
  }

  if (forcefieldUp) {
    drawForceField();
  }

}


function applyForcefield() {
  var rocketCenterX = rocketX + shipWidth/2;
  var rocketCenterY = rocketY + shipHeight/2 + 7;
  // use the variables 'asteroids' an array
  // and 'forcefieldSize' a number

  // Your Code Here...

}


// -------------------------------
// You do not need to change any code below this line
// -------------------------------

function initStars() {
  for (var i = 0; i < 100; i++) {
    stars.push({x: random(0, width), y: random(0, height)});
  }
}

function drawStars() {
  stroke(200);
  strokeWeight(1);
  for(var i = 0; i < stars.length; i++) {
    var star = stars[i];

    if (star.y > height) {
      stars[i] = {x: random(0, width), y: 0};
    }

    line(star.x, star.y, star.x + 4, star.y);
    line(star.x +2, star.y - 2, star.x + 2, star.y + 2);

    if (!exploded) {
      star.y += 2;
    }
  }
}

function drawShip() {
  var centerX = rocketX + shipWidth/2;

  fill(95);
  stroke(240, 20, 0);
  quad(centerX, rocketY + shipHeight, centerX - 9, rocketY + shipHeight + 17, centerX, rocketY + shipHeight + 35, centerX + 9, rocketY + shipHeight + 17)
  stroke(240, 100, 0);
  quad(centerX, rocketY + shipHeight, centerX - 4, rocketY + shipHeight + 12, centerX, rocketY + shipHeight + 25, centerX + 4, rocketY + shipHeight + 12);

  fill(200);
  noStroke();


  rect(rocketX, rocketY, shipWidth, shipHeight);
  triangle(rocketX, rocketY, rocketX + shipWidth, rocketY, rocketX + shipWidth/2, rocketY - 15);

  fill(95);
  for(var i = 1; i < 4; i ++) {
    ellipse(centerX, rocketY + 10 + 10*i, 6, 6);
  }

}

function drawForceField() {
  fill(100, 255, 100, 20);
  stroke(150, 255, 100);
  ellipse(rocketX + shipWidth/2, rocketY + shipHeight/2 + 10, forcefieldSize, forcefieldSize);

}

function initAsteroids() {
  for (var i = 0; i < 50; i++) {
    var asteroid = buildAsteroid();
    asteroids.push(asteroid);
  }
}

function buildAsteroid() {
  var x = random(0, width + 1);
  var y = random([-40, height+ 40]);

  var xSpeed = random(-6, 6);
  var ySpeed = y < 0 ? random(1, 6) : random(-1, -6);

  return {x: x, y: y, xSpeed: xSpeed, ySpeed: ySpeed};
}

function drawAsteroids() {
  if (forcefieldUp) {
    applyForcefield();
  } else {
    checkForImpact();
  }

  for(var i = 0; i < asteroids.length; i++) {
    var asteroid = asteroids[i];

    if( offScreen(asteroid)) {
      asteroids[i] = buildAsteroid();
    }

    fill(105);
    stroke(200);
    strokeWeight(2);
    ellipse(asteroid.x, asteroid.y, asteroidSize, asteroidSize);

    asteroid.x += asteroid.xSpeed;
    asteroid.y += asteroid.ySpeed;
  }
}

function checkForImpact() {
  var rocketCenterX = rocketX + shipWidth/2;
  var rocketCenterY = rocketY + shipHeight/2 + 7;

  for(var i = 0; i < asteroids.length; i++) {
    var asteroid = asteroids[i];
    var distance = dist(rocketCenterX, rocketCenterY, asteroid.x  , asteroid.y);

    if (distance < shipWidth + 20) {
      exploded = true;
    }
  }
}

function drawExplosion() {
  var centerX = rocketX + shipWidth/2;

  noFill();
  stroke(240, 20, 0);
  quad(centerX, rocketY - 30, centerX - 60, rocketY, centerX, rocketY + 30, centerX + 60, rocketY)

  stroke(240, 100, 0);
  quad(centerX, rocketY - 15, centerX - 30, rocketY, centerX, rocketY + 15, centerX + 30, rocketY)
}

function offScreen(asteroid) {
  if (asteroid.y + 100 < 0 ||
      asteroid.y - 100 > height ||
      asteroid.x + 100 < 0 ||
      asteroid.x - 100 > width ) {
    return true;
  } else {
    return false;
  }
}

function mouseClicked() {
  forcefieldUp = !forcefieldUp;
}
```

## Expert Challenge

For this challenge you will have much more freedom. The idea is to use similar code to how we pushed the bubbles with mouse to make a sheep herding game. Here's an example, but your game can look like anything you want!

I used the `noCursor()` function to not display the mouse arrow.

![sheep herder](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/sheep-herder.gif)

Good luck & Have fun!

![elephant](https://s3.amazonaws.com/upperline/curriculum-assets/p5js/elephant.gif)
