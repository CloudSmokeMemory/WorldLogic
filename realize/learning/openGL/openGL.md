# OpenGL 学习日志

目录

|初阶|进阶|高阶|前沿|其他|
|-|-|-|-|-|
|[配置opengl环境](#配置opengl环境)|[](#)|[](#)|[](#)|[学习资源](#学习资源)|
|[](#)|[](#)|[](#)|[](#)|[](#)|

## 学习资源

|资源|介绍|
|-|-|
|[LearnOpenGL][1]|全流程的OpenGL学习`C++`|

## 初阶

### 配置opengl环境

[详细教程][2] `windows + VS + GLFW + glad` `汉语` `图文`

### 创建窗口

流程：

- [引入openGL需要的头文件](#引入openGL需要的头文件) `->` [创建主函数，实例化GLFW窗口](#创建主函数，实例化GLFW窗口) `->` [创建一个窗口对象(GLFWwindow对象)，保存所有窗口数据](#创建一个窗口对象(GLFWwindow对象)，保存所有窗口数据) `->` [初始化GLAD](#初始化GLAD) `->` [设置呈现窗口的大小](#设置呈现窗口的大小) `->` [绘制图像](#绘制图像) `->` [清理GLFW，退出](#清理GLFW，退出) `->` [创建窗口的完整代码](#创建窗口的完整代码)

1. #### 引入openGL需要的头文件

    ```C++
    #include <glad/glad.h>
    #include <GLFW/glfw3.h>
    ```

2. #### 创建主函数，实例化GLFW窗口

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
    `glfwWindowHint()`:第一个参数为：需要配置的选项类别。第二个参数为：对应选项的整数值。详细设置查看：[glfwWindowHint()参数的选项和默认值][3]
    - [GLFW_OPENGL_PROFILE选项的相关值][4]

3. #### 创建一个窗口对象(GLFWwindow对象)，保存所有窗口数据

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
    `glfwCreateWindow()`:前三个参数：窗口宽度，窗口高度，窗口名称。第四个参数为：指定该窗口应使用哪个监视器。[glfwCreateWindow()函数详细介绍][5]

4. #### 初始化GLAD

    ```C++
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))//传递 GLAD 函数来加载特定于操作系统的 OpenGL 函数指针的地址
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }
    ```

5. #### 设置呈现窗口的大小

    ```C++
    glViewport(0, 0, 800, 600);//前两个参数设置窗口左下角的位置。第三个和第四个参数设置渲染窗口的宽度和高度（以像素为单位），我们设置的宽度和高度等于 GLFW 的窗口大小
    void framebuffer_size_callback(GLFWwindow* window, int width, int height);  //回调函数原型
    void framebuffer_size_callback(GLFWwindow* window, int width, int height)//在窗口上注册一个回调函数，该函数在每次调整窗口大小时都会被调用
    {
        glViewport(0, 0, width, height);//更新 OpenGL 视口。
    }  
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);//回调函数在调整帧缓冲区大小时收到帧缓冲区的新大小，该大小可用于framebuffer_size_callback()函数进而更新 OpenGL 视口。
    ```

6. #### 绘制图像

    ```C++
    while(!glfwWindowShouldClose(window))//只有当明确指示停止循环时，循环才会停止
    {
        glfwSwapBuffers(window);//交换颜色缓冲区,用于在此渲染迭代时间内进行渲染，并将它输出到屏幕
        glfwPollEvents();//检查是否触发了任何事件（如键盘输入或鼠标移动事件），更新窗口状态，并调用相应的函数（我们可以通过回调方法注册）。
    }
    ```

7. #### 清理GLFW，退出

    ```C++
    glfwTerminate();//清除所有剩余窗口，清空所有分配内容，并将库设置为未初始化状态
    return 0;
    ```

- #### [创建窗口的完整代码][6]

## 进阶

## 高阶

## 前沿

[1]:https://learnopengl.com/Introduction
[2]:https://blog.csdn.net/sigmarising/article/details/80470054?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param
[3]:http://www.glfw.org/docs/latest/window.html#window_hints_values
[4]:http://www.glfw.org/docs/latest/window.html#GLFW_OPENGL_PROFILE_hint
[5]:http://www.glfw.org/docs/latest/window.html#window_creation
[6]:https://learnopengl.com/code_viewer_gh.php?code=src/1.getting_started/1.1.hello_window/hello_window.cpp