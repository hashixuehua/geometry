# 2.Hello Widget
## 2.1.前提
搭建环境，包括：

* `Visual Studio`: 开发、调试等；
* `QT`： 界面开发，使用QT封装的OpenGL；
* `vcpkg`：包安装管理，环境配置；
* `CMAKE`等；

## 2.2.Hello Widget
1. 使用`QT Creator`创建项目；
2. 配置`cmakelists`：

```c++
   （1）find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets OpenGLWidgets)
   （2）target_link_libraries(GLViewer PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt6::OpenGLWidgets)
```

<img src="../img/cad/image.png" alt="cmake find package" title="cmake find package" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：cmake find package</figcaption>

<img src="../img/cad/image-1.png" alt="cmake link lib" title="cmake link lib" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：cmake link lib</figcaption>

3. 添加Widget类GLView，并配置
<img src="../img/cad/image-2.png" alt="header file" title="header file" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：header file</figcaption>

<img src="../img/cad/image-3.png" alt="source file" title="source file" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：source file</figcaption>

```c++
   #include <QOpenGLWidget>
   #include <QOpenGLFunctions_4_5_Core>
```

4. 创建OpenGL Widget窗口，并将其类提升为新创建的 GLView；
<img src="../img/cad/image-5.png" alt="promote to" title="promote to" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：promote to</figcaption>

5. 创建状态栏
<img src="../img/cad/image-6.png" alt="create status bar" title="create status bar" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：create status bar</figcaption>

6. 在`GLWindow`构造函数中将`GLView`设置为中心窗体；

```c++
GLWindow::GLWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::GLWindow)
{
    ui->setupUi(this);
    setCentralWidget(ui->openGLWidget);
}
```

7. 重写绘制相关虚函数
<img src="../img/cad/image-4.png" alt="overriding" title="overriding" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：overriding</figcaption>

```C++
void GLView::initializeGL()
{
    initializeOpenGLFunctions();
}
void GLView::resizeGL(int w, int h)
{
}
void GLView::paintGL()
{
    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);
}
```

8. 编译，运行

<img src="../img/cad/image-hello widget.png" alt="overriding" title="overriding" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">Widget</figcaption>

!!! attention
    **提示：** `#include <glview.h>`可能会报错，这是编译器一般的处理机制，为了更高效的搜寻标准库和第三方库中的头文件，在本项目中`glview.h`是项目文件，用尖括号包含的话会以标准库和第三方库的方式是寻找，可能找不到，如果想避免这个错误，可以在`cmakelists`中添加如下代码指定头文件搜索路径：

```js
target_include_directories(GLViewer PRIVATE .)
```