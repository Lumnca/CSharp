# :twisted_rightwards_arrows:C#组件 #

<b id='t'></b>

:checkered_flag:[C#窗体组件](#a1)

:checkered_flag:[C#窗体事件](#a2)

<b id='a1'></b>

### :leftwards_arrow_with_hook:C#窗体组件 ###

:arrow_up:[返回目录](#t)

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

对于这些属性可以在属性界面修改，也可以使用代码进行修改与访问。窗体组件获取是通过其Name属性来实现的，对于每个组件都有这个属性，我们可以通过这个来获取到组件的信息和本身，如下：

![](https://github.com/Lumnca/CSharp/blob/master/Images/a1.png)

双击label1组件会添加一个鼠标点击事件，在其中代码添加：

```C#
namespace Test
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void label1_Click(object sender, EventArgs e)
        {
            //点击获取dateTimePicker1组件的Text信息
            label1.Text = dateTimePicker1.Text;
        }
    }
}
```

这样就可以实现点击Label组件会显示dateTimePicker1组件的信息，这就是组件Name属性的运用，我们只需要输入这个属性就可以获取到对应这个属性的组件，获取的该组件是一个组件类型的类，在其类实例下还有其他方法以及该组件的属性，这个属性也就是上面列举出的属性，都可以进行访问与修改。


<b id='a2'></b>

### :leftwards_arrow_with_hook:C#窗体事件 ###

:arrow_up:[返回目录](#t)

在窗体中除了可以通过设置属性改变外观外，还提供了事件来方便窗体的操作。

在打开操作系统后，单击鼠标或者敲击键盘都可以在操作系统中完成不同的任务，例如双击鼠标打开“我的电脑”、在桌面上右击会出现右键菜单、单击一个文件夹后按 F2 键可以更改文件夹的名称等。

实际上这些操作都是 Windows 操作系统中的事件。

在 Windows 窗体应用程序中系统已经自定义了一些事件，在窗体属性面板中单击闪电图标即可查看到窗体中的事件，如下图所示。

![](http://c.biancheng.net/uploads/allimg/190328/4-1Z32QA630Y2.gif)

窗体中常用的事件如下表所示:

|事件	|作用|
|:--:|:---|
|Load|	窗体加载事件，在运行窗体时即可执行该事件|
|MouseClick	|鼠标单击事件|
|MouseDoubleClick|	鼠标双击事件|
|MouseMove|	鼠标移动事件|
|KeyDown|	键盘按下事件|
|KeyUp	|键盘释放事件|
|FormClosing	|窗体关闭事件，关闭窗体时发生|
|FormClosed|	窗体关闭事件，关闭窗体后发生|

如添加一个窗体打开的标签显示，首先是在窗体构主类中添加一个方法：

```C#
namespace Test
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void label1_Click(object sender, EventArgs e)
        {
            label1.Text = dateTimePicker1.Text;
        }
        //添加的加载方法
        private void loading(object sender ,EventArgs e)
        {
            label1.Text = "窗体加载完毕！";
        }
    }
}
```

这里注意的是添加的方法最好类型与给出的事件的函数一致，类型为private void，但是该方法的参数必须含有object sender ,EventArgs e。两个变量，**否者不能作为事件触发函数**，完成后,再打开事件面板，找到load方法，在旁边选择我们写的这个方法：

![](https://github.com/Lumnca/CSharp/blob/master/Images/a2.png)

同样的可以同时添加多个事件：

```C#
namespace Test
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void clickFrom(object sender, EventArgs e)
        {
            label2.Text = "窗体被点击";
        }
        private void loadForm(object sender ,EventArgs e)
        {
            label2.Text = "窗体加载完毕！";
        }
        private void inForm(object sender, EventArgs e)
        {
            label2.Text = "鼠标进入窗体";
        }
        private void outForm(object sender, EventArgs e)
        {
            label2.Text = "鼠标离开窗体";
        }
    }
}
```

然后在组件的事件面板上添加这些方法：

![](https://github.com/Lumnca/CSharp/blob/master/Images/a3.png)

完成后就可以实现鼠标移入移除的显示功能。注意的是，如果方法不能添加被选中，那就说明该方法配置错误或者该方法不能作为该事件触发类型的方法。

