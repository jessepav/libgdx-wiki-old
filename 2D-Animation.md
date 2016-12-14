2D Animation is the technique used to create the illusion of movement using static images. This article describes how to create animations with libGdx.

**You can use the [Animation Class](https://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/g2d/Animation.html).**

## Details ##

An animation consists of multiple frames which are shown in a sequence at set intervals. An animation of a running man can be achieved by taking pictures of him while running and playing those images back sequentially in a loop.

The following image shows a complete cycle of a man running. It is called a sprite sheet. Each box is a sprite and it is called a frame. To create the running animation, the sprites have to be drawn one after the other as time passes.

[[images/sprite-animation1.png]]

The frame rate is the number of times we switch frames per second. Examining the example sprite sheet, we can see that a complete running cycle consists of 30 frames (6 columns and 5 rows). If the character is to complete a cycle in a second, we need to show all the 30 frames in a second. That gives us the frame rate of 30 FPS (Frames Per Second). The time between changes of the frame (the frame time or interval time) is the reciprocal of the FPS, in this case `1 second / 30 = 0.033 seconds per frame`.

In other words, to have an animation at  30 FPS, the current frame needs to be changed every 0.033 seconds.

An animation is a very simple state machine. The running man has 30 states as per the sprite sheet. The numbered frames represent the states a running man goes through.

[[images/sprite-animation2.png]]

A state machine can’t be in 2 or more states at any given time. When the character is in State 1, then the sprite associated with that state is drawn. It stays in that state for 0.033 seconds and once the time has elapsed, it transitions to the next state, which is State 2. This goes on until the last state/frame (30) is met.

If the animation is looping, it returns to the first frame after all frames have been shown.

LibGDX's Animation class can be used to easily manage an animation.

## Example ##

The following code snippet will create an [Animation](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/g2d/Animation.html) [(code)](https://github.com/libgdx/libgdx/blob/master/gdx/src/com/badlogic/gdx/graphics/g2d/Animation.java) using the animation_sheet.png sprite-sheet and renders the animation to the screen. 

Note that a more typical use case would not use a fixed sprite sheet, but would use regions from a TextureAtlas to represent the frames.

The Animation class can use any kind of object to represent a frame, and the type is declared by specifying the animation type, for example `Animation<TextureRegion>`. Typically you will choose something that can be rendered, such as a TextureRegion, as in this example. Note that it would be inadvisable to use the Sprite class to represent frames of an animation, because the Sprite class contains positional data that would not carry from frame to frame.

```java
public class Animator implements ApplicationListener {

	private static final int        FRAME_COLS = 6;         // #1
	private static final int        FRAME_ROWS = 5;         // #2
	
	Animation<TextureRegion>        walkAnimation;          // #3
	Texture                         walkSheet;              // #4
	TextureRegion[]                 walkFrames;             // #5
	SpriteBatch                     spriteBatch;            // #6
	
	float stateTime;                                        // #7
	
	@Override
	public void create() {
		walkSheet = new Texture(Gdx.files.internal("animation_sheet.png")); // #8
		TextureRegion[][] tmp = TextureRegion.split(walkSheet, 
                    walkSheet.getWidth()/FRAME_COLS, 
                    walkSheet.getHeight()/FRAME_ROWS);				    // #9
		walkFrames = new TextureRegion[FRAME_COLS * FRAME_ROWS];
		int index = 0;
		for (int i = 0; i < FRAME_ROWS; i++) {
			for (int j = 0; j < FRAME_COLS; j++) {
				walkFrames[index++] = tmp[i][j];
			}
		}
		walkAnimation = new Animation<TextureRegion>(0.025f, walkFrames); // #10
		spriteBatch = new SpriteBatch();				  // #11
		stateTime = 0f;						    	  // #12
	}

	@Override
	public void render() {
		Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);			         // #13
		stateTime += Gdx.graphics.getDeltaTime();			         // #14
		TextureRegion currentFrame = walkAnimation.getKeyFrame(stateTime, true); // #15
		spriteBatch.begin();
		spriteBatch.draw(currentFrame, 50, 50);				         // #16
		spriteBatch.end();
	}
}
```

### The Setup ###

*#1* and *#2* defines constants representing how many sprites are laid out horizontally and vertically on the sheet. 

*Note*: The sprite-sheet contains frames of equal size and they are all aligned.

*#3* - Declaration of the walkAnimation object which is libGdx’s implementation of animation, with the frame type declared to be TextureRegions.

*#4* - The Texture which will contain the whole sheet as a single image (texture).

*#5* - Declare walkFrames as an array of TextureRegion objects. This array will hold each frame of the animation. The first element will hold the frame from the top left, the second will hold the one right to it and so on. When the last element in the row is reached, the next row follows with the left most element.

*#6* - The SpriteBatch is used to draw the texture onto the screen. 

*#7* - The stateTime is the number of seconds elapsed from the start of the animation. This is used to determine the state of the animation. It is a simple accumulator based on which the animation knows when to change to the next state. 

For example, if the animation is a 30FPS one, then the state change needs to happen at each 33.3 milliseconds. If the update happens at 10 milliseconds, the stateTime accumulates the elapsed time and the animation will change to the next state (frame) at the 4th update.

*#8* - Creates a texture from animation_sheet.png which is placed in the assets directory of the project (check [[how to set up a project | Project setup, running & debugging]])

*#9* - Using the split convenience method on the texture, we obtain a two dimensional array of the frames from the texture. Bear in mind that this works only if the frames are of equal size. Then with the help of a temporary variable, the walkFrames array is populated. This is necessary, as the Animation works with one dimensional arrays only. 

The walkFrames can be populated in any way that suits you, for example by loading each frame from different files, however this is not advisable because of performance issues.

*#10* - This is where the Animation is created. The first parameter tells the animation, how much time is allocated for each frame. This is expressed in seconds. The more frames an animation has, the smaller the time it will be and the smoother it will look. It consumes more memory though. 
The second parameter is the array containing all the frames. The first element is the first frame and so on.

*#11* - Initialises the SpriteBatch which will draw the frame.

*#12* - Resets the stateTime to 0. It will start accumulating the time each render call.

### The render method ###

*#13* - Clears the screen each frame.

*#14* - Adds the time elapsed since the last render to the stateTime.

*#15* - Obtains the current frame. This is given by the animation for the current time. The second variable is the looping. Passing in true, we tell the animation to restart after it reaches the last frame.

*#16* - Renders the current frame onto the screen at (50, 50) using the SpriteBatch.

[[images/sprite-animation3.png]]

Creating an animation is extremely simple by using the following constructor.

| Method signature | Description |
|:-------------------|:--------------|
| `Animation (float frameDuration, TextureRegion... keyFrames)` | The first parameter is the frame time and the second is an array of regions (frames) making up the animation|

## Best practices ##
 * Pack frames into one texture to optimize rendering
 * Settle for a reasonable number of frames depending on the game type. For a retro arcade style, 4 frames suffice, while more realistic looking movements require more frames.

## Assets ##

Get the sprite-sheet [here](images/sprite-animation4.png).