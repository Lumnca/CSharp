# :file_folder:委托，lambda表达式和事件
<p id="title"></p>

## 目录点击链接
:point_right:<a href="#one" >1引用方法<a><br>
:point_right:<a href="#two" >2委托<a><br>
:point_right:<a href="#three" ><a><br>
:point_right:<a href="#four" ><a><br>

 
<h2 id = "one">:book:引用方法</h2>
<a href="#title">:arrow_up:返回目录</a>

委托是寻址的方法的.NET方法版本，在C++中，函数指针只不过是一个指向内存位置的指针，它不是类型安全的。我们无法判断这个指针实际指向什么，像参数和返回类型等
项就更无从知晓的了。而.NET委托安全不同；委托是类型安全的类，它定义，它定义类返回和参数的类型，委托类不仅包含对方法的引用，也可以包含对多个方法的引用

lambda表达式与委托直接相关，当参数是委托类型时，就可以使用lambda表达式实现委托引用的方法。

****
<h2 id = "two">:book:委托</h2>
<a href="#title">:arrow_up:返回目录</a>

### 2.1声明委托 ###

在C#中使用一个类时，分为两个操作阶段。首先，需要定义这个类，即告诉编译器这个类由什么组成。然后实例化该类一个对象（静态不需要），使用委托时，也需要经过这两个步骤，首先定义要使用委托，对于委托，定义它就是告诉编译器这种类型的委托表示那种类型的方法。然后，必须创建该委托的一个或多个实例。编译器在后台将创建表示该委托的一个类。声明语法如下：

```C#
    delegate void IntMethodInvoker(int x);
```
在这个示例中，声明类一个委托IntMethodInvoker，并指定该委托的每个实例都可以包含一个方法的引用，该方法带有一个int参数，并返回void。理解委托的一个要点是他们的类型安全性非常高，在定义委托时，必须给出它所示的方法的签名和返回类型等全部细节。

假定要定义一个委托TwoLongsOp，该委托表示的方法有两个long参数，返回类型为double，可以编写下面的代码：

```C#
    delegate double TwoLongOp(long first,long second);
```
或者要定义一个委托，它表示的方法不带参数，返回一个string型的值：

```C#
    delegate string GetAString();
```
其定义语法与方法类似，且定义的前面要加关键字 delegate ，因为委托基本上一个新类，所以可以在定义类的任何地方定义委托，也就是说可以在另一个类里面定义委托，也可以在任何类的外部定义，还可以在名称空间中把委托定义为顶层对象，根据委托的可见和作用域加上修饰符： public private 等

### 2.2使用委托 ###

下面一段代码为使用过程：

```C#
        private delegate string GetAString();
        static void Main(string[] args)
        {
            int x = 40;
            GetAString firstMethod = new GetAString(x.ToString);
            Console.WriteLine($"String is : {firstMethod ()}");
        }
```
在这段代码中，实例化类型为GetAString的委托，并对其初始化，使其引用整形变量x的ToString（）方法，在C#中，委托在语法上总是接受一个参数的构造函数。这个参数就是委托引用的方法，这个方法必须匹配最初定义委托的签名，所以在这个示例中，如果用不带参数并返回一个字符串类型的方法来初始化firstMethod变量，就会编译错误。注意，因为Tostring是一个实例方法，所以需要实例X来引用。

为了减少输出，在需要委托实例的每个位置可以只传送地址的名称。这称为委托推断：

```C#
    GetAString firstMethod = x.ToString;
```
与上面得到是一样的效果。
### 2.3委托示例 ###

下面实现类简单的示例：

```C#
    //委托声明
    delegate double DoubleOp(double x);
    class Program
    {
        static void Main(string[] args)
        {
            //委托实例，用数组委托
            DoubleOp[] operations = {
                MathOperations.DoubleNum,
                MathOperations.Square
            };
            for(int i=0;i<operations.Length;i++)
            {
                ProcessAndDisplayNumber(operations[i],3.0);
                ProcessAndDisplayNumber(operations[i],5.0);
                Console.WriteLine();
            }
            //委托实例用变量引用
            DoubleOp operation1 = new DoubleOp(MathOperations.DoubleNum);
            var result = operation1(0.6);
            Console.WriteLine(result);
             
        }
            //委托方法调用
        static void ProcessAndDisplayNumber(DoubleOp action,double value)
        {
            //实现调用
            double result = action(value);
            Console.WriteLine($"Value is {value}, result of  operation is {result}");
        }
    }
    class MathOperations
    {
        public static double DoubleNum(double value)=> value * 2;
        public static double Square(double value)=>value * value;
    }
```
这里实例化的是数组，数组的每个元素都指向MathOperations不同的方法调用，最后用一个方法来调用委托，以及委托的值。   
        
        
        
        
        
