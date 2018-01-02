# Building for GWT with a package created from source
When working with LibGDX source and creating a custom package (see https://libgdx.badlogicgames.com/documentation/hacking/Working%20from%20source.html), sometimes one wants to add a new file to LibGDX.
For each new file added to LibGDX, it is necessary to add a new entry for the new file to the gdx/src/com/badlogic/gdx.gwt.xml in order for the package to be used successfully when compiling for GWT. 
See for example https://github.com/libgdx/libgdx/pull/5018/files#diff-13b547f0d1b0872d60d67db4ca0b266d