# :twisted_rightwards_arrows:C#组件 #

:checkered_flag:[C#窗体组件](#a1)

<b id='a1'></b>

### :leftwards_arrow_with_hook:C#窗体组件 ###

:arrow_up:[返回目录](#a1)

`在C#的运用中，其中图形化界面就有C#窗体程序（WindowsForm），在Vs中可以创建一个窗体程序，步骤如下：`

![](http://c.biancheng.net/uploads/allimg/190328/4-1Z32Q50624422.gif)

`在该对话框中选择“Windows 窗体应用程序”，并更改项目名称、项目位置、解决方案名称等信息，单击“确定”按钮，即可完成 Windows 窗体应用程序的创建，如下图所示:`

![](http://c.biancheng.net/uploads/allimg/190328/4-1Z32Q50J3X1.gif)

`在每一个 Windows 窗体应用程序的项目文件夹中，都会有一个默认的窗体程序 Form1.cs，并且在项目的 Program.cs 文件中指定要运行的窗体`

在代码模式下：

```C#
static class Program
{
    /// <summary>
    /// 应用程序的主入口点。
    /// </summary>
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());  
    }
}
```

`系统中默认的控件全部存放到工具箱中，选择“视图”→“工具箱”，如下图所示`

![](http://c.biancheng.net/uploads/allimg/190328/4-1Z32Q51444D3.gif)


`每一个 Windows 窗体应用程序都是由若干个窗体构成的，窗体中的属性主要用于设置窗体的外观。在 Windows 窗体应用程序中右击窗体，在弹出的右键菜单中 选择“属性”命令，弹出如下图所示的属性面板。`

![](http://c.biancheng.net/uploads/allimg/190328/4-1Z32Q60222128.gif)

`在该图中列出的属性分为布局、窗口样式等方面，合理地设置好窗体的属性对窗体的 展现效果会起到事半功倍的作用。窗体的常用属性如下表所示。`

|属性	|作用|
|:--:|:--:|
|Name|	用来获取或设置窗体的名称|
|WindowState|	获取或设置窗体的窗口状态，取值有3种，即Normal（正常）、Minimized（最小化）、Maximized（最大化），默认为 Normal，即正常显示|
|StartPosition|	获取或设置窗体运行时的起始位置，取值有 5 种，即 Manual（窗体位置由 Location 属性决定）、CenterScreen（屏幕居中）、WindowsDefaultLocation（ Windows 默认位置）、WindowsDefaultBounds（Windows 默认位置，边界由 Windows 决定）、CenterParent（在父窗体中居中），默认为 WindowsDefaultLocation|
|Text	|获取或设置窗口标题栏中的文字|
|MaximizeBox|	获取或设置窗体标题栏右上角是否有最大化按钮，默认为 True|
|MinimizeBox|	获取或设置窗体标题栏右上角是否有最小化按钮，默认为 True|
|BackColor|	获取或设置窗体的背景色|
|BackgroundImage	|获取或设置窗体的背景图像|
|BackgroundImageLayout|	获取或设置图像布局，取值有 5 种，即 None（图片居左显示）、Tile（图像重复，默认值）、Stretch（拉伸）、Center（居中）、Zoom（按比例放大到合适大小）|
|Enabled|	获取或设置窗体是否可用|
|Font	|获取或设置窗体上文字的字体|
|ForeColor|	获取或设置窗体上文字的颜色|
|Icon	|获取或设置窗体上显示的图标|

对于这些属性可以在属性界面修改，也可以使用代码进行修改与访问。窗体
