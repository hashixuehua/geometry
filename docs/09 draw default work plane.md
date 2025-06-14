# 9.绘制默认的工作平面
这一节我们将绘制默认的工作平面和当前的工作平面，并用网格线进行填充，这回用到我们此前介绍过的`数据与渲染的桥梁`（`VAO、VBO、EBO`）相关知识。

## 9.1.在顶点着色器中添加世界坐标转换处理
由于OpenGL默认是`y-up`，我们开发的Viewer是`z-up`的，为了让Viewer达到`正确`效果，我们需要叠加`y-up to z-up`的转换矩阵。

```c++
//  y-up to z-up
m_modelMatrix = QMatrix4x4(1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, -1.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f);//(1.0f);
m_modelMatrix = m_modelMatrix.transposed();
```

然后在渲染循环中传递给顶点着色器，

```c++
m_lightShader.setUniformValue("model", m_modelMatrix);
```

## 9.2.定义`WorkPlanePara`
我们定义类`WorkPlanePara`来表示工作平面，

```c++
class WorkPlanePara
{
public:
    WorkPlanePara(const Vector3f& ori, const Vector3f& norm, const Vector3f& lX, const Vector3f& lY, float halfLen, int cntGridPerEdg, float off)
        : origin(ori), normal(norm), localX(lX), localY(lY), halfLength(halfLen), gridCntPerEdge(cntGridPerEdg), offset(off)
    {
    }

    Vector3f origin;
    Vector3f normal;
    Vector3f localX;
    Vector3f localY;
    float halfLength;
    int gridCntPerEdge;
    float offset;
};
```

## 9.3.定义变量并进行工作
然后定义当前`defaultWorkPlane`和`currentWorkPlane`，随后调用`GetWorkPlaneMesh`函数获取对应的mesh，之后添加到当前成员变量`mapName2VMesh`中，
在每个渲染循环中我们进行绘制。

```c++
void Model::setupWorkPlaneMesh()
{
    //  cube
    WorkPlanePara workPlane[2]{
        WorkPlanePara(Vector3f::Zero, Vector3f::BasicZ, Vector3f::BasicX, Vector3f::BasicY, 20000.0f, 10, -1.8f),
        WorkPlanePara(Vector3f::Zero, Vector3f::BasicZ, Vector3f::BasicX, Vector3f::BasicY, 1000.0f, 4, 0.0f) };

    for (size_t cntItem = 0; cntItem < 2; cntItem++)
    {
        TriangleMesh mesh;
        GetWorkPlaneMesh(workPlane[cntItem], mesh);

        auto& name = cntItem == 0 ? ViewerCache::defaultWorkPlane : ViewerCache::currentWorkPlane;
        //mapName2VMesh[name] = MeshInfo(ConvertMesh(mesh));
        mapName2VMesh.insert(make_pair(name, MeshInfo(ConvertMesh(mesh))));
    }
}
```

上述`GetWorkPlaneMesh`函数中，我们根据`WorkPlanePara`的参数构造mesh，包括尺寸、位置、方向以及网格线数量等。

```c++
void Model::drawWorkPlane(QOpenGLShaderProgram& shader)
{
    for (auto& meshInfo : mapName2VMesh)
    {
        shader.setUniformValue("objectColor", meshInfo.second.color.GetWX(), meshInfo.second.color.GetWY(), meshInfo.second.color.GetWZ(), meshInfo.second.color.GetW());
        meshInfo.second.current->Draw(shader);
    }
}
```

## 9.4.效果

!!! note "提示："
    详细请查看本节课程对应的代码工程，我们也用到了智能指针`shared_ptr`来管理堆上的资源(Mesh*)，
    如果有疑问欢迎交流~

如果一切正常，或者遇到的问题被排查解决，那么运行后可以看到如下效果。

<img src="../img/cad/image-27.png" alt="绘制工作平面" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：绘制工作平面</figcaption>
