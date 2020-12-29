libGDX can be used to render to virtual reality headsets using LWJGL's OpenVR (HTC Vive) and OVR (Oculus) modules.

### Required Dependencies

The Maven coordinates can be found using the [LWJGL customize](https://www.lwjgl.org/customize) page.

### Example code

Example code can be found at [https://github.com/badlogic/gdx-vr](https://github.com/badlogic/gdx-vr).

Additionally, the following demos from LWJGL may be useful for debugging:
* [HelloOpenVR.java](https://github.com/LWJGL/lwjgl3/blob/master/modules/samples/src/test/java/org/lwjgl/demo/openvr/HelloOpenVR.java)
* [HelloLibOVR.java](https://github.com/LWJGL/lwjgl3/blob/master/modules/samples/src/test/java/org/lwjgl/demo/ovr/HelloLibOVR.java)

### Roadmap

VR support might become easier relatively soon via the use of [OpenXR](https://www.khronos.org/openxr/). The LWJGL project [plans to support this in the future](https://github.com/LWJGL/lwjgl3/issues/569#issuecomment-643830566). This would remove the need to support both OVR and OpenVR and instead just using OpenXR to support all hardware.