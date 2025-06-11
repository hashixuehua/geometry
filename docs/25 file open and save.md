# 25.文件打开和保存
文件打开和保存是基础和核心功能之一，其重要性在此不做赘述，值得一提的是可以提高用户的使用体验和工作体验，同时提高提高开发者的开发体验。

## 25.1.概述
为了提升基本的开发体验（基本保障），维持良好的情绪和状态（哈哈哈哈哈），作者在`GLViewer`中实现了线数据的读取和保存，当然这是基本的实现，你也可以在此基础上继续进行扩展或实现更复杂和符合实际场景需求的数据导入导出功能，如`dwg`、`sat`、`stl`、`fbx`格式的导入导出等。

作者定义了`1.0`和使用`1.2`版本的json格式，使用腾讯开源的`rapidjson`进行格式的读取和写入。

!!! note "提示"
    1. `RapidJSON`：A fast JSON parser/generator for C++ with both SAX/DOM style API；
    2. `github`：https://github.com/Tencent/rapidjson；
    3. 可参考作者博客、微信公众号(**哈市雪花**)或`RapidJSON`官方文档进行熟悉。

我们需要修改`cmakelist`以使用`RapidJSON`，

```js
set(Dependencies "${CMAKE_SOURCE_DIR}/../../dependencies")

#other code here

#rapidjson
set(RJSON "${Dependencies}/RapidJSON")
if(WIN32)
    include_directories(${Dependencies})
    include_directories(${RJSON}/include)
endif()
```

## 25.2.格式说明
`1.0`版本格式比较简单，Json格式整体为数组（可表示若干空间平面为基准单元的线集合），其中每个对象为同一空间平面上的点线，格式和含义如下，

<img src="../img/cad/image-93.png" alt="`1.0`版本格式和含义" width="300" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：`1.0`版本格式和含义</figcaption>

`1.2`版本是在`1.0`版本基础上扩展的，增加了`layer`和`version`等信息，可参考`doc`目录下的示例文件，

`layer`是数组，其元素为每个图层的数据，包含图层名称、颜色、描述、激活状态信息，

```json
{
  "name": "Layer0",
  "color": [
    255,
    0,
    0
  ],
  "description": "default",
  "activated": false
}
```

`data`中以图层为单位展开详细的数据，其中以工作平面法向为单位继续展开数据，因为一个图层上的数据可以分布在不同的工作平面上，在之后的展开可惨开`1.0`版本的描述。

我们新增了`ReadAndWriteUtils`类用于文件的具体读写实现，相关接口调用逻辑如下，

<img src="../img/cad/image-94.png" alt="`ReadAndWriteUtils`接口调用逻辑" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：`ReadAndWriteUtils`接口调用逻辑</figcaption>

如果一切正常，或者遇到的问题被排查解决，那么运行`GLViewer`，然后创建图层、画线体验保存和读取功能吧，有问题或疑问请查看工程代码或联系我。

<img src="../img/cad/image-92.png" alt="保存和读取功能效果" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：保存和读取功能效果</figcaption>
