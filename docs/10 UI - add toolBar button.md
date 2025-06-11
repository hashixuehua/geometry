# 10.UI：添加工具栏按钮
Qt框架提供丰富的功能库，且在跨平台兼容方面表现出色，使得适配兼容不同的操作系统变得更简单，降低开发和维护成本。

本节课我们来近距离体验和感受使用QT开发UI的过程。

## 10.1.将图标图片添加到资源文件中
将`line.png`等图片添加到`glviewer.qrc`中，便于接下来作为工具按钮图标使用。

```js
<RCC>
    <qresource prefix="/">
    <file>viewcube.png</file>
    <file>rotateLabel.png</file>
    <file>line.png</file>
    <file>arc.png</file>
    <file>arc2.png</file>
    <file>circle.png</file>
    <file>rectangle.png</file>
    </qresource>
</RCC>
```

## 10.2.创建绘制线的按钮
1) 打开项目

首先，我们使用打开`Qt Creator`，

<img src="../img/cad/image-33.png" alt="打开`Qt Creator`" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：打开`Qt Creator`</figcaption>

打开项目，选择项目下的根cmakelists文件，打开，此后需要等待几秒时间待配置完成，

<img src="../img/cad/image-34.png" alt="选择根cmakelists" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：选择根cmakelists`</figcaption>

2) 添加菜单项

我们双击左侧项目文件树中`glwindow.ui`文件，打开UI界面，

<img src="../img/cad/image-36.png" alt="打开UI界面" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：打开UI界面</figcaption>

此时我们先添加菜单栏项，双击`在这里输入`，输入`Draw`，按`Enter`键完成

<img src="../img/cad/image-37.png" alt="双击`在这里输入`" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：双击`在这里输入`</figcaption>

<img src="../img/cad/image-38.png" alt="菜单项`Draw`" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：菜单项`Draw`</figcaption>

3) 添加工具栏

首先，我们添加工具栏，空白处右键，选择`添加工具栏`，之后空白工具栏出现在界面上，

<img src="../img/cad/image-40.png" alt="添加工具栏" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：添加工具栏</figcaption>

<img src="../img/cad/image-41.png" alt="已添加工具栏" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：已添加工具栏</figcaption>

4) 添加工具按钮

<img src="../img/cad/image-43.png" alt="创建action" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：创建action</figcaption>

按上述图片步骤创建`actionLine`，然后左键按住拖动到工具栏上，松开按键，即添加到工具栏了，

<img src="../img/cad/image-44.png" alt="添加工具栏按钮：将action拖动到工具栏" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：添加工具栏按钮：将action拖动到工具栏</figcaption>

还可以在工具栏上右键，选择`添加分隔符`，这样按钮右侧添加了竖向分隔符，排列更美观~

## 10.3.监控并绑定点击事件
注意可以有多种方式实现绑定点击事件，如在`Action编辑器`的`actionLine`行右键，转到槽，选择`triggerred`，确定，即自动创建函数并绑定点击事件，然后填充函数实现即可。

<img src="../img/cad/image-45.png" alt="添加事件" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：添加事件</figcaption>

本文不采用此种方式，本文采用更加灵活可控的代码实现方式，如下。

!!! important
    对了，还是构建运行下，看看是否有错误，正常的话是没有错误的（苦笑）

<img src="../img/cad/image-46.png" alt="构建配置" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：构建配置</figcaption>

还是习惯于使用`Visual Studio 2022`，打开VS2022，在class `glwindow`中添加函数，

```c++
void GLWindow::createActions(void)
{
    connect(ui->actionLine, SIGNAL(triggered()), ui->openGLWidget, SLOT(drawLine()));
}
```

在class `glview`中添加函数，

```c++
void GLView::drawLine()
{
    QMessageBox::information(NULL, "Title", "Content", QMessageBox::Yes | QMessageBox::No, QMessageBox::Yes);
}
```

* 注意添加头文件`#include <QMessageBox>`;
* 注意在`glview.h`中`drawLine`函数声明需要放在`public slots:`后面，因为此函数需要被事件绑定；

```c++
public slots:
    void drawLine();
```

记得在`glwindow`构造函数中调用`createActions`。

## 10.4.构建运行
如果一切正常，或者遇到的问题被排查解决，那么运行后可以看到如下效果。

<img src="../img/cad/image-47.png" alt="Hello Action" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：Hello Action</figcaption>

!!! note "提示"
    现在再来操作操作，看起来有点雏形的样子了~

## 10.5.添加到菜单项中
在本节课程我们添加了菜单项`Draw`和工具栏按钮`Line`，虽然工具栏按钮可以点击触发事件，带哦用绑定的函数了，但菜单项`Draw`下是空白的，我们尝试把`Line`加进去~

<img src="../img/cad/image-48.png" alt="将action添加到菜单项中" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：将action添加到菜单项中</figcaption>

好了，`Line`出现在菜单项`Draw`的下拉列表中了。

<img src="../img/cad/image-49.png" alt="已添加到菜单项下拉列表" width="300" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：已添加到菜单项下拉列表</figcaption>
