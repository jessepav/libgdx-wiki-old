Masking is the technique of hiding portions of an image using the pixel information of another to decide whether a pixel of the original should or should not be shown. There’s more than one way to achieve this effect in libGDX.

## Table of Contents
  * [Masking using glScissor](Masking#1-masking-using-glScissor-rectangle)
  * [Masking using the ScissorStack](Masking#2-masking-using-the-scissorstack-rectangles)
  * [Masking using the ShapeRenderer](Masking#3-masking-using-the-shaperenderer-various-shapes)
  * [Masking using the SpriteBatch](Masking#4-masking-using-the-spritebatch-any-shape)
  * [Masking using Pixmaps](Masking#5-masking-using-pixmaps-any-shape)
## 1. Masking using glScissor (Rectangle)
For the simplest of masking needs here’s a technique that allows us to create and apply a single rectangular mask using OpenGL's Scissor Test. The Scissor Test is a Per-Sample Processing operation that discards Fragments that fall outside of a certain rectangular portion of the screen.
### Step 1 - Preparations
```java
private ShapeRenderer shapeRenderer;

@Override
public void create() {
    /* We can use a SpriteBatch or a ShapeRenderer to draw our masked elements. */
    shapeRenderer = new ShapeRenderer();
    shapeRenderer.setAutoShapeType(true);

    /* Increase the OpenGL line thickness for better visualization. */
    Gdx.gl.glLineWidth(2);
}
```
### Step 2 - Drawing our masked elements
```java
private void drawMasked() {
    /* To activate the scissor test, first enable the GL_SCISSOR_TEST enumerator.
     * Once enabled, pixels outside of the scissor box will be discarded. */
    Gdx.gl.glEnable(GL20.GL_SCISSOR_TEST);

    /* To define the scissor box, use this function: */
    Gdx.gl.glScissor(100, 100, 200, 200);
    /* The x and y is the window-space lower-left position of the scissor box,
     * and width and height define the size of the rectangle. */

    /* Draw our circle to be masked, we could also draw sprites with a SpriteBatch. */
    shapeRenderer.set(Filled);
    shapeRenderer.setColor(Color.RED);
    shapeRenderer.circle(100, 100, 100);

    /* Remember to flush before changing GL states again. */
    shapeRenderer.flush();

    /* Deactivate the scissor test before continuing with further rendering operations. */
    Gdx.gl.glDisable(GL20.GL_SCISSOR_TEST);
}
```
### Step 3 - Drawing the contours for debugging purposes
```java
private void drawContours() {
    shapeRenderer.set(Line);

    /* Draw the circle's contour for comparison. */
    shapeRenderer.setColor(Color.GREEN);
    shapeRenderer.circle(100, 100, 100);

    /* Draw the clipped area contour for comparison. */
    shapeRenderer.setColor(Color.CYAN);
    shapeRenderer.rect(100, 100, 200, 200);
}
```
### Result
```java
@Override
public void render() {
    shapeRenderer.begin();

    drawMasked();
    drawContours();

    shapeRenderer.end();
}
```
![Circle masked by a rectangle](https://i.imgur.com/8iNQ7FM.png)
## 2. Masking using the ScissorStack (Rectangles)
A single rectangle could easily not be enough, here’s a technique that allows us to create and apply multiple rectangular masks using libGDX’s ScissorStack.
### Step 1 - Preparations
```java
/* Some attributes we're gonna need. */
private ShapeRenderer shapeRenderer;
private Rectangle scissors1, scissors2;

@Override
public void create() {
    /* The ScissorStack needs a camera to transform the clipping rectangles. */
    camera = new OrthographicCamera();
    camera.setToOrtho(false, CAMERA_WIDTH, CAMERA_HEIGHT);

    /* We can use a SpriteBatch or a ShapeRenderer to draw our masked elements. */
    shapeRenderer = new ShapeRenderer();
    shapeRenderer.setAutoShapeType(true);
    shapeRenderer.setProjectionMatrix(camera.combined);

    /* Increase the OpenGL line thickness for better visualization. */
    Gdx.gl.glLineWidth(2);

    /* scissors1 and scissors2 store the results of calculateScissors(...). 
     * clipBounds is used to define the x, y, width and height of the clipping rectangles. */
    scissors1 = new Rectangle();
    Rectangle clipBounds = new Rectangle(100, 100, 200, 200);
    ScissorStack.calculateScissors(camera, shapeRenderer.getTransformMatrix(), clipBounds, scissors1);

    scissors2 = new Rectangle();
    clipBounds.set(50f, 50f, 100f, 100f);
    ScissorStack.calculateScissors(camera, shapeRenderer.getTransformMatrix(), clipBounds, scissors2);
}
```
### Step 2 - Drawing our masked elements
```java
private void drawMasked() {
    /* Feed the ScissorStack and store whether it could push the scissors or not. */
    boolean pop1 = ScissorStack.pushScissors(scissors1);
    boolean pop2 = ScissorStack.pushScissors(scissors2);

    /* Draw the elements to be constrained to an area,
     * without masking this would render a red filled circle. */
    shapeRenderer.set(Filled);
    shapeRenderer.setColor(Color.RED);
    shapeRenderer.circle(100, 100, 100);
    shapeRenderer.flush();

    /* Safety check for the situations the scissor fails to be pushed to the stack
     * (happens for example when the window is minimized on desktop or the clipping
     * area is <= 0). */
    if (pop1) {
        ScissorStack.popScissors();
    }
    if (pop2) {
        ScissorStack.popScissors();
    }
}
```
_It is also possible to push multiple rectangles. Only the pixels of the sprites or shapes that are within <b>all</b> of the rectangles will be rendered._

_Also, if your camera moves, you'll need to recalculate the scissor area afterwards._
### Step 3 - Drawing the contours for debugging purposes
```java
private void drawContours() {
    shapeRenderer.set(Line);

    /* The rectangular mask. */
    shapeRenderer.setColor(Color.CYAN);
    shapeRenderer.rect(100, 100, 200, 200);
    shapeRenderer.rect(50, 50, 100, 100);

    /* The masked circle. */
    shapeRenderer.setColor(Color.GREEN);
    shapeRenderer.circle(100, 100, 100);
}
```
### Result
```java
@Override
public void render() {
    ScreenUtils.clear(Color.BLACK);

    shapeRenderer.begin();

    drawMasked();
    drawContours();

    shapeRenderer.end();
}
```
![Circle masked by 2 rectangles](https://i.imgur.com/HEa7EQK.png)
## 3. Masking using the ShapeRenderer (Various Shapes)
Alright rectangles are great but our needs are greater what now. This upcoming technique allows us to create more diversely shaped masks using libGDX’s ShapeRenderer.
### Step 1 - Preparations
```java
private ShapeRenderer shapeRenderer;

@Override
public void create() {
    /* We can use a SpriteBatch or a ShapeRenderer to draw our masked elements. */
    shapeRenderer = new ShapeRenderer();
    shapeRenderer.setAutoShapeType(true);

    /* Increase the OpenGL line thickness for better visualization. */
    Gdx.gl.glLineWidth(2);
}
```
### Step 2 - Draw the mask elements to the depth buffer
```java
private void drawMasks() {
    /* Clear our depth buffer info from previous frame. */
    Gdx.gl.glClear(GL20.GL_DEPTH_BUFFER_BIT);

    /* Set the depth function to LESS. */
    Gdx.gl.glDepthFunc(GL20.GL_LESS);

    /* Enable depth writing. */
    Gdx.gl.glEnable(GL20.GL_DEPTH_TEST);

    /* Disable RGBA color writing. */
    Gdx.gl.glColorMask(false, false, false, false);

    /* Render mask elements. */
    shapeRenderer.set(Filled);
    shapeRenderer.circle(100, 200, 100);
    shapeRenderer.triangle(0, 0, 100, 100, 200, 0);
    shapeRenderer.flush();
}
```
When using a SpriteBatch write this line right after `SpriteBatch.begin()` : `Gdx.gl.glDepthMask(true);`
### Step 3 - Draw the masked elements
```java
private void drawMasked() {
    /* Enable RGBA color writing. */
    Gdx.gl.glColorMask(true, true, true, true);

    /* Set the depth function to LESS. */
    Gdx.gl.glDepthFunc(GL20.GL_EQUAL);

    /* Render masked elements. */
    shapeRenderer.setColor(Color.RED);
    shapeRenderer.circle(100, 100, 100);
    shapeRenderer.flush();
}
```
### Step 4 - Draw the contours for debugging purposes
```java
private void drawContours() {
    /* Disable depth writing. */
    Gdx.gl.glDisable(GL20.GL_DEPTH_TEST);

    shapeRenderer.set(Line);

    /* The circle and triangle masks. */
    shapeRenderer.setColor(Color.CYAN);
    shapeRenderer.circle(100, 200, 100);
    shapeRenderer.triangle(0, 0, 100, 100, 200, 0);

    /* The masked circle. */
    shapeRenderer.setColor(Color.GREEN);
    shapeRenderer.circle(100, 100, 100);
}
```
### Result
```java
@Override
public void render() {
    ScreenUtils.clear(Color.BLACK);

    shapeRenderer.begin();

    drawMasks();
    drawMasked();
    drawContours();

    shapeRenderer.end();
}
```
![Circle masked by another circle and a triangle](https://imgur.com/Pmlfn7M.png)
## 4. Masking using the SpriteBatch (Any shape)
For the demanding GDXer with complex masking needs, this technique allows us to have any mask imaginable and take the alpha channel into account for the first time! For this we’ll be using libGDX’s SpriteBatch.
### Step 1 - Preparations
These are the images we're gonna use:
| ![The mask](https://imgur.com/WPHeXdB.png) | ![The sprite](https://imgur.com/Gf2pQYJ.png) | ![The sprite's inverse alpha](https://imgur.com/sS4xjZD.png) |
| :-: | :-: | :-: |
| [The mask](https://imgur.com/J8dkKVI) | [The sprite to mask](https://imgur.com/JjMCVTh) | [The sprite's inverse alpha](https://imgur.com/P14Mrj4) |

The images in a black background for clarity:
| ![The mask](https://imgur.com/rm13HUV.png) | ![The sprite](https://imgur.com/eGoidRi.png) | ![The sprite's inverse alpha](https://imgur.com/QI5aGQ4.png) |
| :-: | :-: | :-: |
| The mask | The sprite to mask | The sprite's inverse alpha |
```java
/* Some attributes we're gonna need. */
private SpriteBatch spriteBatch;
private Sprite mask, maskedSprite, alphaInvertedMaskedSprite;

@Override
public void create() {
    spriteBatch = new SpriteBatch();

    /* Load the mask containing the alpha information. */
    mask = new Sprite(new Texture("mask.png"));

    /* Load the sprite which will be masked. */
    maskedSprite = new Sprite(new Texture("sprite.png"));
    maskedSprite.setColor(Color.RED);

    /* The technique requires us to provide the inverted alpha version of the sprite we want to mask. */
    alphaInvertedMaskedSprite = new Sprite(new Texture("alphaInvertedSprite.png"));
}
```
### Step 2 - Draw the mask elements to the frame buffer
```java
private void drawMasks() {
    /* Disable RGB color writing, enable alpha writing to the frame buffer. */
    Gdx.gl.glColorMask(false, false, false, true);

    /* Change the blending function for our alpha map. */
    spriteBatch.setBlendFunction(GL20.GL_ONE, GL20.GL_ZERO);

    /* Draw alpha masks. */
    mask.draw(spriteBatch);

    /* This blending function makes it so we subtract instead of adding to the alpha map. */
    spriteBatch.setBlendFunction(GL20.GL_ZERO, GL20.GL_ONE_MINUS_SRC_ALPHA);

    /* Remove the masked sprite's inverse alpha from the map. */
    alphaInvertedMaskedSprite.draw(spriteBatch);

    /* Flush the batch to the GPU. */
    spriteBatch.flush();
}
```
### Step 3 - Draw the masked elements
```java
private void drawMasked() {
    /* Now that the buffer has our alpha, we simply draw the sprite with the mask applied. */
    Gdx.gl.glColorMask(true, true, true, true);

    /* Change the blending function so the rendered pixels alpha blend with our alpha map. */
    spriteBatch.setBlendFunction(GL20.GL_DST_ALPHA, GL20.GL_ONE_MINUS_DST_ALPHA);

    /* Draw our sprite to be masked. */
    maskedSprite.draw(spriteBatch);

    /* Remember to flush before changing GL states again. */
    spriteBatch.flush();
}
```
### Step 4 - Draw the original sprites for debugging purposes
```java
private void drawOriginals() {
    /* Switch to the default blend function */
    spriteBatch.setBlendFunction(GL20.GL_SRC_ALPHA, GL20.GL_ONE_MINUS_SRC_ALPHA);

    /* Draw the source images separately */
    spriteBatch.draw(mask, 0, 256);
    spriteBatch.draw(maskedSprite, 256, 256);
    spriteBatch.draw(alphaInvertedMaskedSprite, 512, 256);
}
```
### Result
```java
@Override
public void render() {
    ScreenUtils.clear(Color.BLACK);

    spriteBatch.begin();

    drawMasks();
    drawMasked();
    drawOriginals();

    spriteBatch.end();
}
```
![Masked sprite and original sprites](https://imgur.com/BN4qUUi.png)
## 5. Masking using Pixmaps (Any shape)
This technique allows the mask to be any image or shape and takes the alpha channel into account. This time we'll be using the libGDX’s Pixmap class.
### Step 1 - Preparations
```java
private ShapeRenderer shapeRenderer;
private SpriteBatch spriteBatch;
private Texture masked, original;
private final int size = 256;

@Override
public void create() {
    /* The ShapeRenderer will only be used to draw the mask contours */
    shapeRenderer = new ShapeRenderer();
    shapeRenderer.setAutoShapeType(true);
    Gdx.gl20.glLineWidth(2);

    /* Needed to render our textures, a ShapeRenderer won't work with this technique. */
    spriteBatch = new SpriteBatch();

    /* The path to the image to mask. */
    FileHandle imagePath = new FileHandle("images/shared/weirdShape.png");

    /* Load the pixels of our image into a Pixmap. */
    Pixmap pixmap = new Pixmap(imagePath);

    /* Have an unaltered version for comparison. */
    original = new Texture(imagePath);

    /* Apply the mask to our Pixmap. */
    pixmap = applyMask(pixmap);

    /* Load the pixel information of the Pixmap into a Texture for drawing. */
    masked = new Texture(pixmap);
}
```
### Step 2 - Applying the mask
```java
private Pixmap applyMask(Pixmap source) {
    /* Create a Pixmap to store the mask information, at the end it will
     * contain the result. */
    Pixmap result = new Pixmap(source.getWidth(), source.getHeight(), Pixmap.Format.RGBA8888);

    /* This setting lets us overwrite the pixels' transparency. */
    result.setBlending(None);

    /* Ignore RGB values unless you want funky results, alpha is for the mask. */
    result.setColor(new Color(1f, 1f, 1f, 1f));

    /* Draw a circle to our mask, any shape is possible since
     * you can draw individual pixels to the Pixmap. */
    result.fillCircle(size / 2, size / 2, size / 2);

    /* Draw a rectangle with half alpha to our mask, this will turn
    * a corner of the original image transparent. */
    result.setColor(1f, 1f, 1f, 0.5f);
    result.fillRectangle(size / 2, size / 2, size / 2, size / 2);

    /* We can also define the mask by loading an image:
     * result = new Pixmap(new FileHandle("image.png")); */

    /* Decide the color of each pixel using the AND bitwise operator. */
    for (int x = 0; x < result.getWidth(); x++) {
        for (int y = 0; y < result.getHeight(); y++) {
            result.drawPixel(x, y, source.getPixel(x, y) & result.getPixel(x, y));
        }
    }

    return result;
}
```
### Step 3 - Drawing the original and masked images
```java
private void drawImages() {
    /* Draw the original image in blue first to see transparency taking place. */
    spriteBatch.setColor(Color.BLUE);
    spriteBatch.draw(original, 0, 0, size, size);

    /* Draw the masked image in red on top. */
    spriteBatch.setColor(Color.RED);
    spriteBatch.draw(masked, 0, 0, size, size);
}
```
### Step 4 - Drawing the contours of the mask for debugging purposes
```java
private void drawContours() {
    /* Draw the contour of the circle and rectangle used as masks. */
    shapeRenderer.setColor(Color.CYAN);
    shapeRenderer.circle(size / 2f, size / 2f, size / 2f);
    shapeRenderer.rect(size / 2f, 0, size / 2f, size / 2f);
}
```
### Result
```java
@Override
public void render() {
    ScreenUtils.clear(Color.BLACK);

    spriteBatch.begin();
    drawImages();
    spriteBatch.end();

    shapeRenderer.begin();
    drawContours();
    shapeRenderer.end();
}
```
![Original and masked images + contours](https://imgur.com/93DNO1z.png)

In red: The masked image

In blue: The original image

In purple: The masked image with transparency, blending with the original image