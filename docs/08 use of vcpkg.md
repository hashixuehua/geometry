# 8.关于vcpkg的使用
!!! note "补充："
    `《课程上》`暂不涉及`OpenCV`和`assimp`库的使用，但在`cmakelists`文件及工具代码中使用了`OpenCV`库，建议根据如下教程安装`OpenCV`。

## 8.1.通过vcpkg安装OpenCV
`Vcpkg`是由`Microsoft`和`C++社区`维护的免费开源`C/C++包管理器`，可在Windows、macOS和Linux上运行。它是核心的C++工具，使用C++和CMake脚本编写。它旨在解决管理C/C++库的独特难题。

vcpkg在与`CMake`、`MSBuild`环境集成方面具有优势。

```js
cd 到vcpkg安装目录
# 如此前运行过（比如在安装vcpkg时）`./vcpkg integrate install`，则无须再运行
./vcpkg integrate install
./vcpkg install opencv4:x64-windows
```

然后在`cmakelists`中添加：

```js
set(OpenCV_ROOT "${VCPKG_INSTALLED_DIR}/x64-windows/share/opencv4")
find_package(OpenCV REQUIRED)
```

其中`set(OpenCV_ROOT "${VCPKG_INSTALLED_DIR}/x64-windows/share/opencv4")`也可以通过设置电脑环境变量的方式，
或者通过在`cmakelists`中设置`vcpkg`为构建工具链：

```js
# --- vcpkg
if (DEFINED ENV{VCPKG_ROOT} AND NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    message(STATUS "VCPKG_ROOT: $ENV{VCPKG_ROOT}")
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake" CACHE STRING "")
endif()
```

这样可通过`vcpkg`找到通过其安装的依赖库，包括`OpenCV`库，是不是方便了很多？我们可以使用`vcpkg`统一管理依赖库，然后在不同的项目中方便的使用；

!!! attention
    1. 在安装完vcpkg后一定要运行命令`.\vcpkg integrate install`集成至本机环境，否则cmake可能无法正确设置vcpkg为构建工具链。如果此前没有运行，现在运行也来得及~
    2. 还记得此前安装完vcpkg后我们设置的如下环境变量吗？这是一个好习惯

<img src="../img/cad/image-26.png" alt="vcpkg默认配置" title="vcpkg默认配置" width="400" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：vcpkg默认配置</figcaption>

## 8.2.通过vcpkg安装assimp（按需）
如果需要使用`assimp`库，相信你已经知道怎么通过`vcpkg`来安装，并在`cmakelists`中配置使用了。

```js
cd 到vcpkg安装目录
# 如此前运行过`./vcpkg integrate install`，则无须再运行
./vcpkg integrate install
./vcpkg install assimp
```

请回顾上述内容，可参考下面的链接，如果有必要的话，

`https://github.com/assimp/assimp/blob/master/Build.md#install-on-all-platforms-using-vcpkg`
