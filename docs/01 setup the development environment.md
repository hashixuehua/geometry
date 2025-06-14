# 1.QT+OpenGL开发环境搭建
开发环境搭建是一件简单但又耗时的事情，毫无疑问，这是本系列课程开始的必要条件，你可以逐个的去下载安装和配置，就像玩游戏那样，逐个关卡的去通关。

!!! note "经验积累的机会"
    相信我，在完成本节课程后，你的能力将会得到较大提升。

## 1.1.Visual Studio 2022
* 官网下载链接：https://visualstudio.microsoft.com/zh-hans/vs/

## 1.2.CMAKE
* 官网下载链接：https://cmake.org/download/

## 1.3.QT
* 官网下载链接：https://www.qt.io/download-qt-installer
国内镜像如：https://mirrors.ustc.edu.cn/qtproject/

## 1.4.mingw-w64
* https://www.mingw-w64.org/

环境变量：

* `MinGW_HOME = D:\software\mingw64`
* Path添加 `%MinGW_HOME%\bin`

## 1.5.ninja
* https://ninja-build.org/

环境变量：

* `Path`添加`ninja.exe`所在目录，如作者的是：`D:\software\ninja-win`

## 1.6.vcpkg
* github链接：https://github.com/microsoft/vcpkg

安装后配置环境变量：

* `VCPKG_ROOT = D:\git\vcpkg`
* 系统环境变量`Path`中添加 `%VCPKG_ROOT%`
* (可选) `VCPKG_DEFAULT_TRIPLET = x64-windows`

## 1.7.git
* 下载链接：https://git-scm.com/