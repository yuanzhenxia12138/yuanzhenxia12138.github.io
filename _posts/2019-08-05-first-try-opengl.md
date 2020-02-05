---
layout: post
title:  "Use OpenGL to draw a triangle"
date:   2019-08-11 00:50:54
categories: OpenGL technical
tags: OpenGL technical
---

* content
{:toc}
It is my first time to use OpenGL and I need to use OpenGL to draw an OBJ file which represents a 3D model. However, I never used OpenGL before. So, I need to learn how to draw a simple tirangle by using OpenGL first.



## References 
https://www.cnblogs.com/llllllvty/p/10206360.html

## How OpenGL works?

### VAO & VBO
In OpenGL everything is in 3D space, but the screen and window are a 2D array of pixels so a large part of OpenGL's work is about transforming all 3D coordinates to 2D pixels that fit on your screen.

![d](https://github.com/sunliancheng/image/blob/master/opengl/1520159-20190101223606976-674085124.png?raw=true)

The picture represents the processes how OpenGL works and we can handle parts of shader in the processes whose backgrounds are blue.

The first process is called "vertex input" and in this experiment, we will new a triangle and save it as an OBJ file.



![d](https://github.com/sunliancheng/image/blob/master/opengl/1520159-20190102161858876-1984362958.png?raw=true)

The C++ code is as following.

		float vertices[]{
		  -0.5f, -0.5f,0.0f,
		  0.5f,-0.5f,0.0f,
		  0.0f,0.5f,0.0f
		};



Then, CPU will give the array which contains the OBJ file to GPU and GPU uses VBO(vertex buffer objects) to storage many vertices in video memory. Now, the VBO object is the first OpenGL object that we can contact and we need to new a VBO object by the following code.

		unsigned int VBO;
		glGenBuffers(1, &VBO);

Also, we can generate by this way.

		unsigned int VBO[n];
		glGenBuffers(n, VBO);
		

OpenGL has many types of buffer objects and the type of VBO is called "GL_ARRAY_BUFFER". Now, we should bind the new generated VBO to the target of GL_ARRAY_BUFFER as it is showed in the following code.

		glBindBuffer(GL_ARRAY_BUFFER, VBO);


After binding, we can use glBufferData function to copy the vertex data into buffer memory as following.

		glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW)

The fourth parameter is telling GPU how to deal with the data and the parameter has three types:

GL_STATIC_DRAW:  Almost no change in the data.

GL_DYNAMIC_DRAW: The data changes frequently.

GL_STREAM_DRAW:  The data will change every drawing.


Now, we need to use VAO(vertex array objects) to tell GPU which part of the OBJ file represents 3D coordinates and which part represents UV. And a VAO can storage the following things:

glEnableVertexAttribArray or glDisableVertexAttribArray

glVertexAttribPoints

glVertexAttribPointer

Every VAO has a list of points attributes and each of them storage the position where the attribute is. To be honest, I do not understand this very much.

![d](https://github.com/sunliancheng/image/blob/master/opengl/1520159-20190103203236610-2123426448.png?raw=true)



The correct order is like that:

		unsigned int VAO;
		glGenVertexArrays(1, &VAO);
		glBindVertexArray(VAO);
		
		unsigned int VBO;
		glGenBuffers(1, &VBO);
		glBindBuffer(GL_ARRAY_BUFFER, VBO);
		glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);


### Vertex shader

Here is a Vertex shader and we just use it.

		const char* vertexShaderSource =
		"#version 330 core                                          \n"
		"layout (location = 0) in vec3 aPos;                      \n"
		"void main(){                                             \n"
		"    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);}    \n";


Now, we need to compile the shader.

		unsigned int vertexShader;
		vertexShader = glCreateShader(GL_VERTEX_SHADER);
		
		glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);
		glCompileShader(vertexShader);


### Fragment Shader


Here is a fragment Shader:

		const char* fragmentShaderSource =
		"#version 330 core                              \n "
		"out vec4 FragColor;                            \n "
		"void main(){                                   \n "
		"    FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);} \n ";

The array of the four elements contains red, green, blue and alpha and each of them is between 0.0 to 1.0.

Now, compile fragment shader.

		unsigned int fragmentShader;
		fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
		glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
		glCompileShader(fragmentShader);


### Shader Program

Now, we need to link all the compiled shaders in a shaer program object and activate it when rendering.

Create a new shader program:

		unsigned int shaderProgram;
		shaderProgram = glCreateProgram();
		
		glAttachShader(shaderProgram, vertexShader);
		glAttachShader(shaderProgram,fragmentShader);
		glLinkProgram(shaderProgram);

		glUseProgram(shaderProgram);
		
### Linking Vertex Attributes

Now, we have to define that which part of the vertex data represents the part of vertex shader.

Our vertex buffer data will be resolved like the following.

![d](https://github.com/sunliancheng/image/blob/master/opengl/1520159-20190103234141958-1831009713.png?raw=true)


Through this, we can use glVertexAttribPointer function to tell OpenGL how to resolve Vertex data.

		glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
		glEnableVertexAttribArray(0);  

*The first parameter represents our vertex attributes. We defined layout(location=0) as the vertex's Location in vertex shader. We hope to transfer vertex data in the pointer attribute and therefore, we set it as 0.

*The second parameter represents the size of vertex attribute.

*The fourth parameter represents whether should we standardize parameter(from 0 to 1).

*The fifth parameter is called 'stride'.


### Drawing object
Finally, the glDrawArrays function provided by OpenGL can draw Primitive.

		glUseProgram(shaderProgram);
		glBindVertexArray(VAO);
		glDrawArrays(GL_TRIANGLES, 0, 3);

The second parameter is the start index of vertex data and the third parameter is telling GPU how many points need we draw.


## Thinking

I still know a little about OpenGL.

Recently, I know I have little time to prepare for my GRE & TOEFL test as I need to participate in my GRE test in 09/20/2019. I need to work hard. Come on!!!

I think the way that I learn to code by writing blog is useful as it helps me concentrate on my current work.
