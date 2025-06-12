# 14.Draw Curve
由于在上节介绍和搭建的框架体系中已经在相关模块中实现了`Arc`（三点定圆）、`Arc2`（圆心、起点、终点方式定圆）、`Circle`、`Rectangle`相关处理和绘制，本节课程我们直接添加UI和入口函数即可实现上述Curve的绘制。

## 14.1.Draw Arc
在此前课程中我们知道怎样创建action，然后添加到工具栏和菜单项中，本节课程我们采用另一种方式添加`actionArc`，当然也可以选择继续按此前讲的方式添加。

首先记得将`arc.png`添加到`glviewer.qrc`中便于在项目中使用。然后`Ctrl+F`在整个解决方案中搜索`actionLine`，然后在对应位置参考添加`actionArc`。

!!! attention
    不需要对`XXX_autogen`目录下的`ui_glwindow.h`中的内容进行编辑，此文件为自动生成。

在`GLWindow`的`createAction`函数中添加如下实现，

```c++
connect(ui->actionArc, SIGNAL(triggered()), ui->openGLWidget, SLOT(drawArc()));
```

在`GLView`中添加`drawArc`函数：

```c++
void GLView::drawArc()
{
    drawCurve(ViewerStatus::DrawingArc, 0);
}
```

如果一切正常，或者遇到的问题被排查解决，那么运行后并使用`Arc`绘制可以看到如下效果。

<img src="../img/cad/image-60.png" alt="三点定圆方式绘制圆弧" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：三点定圆方式绘制圆弧</figcaption>

## 14.2.Draw Arc2 & Circle & Rectangle
参考`Draw Arc`章节内容来实现，博主不在此絮絮叨叨，请读者自行实现，如果遇到问题可参考本节课程对应工程代码。

<img src="../img/cad/image-61.png" alt="多种线类型绘制" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：多种线类型绘制</figcaption>

!!! note "提示"
    1. 建议结合代码跟着此前课程中的接口调用逻辑体系图详细的熟悉一种Curve的绘制流程，熟悉和加深对框架和机制的理解，包括对应的命令网状结构、交互操作、PreviewData构造预览和完成数据的过程、使用了哪些CGLib接口以及怎样使用的等；
    2. 也许你会有疑问这种功能性的代码需要去详细“学习”吗？建议继续学习以对框架体系和关键过程有基本的了解，这是本系列课程值得学习的内容点，也是继续学习后续课程内容的基础。
