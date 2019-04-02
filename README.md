# GLSL Shader Includes
A utility class which allows the end user to make use of the include statement in a shader file.
This is a C++ class but any programmer that has basic knowledge of his / her programming language of choice, should be able to quickly convert the code to another language within a couple minutes. It is that simple.

### Introduction
The sole purpose of this class is to load a file and extract the text that is in it. In theory, this class could be used for a variety of text-processing purposes, but it was initially designed to be used to load shader source code for GLSL, as it does not have a built-in function that lets you include another source files easily.

### Using this class
Since the entire class is a static class, you only have to add this to to your project:

```cpp
#include "Shadinclude.hpp"
```
```cpp
std::string shaderSource = Shadinclude::load("./path/to/shader.extension", "customKeyword");
```

This will (recursively) extract the source code from the first shader file.

Now, you might be wondering, what is the point of using your code for something so trivial as to loading a file and calling the "std::getline()" function on it?

Well, besides loading the shader source code from a single file, the loader also supports custom keywords that allow you to include external files inside your shader source code! Since all of this loading is still fairly trivial but cumbersome to write over and over again, it has been uploaded to this repository for you to use.

### Example
*Vertex shader [./resources/shaders/basic.vs]*
```glsl
#version 330 core
layout (location = 0) in vec3 position;

// Include other files
#include include/functions.incl
#include include/uniforms.incl

void main()
{
    position += doFancyCalculationA() * offsetA;
    position += doFancyCalculationB() * offsetB;
    position += doFancyCalculationC() * offsetC;

    gl_Position = vec4(position, 1.0);
}
```

*Utility functions [./resources/shaders/include/functions.incl]*
```glsl
vec3 doFancyCalculationA()
{
    return vec3(1.0, 0.0, 1.0);
}

vec3 doFancyCalculationB()
{
    return vec3(0.0, 1.0, 0.0);
}

vec3 doFancyCalculationC()
{
    return vec3(0.0, 0.0, 1.0);
}
```

*Uniforms [./resources/shaders/include/uniforms.incl]*
```glsl
uniform vec3 offsetA;
uniform vec3 offsetB;
uniform vec3 offsetC;
```

*Result*
```glsl
#version 330 core
layout (location = 0) in vec3 position;

vec3 doFancyCalculationA()
{
    return vec3(1.0, 0.0, 1.0);
}

vec3 doFancyCalculationB()
{
    return vec3(0.0, 1.0, 0.0);
}

vec3 doFancyCalculationC()
{
    return vec3(0.0, 0.0, 1.0);
}

uniform vec3 offsetA;
uniform vec3 offsetB;
uniform vec3 offsetC;

void main()
{
    position += doFancyCalculationA() * offsetA;
    position += doFancyCalculationB() * offsetB;
    position += doFancyCalculationC() * offsetC;

    gl_Position = vec4(position, 1.0);
}
```
