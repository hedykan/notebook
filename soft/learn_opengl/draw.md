# 作图
有了着色器程序后，我们就可以开始朝gpu传输数据，并且要求其开始作图了。  
## 向gpu上传顶点数据
首先，定义在坐标系中要作画的顶点。
```c
    // 定义顶点数组
    float vertices[] = {
        0.5f, 0.5f, 0.0f,   // 右上
        0.5f, -0.5f, 0.0f,  // 右下
        -0.5f, -0.5f, 0.0f, // 左下
        -0.5f, 0.5f, 0.0f   // 左上
    };
```
由于每次作画可能用到同一个顶点，如果重复定义这个顶点将会提高开销，所以可以使用顶点数组索引去指定当前所需使用的顶点。
```c
    unsigned int indices[] = {
        // note that we start from 0!
        0, 1, 3, // first Triangle
        1, 2, 0  // second Triangle
    };
```
有了数据后，就需要将顶点数组对象，顶点数组缓冲区对象，索引数组缓冲区对象，创建绑定到gpu中去了。
```c
    // unsigned int VAO, VBO, EBO
    glGenVertexArrays(1, VAO);
    glGenBuffers(1, VBO);
    glGenBuffers(1, EBO);

    // 绑定顶点数组
    glBindVertexArray(*VAO);
    // 绑定顶点缓冲区数组，设置为顶点缓冲区参数
    glBindBuffer(GL_ARRAY_BUFFER, *VBO);
    // 传入顶点缓冲区数组，定义顶点缓冲区长度，设置为静态作画
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
    // 绑定索引缓冲区数组，设置为索引缓冲区参数
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, *EBO);
    // 传入索引缓冲区数组，定义索引缓冲区长度，设置为静态作画
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```
上传后，要指定解析方式，使OpenGL懂得如何使用了传输上来的顶点数据和索引数据。
```c
    // 设置顶点解析参数
    // 参数1：顶点着色器中设置layout(location = 0)定义了position的位置，我们希望数据传入到这个顶点属性，所以设置为0
    // 参数2：指定顶点属性的大小。顶点属性是一个vec3，是三单位向量，所以大小是3
    // 参数3：指定数据的类型
    // 参数4：数据是否被标准化，为GL_TRUE则会标准化映射到0-1之间
    // 参数5：参数被叫做步长，它告诉OpenGL在连续顶点之间的间隔，我们有三个顶点做三角形，所以顶点步长为3 * sizeof(float)
    // 参数6：数组的其实位置，我们定为开头0
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void *)0);
    // 启用位置0的属性，否则将不可见，即无法使用
    glEnableVertexAttribArray(0);

    glBindBuffer(GL_ARRAY_BUFFER, 0);
    glBindVertexArray(0);
```
## 开始作图
设置完成后，就可以开始画图了，在渲染循环中，可以开始进行作画了。
```c
    // 渲染循环中
    glUseProgram(shaderProgram);
    // 使用
    glBindVertexArray(VAO);
    // 开始画图，绘制三角形，共六个顶点
    glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```
推出后清除gpu数据。
```c
    glDeleteVertexArrays(1, &VAO);
    glDeleteBuffers(1, &VBO);
    glDeleteBuffers(1, &EBO);
    glDeleteProgram(shaderProgram);
```