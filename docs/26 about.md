# 26.关于
关于功能比较简单，我们自定义`AboutDialog`类，其继承自`QDialog`，通过若干控件及布局填充窗体，并通过定义事件绑定实现点击链接跳转等功能，值得一提的是关于功能也比较重要，是用户与作者联系的窗口之一。

!!! note "提示"
    详细代码请探究工程代码。

<img src="../img/cad/image-90.png" alt="“关于”弹窗效果" width="800" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：“关于”弹窗效果</figcaption>

!!! note "提示"
    我们同时还添加了打开网站的工具栏按钮，可点击打开`cglib.net`以查看更多信息，包括本课程的文档教程及相关链接信息，**作者会以`cglib.net`网站为入口进行信息的更新和展示，欢迎关注**。

```c++
void GLWindow::website(void)
{
    QDesktopServices::openUrl(QUrl("https://www.cglib.net", QUrl::TolerantMode));
}

void GLWindow::about(void)
{
    aboutDlg->exec();
}
```

我们同时还更新了主窗体左上角的图标，可以直接用`QT Creator`打开，然后在UI界面上设置主窗体的icon，或者在`glwindow.ui`中的`windowTitle`元素下方添加`windowIcon`元素，记得将图标添加到`glviewer.qrc`中。

```js
<property name="windowIcon">
 <iconset resource="../resources/glviewer.qrc">
  <normaloff>:/about.png</normaloff>:/about.png</iconset>
</property>
```

<img src="../img/cad/image-91.png" alt="程序图标设置" width="600" align="middle" style="display: block; margin-left: auto; margin-right: auto;"/>
<figcaption style="text-align: center;">图：程序图标设置</figcaption>
