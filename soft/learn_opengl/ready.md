# 环境构建准备

## GLFW库的构建
我已经重新构建这个库三次了，在此记录下使用cmake构建mingw版本GLFW库的方法。  
[具体教程地址](https://learnopengl-cn.github.io/01%20Getting%20started/02%20Creating%20a%20window/)  
[CMake下载地址](https://cmake.org/download/)  
CMake是一个跨平台的、开源的构建工具。使用它的目的是通过执行CMakeList生成目标平台的makefile，减少编写makefile的时间，以达到跨平台的效果。  
[GLFW下载地址](https://www.glfw.org/download.html)  
下载完毕后，首先安装cmake。  
安装完cmake后，解压GLFW压缩包到任意指定位置。  
在解压的文件夹中，可以新建一个build文件夹，防止源文件被污染，虽然这一步不是必要的。  
打开cmake，在源文件夹选择刚刚解压的文件夹。  
在构建文件夹选择中选择刚刚建立的build文件夹。  
然后点击configure设置，如果是mingw32的话，选择MinGW32 makefile，注意此时你的make应用程序应叫做`mingw32-make`，否则会出现找不到make应用程序的情况。  
如果正常，将会出现一个设置页面，勾选BUILD_SHARE_LIBS选项，这将会编辑出动态库`libglfw3dll.a glfw3.dll`，不勾选的话则会编译出静态库`libglfw3.a`。  
构建完成后，将会在build中出现一堆文件，通过命令行在build文件夹下执行make命令，然后将在`build/src`中出现根据当前系统编译的`glfw3.dll libglfw3.a libglfw3dll.a`三个文件， 如果上一步没有勾选BUILD_SHARE_LIBS选项，这一步中会出现`libglfw3.a`一个文件，否则出现的将是`libglfw3.a libglfw3dll.a`。  
将生成的文件分别放入mingw32中的`lib/`文件夹中。  
回到glfw3目录，这里有个`include/`文件夹，里面包含一个`GLFW/`文件夹，把他放到mingw32的`include/`文件夹中即可。  
记得在编译的时候添加编译参数`-lglfw3`或`-lglfw3dll`，分别为静态链接和动态链接。  
记得一步一步操作，如果出错了，建议删除文件夹或者重新建立一个文件夹来重新操作和编译，多试几次就好了。 
## TODO
1. 当前使用静态编译会出问题，无法找到指定函数   
    已解决，[官方文档](https://www.glfw.org/docs/latest/build_guide.html#build_link_win32)表示如果要使用静态方式编译需要多链接一个`gdi32`库，在编译函数中添加`-lgdi32`解决
    ```
    官方原文
    The static version of the GLFW library is named glfw3. When using this version, it is also necessary to link with some libraries that GLFW uses.

    When using MinGW to link an application with the static version of GLFW, you must also explicitly link with gdi32. Other toolchains including MinGW-w64 include it in the set of default libraries along with other dependencies like user32 and kernel32.
    ```

## GLAD库的构建
[GLAD下载地址](https://glad.dav1d.de/)  
GLAD, Api中gl选择3.3版本，保持Generate a loader被勾选，然后Generate。  
GLAD库的安装就很简单了。  
一般你按照上面的选择下载了压缩包后，压缩包里就有相应的include文件夹，这个文件夹里包含`glad/ KHR/`这两个文件夹。  
放到mingw32的include文件夹后就好。  
另一个src文件夹中的`glad.c`文件，放到自己项目的src中就好，编译的时候先编译它，最后一起链接，就没问题了。 

## stb_image库构建
直接在需要使用的源文件夹中导入stb_image.h  
```c
// 注意要定义STB_IMAGE_IMPLEMENTATION，不然会出现未定义的情况，也要注意不要重定义
#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"
```

## 编译环境构建
其实就是写好makefile，确定编译链接的顺序啦。  
按照c编译的传统，首先编译出.o文件，然后再链接.o文件生成目标程序。  
示例如下
```makefile
# 这里使用的是glfw3的动态库glfw3.dll，需要在你的源文件里添加glfw3.dll，如果缺少这个动态库将会报错
# COMP = -lglfw3dll -lopengl32 --std=c++11
# 静态库编译参数
COMP = -lglfw3 -lgdi32 -lopengl32 --std=c++11 
OBJ_FILE = main.o glad.o
main.exe: $(OBJ_FILE)
	g++ $(OBJ_FILE) $(COMP) -o main.exe -g
main.o: main.c
	g++ main.c -c -g
glad.o: glad.c
	g++ glad.c -c -g

clean:
	rm *.o
```