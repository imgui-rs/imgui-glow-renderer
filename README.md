# imgui-glow-renderer

Renderer for [`imgui-rs`][imgui] using the [`glow`] library for OpenGL.

This is heavily influenced by the [example from upstream](https://github.com/ocornut/imgui/blob/fe245914114588f272b0924538fdd43f6c127a26/backends/imgui_impl_opengl3.cpp).

## Basic usage

A few code [examples](https://github.com/imgui-rs/imgui-glow-renderer/tree/main/examples)
are provided in the source.

In short, create either an [`AutoRenderer`] (for basic usage) or [`Renderer`]
for slightly more customizable operation, then call the `render(...)`
method with draw data from [`imgui`].

## OpenGL (ES) versions

This renderer is expected to work with OpenGL version 3.3 and above, and
OpenGL ES version 3.0 or above. This should cover the vast majority of even
fairly dated hardware. Please submit an issue for any incompatibilities
found with these OpenGL versions, pull requests to extend support to earlier
versions are welcomed.

## Scope

Consider this an example renderer. It is intended to be sufficent for simple
applications running imgui-rs as the final rendering step. If your application
has more specific needs, it's probably best to write your own renderer, in
which case this can be a useful starting point. This renderer is also not
foolproof (largely due to the global nature of the OpenGL state). For example,
a few "internal" functions are marked `pub` to allow the user more
fine-grained control at the cost of allowing potential rendering errors.

## sRGB

When outputting colors to a screen, colors need to be converted from a
linear color space to a non-linear space matching the monitor (e.g. sRGB).
When using the [`AutoRenderer`], this library will convert colors to sRGB
as a step in the shader. When initialising a [`Renderer`], you can choose
whether or not to include this step in the shader or not when calling
[`Renderer::initialize`].

This library also assumes that textures have their internal format
set appropriately when uploaded to OpenGL. That is, assuming your texture
is sRGB (if you don't know, it probably is) the `internal_format` is
one of the `SRGB*` values.