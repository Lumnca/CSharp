# :maple_leaf:数组与元组

<p id="title"></p>

## 目录点击链接 ##
:point_right:<a href="#one" >1LINQ概述<a><br>
:point_right:<a href="#two" >2标准的查询操作符<a><br>


 ***
<p id = "one"></p>  
  
## :four_leaf_clover:LINQ概述 ##

<a href="#title">:arrow_up:返回目录</a>

LINQ(语言集成查询)在C#编程语言中集成来查询语法，可以用相同的语言访问不同的数据源。LINQ提供了不同的数据源的抽象层，所以可以使用相同的语法。

### :leaves:1.1LINQ列表和实体 ###

首先是对于数据的实现，语言查询依靠于成员数据，像下面一样，我们对一个学生类：

```C#
    public class Student
    {
        public string StudentName;
        public int StudentID;
        public string Sex;
        public int Score;

        public Student(string n,int i,string s,int c)
        {
            StudentName = n;
            StudentID = i;
            Sex = s;
            Score = c;
        }

        public void show()
        {
            Console.WriteLine($"{StudentName}'s Score :{Score}");
        }
    }
```

像这样有了数据的，我们想对数据进行筛选，就需要LINQ语法。

### :leaves:1.2LINQ查询 ###

首先我们来看一下简单的数据筛选，筛选出一个数组中为偶数的数组：

```C#
     //声明数组
     int[] Nums = new int[]{1,5,8,9,7,24,67,91,36,49,12,78};
     //LINQ声明需要System.Linq名称空间
     var qurey = from val in Nums
     //条件筛选
     where val%2==0
     //数据返回
     select val;
     
     foreach (var item in qurey)
     {
          //循环遍历符合元素
          Console.WriteLine(item);
     }
```

中间的子句from,where,orderby,descending,和select都是这个查询中预定的关键字，查询表达式必须以from子句开头，以select或group子句结束，在这个子句之间，可以使用where，orderby，join，let和其他from子句。                  

### :leaves:1.3推迟查询的执行 ###

在运行定义查询表达式时，查询就不会运行，查询数据会在迭代数据项时运行，所以在迭代进行前，插入数据会改变迭代的数据。

```C#
            var Nums = new List<int>();
            Nums.AddRange(new int[]{
                1,2,3,4,5,6
            });
            var qurey = from val in Nums
            where val%2==0
            select val;
            
            Nums.Add(7);
            Nums.Add(8);
            Nums.Add(9);
            foreach (var item in qurey)
            {
                Console.WriteLine(item);
            }
```

像上面在查询语句后面添加的7，8，9但是由于查询语句是在迭代中进行，所以7，8，9后面加在里面的查询中。

 ***
<p id = "two"></p>  
  
## :four_leaf_clover:标准的查询操作符 ##

<a href="#title">:arrow_up:返回目录</a>

Where,OrderByDescending 和Select只是LINQ定义的几个查询操作符，LINQ查询为最常用的操作符定义了一个声明语法，还有许多查询操作符可用于Enumerable类，下面表详细说明：

| 标准查询操作符 | 说明 |
|:---:|:-----|
|Where  与 `OfType<TResult>`|筛选操作符定义类返回元素的条件，在Where查询操作符中可以使用谓词，例如，lambda表达式定义的谓词，来返回布尔值，OfType`<TResult>`根据类型筛选元素，只返回TResult类型的元素。|
|Select 与 SelectMany | 投射操作符用于把对象转换为另一个类型的新对象，Select和SelectMany定义了根据选择器函数选择结果值的投射。|
|OrderBy,ThenBy,OrderByDescending,ThenByDescending,Reverse|排序操作符改变所返回的元素的顺序，OrderBy按照升序排序，OrderByDescending按照降序排序，如果第一次排序的结果很类似就可以使用ThenBy和ThenByDescending操作符进行第二次排序，Reverse反转集合元素的顺序。|
|Join,GroupJoin|连接操作符用于合并不直接相关的集合，使用Join操作符，可以根据键选择器函数连接两个集合，这类似于SQL中的JOIN，GroupJoin操作符连接两个集合，组合其结果|
|GroupBy,ToLookup|组合操作符把数据放在组中，GroupBy操作符组合有公共键的元素。ToLookup通过创建一个一对多字典，来组合元素。|

















