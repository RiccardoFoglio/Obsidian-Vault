```C++
#include <glad/glad.h>
#include <GLFW/glfw.3>
#include <iostream>
```

```C++
int main() { // glfw init and configuration glfwInit();
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3); 
glfwWindowHint(GLFW_OPENGL_PROFILE,GLFW_OPENGL_CORE_PROFILE); 
...
```

These are used to set the project and the OpenGL version to 3.3

# Create a Window

```C++
// glfw window creation 
GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL" , NULL, NULL); 
if (window == NULL) 
{ 
	std::cout << "Failed to create GLFW window" << std::endl; 
	glfwTerminate(); 
	return -1; 
} 
glfwMakeContextCurrent(window);
```

Usage:` GLFWwindow* glfwCreateWindow(int width, int height, const char *title, GLFWmonitor* monitor, GLFWwindow *share)`
- `width`, `height`: The desired width and height, in screen coordinates, of the window. Must be greater than zero.
- `title`: The initial, UTF-8 encoded window title. 
- `monitor`: The monitor to use for full screen mode, or NULL for windowed mode. 
- `share`: The window whose context to share resources with, or NULL to not share resources
Returns:
 - `GLFWwindow`: the handle of the created window, 
 - `NULL`: if an error occurred.
## FULL Screen
```C++
// Retrieve the primary monitor 
GLFWmonitor* primaryMonitor = glfwGetPrimaryMonitor(); 
// Retrieve the video setting of the monitor 
const GLFWvidmode* mode = glfwGetVideoMode(primaryMonitor); 
// glfw window creation 
// -------------------- 
GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "Example_00", primaryMonitor, NULL);
```
## Set Viewport
size of the window is needed : `glViewport(0, 0, 800, 600);`
- `x`,`y` : Specify the lower left corner of the viewport rectangle, in pixels. The initial value is (0,0).
- `width`,`height`: Specify the width and height of the viewport. When a GL context is first attached to a window, width and height are set to the dimensions of that window.

`void framebuffer_size_callback(GLFWwindow* window, int width, int height);`
used to update the resized window by a user
```C++
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{ 
	glViewport(0, 0, width, height); 
}
```

### Render Loop
```C++
// render loop 
while (!glfwWindowShouldClose(window)) 
{ 
	glfwSwapBuffers(window); 
	glfwPollEvents(); 
}
// glfw: terminate, clearing all previously allocated GLFW resources. 
glfwTerminate(); 
return 0;
```
### Swap Buffers
This function swaps the front and back buffers of the specified window.
`void glfwSwapBuffers (GLFWwindow * window)`
- `window`: The window whose buffers to swap
# User Input
Usage: `int glfwGetKey(GLFWwindow * window, int key)`
- `window`: The desired window.
- `key`: The desired keyboard key. GLFW_KEY_UNKNOWN is not a valid key for this function.
Returns:
- GLFW_PRESS
- GLFW_RELEASE
All macros: https://www.glfw.org/docs/3.3/group__keys.html
```C++
void processInput(GLFWwindow* window) 
{ 
	if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS) 
		glfwSetWindowShouldClose(window, true); 
}
```

The `processInput()` function is called every iteration of the render loop.
# Rendering Commands
`glClear()`can be leveraged to clear the screen with a color of our choice. The possible bits that can be set are:
- `GL_COLOR_BUFFER_BIT`
- `GL_DEPTH_BUFFER_BIT`
- `GL_STENCIL_BUFFER_BIT`

the color used to clear the screen has to be specified using the `glClearColor()`
```C++
// render loop 
while (!glfwWindowShouldClose(window))
{
processInput(window); 

// rendering commands 
glClearColor(0.2f, 0.3f, 0.3f, 1.0f); 
glClear(GL_COLOR_BUFFER_BIT); 

glfwSwapBuffers(window); 
glfwPollEvents(); 
}
```

# Shaders
Shaders are written in the OpenGL Shading Language (GLSL).
![[Pasted image 20241130153528.png]]

Vertex Data: A list of 3D coordinates is passed as input to the graphics pipeline.

Normalized Device Coordinates (NDC) : The normalized device coordinates are a small space where the x, y, and z values vary from -1.0 to 1.0. Any coordinates that fall outside this range will be discarded/clipped and won't be visible on the screen.

GPU memory is managed via the so-called vertex buffer objects (VBO)
```C++
unsigned int VBO; 
glGenBuffers(1, &VBO);

glBindBuffer(GL_ARRAY_BUFFER, VBO);
```
OpenGL has many types of buffer objects. The buffer type for a vertex buffer object is `GL_ARRAY_BUFFER`. 
OpenGL allows developers to bind to several buffers at once as long as they have a different buffer type.

Copy vertices into the buffer's memory
```C++
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```
This function creates and initializes a buffer object's data store for the buffer object currently bound. Any pre-existing data store is deleted. The new data store is created with the specified size in bytes and usage.
## Inside Vertex Shader
```C++
#version 330 
core layout (location = 0) in vec3 aPos; 
void main() 
{ 
	gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0); 
}
```

Shader can be stored inside a const C string. It will be dynamically compiled at run-time from its source code. To this aim, a shader object is created, again referenced by an ID by using the `glCreateShader()` function inside an `unsigned int`

Then attach shader code to shader object and compile
```C++
glShaderSource(vertexShader, 1, &vertexShaderSource, NULL); glCompileShader(vertexShader);

int success; 
char infoLog[512]; 
glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success); 
if (!success) 
{ 
	glGetShaderInfoLog(vertexShader, 512, NULL, infoLog); 
	std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl; 
}
```
## Inside Fragment Shader code
```C++
#version 330 
core out vec4 FragColor; 
void main() 
{ 
	FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f); 
}
```
The fragment shader is in charge of calculating the color output of pixels. RGBA technique.

Compilation:
```C++
// fragment shader 
unsigned int fragmentShader = glCreateShader(GL_FRAGMENT_SHADER); 
glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL); 
glCompileShader(fragmentShader); 
// check for shader compile errors 
glGetShaderiv(fragmentShader, GL_COMPILE_STATUS, &success); 
if (!success) 
{ 
	glGetShaderInfoLog(fragmentShader, 512, NULL, infoLog); 
	std::cout << "ERROR::SHADER::FRAGMENT::COMPILATION_FAILED\n" << infoLog << std::endl; 
}
```

Now link both shader objects into a shader program that can be rendered.
```C++
unsigned int shaderProgram = glCreateProgram();

glAttachShader(shaderProgram, vertexShader); 
glAttachShader(shaderProgram, fragmentShader); 
glLinkProgram(shaderProgram);

glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success); 
if (!success) 
{ 
	glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog); 
	std::cout << "ERROR::SHADER::PROGRAM::LINKING_FAILED\n" << infoLog << std::endl;
}

glUseProgram(shaderProgram);
```

Once linked to the program, the vertex and fragment shaders can be deleted:
```C++
glDeleteShader(vertexShader); 
glDeleteShader(fragmentShader);
```

![[Pasted image 20241130164920.png]]
Knowing this structure, it is possible to tell OpenGL how it should interpret the vertex data (per vertex attribute) using `glVertexAttribPointer`:
```C++
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0); glEnableVertexAttribArray(0);
```
- `type`: Specifies the data type of each component in the array. The symbolic constants `GL_BYTE`, `GL_UNSIGNED_BYTE`, `GL_SHORT`, `GL_UNSIGNED_SHORT`, `GL_INT`, and `GL_UNSIGNED_INT`. The initial value is `GL_FLOAT`. 
- `normalized`: Specifies whether fixed-point data values should be normalized (`GL_TRUE`) or converted directly as fixed-point values (`GL_FALSE`) when they are accessed. 
- `stride`: Specifies the byte offset between consecutive generic vertex attributes. If stride is 0, the generic vertex attributes are understood to be tightly packed in the array. The initial value is 0. 
- `pointer`: Specifies an offset of the first component of the first generic vertex attribute in the array in the data store of the buffer currently bound to the `GL_ARRAY_BUFFER` target. The initial value is 0.

### Summary
```C++
// 0. copy vertices array in a buffer for OpenGL to use 
glBindBuffer(GL_ARRAY_BUFFER, VBO); 
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW); 
// 1. set the vertex attributes pointers 
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0); 
glEnableVertexAttribArray(0); 
// 2. use shader program to render an object 
glUseProgram(shaderProgram); 
// 3. draw the object 
OpenGLDrawingFunction();
```

The above process should be repeated every time an object has to be draw. Binding the appropriate buffer objects and configuring all vertex attributes for each of those objects quickly becomes a cumbersome process. For this reason, there is a way to store all these state configurations into an object and simply bind this object to restore its state.

A **Vertex Array Object** (also known as VAO) can be bound just like a vertex buffer object and any subsequent vertex attribute calls from that point on will be stored inside the VAO. This has the advantage that when configuring vertex attribute pointers developer only has to make those calls once and whenever the object has to be draw. This makes switching between different vertex data and attribute configurations as easy as binding a different VAO. All the state is stored inside the VAO.

```C++
// ..:: Initialization code (done once (unless your object frequently changes)) :: .. 
// 1. bind Vertex Array Object 
glBindVertexArray(VAO); 
// 2. copy our vertices array in a buffer for OpenGL to use 
glBindBuffer(GL_ARRAY_BUFFER, VBO); glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW); 
// 3. then set our vertex attributes pointers 
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0); glEnableVertexAttribArray(0);
[...] 
// ..:: Drawing code (in render loop) :: .. 
// 4. draw the object 
glUseProgram(shaderProgram); 
glBindVertexArray(VAO); 
someOpenGLFunctionThatDrawsOurTriangle();
```

A VAO is created that stores vertex attribute configuration and which VBO to use. 
Usually, when multiple objects have to be drawn, first it is necessary to generate/configure all the VAOs (and thus the required VBO and attribute pointers) and store those for later use. 
When one of the objects has to be drawn, the corresponding VAO is taken, bonded, the object drawn and the VAO unbound again.

## Drawing
```C++
glUseProgram(shaderProgram); 
glBindVertexArray(VAO); 
glDrawArrays(GL_TRIANGLES, 0, 3);
```

The Element Buffer Objects (EBO) is a buffer, just like a vertex buffer object, that stores indices that OpenGL uses to decide what vertices to draw. This so-called indexed drawing is exactly the solution to the problem.
```C++
// set up vertex data (and buffer(s)) and configure vertex attributes 
float vertices[] = { 0.5f, 0.5f, 0.0f, // top right 
					0.5f, -0.5f, 0.0f, // bottom right 
					-0.5f, -0.5f, 0.0f, // bottom left 
					-0.5f, 0.5f, 0.0f // top left 
					}; 

unsigned int indices[] = { 0, 1, 3, // first Triangle 
						  1, 2, 3 // second Triangle 
						  };
```

```C++
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO); 
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);

//glDrawArrays(GL_TRIANGLES, 0, 6); 
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```
![[Pasted image 20241130170708.png]]

```C++
// 1. bind Vertex Array Object 
glBindVertexArray(VAO); 
// 2. copy our vertices array in a vertex buffer for OpenGL to use 
glBindBuffer(GL_ARRAY_BUFFER, VBO); glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW); 
// 3. copy our index array in a element buffer for OpenGL to use 
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO); glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW); 
// 4. then set the vertex attributes pointers 
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0); glEnableVertexAttribArray(0);
glUseProgram(shaderProgram); glBindVertexArray(VAO); glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0); // glBindVertexArray(0); // no need to unbind it every time
```
## GLSL
Shaders are written in the C-like language GLSL.
```C++
#version version_number 
in type in_variable_name; 
in type in_variable_name; 

out type out_variable_name;

uniform type uniform_name; 

void main() { 
	// process input(s) and do some weird graphics stuff 
	... 
	// output processed stuff to output variable 
	out_variable_name = weird_stuff_we_processed;
}
```

GLSL has most of the default basic types: `int`, `float`, `double`, `uint` and `bool`.
GLSL also features two container types, namely `vectors` and `matrices`

A vector in GLSL is a 2,3 or 4 component container for any of the basic types
```C++
vec2 someVec; 
vec4 differentVec = someVec.xyxx; 
vec3 anotherVec = differentVec.zyw; 
vec4 otherVec = someVec.xxxx + anotherVec.yxzy;

vec2 vect = vec2(0.5, 0.7); 
vec4 result = vec4(vect, 0.0, 0.0); 
vec4 otherResult = vec4(result.xyz, 1.0);
```
### Uniforms
Uniforms are another way to pass data from an application on the CPU to the shaders on the GPU. 
Uniforms are however slightly different compared to vertex attributes.
Uniforms are global
```C++
#version 330 core 
out vec4 FragColor; 
uniform vec4 ourColor; // this variable is set in the OpenGL code. 
void main() { FragColor = ourColor; }
```

Change the color with time:
```C++
// be sure to activate the shader before any calls to glUniform 
glUseProgram(shaderProgram); 
// update shader uniform 
double timeValue = glfwGetTime(); 
float greenValue = static_cast(sin(timeValue) / 2.0 + 0.5); 
int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor"); 
glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);
```

## Shader Class
```C++
class Shader 
{ public: 
	 unsigned int ID; 
	 // constructor generates the shader on the fly 
	 Shader(const char* vertexPath, const char* fragmentPath) 
	 // use/activate the shader 
	 void use() 
	 // utility uniform functions 
	 void setBool(const std::string& name, bool value) const 
	 void setInt(const std::string& name, int value) const 
	 void setFloat(const std::string& name, float value) const 
 };

// activate the shader 
void use() { 
	glUseProgram(ID); 
}
// utility uniform functions 
void setBool(const std::string& name, bool value) const 
{ 
	glUniform1i(glGetUniformLocation(ID, name.c_str()), (int)value); 
} 
void setInt(const std::string& name, int value) const 
{ 
	glUniform1i(glGetUniformLocation(ID, name.c_str()), value); 
} 
void setFloat(const std::string& name, float value) const 
{ 
	glUniform1f(glGetUniformLocation(ID, name.c_str()), value); 
}
```

Using the shader class:
```C++
Shader ourShader("shader.vs", "shader.fs"); 
... 
// render loop 
while (!glfwWindowShouldClose(window)) 
{ 
	// draw the triangle
	ourShader.use(); 
	glBindVertexArray(VAO);
	glDrawArrays(GL_TRIANGLES, 0, 3);
	... 
}
```
# Texture
A texture is a 2D image used to add detail to an object.
```C++
float textCoords[] = { 
	0.0f, 0.0f, // lower-left corner 
	1.0f, 0.0f, // lower-right corner 
	0.5f, 1.0f, // top-center corner 
};
```
- `GL_REPEAT`: The default behavior for textures. Repeats the texture image.
- `GL_MIRRORED_REPEAT`: Same as `GL_REPEAT` but mirrors the image with each repeat.
- `GL_CLAMP_TO_EDGE`: Clamps the coordinates between 0 and 1. The result is that higher coordinates become clamped to the edge, resulting in a stretched edge pattern. 
- `GL_CLAMP_TO_BORDER`: Coordinates outside the range are now given a user-specified border color
![[Pasted image 20241130172845.png]]

`glTexParameter`
- `target`: Specifies the target texture, which must be either `GL_TEXTURE_2D`, `GL_TEXTURE_3D`, etc. 
- `pname`: Specifies the symbolic name of a single-valued texture parameter. It can be one of the following: `GL_TEXTURE_WRAP_S`, `GL_TEXTURE_WRAP_T`, `GL_TEXTURE_WRAP_R`, `GL_TEXTURE_MIN_FILTER`, or `GL_TEXTURE_MAG_FILTER`.
- `param`: Specifies the value of `pname`.

Border Color
```C++
float borderColor[] = { 1.0f, 1.0f, 0.0f, 1.0f }; 
glTexParameterfv(GL_TEXTURE_2D, GL_TEXTURE_BORDER_COLOR, borderColor);
```

Texture Filtering
- `GL_NEAREST`  is the default texture filtering method of OpenGL.  OpenGL selects the texel that center is closest to the texture coordinate. 
- `GL_LINEAR` takes an interpolated value from the texture coordinate's neighboring texels, approximating a color between the texels.
![[Pasted image 20241130174532.png]]
## Mipmaps
After a certain distance threshold from the viewer, OpenGL will use a different mipmap texture that best suits the distance to the object. Because the object is far away, the smaller resolution will not be noticeable to the user. OpenGL is then able to sample the correct texels, and there's less cache memory involved when sampling that part of the mipmaps.
## Textured Object
To use textures into an OpenGL application, it is necessary to load them
In this course, the `stb_image.h` library is used.

To load an image using `stb_image.h` it is possible to call the `stbi_load` function:
```C++
int width, height, nrChannels; 
unsigned char* data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
```

Generate the texture object
```C++
unsigned int texture; 
glGenTextures(1, &texture);

glBindTexture(GL_TEXTURE_2D, texture);
```

Set texture wrapping and filtering parameters
```C++
// set the texture wrapping parameters 
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
// set texture wrapping to GL_REPEAT (default wrapping method) 
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT); 
// set texture filtering parameters 
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR); glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

Textures are generated with glTexImage2D
```C++
glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
```
- The first argument specifies the texture target; setting this to `GL_TEXTURE_2D` means this operation will generate a texture on the currently bound texture object at the same target (so any textures bound to targets `GL_TEXTURE_1D` or `GL_TEXTURE_3D` will not be affected). 
- The second argument specifies the mipmap level for which a texture has to be created manually, 0 means that is left at the base level.
- The third argument tells OpenGL in what kind of format the texture has to be stored. The sample image has only RGB values so the texture is stored with RGB values as well.
- The 4th and 5th argument sets the width and height of the resulting texture. These values should have been stored earlier when loading the image, so the corresponding variables are used.
- The 6th argument should always be 0 (used for legacy stuff).
- The 7th and 8th argument specify the format and datatype of the source image. The image is loaded with RGB values and these values are stored as chars (bytes) so the corresponding values are passed.
- The last argument is the actual image data.

Generate the required mipmaps for the currently bound texture
```C++
glGenerateMipmap(GL_TEXTURE_2D);
```

free image memory after generating texture and mipmaps
```C++
stbi_image_free(data);
```

Apply the texture:
```C++
float vertices[] = { 
    // positions         // colors           // texture coords 
	0.5f, 0.5f, 0.0f,    1.0f, 0.0f, 0.0f,   1.0f, 1.0f,  // top right 
	0.5f, -0.5f, 0.0f,   0.0f, 1.0f, 0.0f,   1.0f, 0.0f,  // bottom right 
	-0.5f, -0.5f, 0.0f,  0.0f, 0.0f, 1.0f,   0.0f, 0.0f,  // bottom left 
	-0.5f, 0.5f, 0.0f,   1.0f, 1.0f, 0.0f,   0.0f, 1.0f   // top left 
};
```
![[Pasted image 20241130180523.png]]


```C++
// position attribute 
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)0); glEnableVertexAttribArray(0); 
// color attribute 
glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(3 * sizeof(float))); glEnableVertexAttribArray(1); 
// texture coord attribute 
glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float))); glEnableVertexAttribArray(2);
```

vertex shader:
```C++
#version 330 core 
layout (location = 0) in vec3 aPos; 
layout (location = 1) in vec3 aColor; 
layout (location = 2) in vec2 aTexCoord; 

out vec3 ourColor; 
out vec2 TexCoord; 

void main() 
{ 
	gl_Position = vec4(aPos, 1.0); 
	ourColor = aColor; 
	TexCoord = aTexCoord; 
}
```

fragment shader:
```C++
#version 330 core 
out vec4 FragColor; 
in vec3 ourColor; 
in vec2 TexCoord; 
uniform sampler2D ourTexture; 

void main() 
{ 
	FragColor = texture(ourTexture, TexCoord); 
}
```

render loop:
```C++
// bind Texture 
glBindTexture(GL_TEXTURE_2D, texture); 
// render container 
ourShader.use(); 
glBindVertexArray(VAO); 
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

## Multi-textured objects

It is possible to assign multiple textures to the same shader using the `glUniform` function
The location of a texture is more commonly known as a texture unit.

```C++
// bind textures on corresponding texture units 
glActiveTexture(GL_TEXTURE0); 
glBindTexture(GL_TEXTURE_2D, texture1); 
glActiveTexture(GL_TEXTURE1); 
glBindTexture(GL_TEXTURE_2D, texture2);
```

OpenGL have at least a minimum of 16 texture units to be used that can be activated using `GL_TEXTURE0` to `GL_TEXTURE15`

```C++
#version 330 core 
out vec4 FragColor; 
in vec3 ourColor; 
in vec2 TexCoord; 
uniform sampler2D texture1; 
uniform sampler2D texture2; 
void main() 
{ 
	FragColor = mix(texture(texture1, TexCoord), texture(texture2, TexCoord), 0.2); 
}
```

A value of 0.2 will return 80% of the first input color and 20% of the second input color

Remember: when using .png files, there's also an alpha channel --> necessary to specify GL_RGBA, or OpenGL will incorrectly interpret the image data

Rendering procedure must be changed for rendering multiple textures:
```C++
glActiveTexture(GL_TEXTURE0); glBindTexture(GL_TEXTURE_2D, texture1); glActiveTexture(GL_TEXTURE1); glBindTexture(GL_TEXTURE_2D, texture2); 

ourShader.use(); 
glBindVertexArray(VAO); 
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

flipped texture:
```C++
stbi_set_flip_vertically_on_load(true);
```

# Transformations

GLM is OpenGL Mathematics, used to make calculations easier.
```C++
#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>
#include <glm/gtc/type_ptr.hpp>
```

Usage example: translate a vector (1,0,0) by (1,1,0), rotate and scale
```C++
glm::vec4 vec(1.0f, 0.0f, 0.0f, 1.0f); 
glm::mat4 trans = glm::mat4(1.0f); 

trans = glm::translate(trans, glm::vec3(1.0f, 1.0f, 0.0f));
trans = glm::rotate(trans, glm::radians(90.0f), glm::vec3(0.0, 0.0, 1.0)); 
trans = glm::scale(trans, glm::vec3(0.5, 0.5, 0.5));

vec = trans * vec; 
std::cout << vec.x << vec.y << vec.z << std::endl;
```

Update vertex shader:
```C++
#version 330 core 
layout (location = 0) in vec3 aPos; 
layout (location = 1) in vec2 aTexCoord; 
out vec2 TexCoord; 
uniform mat4 transform; 
void main() 
{ 
	gl_Position = transform * vec4(aPos, 1.0f); 
	TexCoord = vec2(aTexCoord.x, aTexCoord.y); 
}
```

to pass the trans matrix to the shader:
```C++
unsigned int transformLoc = glGetUniformLocation(ourShader.ID, "transform");
glUniformMatrix4fv(transformLoc, 1, GL_FALSE, glm::value_ptr(trans));
```

## Transforms over Time
```C++
glm::mat4 transform = glm::mat4(1.0f); 
transform = glm::translate(transform, glm::vec3(0.5f, -0.5f, 0.0f)); 
transform = glm::rotate(transform, (float)glfwGetTime(), glm::vec3(0.0f, 0.0f, 1.0f));
```

The order in which the transformations are applied leads to different results.

# Coordinate Systems

OpenGL expects all the vertices to be in normalized device coordinates after each vertex shader run, to make them visible in the screen. That is, the x, y and z coordinates of each vertex should be between -1.0 and 1.0;

There are a total of 5 different coordinate systems to be considered:
- Local space (or Object space) 
- World space 
- View space (or Eye space) 
- Clip space 
- Screen space
![[Pasted image 20241201151544.png|500]]
### World Transform
World Transform (or object transform) is defined by the model matrix, It allows you to convert the vertices (and normals) of the models from the local space (e.g., the coordinate space used in the tool in which the object was modeled) to the world space (world coordinate system).
### View Transform
View Transform converts the vertices of an object from worldspace to view-space. 
It is defined by the view matrix, calculated as the inverse matrix of the transformation matrix used to position and orient the camera in the world. 

Commonly, the first column represents the Right (X) vector, the second is associated to the Up (Y) vector, the third to the Forward (Z) vector, while the fourth is the translation vector of the space represented by the transformation matrix.

![[Pasted image 20241201153913.png]]

If you want to observe the scene from another point of view, practically, the camera is not moved and oriented but a transformation (described by the view matrix) is applied to all the objects in the scene so that they appear in front of the camera in the same how they would look if the camera had actually been moved.
### Projection Transform

Projection Transform converts from view-space coordinates to clip-space (window coordinate system), using the projection matrix. 
Multiplying by this projection matrix, the frustum of the camera becomes a cube and the objects are "deformed" according to the characteristics of the camera
![[Pasted image 20241201155050.png]]

The projection matrix to transform view coordinates to clip coordinates usually takes two different forms, where each form defines its own unique frustum: orthographic projection matrix or perspective projection matrix.
![[Pasted image 20241201155240.png]]


An orthographic projection matrix defines a cube-like frustum box. The frustum defines the visible coordinates and is specified by a width, a height and a near and far plane.
```C++
float aspect = (float)SCR_WIDTH / SCR_HEIGHT; 
projection = glm::ortho(-aspect, aspect, -1.0f, 1.0f, 0.1f, 100.0f);
```

A perspective projection matrix can be created in GLM using a GLM's built-in function:
```C++
glm::mat4 proj = glm::perspective(glm::radians(45.0f), (float)width / (float)height, 0.1f, 100.0f);
```
FOV value, set aspect ratio, then set near and far plane of frustum.

### Viewpoint Transform
Viewport Transform is applied to adapt the image to the screen. More specifically, OpenGL uses the parameters from `glViewPort` to map the normalized-device coordinates to screen coordinates where each coordinate corresponds to a point on the screen
![[Pasted image 20241201160425.png]]

# Overview
![[Pasted image 20241201160516.png]]
$$
V_{clip} = M_{projection} \ \cdot \ M_{view} \ \cdot \ M_{model} \ \cdot \ M_{local}
$$
The resulting vertex should then be assigned to `gl_Position` in the vertex shader and OpenGL will then automatically perform perspective division and clipping.

The model matrix consists of translations, scaling and/or rotations to be applied to transform all object's vertices to the global world space.
To transform the plane used in the previous examples by rotating it on the x-axis so it looks like it's laying on the floor, the model matrix looks like this:
```C++
glm::mat4 model = glm::mat4(1.0f); 
model = glm::rotate(model, glm::radians(-55.0f), glm::vec3(1.0f, 0.0f, 0.0f));
```
By multiplying the vertex coordinates with this model matrix, the vertex coordinates are transformed to world coordinates.

A view matrix is created.
To move a camera backwards (so the object become visible as by default the camera is located at the world origin (0,0,0)), is the same as moving the entire scene forward.
That is exactly what a view matrix does, the entire scene is moved around inversed to where the camera has to be moved.

Because the camera has to be moved backwards and since OpenGL is a right-handed system, the camera is moved in the positive z-axis. To this aim the whole scene is translated towards the negative z-axis (this gives the impression that we are moving backwards). Therefore, the view matrix looks like this:
```C++
glm::mat4 view = glm::mat4(1.0f); 
view = glm::translate(view, glm::vec3(0.0f, 0.0f, -3.0f));
```

The last thing to define is the projection matrix. To use the perspective projection, it is possible to declare a projection matrix like this:
```C++
glm::mat4 projection; 
projection = glm::perspective(glm::radians(45.0f), 800.0f / 600.0f, 0.1f, 100.0f);
```


After creating the transformation matrices, they have to be passed to the shaders.

```C++
#version 330 core 
layout (location = 0) in vec3 aPos; 
... 
uniform mat4 model; 
uniform mat4 view; 
uniform mat4 projection; 
void main() { 
	gl_Position = projection * view * model * vec4(aPos, 1.0); ... 
}
```

```C++
// retrieve the matrix uniform locations
unsigned int modelLoc = glGetUniformLocation(ourShader.ID, "model"); 
unsigned int viewLoc = glGetUniformLocation(ourShader.ID, "view"); 
// pass them to the shaders (3 different ways)
glUniformMatrix4fv(modelLoc, 1, GL_FALSE, glm::value_ptr(model)); 
glUniformMatrix4fv(viewLoc, 1, GL_FALSE, &view[0][0]); 
ourShader.setMat4("projection", projection);
```
## 3D Objects

To fix issues with triangles forming cubes, we introduce the z-buffer to OpenGL, used for depth information.

Enable Depth Testing : `glEnable(GL_DEPTH_TEST);` 
Before each render iteration, we clear the z-buffer : `glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);`

Multiple cubes in different positions:
```C++
glm::vec3 cubePositions[] = { 
	glm::vec3(0.0f, 0.0f, 0.0f), 
	glm::vec3(2.0f, 5.0f, -15.0f), 
	glm::vec3(-1.5f, -2.2f, -2.5f), 
	glm::vec3(-3.8f, -2.0f, -12.3f), 
	glm::vec3(2.4f, -0.4f, -3.5f), 
	glm::vec3(-1.7f, 3.0f, -7.5f), 
	glm::vec3(1.3f, -2.0f, -2.5f), 
	glm::vec3(1.5f, 2.0f, -2.5f), 
	glm::vec3(1.5f, 0.2f, -1.5f), 
	glm::vec3(-1.3f, 1.0f, -1.5f) 
};
```
Update the model matrix:
```C++
glBindVertexArray(VAO); 
for (unsigned int i = 0; i < 10; i++) 
{ 
	glm::mat4 model = glm::mat4(1.0f); 
	model = glm::translate(model, cubePositions[i]); float angle = 20.0f * i; 
	model = glm::rotate(model, glm::radians(angle), glm::vec3(1.0f, 0.3f, 0.5f));
	ourShader.setMat4("model", model); 
	glDrawArrays(GL_TRIANGLES, 0, 36); 
}
```
# Camera

The camera/view space can be regarded as the space in which vertex coordinates are transformed as they are seen from the camera's perspective. 
The camera represents the origin of the scene. 

The view matrix transforms all the world coordinates into view coordinates that are relative to the camera's position and direction.
To define a camera the following elements are needed: its position in world space, the direction it's looking at, a vector pointing to the right and a vector pointing upwards from the camera.

In other words, a new coordinate system is created with 3 perpendicular unit axes with the camera's position as the origin.

## `LookAt` Matrix

It is worth noticing that the rotation (left matrix) and translation (right matrix) parts are inverted (transposed and negated respectively) since the entire world has to be rotated and translated in the opposite direction of where the camera should be. 

Using this `LookAt` matrix as a view matrix effectively transforms all the world coordinates to the view space just defined.

In other words, the `LookAt` matrix creates a view matrix that looks at a given target.
- R = right vector
- U = up vector
- D = direction vector
- P = camera's position vector
![[Pasted image 20241201173145.png]]

Using GLM we can create a `LookAt` matrix
```C++
glm::mat4 view; 
view = glm::lookAt(glm::vec3(0.0f, 0.0f, 3.0f), 
				   glm::vec3(0.0f, 0.0f, 0.0f), 
				   glm::vec3(0.0f, 1.0f, 0.0f));
```

Camera Rotation around the scene:
```C++
float radius = 10.0f; 
float camX = static_cast(sin(glfwGetTime()) * radius); 
float camZ = static_cast(cos(glfwGetTime()) * radius); 
view = glm::lookAt(glm::vec3(camX, 0.0f, camZ), 
				   glm::vec3(0.0f, 0.0f, 0.0f), 
				   glm::vec3(0.0f, 1.0f, 0.0f));
```

## Walk Around
Camera Setup
```C++
glm::vec3 cameraPos = glm::vec3(0.0f, 0.0f, 3.0f); 
glm::vec3 cameraFront = glm::vec3(0.0f, 0.0f, -1.0f); 
glm::vec3 cameraUp = glm::vec3(0.0f, 1.0f, 0.0f);

glm::mat4 view = glm::lookAt(cameraPos, 
							 cameraPos + cameraFront, 
							 cameraUp);
```

Input Processing
```C++
void processInput(GLFWwindow* window) 
{ 
	... 
	const float cameraSpeed = 0.05f; 
	if (glfwGetKey(window, GLFW_KEY_W) == GLFW_PRESS)
		cameraPos += cameraSpeed * cameraFront; 
	if (glfwGetKey(window, GLFW_KEY_S) == GLFW_PRESS)
		cameraPos -= cameraSpeed * cameraFront;
	if (glfwGetKey(window, GLFW_KEY_A) == GLFW_PRESS)
		cameraPos -= glm::normalize(glm::cross(cameraFront, cameraUp)) * cameraSpeed; 
	if (glfwGetKey(window, GLFW_KEY_D) == GLFW_PRESS) 
		cameraPos += glm::normalize(glm::cross(cameraFront, cameraUp)) * cameraSpeed; 
}
```
To avoid different speed movements due to processing powers of the machines, we can set up a `deltaTime` variable that stores the time it took to render the last frame.
All velocities can be multiplied with `deltaTime` in this way, the velocity of the camera will be balanced accordingly.
```C++
float deltaTime = 0.0f; 
float lastFrame = 0.0f;

float currentFrame = static_cast(glfwGetTime()); 
deltaTime = currentFrame - lastFrame; 
lastFrame = currentFrame;

float cameraSpeed = static_cast(2.5 * deltaTime);
```
## Look Around

Euler angles: pitch yaw roll
![[Pasted image 20241201175113.png|500]]

For camera system, only yaw and pitch are needed.
- yaw : left - right
- pitch : up - down

To make sure the camera points towards the negative z-axis by default it is necessary to give the yaw a default value of a 90-degree clockwise rotation

```C++
float yaw = -90.0f;
float pitch = 0.0f;

glm::vec3 front;
front.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch)); 
front.y = sin(glm::radians(pitch)); 
front.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch));
```

Mouse Input:
To control the yaw and pitch value, it is possible to use the mouse movements. 
Horizontal mouse movement could affect the yaw
Vertical mouse-movement could affect the pitch

The idea is to store the last frame's mouse positions and calculate in the current frame how much the mouse values changed.

GLFW used to hide and capture mouse
```C++
glfwSetInputMode(window, GLFW_CURSOR, GLFW_CURSOR_DISABLED);
```

To calculate the pitch and yaw values, set up a mouse callback function:
```C++
void mouse_callback(GLFWwindow* window, double xposIn, double yposIn)
```
The `xposIn`, `yposIn` represent the current mouse positions.

After registering the callback using the following function, each time the mouse moves, the `mouse_callback` function is called:
```C++
glfwSetCursorPosCallback(window, mouse_callback);
```

a fly style camera:
- Calculate the mouse's offset since the last frame. 
- Add the offset values to the camera's yaw and pitch values. 
- Add some constraints to the minimum/maximum pitch values. 
- Calculate the direction vector.

1. Initialize to center of application:
```C++
// 800x600 app:
float lastX = 800.0f / 2.0; 
float lastY = 600.0 / 2.0;
```

2. mouse callback function:
```C++
float xoffset = xpos - lastX; 
float yoffset = lastY - ypos; // reversed since y-coordinates go bottom-top 

lastX = xpos; 
lastY = ypos; 

float sensitivity = 0.1f; 
xoffset *= sensitivity; 
yoffset *= sensitivity; 

yaw += xoffset; 
pitch += yoffset;
```

3. constraints are added to avoid weird camera movements
```C++
if (pitch > 89.0f) pitch = 89.0f; 
if (pitch < -89.0f) pitch = -89.0f;
```

4. calculate the actual direction vector
```C++
glm::vec3 front; 
front.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch)); 
front.y = sin(glm::radians(pitch)); 
front.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch)); 
cameraFront = glm::normalize(front);
```

To avoid sudden jumps when exiting the focus of the application:
```C++
if (firstMouse) { 
	lastX = xpos; 
	lastY = ypos; 
	firstMouse = false; 
}
```

## Zoom

To implement a zooming interface, it is possible to manipulate the field of view. To zoom in, the mouse's scroll wheel can be used by registering a new callback function:
```C++
void scroll_callback(GLFWwindow* window, double xoffset, double yoffset) 
{ 
	fov -= (float)yoffset; 
	if (fov < 1.0f) 
		fov = 1.0f; 
	if (fov > 45.0f) 
		fov = 45.0f; 
}

glm::mat4 projection = glm::perspective(glm::radians(fov), (float)SCR_WIDTH / (float)SCR_HEIGHT, 0.1f, 100.0f); 
ourShader.setMat4("projection", projection);

glfwSetScrollCallback(window, scroll_callback);
```