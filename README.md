# cimguizmo

cimguizmo with CMake support. We have only designed this project to be built as a dynamic library, but it should be easy to modify the CMake file to support statically linking.

## Building

Create project files with cmake:

```bash
cmake -B build -D CMAKE_BUILD_TYPE=Release -D CMAKE_LIBRARY_OUTPUT_DIRECTORY=Release -D IMGUIZMO_CIMGUI="<cimgui project location>"
```

And build:

```
cmake --build build --config Release
```

### Notes:

- On Windows, the build will fail due to linking errors if your cimgui doesn't export the ImGui symbols. See [our cimgui-feetype](https://github.com/Voltstro-Studios/cimgui-freetype/blob/master/CMakeLists.txt#L45) for an example on how to do that.
- If your cimgui dynamic libraries are not located at `<cimgui project location>/build/Release`, you can change where look for them using `IMGUIZMO_CIMGUI_OUTPUT`.

## Builds

You can grab the built dynamic libraries off [from our Azure CI](https://dev.azure.com/Voltstro-Studios/cimgui-freetype/_build?definitionId=10&view=runs).

---

This is a c-api wrapper for ImGuizmo (https://github.com/CedricGuillemet/ImGuizmo)  which is a guizmo for Dear ImGui (https://github.com/ocornut/imgui)

ImGuizmo is a submodule of this repo. To do generation again in case ImGuizmo is changed, you should update ImGuizmo, make sure that cimgui repository is a sibling folder to this repository folder and execute generator/generator.bat(.sh)
