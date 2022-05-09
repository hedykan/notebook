# 纹理

## 需要先引入stb_image库
具体引入查看[构建准备](./ready.md)

## 创建纹理
通过函数创建纹理
```c
unsigned int createTexture(char *addr)
{
    int width, height, nrChannels;
    stbi_set_flip_vertically_on_load(1);
    unsigned char *data = stbi_load(addr, &width, &height, &nrChannels, 0);
    // 创建纹理id
    unsigned int texture;
    glGenTextures(1, &texture);
    // 绑定纹理id，绑定值为2d纹理
    glBindTexture(GL_TEXTURE_2D, texture);

    // 为当前绑定的纹理对象设置环绕、过滤方式
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

    // 输入图片生成纹理
    // 参数1：指定纹理目标，我们之前绑定的是GL_TEXTURE_2D
    // 参数2：为纹理指定多级渐远纹理的级别，设为0，基本级别
    // 参数3：告诉OpenGL将纹理存储为何种格式，由于图像只有rgb值，所以存为GL_RGB
    // 参数5，6：宽度和高度
    // 参数7：历史遗留问题，总应该设为0
    // 参数8，9：定义了原图格式和数据类型
    // 参数10：传入数据
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
    // 生成多级渐远图像
    glGenerateMipmap(GL_TEXTURE_2D);
    stbi_image_free(data);
    return texture;
}
```

## 使用纹理
```c
    // 创建纹理
    unsigned int texture1 = createTexture("data/girl.jpg");
    unsigned int texture2 = createTexture("data/wall.jpg");
    glUseProgram(shaderProgram);
    // 绑定到uniform中，分别设定到纹理单元0和1
    glUniform1i(glGetUniformLocation(shaderProgram, "texture1"), 0);
    glUniform1i(glGetUniformLocation(shaderProgram, "texture2"), 1);

    // 渲染循环
    // 在绑定纹理之前先激活纹理单元0
    glActiveTexture(GL_TEXTURE0);
    glBindTexture(GL_TEXTURE_2D, texture1);
    // 在绑定纹理之前先激活纹理单元1
    glActiveTexture(GL_TEXTURE1);
    glBindTexture(GL_TEXTURE_2D, texture2);
```