Masking is the technique of hiding portions of an image using the pixel information of another to decide whether a pixel of the original should or should not be shown. There’s more than one way to achieve this effect in libGDX.

# Table of Contents

  * [Masking using the ScissorStack](Masking#1-masking-using-the-scissorstack-rectangles)

# 1. Masking using the ScissorStack (Rectangles)

## Step 1 - Preparations

```java
/* Some attributes we're gonna need. */
private OrthographicCamera camera;
private ShapeRenderer shapeRenderer;
private Rectangle scissors, clipBounds;

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

    /* Some rectangles the ScissorStack needs to perform the clipping, "clipBounds"
     * defines the area where the clipping will happen. */
    scissors = new Rectangle();
    clipBounds = new Rectangle(100, 100, 200, 200);
}
```
## Step 2 - Drawing our masked elements
```java
private void drawMasked() {
    /* Feed the ScissorStack and store whether it could push the scissors or not. */
    ScissorStack.calculateScissors(camera, shapeRenderer.getTransformMatrix(), clipBounds, scissors);
    boolean pop = ScissorStack.pushScissors(scissors);

    /* Draw the elements to be constrained to an area. */
    shapeRenderer.set(Filled);
    shapeRenderer.setColor(Color.RED);
    shapeRenderer.circle(100, 100, 100);
    shapeRenderer.flush();

    /* Safety check for the situations the scissor fails to be pushed to the stack
     * (happens for example when the window is minimized on desktop). */
    if (pop) {
        ScissorStack.popScissors();
    }
}
```
_It is also possible to push multiple rectangles. Only the pixels of the sprites or shapes that are within <b>all</b> of the rectangles will be rendered._
## Step 3 - Drawing the contours for debugging purposes
```java
private void drawContours() {
    /* The rectangular mask. */
    shapeRenderer.set(Line);
    shapeRenderer.setColor(Color.CYAN);
    shapeRenderer.rect(100, 100, 200, 200);
    /* The masked circle. */
    shapeRenderer.setColor(Color.GREEN);
    shapeRenderer.circle(100, 100, 100);
}
```
## Result
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
![Circle masked by rectangle](https://i.imgur.com/8iNQ7FM.png)