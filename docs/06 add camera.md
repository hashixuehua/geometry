# 6.相机：引入`Camera`类
## 6.1.引入CGLib库
* 下载cglib：[cglib.net](cglib.net/intro/)
* 课程目录下新建`dependencies`文件夹，将cglib库放进去；注意在`.gitignore`文件中添加如下两行，以忽略依赖库文件；

```js
#dependencies
dependencies/
```

* 在`src`目录下的`cmakelists`文件中的`find_package`后添加如下内容以使用CGLib库，

```js
#CGLib
set(CGLib "${CMAKE_SOURCE_DIR}/../../dependencies/CGLib")
if(WIN32)
    include_directories(${CGLib}/include)testFunc/lib)
    link_directories(${CGLib}/bin/win64/Release)
    link_libraries(CGLib.lib)
endif()
```

* 还有一件事，迟早得做，现在做了吧，把CGLib的dll和pdb文件拷贝到本项目的输出目录中（GLViewer.exe所在目录），默认是这里`out\build\x64-RelWithDebInfo\src`，如果没有更改的话。

!!! important
    建议用`ReleaseWithDebugInfo`配置进行开发和调试。由于`CGLib`是`Release`下编译的，对外接口的参数有`STL`容器，这样在程序中用`Debug`配置时运行就会出现报错。当然后续会考虑封装参数来避免此问题。

<img src="../img/cad/releaseWithDebugInfo.png" alt="建议使用releaseWithDebugInfo配置开发和调试" title="切换到cmake项目视图" width="700" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：建议使用releaseWithDebugInfo配置开发和调试</figcaption>

## 6.2.添加ViewerSetting类

获取当前客户端**设备像素比**，并进行适配。

!!! note "补充："
    `设备像素比（devicePixelRatio）`：指当前显示设备的物理像素分辨率与CSS像素分辨率之比。这个比值可以理解为像素大小的比率：一个CSS像素的大小与一个物理像素的大小。简单来说，它告诉浏览器应使用多少屏幕实际像素来绘制单个CSS像素。

* 在VS2022中切换到cmake项目视图，
<img src="../img/cad/image-20.png" alt="切换到cmake项目视图" title="切换到cmake项目视图" width="300" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：切换到cmake项目视图</figcaption>

<img src="../img/cad/image-21.png" alt="新建类" title="新建类" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：新建类</figcaption>

在弹出`建议的cmake脚本更改建议`窗体，我们取消即可，认真的同学应该明白个中缘由~
<img src="../img/cad/image-24.png" alt="取消对cmakelists的修改" title="取消对cmakelists的修改" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：取消对cmakelists的修改</figcaption>

如果项目视图的src下没有出现新添加的`ViewerSetting`代码文件，可能因为cmake配置需要更新，点击`配置缓存`或`删除缓存并重新配置`,
<img src="../img/cad/image-23.png" alt="配置cmake缓存" title="配置cmake缓存" width="300" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：配置cmake缓存</figcaption>

在`ViewerSetting.h`中添加代码，并记得在cpp文件中定义。这是因为不同客户端屏幕的缩放比例可能不同，需要进行适配，

```c++
class ViewerSetting
{
public:
    //  适配不同客户端屏幕缩放比例
    static float devicePixelRatio;
};
```
我们需要读取当前客户端的这个值并记录：在main.cpp中添加如下代码，
```c++
ViewerSetting::devicePixelRatio = QGuiApplication::primaryScreen()->devicePixelRatio();
```

```c++
#include <QApplication>
#include <QGuiApplication.h>
#include <QScreen.h>

#include "glwindow.h"
#include "ViewerSetting.h"

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    ViewerSetting::devicePixelRatio = QGuiApplication::primaryScreen()->devicePixelRatio();
    GLWindow w;
    w.show();
    return a.exec();
}
```

恭喜你，添加成功，但可能编译报错哦，我们使用了QT的Gui库，可能找不到，我们还需要更新下`cmakelists`文件对应位置添加如下脚本代码，

```js
find_package(Qt6 REQUIRED COMPONENTS Gui)
target_link_libraries(GLViewer PRIVATE Qt6::Gui)
```

!!! note "注意："
    记得编译一下，确保通过；

## 6.3.添加ViewerUtils类

```c++
//  ViewerUtils.h
#pragma once
#include "CGUtils/CGUtils.h"

using namespace CGUTILS;

class ViewerUtils
{
public:
    //  返回6个坐标轴（包含正负轴）中与dir最贴近的轴
    static Vector3f normalizeToAxis(const Vector3f& dir, const Vector3f& localX, const Vector3f& localY, const Vector3f& localZ);
};
```

```c++
//  ViewerUtils.cpp
#include "ViewerUtils.h"

Vector3f ViewerUtils::normalizeToAxis(const Vector3f& dir, const Vector3f& localX, const Vector3f& localY, const Vector3f& localZ)
{
    double angleX = localX.Angle(dir);
    double angleY = localY.Angle(dir);
    double angleZ = localZ.Angle(dir);
    double rAng90 = PI / 2.0;
    bool bTX = angleX > rAng90;
    bool bTY = angleY > rAng90;
    bool bTZ = angleZ > rAng90;
    if (bTX)
        angleX = PI - angleX;
    if (bTY)
        angleY = PI - angleY;
    if (bTZ)
        angleZ = PI - angleZ;

    if (angleX < angleY && angleX < angleZ)
        return bTX ? -localX : localX;
    else if (angleY < angleX && angleY < angleZ)
        return bTY ? -localY : localY;
    else// if (angleZ < angleX && angleZ < angleY)
        return bTZ ? -localZ : localZ;
}
```

聪明的你已经发现，我们也成功使用了此前引入的`CGLib`库~

## 6.4.添加camera.h
直接将`camera.h`文件复制到`src`下，或者用上述添加类的办法添加`camera.h`，然后复制代码进去。

确保编译通过，如果不通过就解决问题，直到通过，解决问题的办法很多，比如看本课程系列对应视频，教程文档，或者搜索，或者在群里问......

然后来到了本节课程的重头戏部分，使用camera吧~

* 在`GLView`类中添加`Camera`成员字段，

```c++
Camera m_camera = Camera(this, QVector3D(0.0f, 6.0f, 10.0f));
```

* 在`GLView.cpp`中`initializeGL`函数实现中添加如下代码，更新时间，

```c++
m_camera.lastFrame = QTime::currentTime().msecsSinceStartOfDay() / 1000.0;
```

* 在`resizeGL`函数实现中添加如下代码，每次窗口尺寸变化都会更新，注意我们偷偷添加了`glViewport`调用，是为了让渲染管线正常工作，想起来了没？在处理为标准化设备坐标后，需要通过视口变换映射到屏幕像素上。

```c++
glViewport(0, 0, w, h);
m_camera.SCR_WIDTH = w;
m_camera.SCR_HEIGHT = h;
```

* 我们来到了`paintGL`函数实现，首先添加如下代码，更新每个渲染循环的时间间隔`deltaTime`，

```c++
float currentFrame = QTime::currentTime().msecsSinceStartOfDay() / 1000.0;
m_camera.deltaTime = currentFrame - m_camera.lastFrame;
m_camera.lastFrame = currentFrame;
```

如果你这几天睡得足，那么你应该记得我们此前设置的`view-matrix`和`projection-matrix`为单位矩阵，那是临时的，赋予它意义的时刻到了，

```c++
m_projectionMat.setToIdentity();
m_projectionMat.perspective(/*qDegreesToRadians*/(m_camera.Zoom), (float)m_camera.SCR_WIDTH / (float)m_camera.SCR_HEIGHT, 0.1f, 100.0f);

m_viewMat = m_camera.GetViewMatrix();
```

* 嗯？应该是鼠标控制相机的运转呀，对的，我们一起来实现~
重写`event`函数，让`camera`捕获鼠标、键盘事件，嗯，至于怎么处理是它的事情了，

```c++
//  GLView.h
virtual bool event(QEvent* e);

//  GLView.cpp
bool GLView::event(QEvent* e)
{
    makeCurrent();

    if (m_camera.handle(e))
        update();

    doneCurrent();
    return QWidget::event(e);
}
```

如果正常的话，你可以通过鼠标操作相机，试着按住鼠标左键转动、按住鼠标右键移动、滚动鼠标滚轮，你会看到不同的效果~

<img src="../img/cad/image-25.png" alt="相机工作" title="相机工作" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：相机工作</figcaption>

!!! note "思考："
    为什么鼠标在三角形上左键旋转时，没有以此为旋转中心呢？

我们回顾下本节所作的事情，添加相机，并在GLView中去使用调用它，至于其它事情都是围绕这个目的展开的。

!!! note "提示："
    作者在`视频课程`中对相机原理和实现有详细的讲解，包括原理和代码逻辑，欢迎观看。