# 15.添加一些场景元素
现在`GLView`场景中的元素比较少，虽然可以绘制线了，但还没有组件（component），为了丰富下场景，我们创建一些默认的组件。

我们新建并实现`DrawingComponentHolder`，其使用`CGLib`库接口创建了一些组件，然后提供管理、配置和绘制功能。我们来看下实现后其public函数的调用逻辑，

<img src="../img/cad/image-67.png" alt="`DrawingComponentHolder`接口调用逻辑" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：`DrawingComponentHolder`接口调用逻辑</figcaption>

相信大家对`setup`、`draw`这两个函数的功能已经不陌生了，详细请看工程代码吧~

!!! important
    + 本节课程我们对`MeshInfo`进行了扩展，添加了`selected`字段，标识其选中状态；
    + 随着课程的继续，`DrawingComponentHolder`的功能会逐渐丰富，提供如选中管理、复制、隐藏、显示模式设置等功能；
    对了，我们还删除了此前默认绘制的三角形以及相关代码，同时维护了`Mesh`的析构函数，释放对应资源。

!!! note "感想"
    点滴的前进和维护扩展推进系统不断的完善，发现了，那就去做吧。

如果一切正常，或者遇到的问题被排查解决，那么运行之后的效果如下，效果不够好？跟着课程继续吧~

<img src="../img/cad/image-68.png" alt="丰富了场景元素" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：丰富了场景元素</figcaption>
