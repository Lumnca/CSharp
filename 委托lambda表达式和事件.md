# :file_folder:委托，lambda表达式和事件
<p id="title"></p>

## 目录点击链接
:point_right:<a href="#one" >1引用方法<a><br>
:point_right:<a href="#two" >2委托<a><br>
:point_right:<a href="#three" >3lambda表达式<a><br>
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
   
### 2.4 Action<T>和Func<T>委托 ###
除了为每个参数和返回定义一个新委托外，还可以是用Action和Func委托，Action委托表示引用一个void返回类型的方法。可以增加T的参数来引用参数不同的方法：

```C#
        static void Main(string[] args)
        {
            //对无参数方法的引用 
            Action act = new Action(show);
            //对一个参数方法的引用
            Action<int> va = new Action<int>(show);
            act();
            va(99);
        }
        //Action只能够引用void型的方法
        static void show(){
            Console.WriteLine("Hello World!");
        }
        static void show(int i){
            Console.WriteLine(i);
        }
```
同样的Func其实就是对有返回值的方法进行引用，只不过不一样的是它要对返回类型进行传递参数；

```C#
        static void Main(string[] args)
        {
            //对Square引用，返回值为double，参数为double，所以有两个
            Func<double,double> f1 = new Func<double,double>(Square);
            var result = f1(2.5);
            Console.Write(result);

        }
        static  int Add(int a,int b) => a+b;
        static double Square(double d) => d*d;
```
对于含有2个参数的：

```C#
         Func<int ,int ,int> f2 = new Func<int ,int ,int>(Add);
         var result = f2(5,6);
         Console.Write(result);
```
### 2.5多播 ###
前面使用的每个委托都只包含一个方法调用，调用委托的次数与调用方法的次数相同。如果要调用多个方法，就需要多次显式调用这个委托，使用多播可以通过一个委托可以调用多个方法：

```C#
        static void Main(string[] args)
        {
            Action act = new Action(show);
            act += display;

            act();

        }
        static void show() => Console.WriteLine("Hello World!");
        static  void display() => Console.WriteLine("I am LMC");
```
在上面的示例中，因为要储存对两个方法的引用，所以示例化了一个委托数组。而这里只是在同一个委托上添加两个操作，多播委托可以使用运算符+和+=，另外还可以像
下面的代码一样：

```C#
        Action act1 = new Action(show);
        Action act2 = new Action(display);
        Action acts = act1 + act2;
        acts();
```
当然也可以直接在act1后面直接加上act2直接改变原来的委托。使用-和-=删除方法,但这种形式只能返回void；否则就只能得到委托调用的最后一个方法的结果。对于多播问题，一般会按照添加顺序进行，如果中途某一个出现错误，则会停止迭代，当然可以使用Delegate类定义GetInvocationList()方法，它返回一个委托数组，可以在这个数组里面使用try和catch来获取异常以及继续迭代。

### 2.6匿名方法 ###
匿名方法既是没有命名的方法，在委托的时候不传如方法，而直接写上：

```C#
        static void Main(string[] args)
        {
            string mid = ",middle part";
            Func<string,string> anonDel = delegate(string param)
            {
                param += mid;
                param +="and this was added to the string";
                return param;
            };
            Console.WriteLine(anonDel("Start of string"));
        }
```
可以看出匿名函数可以减少代码编写，不必定义，还可以引用外部的变量。使用匿名要注意两点： 在匿名方法中不能调用跳转语句如：break，goto等；在匿名方法中不能访问不安全的代码，也不能访问在匿名方法外边的ref，out参数，但可以使用在匿名方法外部定义的其他变量。

****
<h2 id = "three">:book:lambda表达式</h2>
<a href="#title">:arrow_up:返回目录</a>

对于上面的匿名方法我们可以使用lambda表达式：

```C#
            string mid = ",middle part";
            Func<string,string> lambda = param =>
            {
                param += mid;
                param +="and this was added to the string";
                return param;
            };
            Console.WriteLine(lambda("Start of string"));
```
注意lambda只是一个变量名，并不是系统自带的，你可以命名不同的名字。lambda表达式只是需要满足这样的写法即可。

### 3.1参数 ###
lambda表达式有几种定义参数的方式。如果只有一个参数，只写出参数名就足够了，代码的lambda表达式使用了参数s。因为委托类型定义了一个string参数，所以s的类型就是string。实现代码string.Format()方法来返回一个字符串，在调用该委托时，就把该字符串最终写入控制台：

```C#
     Func<string,string> oneParam = s =>
     $"change uppercase {s.ToUpper()}";
     Console.WriteLine(oneParam("usa"));
```
s为传入参数，对应Func第二个参数strig类型， =>为方法传递值，如果委托使用多个参数，就把这些参数名放在圆括号中如下：

```C#
      Func<int ,int ,int> twoParam = (x,y) => x*y;
      Console.WriteLine(twoParam(5,6));
```
### 3.2 多行代码 ###
如果lambda表达式只有一条语句，在方法块内就不需要花括号和return语句，因为编译器会自动添加，但是如果代码超过一行就需要自己写return语句如上面的匿名方法调用。

### 3.3 闭包 ###
可以通过lambda表达式访问外部的变量，这称为闭包，闭包是非常好用的功能，但是如果使用不当，也会非常危险

```C#
            int result = 0;
            Func<int ,int ,int> twoParam = (x,y) => 
            {
                //调用外部的result
                result = x*y;
                return result;
            };
            Console.WriteLine(twoParam(5,6));
```
在下面这种情况下：

```C#
    int result = 2;
    Func<int ,int ,int> twoParam = (x,y) => 
   {
      result += x*y;
      return result;
   };
   result = 5;
   Console.WriteLine(twoParam(5,6));
```
result在后面进行来修改，这就会修改初始值，改为调用result = 5 的形式，如果只是赋值，则不会有影响。








        
        










