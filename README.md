CADRays - GPU-accelerated photorealistic renderer 
=================================================

## Overview

CADRays is a fast GPU accelerated, unbiased physically-based renderer.
It is based on the open-source photorealistic rendering solution from Open CASCADE Technology (OCCT) platform.

With CADRays, you can see (or show to others) what your product will look in reality just having a digital model of a product.
And physical correctness allows achieving stunning results faster, with minimum parameters to tweak.

## Efficiency

CADRays uses an optimized GPU path tracing rendering engine and acceleration structures from OCCT.
Thanks to platform-independent design, GPU acceleration will work on both AMD and NVIDIA graphics cards or even on integrated Intel GPUs
(with rendering performance limited only by the graphics hardware capabilities).

## Interactivity

CADRays' on-screen viewport is the final rendering result.
Thus it provides immediate feedback to adjust the parameters of materials, light sources and a camera with a fully interactive frame rate.
CADRays uses progressive rendering mode starting with a noisy image and then progressively refining it towards the final result.
And, of course, you can stop the rendering process at a point where you find the noise level acceptable.

## Materials

CADRays features a simple but robust double layered material model allowing to simulate most common material types such as glossy surfaces, glass, metal, or car paint.
You can mix different material properties in the desired proportions in order to create a wide variety of realistic surfaces.

## Camera

CADRays supports both perspective and orthographic camera models.
The second one allows to render the scene without perspective effects which can be useful for technical and architectural visualization tasks.

## Model Import

STEP, IGES, and BREP formats are supported out of the box; in addition, all modeling capabilities of OCCT are available through the interactive Tcl console.
Conventional triangulation formats such as OBJ, PLY, STL are also supported. So you can combine CAD models and meshes in a single scene.

## Building CADRays

Before building CADRays, make sure you have all the required prerequisite libraries installed.

| Component | Requirement |
|-----------|-------------|
| OCCT      | OCCT [https://dev.opencascade.org/](https://dev.opencascade.org/) |
| Tcl       | Tcl/Tk 8.6.3+ [https://www.tcl.tk/software/tcltk/download.html](https://www.tcl.tk/software/tcltk/download.html) |
| Freetype  | FreeType 2.4.11-2.7.1 [https://sourceforge.net/projects/freetype/files](https://sourceforge.net/projects/freetype/files) |
| FreeImage | FreeImage 3.17.0+ [https://sourceforge.net/projects/freeimage/files](https://sourceforge.net/projects/freeimage/files) |
| Assimp    | Assimp [http://www.assimp.org/index.php/downloads](http://www.assimp.org/index.php/downloads) |
| GLFW      | GLFW [http://www.glfw.org/download.html](http://www.glfw.org/download.html) |

**CMake**-based build process is a standard way to produce the binaries of CADRays from sources. CADRays requires CMake version 3.13 or later.

## Running CADRays

The installation folder contains executable **CADRays.exe** to run the application.

## Usage

Here is the [video tutorial](https://www.youtube.com/watch?v=D6_uGxmhuVk).

## Licensing

CADRays is available under the MIT License (MIT).
See the [LICENSE.txt](LICENSE.txt) file.

## Credits

We are grateful to authors of the following third-party developments:

| Name               | Web site                                                                                                 |
|--------------------|----------------------------------------------------------------------------------------------------------|
| Inih               | [https://github.com/benhoyt/inih](https://github.com/benhoyt/inih)                                       |
| IMGUI              | [https://github.com/ocornut/imgui](https://github.com/ocornut/imgui)                                     |
| IMGuizmo           | [https://github.com/CedricGuillemet/ImGuizmo](https://github.com/CedricGuillemet/ImGuizmo)               |
| Tiny file dialogs  | [https://sourceforge.net/projects/tinyfiledialogs/](https://sourceforge.net/projects/tinyfiledialogs/)   |
| IconFontCppHeaders | [https://github.com/juliettef/IconFontCppHeaders](https://github.com/juliettef/IconFontCppHeaders)       |
| STB                | [https://github.com/nothings/stb](https://github.com/nothings/stb)                                       |
| OpenGL-Registry    | [https://github.com/KhronosGroup/OpenGL-Registry](https://github.com/KhronosGroup/OpenGL-Registry)       |
| GL3W               | [https://github.com/skaslev/gl3w](https://github.com/skaslev/gl3w)                                       |
| Assimp             | [http://www.assimp.org](http://www.assimp.org)                                                           |
| GLFW               | [http://www.glfw.org](http://www.glfw.org)                                                               |
