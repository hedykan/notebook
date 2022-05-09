# 创建着色器

## 顶点着色器
首先得先编译着色器中间文件。  
```c
const char *vertexShaderSource = "#version 330 core\n"
                                 "layout (location = 0) in vec3 aPos;\n"
                                 "void main()\n"
                                 "{\n"
                                 "   gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);\n"
                                 "}\0";

// 编译顶点着色器
unsigned int vertexShaderCreate()
{
    // 获取顶点着色器id
    unsigned int vertexShader = glCreateShader(GL_VERTEX_SHADER);
    // 导入顶点着色器的源代码
    glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);
    // 编译顶点着色器
    glCompileShader(vertexShader);
    return vertexShader;
}
```
## 片段着色器
```c
const char *fragmentShaderSource = "#version 330 core\n"
                                   "out vec4 FragColor;\n"
                                   "void main()\n"
                                   "{\n"
                                   "   FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"
                                   "}\n\0";

// 编译片段着色器
unsigned int fragmentShaderCreate()
{
    // 获取片段着色器id
    unsigned int fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
    // 导入片段着色器源代码
    glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
    // 编译片段着色器
    glCompileShader(fragmentShader);
    return fragmentShader;
}
```

## 链接着色器程序
有了顶点着色器文件和片段着色器文件后，需要对他们进行链接，然后生成着色器程序。  
```c
// 链接着色器
unsigned int shaderLink()
{
    // 获取编译错误
    int success;
    char infoLog[512];
    // 获取顶点/片段着色器id
    unsigned int vertexShader = vertexShaderCreate();
    glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);
    if (!success)
    {
        glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);
        std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n"
                  << infoLog << std::endl;
    }
    unsigned int fragmentShader = fragmentShaderCreate();
    glGetShaderiv(fragmentShader, GL_COMPILE_STATUS, &success);
    if (!success)
    {
        glGetShaderInfoLog(fragmentShader, 512, NULL, infoLog);
        std::cout << "ERROR::SHADER::FRAGMENT::COMPILATION_FAILED\n"
                  << infoLog << std::endl;
    }
    // 获取着色器程序id
    unsigned int shaderProgram = glCreateProgram();
    glAttachShader(shaderProgram, vertexShader);
    glAttachShader(shaderProgram, fragmentShader);
    // 链接程序
    glLinkProgram(shaderProgram);
    glDeleteShader(vertexShader);
    glDeleteShader(fragmentShader);
    return shaderProgram;
}
```
## 使用外部文件作为着色器程序输入
```c
// 着色器代码获取函数
char *getShaderStr(char *fileAddr)
{
    FILE *fp;
    if ((fp = fopen(fileAddr, "rb")) == NULL)
    {
        return NULL;
    }
    // 移动到末尾，获取文件长度
    fseek(fp, 0, SEEK_END);
    int fileLen = ftell(fp);
    // 移动到队头，准备获取全文
    fseek(fp, 0, SEEK_SET);
    char *tmp = (char *)malloc(sizeof(char) * fileLen + 1);
    tmp[fileLen] = '\0'; // 如果不加EOF，可能会出现数组越界
    fread(tmp, fileLen, sizeof(char), fp);
    fclose(fp);
    int i;
    // for (i = 0; i < fileLen; ++i)
    // {
    //     printf("%c", tmp[i]);
    // }

    return tmp;
}
```
## 顶点着色器与片段着色器的使用方法
```c
// 顶点着色器
#version 330 core
// 使用location传入数据
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aColor;

// 使用uniform全局变量传入数据
uniform vec3 ourPos;

void main()
{
    gl_Position = vec4(aPos + ourPos, 1.0);
}

// 片段着色器
#version 330 core
out vec4 FragColor;

uniform vec4 ourColor;

void main()
{
    FragColor = ourColor;
}

// 将数据输入到uniform的方法
glUniform1f(glGetUniformLocation(shaderProgram, "uniformName"), 1.0f);
```