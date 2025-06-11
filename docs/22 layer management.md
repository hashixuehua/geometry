# 22.图层管理
三维软件中的图层管理对于提高设计效率、增强设计成果的可读性等方面具有重要意义，用户可以轻松地组织和区分不同类型的对象，从图层角度控制元素显隐以提高工作效率和体验。

接口调用逻辑比较清晰，我们来看接口调用逻辑，

<img src="../img/cad/image-87.png" alt="“图层管理”接口调用逻辑" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：“图层管理”接口调用逻辑</figcaption>

1. `GLView.layerSetting`调用展示图层设置窗体`LayerSettingDialog`，随后对图层数据进行初始化并展示；
2. 在`LayerSettingDialog`中可以进行图层编辑，包括图层数据的增删改，数据会同步到`LayerCache`中；
3. 当删除图层时会调用`OnLayerRemoved`回调函数，对所属目标图层的线数据进行删除；同样当修改图层颜色时，会调用`OnLayerColorChanged`重新绘制以正确的显示线颜色；
4. 在每个渲染循环中，`DrawingLinesHolder`会根据最新的图层数据进行线的绘制。

!!! note "提示"
    `LayerCache`和`ViewerSetting`一样是状态更新类功能的模块，`LayerCache`存储图层数据并对外提供增删改查接口。

有了上述知识去详细的探究代码将会容易的多，带着好奇心去探究吧~

如果一切正常，或者遇到的问题被排查解决，记得运行和使用图层设置功能，之后绘制线数据，效果将和下图类似，有问题或疑问请查看工程代码或联系我。

<img src="../img/cad/image-86.png" alt="图层设置及效果" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：图层设置及效果</figcaption>
