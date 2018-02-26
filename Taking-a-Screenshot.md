```java
byte[] pixels = ScreenUtils.getFrameBufferPixels(0, 0, Gdx.graphics.getBackBufferWidth(), Gdx.graphics.getBackBufferHeight(), true);

// this loop makes sure the whole screenshot is opaque and looks exactly like what the user is seeing
for(int i = 4; i < pixels.length; i += 4) {
    pixels[i - 1] = (byte) 255;
}

Pixmap pixmap = new Pixmap(Gdx.graphics.getBackBufferWidth(), Gdx.graphics.getBackBufferHeight(), Pixmap.Format.RGBA8888);
BufferUtils.copy(pixels, 0, pixmap.getPixels(), pixels.length);
PixmapIO.writePNG(Gdx.files.external("mypixmap.png"), pixmap);
pixmap.dispose();
```