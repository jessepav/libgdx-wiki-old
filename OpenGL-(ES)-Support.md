# What is what (GL, GL ES, GLSL)?

Whenever libGDX is talking about GL20 or GL30, it is in fact referring to GL ES 2.0 and GL ES 3.0. OpenGL ES can be seen as a subset of OpenGL and is designed for embedded systems (smartphones in particular).

By default, version 2.0 of OpenGL ES is used, but libGDX can be configured to use 3.0 as well. See for example:

```java
LwjglApplicationConfiguration cfg = new LwjglApplicationConfiguration();
// ...
cfg.useGL30 = true;
```

## And what about GLSL (ES)?
GLSL (or GLSL ES on mobile and web) is the language used for shaders.

The versioning is somewhat confusing.

#### GLSL:

| Open GL Version | GLSL Version |
| --- | --- |
| 2.0 | 110 |
| 2.1 | 120 |
| 3.0 | 130 |
| 3.1 | 140 |
| 3.2 | 150 |
| 3.3 | 330 |
| 4.0 | 400 |
| 4.1 | 410 |
| 4.2 | 420 |
| 4.3 | 430 |
| 4.4 | 440 |
| 4.5 | 450 |
| 4.6 | 460 |

#### GLSL ES:

| OpenGL ES Version | GLSL EL Version | Based on GLSL Version (OpenGL) |
| --- | --- | --- |
| 2.0 | 100 | 120 (2.1) |
| 3.0 | 300 es | 330 (3.3) |

## Platform specificities
### Desktop (Windows, Mac, Linux)
On Desktop, libGDX is mapping all its graphics calls to OpenGL. 

**GL ES 2.0** is roughly based on Open GL 2.0, however, there are some incompatibilities that weren't resolved until Open GL 4.1. To mimic GL ES 2.0, libGDX does not request any specific OpenGL version, so the driver will be more forgiving.

**GL ES 3.0** is the successor of OpenGL ES 2.0. On desktop, OpenGL 4.3 provides full compatibility with OpenGL ES 3.0. For mimicking GL ES 3.0 on desktop, one can specify the exact OpenGL version, that should be used. Please note, that MacOS only supports the [OpenGL 3.2 core](https://www.khronos.org/opengl/wiki/OpenGL_Context#OpenGL_3.2_and_Profiles) profile.

### Android, iOS
**iOS:** OpenGL ES 3.0 support is experimental

**Android:** if your application requires OpenGL ES 2 to function please add the following to the Android Manifest: `<uses-feature android:glEsVersion="0x00020000" android:required="true" />` 

If your Android application requires OpenGL ES 3, please add the following to the manifest:
`<uses-feature android:glEsVersion="0x00030000" android:required="true" />`

This prevents your application from being seen by unsupported devices in the Play Store (formerly Android Market)

### Web
On Web, the graphic stuff is handled by WebGL. Web only supports GL ES 2.0.


## Precision modifiers ##
OpenGL ES 2.0 requires the specification of precision modifiers for attributes, uniforms and locals. Desktop OpenGL does not support this. You will have to guard against that in your fragment shader with something similar to this code snippet:

```java
#ifdef GL_ES 
#define LOW lowp
#define MED mediump
#define HIGH highp
precision mediump float;
#else
#define MED
#define LOW
#define HIGH
#endif
```

This will define the LOWP, MED, and HIGH macros to equivalent OpenGL ES precision modifiers and sets the default precision for float to medium. This will only happen on platforms actually running OpenGL ES, on the desktop, the macros are defined to be empty.

## OpenGL ES 2.0 Documentation ##
* [OpenGL ES 2.0 Reference Pages](http://www.khronos.org/opengles/sdk/docs/man/ "OpenGL ES 2.0 Reference Pages")
* [OpenGL ES 2.0 Reference Card](http://www.khronos.org/opengles/sdk/docs/reference_cards/OpenGL-ES-2_0-Reference-card.pdf "OpenGL ES 2.0 Reference Card")
* [OpenGL ES 2.0 Specification](http://www.khronos.org/registry/gles/#specs2 "OpenGL ES 2.0 Specification")
* [OpenGL ES Programming Guide for iOS](https://developer.apple.com/library/ios/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/Introduction/Introduction.html "OpenGL ES Programming Guide for iOS"): the core concepts are valid for other platforms as well
* [OpenGL ES 2.0 Pipeline Structure](http://en.wikibooks.org/wiki/OpenGL_Programming/OpenGL_ES_Overview#OpenGL_ES_2.0_Pipeline_Structure "OpenGL ES 2.0 Pipeline Structure")