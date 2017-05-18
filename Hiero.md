# Write generated BitmapFont to a file:


```kotlin

generateFont(16,2f, "data/fonts/pf_ronda_seven.ttf")


fun generateFont(size: Int, outline: Float, path: String)
{
    Lwjgl3Application(object : ApplicationAdapter()
    {
        override fun create()
        {
            val info = BitmapFontWriter.FontInfo()
            info.padding = BitmapFontWriter.Padding(1, 1, 1, 1)


            val param = FreeTypeFontParameter()
            param.size = size
            param.borderWidth = outline
            param.borderColor = Color.BLACK
            param.borderStraight = true
            param.minFilter = Texture.TextureFilter.Linear
            param.magFilter = Texture.TextureFilter.MipMapLinearLinear
            param.characters = Hiero.EXTENDED_CHARS
            param.packer = PixmapPacker(512, 512, Pixmap.Format.RGBA8888, 2, false, SkylineStrategy())

            val generator = FreeTypeFontGenerator(Gdx.files.internal(path))
            val data = generator.generateData(param)
            val name ="pf_ronda_seven_x${size}"
            BitmapFontWriter.setOutputFormat(BitmapFontWriter.OutputFormat.XML)
            BitmapFontWriter.writeFont(data, arrayOf("$name.png"),
                    Gdx.files.absolute("generated/$name.fnt"), info, 512, 512)
            BitmapFontWriter.writePixmaps(param.packer.pages, Gdx.files.absolute("generated"), "$name")

            System.exit(0)
        }
    }, Lwjgl3ApplicationConfiguration())
}

```