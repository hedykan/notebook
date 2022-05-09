# 创建窗口
## 初始化
在导入完毕GLFW库和GLAD库后，便可以开始建立渲染画面了。  
```c
    // glfw初始化
    glfwInit();
    // 设定glfw版本为3.3 和核心模式
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
```
这将初始化glfw，并设定使用的openGL版本为3.3，并设置为核心模式。  
## 创建窗口
之后通过`glfwCreateWindow`函数创建一个窗口指针，并绑定到OpenGL的主上下文中，后续通过这个指针进行该窗口的操作。  
```c
    // 窗口为800*600，窗口名为LearnOpenGL，后两位先忽略
    GLFWwindow *window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);
    // 检测是否获取到窗口指针
    if (window == NULL)
    {
        std::cout << "failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    // 设置为主上下文
    glfwMakeContextCurrent(window);
```
## 初始化GLAD
通过初始化GLAD库，使用包装好的简单的OpenGL函数。  
```c
    // 初始化glad
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }

    // 设置视口大小，这是包装好的glad函数
    glViewport(0, 0, 800, 600);
```

## 建立渲染循环
以上都初始化后，就可以开始渲染画面了。  
```c
    // 检测各种退出信号
    while (!glfwWindowShouldClose(window))
    {
        // 自己编写的输入处理函数
        processInput(window);
        // 双缓冲函数
        glfwSwapBuffers(window);
        glfwPollEvents();
    }
    
    void processInput(GLFWwindow *window) {
        // 检测获取到的按键是否复合
        if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS) {
            glfwSetWindowShouldClose(window, true);
        }
        if(glfwGetKey(window, GLFW_KEY_A) == GLFW_PRESS) {
            glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
            glClear(GL_COLOR_BUFFER_BIT);
        }
        if(glfwGetKey(window, GLFW_KEY_B) == GLFW_PRESS) {
            glClearColor(0.3f, 0.4f, 0.4f, 1.0f);
            glClear(GL_COLOR_BUFFER_BIT);
        }
    }
```
## 清除并退出
通过关闭函数退出渲染循环后，自然需要清理和归还内存，使用`glfwTerminate`函数清除。  
```c
    // 清除函数
    glfwTerminate();
    return 0;
```