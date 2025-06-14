# 21.复制
复制功能是三维软件的基础功能，可以提高用户的操作和设计效率。本节课程我们实现选中元素的复制功能，包括线、组件，以及复制过程的预览效果和命令提示过程。

同样，复制功能实现的代码量不大，但贯穿了框架体系的多个关键模块，其实也是依赖和复用了框架体系的能力，来实现预览、命令提示、操作数据处理等过程，然后添加对应实现，继续为基础框架抽象和添加基础能力。我们来看工作机制相关的逻辑流程。

<img src="../img/cad/image-80.png" alt="“复制”接口调用逻辑" width="800" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：“复制”接口调用逻辑</figcaption>

1. 点击按钮调用`copy`进入复制状态，同时调用`drawCurve`处理预览和完成数据；
2. 通过`ViewerSetting.previewData`处理鼠标左键点击事件，第一个点作为复制的基点，后续的连续点作为复制到的目标点，在完成时调用`DrawingLinesHolder.CopySelectedData`和`DrawingComponentHolder.copySelectedData`添加复制操作产生的数据；
3. 在每个渲染循环中调用`DrawingLinesHolder.DrawCopyElementsPreview`和`DrawingComponentHolder.drawCopyElementsPreview`将预览效果绘制到`GLView`中；而复制操作完成添加的数据由对应的`draw`进行绘制；
4. 当`ESC`键按下时，调用`ViewerSetting.ClearCopyImData`清除相关临时数据；

在绘制预览数据时采用了实例化渲染的思路，由于此前渲染选中效果时已经将选中数据与渲染的桥梁`setup`完成，这里只需要更新复制操作偏移计算的（选中元素的）`modelMatrix`后再次绘制即可。

<img src="../img/cad/image-81.png" alt="小学生水平的绘画巨作" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：小学生水平的绘画巨作</figcaption>

小学生水平的绘画巨作来了（苦笑），我们来结合框选（线）、点选（组件）来使用复制功能，记得可以根据命令提示系统来实现精确的复制操作哦。

<img src="../img/cad/image-82.png" alt="对“巨作”进行复制" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：对“巨作”进行复制</figcaption>

!!! attention
    我们顺便做了优化：将`DrawingComponentHolder`中的`bodys`、`meshes`元素类型改为了对应对象的指针，这样`vector`容器在容量达到阈值时拷贝以开辟更大的容量空间情况下，不会有太多的性能损耗，同时`getSelectedBody`返回的数据也不至于会偶发的失效（那样的话太不可靠啦）；同时要记得管理好创建在堆上的这些资源哦，在析构函数中、删除组件时等场景中记得释放。

有了上述知识去详细的探究代码将会容易的多，带着好奇心去探究吧~
