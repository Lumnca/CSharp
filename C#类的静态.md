# :file_folder:C#类的静态
<p id="title"></p>
<h2>目录</h2>
:point_right:<a href="#a1">1.1静态变量<a><br>
:point_right:<a href="#a2">1.2静态类<a><br>
:point_right:<a href="#a3">1.3静态方法<a><br>
:point_right:<a href="#a4">1.4静态构造函数<a><br>
<p id="a1"></p><br>

## :blue_book:1.1静态变量
<a href="#title">:arrow_up:返回目录</a><br>
静态变量使用 static 修饰符进行声明，在类被实例化时创建，通过类进行访问不带有 static 修饰符声明的变量称做非静态变量.<br>

static变量在对象被实例化时创建，通过对象进行访问一个类的所有实例的同一C#静态变量都是同一个值,同一个类的不同实例的同一非静态变量可以是不同的值.
静态函数的实现里不能使用非静态成员,如非静态变量、非静态函数等.<br>
我们来通过类来说明一下,先声明一个Point类<br>

```C#
    class Point
    {
        static private int x;
        private int y;
        public void SetX(int a)
        {
            x = a;
        }
        public void SetY(int a)
        {
            y = a;
        }
        public void show()
        {
            Console.WriteLine("X:"+x+'\n'+"Y:"+y);
        }
    }
```
然后我们再实例化

```C#
  var p1= new Point();
  var p2 = new Point();
  p1.SetX(1);
  p1.SetY(2);
  p1.show();
  p2.SetX(3);
  p2.SetY(4);
  p2.show();
```
在这里p1的show()显示的是X:1,Y:2，p2的show()显示的是X:3,Y:4.和我们赋值的是一样的,但是如果我们对p1再次调用show(),你会发现p1的X为3,而不是刚开始的1.
我们继续赋值：
```C#
  p1.SetX(7);
  p1.SetY(8);
  p1.show();//这里p1的X：7,Y：8
  p2.show();//这里p2的X：7,Y：4
  p1.show();//这里p2的X：7,Y：8
  p2.SetX(10);
  p1.show();//这里p1的X：10,Y：8
  p2.show();//这里p2的X：10,Y：4  
```
这里就说明了p1和p2其实用的都是X是一样的值,也就是说对于静态变量来说,所有的实例化对象对这个变量都是相同的引用,比如下面这个类,我们想通过用了静态变量来得到最高分:<br>
```C#
    class RecordScore
    {
        private static int MaxScore{get;set;}=-1;
        public void SexScore(int score)
        {
            if(score>MaxScore)
            {
                MaxScore =score;
            }
        }
        public void ShowMaxScore()
        {
            Console.WriteLine("最高成绩为:"+MaxScore);
        }
    }
```

然后再实例化对象:
```C#
         var r1 = new RecordScore();
         var r2 = new RecordScore();
         var r3 = new RecordScore();
         r1.SexScore(94);
         r2.SexScore(97);
         r3.SexScore(95);
         r1.ShowMaxScore();//显示97
```
这里是因为我们通过判断是否为最大值再允许MaxScore被赋值,从而就达到了记录最大值.这种用法可以用到学生成绩管理中,在实例化对象时记录分数,然后在每个学生对象中都能查询最高分,静态变量核心就是想每个对象的这个量是统一的,是每个对象都拥有的一个相同值的变量.
<p id="a2"></p>

## :blue_book:1.2静态类
<a href="#title">:arrow_up:返回目录</a><br>
声明为static，它仅包含静态成员，不能用new静态类的实例。使用静态类来包含不与特定对象关联的方法。<br>
功能：仅包含静态成员，不能被实例化，是密封的，不能包含实例构造函数，可包含静态构造函数以分配初始值或设置某个静态变量。<br>
优点：编译器能够执行检查以确保不致偶然地添加势力成员。编译器将保证不会创建此类的实例。<br>
其实这一类的运用很多,我们使用的Math就是这样的类型,下面我们来编写自己的Math类:
```C#
    static class MyMath
    {
        //注意全部是静态成员
        public static double e{get;set;} = 2.71818;
        public static int cube(int x)
        {
            return x*x*x;
        }
    }
```
这里我只写了一个自然常数e,和一个计算三次方的cube方法.下面我们在主方法中调用.
```C#
        int a;
        //方法调用
        a = MyMath.cube(5);
        //字段调用
        Console.WriteLine(MyMath.e);
        Console.WriteLine(a);
```
这里我们不能将对象实例化,而直接使用类名调用,与我们用Math的方式一样,这种适用于不用实例化对象的类型.
<p id="a3"></p>

## :blue_book:1.3静态方法
<a href="#title">:arrow_up:返回目录</a><br>
是一种特殊的成员方法，不属于类的某一个具体的实例.非静态方法可以访问类中的任何成员，而静态只能访问类中的静态成员。<br>
如下面声明一个点类:
```C#
    class Point
    {
        public double x;
        public double y;
        public Point(double a,double b)
        {
            x = a;
            y = b;
        }
        public static double distance(Point p1,Point p2)
        {
            return Math.Sqrt((p1.x-p2.x)*(p1.x-p2.x)+(p1.y-p2.y)*(p1.y-p2.y));
        }
    }
```
这里面有一个计算两点距离的静态 distance方法,因为这个方法是需要两个点,而且这个方法我们不想给每个对象,就需要使用静态方法.调用方法也是直接引用类名调用：
```C#
        var p1 = new Point(0,0);
        var p2 = new Point(3,4);
        var len = Point.distance(p1,p2);
        Console.WriteLine(len);
```
这样我们计算的距离就是一个脱离对象实例化的数据,也就是说这个方法成员调用不到,是属于类的方法.这种方法可以减少内存消耗和程序的美化.
<p id="a4"></p>

## :blue_book:1.4静态构造函数
<a href="#title">:arrow_up:返回目录</a><br>
用于对静态字段、只读字段等的初始化。添加static关键字，不能添加访问修饰符，因为静态构造函数都是私有的，类的静态构造函数在给定应用程序域中至多执行一次：只有创建类的实例或者引用类的任何静态成员才激发静态构造函数静态构造函数是不可继承的，而且不能被直接调用。如我们创立一个原点类:
```C#
    class OPoint
    {
        //成员只读
        public static readonly double x;
        public static readonly double y;
        //静态构造函数
        static OPoint()
        {
            x = 0;
            y = 0;
            Console.WriteLine("("+x+","+y+")");
        }
    }
```
然后我们再实例化对象：
```C#
        var o1 = new OPoint();
        var o2 = new OPoint();
```
这里不需要调用,在你实例化对象时就直接能够显示原点,而且不管你实例化多少对象,他都只能显示一个,也就是说这个构造函数只能执行一次.而且也只能只是第一次调用时比如,我们改上面程序的构造函数主体：
```C#
        static OPoint()
        {
            x = 0;
            y = 0;
            Console.WriteLine("("+x+","+y+")");
            x +=1;
            y +=1;
        }
```
看这个方法实际是想让在实例化一个对象后,让后面的实例化对象的x，y依次加1.然后实际上只有一个(0,0)其他的实例化对象不会调用这个,从而说明了这个静态构造函数只调用一次,这种方式可以用在我们想对第一次的记录,从而达到某种效果.
#### 本章类容已完结,可能以后会有修改，如有问题或者错误可以联系作者.  Email：724119519@qq.com :bowtie::bowtie::bowtie: ####
#### 最近更新时间：2018.8.31 16：34 ####
