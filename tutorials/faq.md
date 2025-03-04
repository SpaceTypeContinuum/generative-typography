# FAQ and Random Tidbits

* [Mirroring Images](#mirroring-images)
* [if {} else if {} else {}](#if--elseif--else)
* [Custom images with p5 for copy() and mask()](#custom-images-with-p5-for-copy-and-mask)
* [Variable Fonts](#variable-fonts)

## Mirroring Images

By default, the images that we receive from a camera capture are not mirrored — we receive exactly what the camera sees. To mirror your capture (like how [zoom mirrors your video](https://www.pocket-lint.com/apps/news/152025-why-is-zoom-video-backwards-and-how-to-fix-it)), you could flip the entire canvas horizontally:

```js
scale(-1, 1)
```

However, this will also mirror text or shapes that you draw onto the canvas (i.e. your letters will be drawn backwards!)

The easiest way to flip the capture without changing your text/ASCII is to **read your image backwards** from right to left. If you have a loop like this:

```js
temp_img = capture.get()

for (let x = 0; x < width; x += 5) {
  for (let y = 0; y < height; y += 5) {
    let referencePixel = temp_img.get(x, y)
  }
}
```

You can read the capture image in reverse by _starting_ on the right side (`width`) and working backwards to `0`:

```js
let referencePixel = temp_img.get(width - x, y)
```

In essence, you are still filling your grid from left --> right, but you are reading the video capture from right --> left.

## if {} else if {} else {}

[Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)

If you have multiple conditional statements that overlap and you _only_ want to run one of them, you can use an `else if` statement:

```js
if (x < 100) {
  // if x is between [0, 100) mmake it red
  fill('red')
} else if (x < 200) {
  // OTHERWISE if x is between [100, 200) make it green
  fill('green')
} else if (x < 300) {
  // OTHERWISE if x is between [200, 300) make it blue
  fill('blue')
} else {
  // OTHERWISE make it yellow
  fill('yellow')
}
```

With an `else if` statement, it is implied that the _previous_ conditions were false. For example:

```js
if (x < 100) {
  ...
} else if (x < 200) { 
  // Here we know that x is at least 100 (x ≥ 100)
} else if (x < 300) {
  // Here we know that x is at least 200 (x ≥ 100 AND x ≥ 200)
}
```

Compare this to using `if` statements:

```js
if (x < 100) {
  // if x is less than 100 make it red
  fill('red')
} 

if (x < 200) {
  // Also if x is less than 200 make it green
  fill('green')
}

if (x < 300) {
  // Also if x is less than 300 make it blue
  fill('blue')
} else {
  // OTHERWISE make it yellow
  fill('yellow')
}
```

In this example, if x is less than 100, we'll run these fill commands:

```js
fill('red')
fill('green')
fill('blue')
```

Which is not what we want -- the blue fill will override our previous colors ) :

We can _remove_ the overlapping conditions by making them more strict:

```js
if (x < 100) { ... }
if (100 <= x && x < 200) { ... } // run if 100 ≤ x AND x < 200
if (200 <= x && x < 300) { ... } // run if 200 ≤ x AND x < 300

```

However, that's more verbose and means we have to do more logical calculation as the human to make sure none of the conditions are overlapping as our program changes. 

There's no big harm in doing it this way so choose what works for you as you're coding ( :

## Custom images with p5 for copy() and mask()

If you need to use image-specific operations like `copy()` or `mask()`, you can generate an image on the fly within P5.

To do this, you'll need to draw on a separate graphics layer:

```js
let layer = createGraphics(400, 400)
layer.textAlign(CENTER, CENTER)
layer.textSize(100)
layer.fill('white')
layer.text('hello', width / 2, height / 2)
```

and then copy it onto an image object:

```js
img = createImage(width, height)
img.copy(layer, 
         0, 0, width, height, 
         0, 0, width, height)
```

* [Sketch Example with copy()](https://editor.p5js.org/kyeah/sketches/s3RTOYFrU)
* [Sketch Example with mask()](https://editor.p5js.org/kyeah/sketches/v-NylzfyT)

<img src="https://lh3.googleusercontent.com/pw/ACtC-3eX5MHDJtMRhZ1PabJdF1yacAiuFKK5IQIZEPSwtNiCGlAPOwxMWRy40PIaREXP2_NQ7Y0IS943mfLmCHdYJtCCW7nf9-cSHiqEnp_dJEEezEkDhwjr4qpGjyVKvKacSMsphrPDKLmiVzWL8O7L3NLYtw=s800-no" style="max-width:400px"/>

## Variable Fonts

Although P5 doesn't have native variable font support, it can manipulate the DOM and HTML/CSS. The text does not live in the canvas and cannot use most p5 native 
functions, but can manipulated by updating the CSS.

![](https://lh3.googleusercontent.com/pw/ACtC-3ffvF6YWuPTX-BwPrwPZmL-322SBsk5l8OfPZzzlakGxCOkMN2n3e5mIW6pGYccKnFhWvIdEGao3FGFYfDa8KbP-ATNr1VFgI5Xyp-zjGz-C6RLi-1k9pZw3CCVfXK17vEY-gOGx6WY_iD0kzTcL__3RA=w392-h356-no?authuser=0)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/JBdfVROrw)
