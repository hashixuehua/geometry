# 22.删除
删除功能同样是设计软件的基础功能，可以方便的删除不需要的目标元素，结合选择功能的使用可以提高工作效率。

本节课程我们实现删除功能，支持删除当前选择的元素，如线、组件。功能实现的代码量比较少，同时逻辑较简单，我们来看下接口调用逻辑。

<img src="../img/cad/image-83.png" alt="“删除”接口调用逻辑" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：“删除”接口调用逻辑</figcaption>

1. 点击按钮调用`GLView.deleteSelected`，直接调用`Model.DeleteSelectedElements`；
2. `Model.DeleteSelectedElements`调用`DrawingLinesholder.deleteSelectedLines`和`DrawingComponentHolder.deleteSelectedComponents`实现对选择元素的删除功能。

在本节课程中我们优化了`ModelNode`的实现，将`curveId`的类型改为了`const CurveData*`，方便在删除目标线时删除相关的`ModelNode`数据。

```c++
//  delete nodes
for (auto itrItem = modelNodes.begin(); itrItem != modelNodes.end(); /*++itrItem*/)
{
    auto itrFind = mapCurveData.find(itrItem->curveId);
    if (itrFind != mapCurveData.end())
    {
        itrItem = modelNodes.erase(itrItem);
        continue;
    }

    ++itrItem;
}
```

运行和体验添加的新功能吧，有问题或疑问请查看工程代码或联系我。
