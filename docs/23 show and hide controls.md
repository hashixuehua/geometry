# 23.显隐控制
‌三维软件中的显示隐藏功能对于提高工作效率和简化视图具有重要作用‌。通过隐藏不相关的对象，用户可以更专注于当前正在处理的部分，从而提高操作的准确性和效率。

显隐功能的实现较为简单，主要是更新`ViewerSetting`中显隐状态，同时在渲染循环中根据显隐状态决定是否绘制对应的线、组件以及预览效果。接口调用逻辑如下，

<img src="../img/cad/image-85.png" alt="“显隐控制”接口调用逻辑" width="500" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：“显隐控制”接口调用逻辑</figcaption>

（不过多赘述，详见代码~）