# 15.Draw Image
此前的课程中我们有过使用图片作为纹理的经验，在绘制mesh前绑定纹理资源，绘制出表面贴有图片的效果，本节课程我们将交互式的绘制图片，同样采用纹理技术路线。

<img src="../img/cad/image-ralation-image.png" alt="模块逻辑关系图" width="700" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：模块逻辑关系图</figcaption>

我们跟着模块逻辑关系图来梳理步骤，

1. 首先将`drawImage.png`添加到`glviewer.qrc`中以便在项目中使用；
2. 参考此前课程添加`actionImage`，两种方式中选一种即可；
3. 进入绘制命令后首先使用`QFileDialog`打开目标图片，并将其设置到图片绘制的上下文中；
4. 此前课程中搭建的框架体系中已经在相关模块中实现了`draw image`相关处理和绘制，命令和预览采用`draw line`的方式，但操作完成之后根据`previewData.getCompleteLines`获取的线数据作为定位和尺寸构建图片绘制数据；
5. 在每个渲染循环中绘制图片，输出到`GLView`中。

我们来看其public函数的调用逻辑，

<img src="../img/cad/image-64.png" alt="`DrawingImageHolder`接口调用逻辑" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：`DrawingImageHolder`接口调用逻辑</figcaption>

1. `currentImageFile`字段，进入`drawImage`状态时，打开图片文件并调用`model.SetImageDrawingFile`函数将其设置到该字段；当然同时进入`drawLine`状态；
2. `TryToSetup`，在`PreviewData.getCompleteLines`完成并获取绘制数据后调用该函数进行图片绘制定位和尺寸的确定以及数据与渲染桥梁的搭建；
3. `DrawImage`，在每个渲染循环中调用该函数将图片会知道`GLView`中；

!!! important
    `drawImage`命令同时进入了`drawLine`状态，也就是复用对应的绘制预览、操作、完成数据的获取等流程，在尔后的数据更新过程中调用图片绘制的`TryToSetup`。

如果一切正常，或者遇到的问题被排查解决，那么运行后并使用`draw image`绘制后的效果和下面很像，有可能更漂亮哦，取决于你的审美眼光~

<img src="../img/cad/image-62.png" alt="连续绘制图片" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：连续绘制图片</figcaption>
