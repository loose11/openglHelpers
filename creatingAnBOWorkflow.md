#Creating an standard BO workflow in OpenGL

##Create a Vertex Array Object (VAO)
The Vertex Array Object is an array, which discribes the pattern of your datasource. It says OpenGL how your e.g data array are structured.

```
GLuint vao;
glGenVertexArrays(1, &vao);  // Create an object
glBindVertexArray(vao);		  // Bind it to the OpenGL Context
```

##Create a Buffer Array Object (VBO)
The Buffer Array Object holds mostly the whole object data. For example:

```
GLfloat vertices[] = {
  //Position	 Color
   -0.5f,  0.5f, 1.0f, 0.0f, 0.0f,	
	0.5f,  0.5f, 0.0f, 1.0f, 0.0f,
    0.5f, -0.5f, 0.0f, 0.0f, 1.0f,
   -0.5f, -0.5f, 1.0f, 1.0f, 1.0f
}
```

You can see, every 2 floats are the position of the vertex and the following 3 floats are the  color for this vertice (RGB-Color).

Here we are to generate an VBO

```
GLunit vbo; 
glGenBuffers(1, &vbo);	// Create an VB Object
glBindBuffer(GL_ARRAY_BUFFER, vbo);	// Bind it to the ARRAY_BUFFER
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW); // Fill it

```

##Create the shaders
###Write down the vertex shader

```
const GLchar* vertexSource =
    "#version 400 core\n"
    "in vec2 position;"
    "in vec3 color;"
    "out vec3 Color;"
    "void main() {"
    "   Color = color;"
    "   gl_Position = vec4(position, 0.0, 1.0);"
    "}";
```
###Write down the fragment shader
```
const GLchar* fragmentSource =
    "#version 400 core\n"
    "in vec3 Color;"
    "out vec4 outColor;"
    "void main() {"
    "   outColor = vec4(Color, 1.0);"
    "}";
```

###After all we have to compile them
```
	// Create and compile the vertex shader
    GLuint vertexShader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertexShader, 1, &vertexSource, NULL);
    glCompileShader(vertexShader);
 
    // Create and compile the fragment shader
    GLuint fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragmentShader, 1, &fragmentSource, NULL);
    glCompileShader(fragmentShader);
 
    // Link the vertex and fragment shader into a shader program
    GLuint shaderProgram = glCreateProgram();
    glAttachShader(shaderProgram, vertexShader);
    glAttachShader(shaderProgram, fragmentShader);
    glLinkProgram(shaderProgram);
    glUseProgram(shaderProgram);
```

###Now we are ready to say our vertex array object the data structure
```
   // Specify the layout of the vertex data
    GLint posAttrib = glGetAttribLocation(shaderProgram, "position");
    glEnableVertexAttribArray(posAttrib);
    glVertexAttribPointer(posAttrib, 2, GL_FLOAT, GL_FALSE, 7 * sizeof(GLfloat), 0);
 
    GLint colAttrib = glGetAttribLocation(shaderProgram, "color");
    glEnableVertexAttribArray(colAttrib);
    glVertexAttribPointer(colAttrib, 3, GL_FLOAT, GL_FALSE, 7 * sizeof(GLfloat), (void*)(2 * sizeof(GLfloat)));
 
```


##Use all in your main loop

Clear your window with

```
glClear(GL_COLOR_BUFFER_BIT| GL_DEPTH_BUFFER_BIT);
```

Futher more say which shader you are using

```
glUseProgram(shaderProgram);
```

Bind vao to the context

```
glBindVertexArray(vao);
```
Draw your data

```
glDrawArrays(GL_TRIANGLES, 0, 3);
```

***After all flush the memory***




