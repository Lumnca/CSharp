## 1.1静态变量
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
这里就说明了p1和p2其实用的都是X是一样的值,也就是说对于静态变量来说,所有的实例化对象对这个变量都是相同的引用<br>
#### 本章类容还在不断更新中......如有问题或者错误可以联系作者.  Email：724119519@qq.com :bowtie::bowtie::bowtie: ####
#### 最近更新时间：2018.8.30 23：00 ####
