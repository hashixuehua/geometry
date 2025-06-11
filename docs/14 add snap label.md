# 14.Add snap label
`鼠标捕捉吸附标识`主要作用是提高用户在使用鼠标时的操作效率和准确性‌。当鼠标移动到某个特定区域时，捕捉吸附标识会触发鼠标的吸附功能，使得鼠标指针能够自动吸附到该区域，从而减少移动距离和操作时间，能够显著提升用户的操作体验和效率‌。

我们把此功能的实现归类到`DrawingViewerElemHolder`中，我们来看下实现后其public函数的调用逻辑，

<img src="../img/cad/image-65.png" alt="`DrawingViewerElemHolder`接口调用逻辑" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：`DrawingViewerElemHolder`接口调用逻辑</figcaption>

好像没有增加接口，没有看错？画错？设计错吧？是的，`snapLabel`的配置、绘制过程放在了已有的`setup`、`drawViewElement`中，并没有对外新增接口，而内部的修改如下，

* 新增：`setupSnap`private函数，负责`snapLabel`的配置，完成其数据到渲染桥梁的搭建；
* 扩展：`drawViewElement`函数，使其能够兼容绘制没有纹理贴图的viewer元素，如`snapLabel`；
* 扩展：`ViewerCache`类，增加字段；
* 扩展：在`GLView.paintGL`中调用绘制，代码如下；

```c++
//  snap label
if (ViewerSetting::previewData.previewNextPt)
{
    QMatrix4x4 modelSnap;
    modelSnap.translate(m_modelMatrix * QVector3D(ViewerSetting::previewData.previewNextPt->X, ViewerSetting::previewData.previewNextPt->Y, ViewerSetting::previewData.previewNextPt->Z));

    //modelSnap = m_modelMatrix * modelSnap;
    m_lightShader.setUniformValue("model", modelSnap);
    m_model->DrawViewElement(m_lightShader, ViewerCache::snapLabel);
}
```

可以看到我们没有做太多的改动即实现了`snapLabel`的绘制，也没有在`DrawingViewerElemHolder`中新增字段，其构建好的绘制数据放在了已有的`map<string, MeshInfo> mapName2VMesh`中；

尽管如此，我们还是做了关键的事情的，比如`setupSnap`，构建了一个只有圆形边框的mesh，这个也就是显示出来的效果，详细请查看函数实现~

如果一切正常，或者遇到的问题被排查解决，那么鼠标捕捉吸附效果和下图很像。

<img src="../img/cad/image-66.png" alt="捕捉吸附标识效果" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：捕捉吸附标识效果</figcaption>
