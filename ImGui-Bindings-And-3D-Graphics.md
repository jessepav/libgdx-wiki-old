It is possible to use the [SpaiR/imgui-java](https://github.com/SpaiR/imgui-java) bindings project on top of a 3D scene in libGDX.

### Required Dependencies
```
"io.imgui.java:imgui-java-binding:<version>"
"io.imgui.java:imgui-java-lwjgl3:<version>"
"io.imgui.java:imgui-java-natives-linux:<version>"
"io.imgui.java:imgui-java-natives-linux-x86:<version>"
"io.imgui.java:imgui-java-natives-macos:<version>"
"io.imgui.java:imgui-java-natives-windows:<version>"
"io.imgui.java:imgui-java-natives-windows-x86:<version>"
```

### Example Minimal Usage

Initialize ImGui at the beginning of the program:
```
ImGuiImplGlfw imGuiGlfw = new ImGuiImplGlfw();
ImGuiImplGl3 imGuiGl3 = new ImGuiImplGl3();
```

In `create()`:
```
// create 3D scene
GLFWErrorCallback.createPrint(System.err).set();
if (!glfwInit())
{
   throw new IllegalStateException("Unable to initialize GLFW");
}
ImGui.createContext();
final ImGuiIO io = ImGui.getIO();
io.setIniFilename(null);
ImGuiTools.setupFonts(io);

windowHandle = ((Lwjgl3Graphics) Gdx.graphics).getWindow().getWindowHandle();

imGuiGlfw.init(windowHandle, true);
imGuiGl3.init(glslVersion);
```

In `render()`:
```
// render 3D scene
imGuiGlfw.newFrame();
ImGui.newFrame();
ImGui.button("I'm a Button!");
ImGui.render();
imGuiGl3.renderDrawData(ImGui.getDrawData());
glfwSwapBuffers(windowHandle);
glfwPollEvents();
```

In `dispose()`:
```
imGuiGl3.dispose();
imGuiGlfw.dispose();
ImGui.destroyContext();
```

Result:

<img src="https://i.imgur.com/cnfOMDR.png" alt="Screenshot of ImGui in libGDX" width="500"/>