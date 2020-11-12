# OpenGL 学习日志

## 目录

|初阶|进阶|高阶|前沿|其他|
|-|-|-|-|-|
|[配置opengl环境](#配置opengl环境)|[](#)|[](#)|[](#)|[学习资源](#学习资源)|
|[创建窗口](#创建窗口)|[](#)|[](#)|[](#)|[](#)|
|[](#)|[](#)|[](#)|[](#)|[](#)|
|[](#)|[](#)|[](#)|[](#)|[](#)|

## 学习资源

|资源|介绍|
|-|-|
|[LearnOpenGL][1]|全流程的OpenGL学习`C++`|

## 初阶

### 配置opengl环境

[详细教程][2] `windows + VS + GLFW + glad` `汉语` `图文`

### 创建窗口

- [头文件](#引入openGL需要的头文件 "引入openGL需要的头文件") `->` [初始化](#实例化GLFW窗口 "创建主函数，实例化GLFW窗口") `->` [创建窗口](#创建一个窗口对象 "创建一个窗口对象（GLFWwindow对象），保存所有窗口数据") `->` [初始化GLAD](#初始化GLAD "初始化GLAD") `->` [设置视窗](#设置呈现窗口的大小 "设置呈现窗口的大小") `->` [绘制图像](#绘制图像 "绘制图像") `->` [退出](#清理GLFW "清理GLFW，退出") `->` [完整代码](#创建窗口的完整代码 "创建窗口的完整代码")

#### 引入openGL需要的头文件

```C++
#include <glad/glad.h>
#include <GLFW/glfw3.h>
```

#### 实例化GLFW窗口

```C++
int main()
{
    glfwInit();//初始化GLFW，只有这样才能配置窗口的各项参数
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);//设置客户端API的兼容的主要型号为3
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);//设置客户端API的兼容的最低型号为3
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);//指定要为哪个 OpenGL 配置文件创建上下文
    //glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);//指定 OpenGL 上下文是否应与正向兼容，即删除请求版本的 OpenGL 中所有弃用的功能

    return 0;
}
```

说明：

- `glfwWindowHint()`:第一个参数为：需要配置的选项类别。第二个参数为：对应选项的整数值。详细设置查看：[glfwWindowHint()参数的选项和默认值][3]
- [GLFW_OPENGL_PROFILE选项的相关值][4]

#### 创建一个窗口对象

```C++
GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);//要求窗口宽度和高度作为其前两个参数。第三个参数允许我们为窗口创建一个名称.可以忽略最后两个参数。
if (window == NULL)//如果返回NULL，即未能成功创建窗口对象
{
    std::cout << "Failed to create GLFW window" << std::endl;
    glfwTerminate();//清除所有剩余窗口，清空所有分配内容，并将库设置为未初始化状态
    return -1;
}
glfwMakeContextCurrent(window);//告诉 GLFW 将窗口的上下文作为当前线程上的主上下文
```

说明：

- `glfwCreateWindow()`:前三个参数：窗口宽度，窗口高度，窗口名称。第四个参数为：指定该窗口应使用哪个监视器。[glfwCreateWindow()函数详细介绍][5]

#### 初始化GLAD

```C++
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))//传递 GLAD 函数来加载特定于操作系统的 OpenGL 函数指针的地址
{
    std::cout << "Failed to initialize GLAD" << std::endl;
    return -1;
}
```

#### 设置呈现窗口的大小

```C++
glViewport(0, 0, 800, 600);//前两个参数设置窗口左下角的位置。第三个和第四个参数设置渲染窗口的宽度和高度（以像素为单位），我们设置的宽度和高度等于 GLFW 的窗口大小
void framebuffer_size_callback(GLFWwindow* window, int width, int height);  //回调函数原型
void framebuffer_size_callback(GLFWwindow* window, int width, int height)//在窗口上注册一个回调函数，该函数在每次调整窗口大小时都会被调用
{
    glViewport(0, 0, width, height);//更新 OpenGL 视口。
}  
glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);//回调函数在调整帧缓冲区大小时收到帧缓冲区的新大小，该大小可用于framebuffer_size_callback()函数进而更新 OpenGL 视口。
```

#### 绘制图像

```C++
while(!glfwWindowShouldClose(window))//只有当明确指示停止循环时，循环才会停止
{
    glfwSwapBuffers(window);//交换颜色缓冲区,用于在此渲染迭代时间内进行渲染，并将它输出到屏幕
    glfwPollEvents();//检查是否触发了任何事件（如键盘输入或鼠标移动事件），更新窗口状态，并调用相应的函数（我们可以通过回调方法注册）。
}
```

#### 清理GLFW

```C++
glfwTerminate();//清除所有剩余窗口，清空所有分配内容，并将库设置为未初始化状态
return 0;
```

#### [创建窗口的完整代码][6]

### 创建三角形

|概念|中译|简单介绍|
|-|-|-|
|[graphics pipeline][7]|图形管线|将 3D 坐标转换为 2D 坐标，将 2D 坐标转换为实际彩色像素的地方|
|[shaders][8]|着色器|将一组 3D 坐标作为输入，并将这些坐标转换为屏幕上的彩色 2D 像素的小程序|
|[OpenGL Shading Language (GLSL)][9]|OpenGL 着色器语言|顾名思义|
|[vertex shader][11]|顶点着色器|将 3D 坐标转换为不同的 3D 坐标|
|[geometry shader][12]|几何着色器|将形成三角形的顶点集合作为输入，并且能够通过发射新的顶点来生成其他形状以形成新的三角形|
|[rasterization stage][13]|栅格化阶段|将生成的三角形映射到最终屏幕上的相应像素，从而生成片段着色器使用的片段|
|[fragment shader][14]|片段着色器|计算像素的最终颜色|
|[alpha test and blending][15]|Alpha 测试和混合|检查片段的相应深度值(使用这些值检查生成的片段是否位于其他对象的前面或后面，并相应地丢弃)和Alpha值，并相应地混合对象|

## 进阶

## 高阶

## 前沿

[1]:https://learnopengl.com/Introduction
[2]:https://blog.csdn.net/sigmarising/article/details/80470054?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param
[3]:http://www.glfw.org/docs/latest/window.html#window_hints_values
[4]:http://www.glfw.org/docs/latest/window.html#GLFW_OPENGL_PROFILE_hint
[5]:http://www.glfw.org/docs/latest/window.html#window_creation
[6]:https://learnopengl.com/code_viewer_gh.php?code=src/1.getting_started/1.1.hello_window/hello_window.cpp
[7]:https://www.cnblogs.com/leixinyue/p/11076220.html
[8]:https://baike.baidu.com/item/%E7%9D%80%E8%89%B2%E5%99%A8/411001?fr=aladdin
[9]:https://www.cnblogs.com/brainworld/p/7445290.html
[11]:https://baike.baidu.com/item/%E9%A1%B6%E7%82%B9%E7%9D%80%E8%89%B2%E5%99%A8/4104625?fr=aladdin
[12]:https://www.cnblogs.com/chen9510/p/11459431.html
[13]:https://baike.baidu.com/item/%E6%A0%85%E6%A0%BC%E5%8C%96/1180810
[14]:https://cloud.tencent.com/developer/article/1338956
[15]:https://blog.csdn.net/blues1021/article/details/47177693
[10]:
[16]: