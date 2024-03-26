# OpenGL

<https://antongerdelan.net/opengl/index.html>

[Glad](https://glad.dav1d.de/)
[GLFW](https://www.glfw.org/)

GLFW: An Open Source, multi-platform library for OpenGL, OpenGL ES and Vulkan
development on the desktop. It provides a simple API for creating windows,
contexts and surfaces, receiving input and events.

Glad: Multi-Language GL/GLES/EGL/GLX/WGL Loader-Generator based on the official specs.

[vertex buffer objects (VBO)](https://www.khronos.org/opengl/wiki/Vertex_Specification#Vertex_Buffer_Object)
[vertex array object (VAO)](https://www.khronos.org/opengl/wiki/Vertex_Specification#Vertex_Array_Object)
stores states required to supply vertex data

Vertex shader: transforms the vertices from model space to clip space.
Fragment shader: calculates pixels' color.

| Space        | Hand | Matrix     |
| ------------ | ---- | ---------- |
| Local/Object | R    |            |
| World/Model  | R    | Model      |
| View/Camera  | R    | View       |
| Clip         | L    | Projection |
| Screen       | X    |            |

## Look-at Matrix

The idea is to move the camera to the origin and right to x-axis, up to y-axis
and directing z-axis.

## Perspective Projection

<https://www.songho.ca/opengl/gl_projectionmatrix.html#infinite>

We want to find projecection matrix $P$ from view space to clip space.

$(x_v,y_v,z_v,w_v)$ is mapped to
$(nx_v/z_v,ny_v/z_v,n,1)$ on the near plane in the clip space.
$(l,b,n,1)$ in the view space is mapped to $(-1,-1,-1,1)$ in the clip space.
$(r,t,f,1)$ in the view space is mapped to $(1,1,1,1)$ in the clip space.

$x_c/w_c=A(nx_v/z_v)+B$ cross points $(l,-1)$ and $(r,1)$.
Solutions are $A=\frac{2}{r-l}$ and $B=-\frac{r+l}{r-l}$,
$$x_c/w_c=(\frac{2n}{r-l}x_v+\frac{r+l}{r-l}z_v)/(-z_v)$$
Therefore, $w_c=-z_v$ is a possible solution for the last row of $P$
and $P_{1,-}=(\frac{2n}{r-l},0,\frac{r+l}{r-l},0)$.

$(P_{3,3}z_v+P_{3,4}w_v)/w_c=z_c/w_c=1$ passes $(z_v,w_v)=(-n,-1)$ and
$(z_v,w_v)=(-f,1)$.
Solutions are $P_{3,3}=-\frac{f+n}{f-n}$ and $P_{3,4}=-\frac{2fn}{f-n}$.

## GLSL

[GLSL](https://www.khronos.org/opengl/wiki/Core_Language_(GLSL))

### Data Type

* vecn: a vector of n ﬂoats.
* bvecn: a vector of n booleans.
* ivecn: a vector of n integers.
* uvecn: a vector of n unsigned integers.
* dvecn: a vector of n double components.

### Data IO

* in
* out
* uniform: global

Swizzling syntax

## Pitfall

Be sure to include GLAD before GLFW. The include file for GLAD includes the required
OpenGL headers behind the scenes (like GL/gl.h) so be sure to include GLAD before
other header files that require OpenGL (like GLFW).