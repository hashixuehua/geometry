# 23.线框模式
在三维软件中线框模式是最基本的显示模式之一，用户在这种简化的视觉表示方式下，可以更清晰地看到模型的结构，便于进行编辑和调整‌。

一般不会直接采用类似`glPolygonMode(GL_FRONT_AND_BACK, GL_LINE)`的方式切换为线框模式，那样线会很密很杂，因为以三角面边线的方式绘制，而通常用户不会关心组件细分之后的三角面情况，而更关心组件本身的轮廓信息。

本节课程以仅绘制轮廓线的方式显示模型线框。

```c++
void Mesh::DrawWireframe(QOpenGLShaderProgram& shader)
{
    shader.setUniformValue("objectColor", 0.f, 0.f, 0.f, 1.f);
    m_glFunc->glLineWidth(1.0f);
    QOpenGLVertexArrayObject::Binder bind2(&m_VAOEdge);
    m_glFunc->glDrawElements(GL_LINES, static_cast<unsigned int>(edges.size()), GL_UNSIGNED_INT, 0);
}
```

!!! note "补充"
    `Mesh`中的轮廓线数据来源于原始的几何体`Body`，在`CGLib`中`Body`是采用一种类似`BRep`的简化表达，可以拿到组成`Body`的`Face`的轮廓数据，从而在网格化后保留下来并在转换为`Mesh`过程中进行传递。

同样线框模式的标识`wireframeMode`被存放在了`ViewerSetting`中，我们回顾下，`ViewerSetting`的作用是存储Viewer相关设置数据并提供接口。

我们来看下接口调用逻辑，

<img src="../img/cad/image-89.png" alt="“线框模式”接口调用逻辑" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：“线框模式”接口调用逻辑</figcaption>

1. 点击按钮调用`GLView.wireFrame`函数，进而调用`ViewerSetting.SwitchWireframeMode`切换显示模式；
2. 在每个渲染循环中`DrawingComponentHolder`会根据显示模式来决定渲染方式，如果是现况模式，则`Mesh.Draw`会以仅绘制轮廓线的方式渲染。

如果一切正常，或者遇到的问题被排查解决，切换为线框模式后将有下图类似的效果，有问题或疑问请查看工程代码或联系我。

<img src="../img/cad/image-88.png" alt="线框模式效果" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：线框模式效果</figcaption>

