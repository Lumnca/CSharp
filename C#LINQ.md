# :maple_leaf:数组与元组

<p id="title"></p>

## 目录点击链接 ##
:point_right:<a href="#one" >1LINQ概述<a><br>
:point_right:<a href="#two" >2标准的查询操作符<a><br>
:point_right:<a href="#three" >3集合操作<a><br>
:point_right:<a href="#four" >4分区<a><br>


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

### :leaves:2.1筛选 ###

首先是基于成员数据的条件筛选，使用where子句，可以合并多个表达式，例如，找出1-10大于5且为偶数的数据，转递给where子句的表达式的结果类型应是布尔类型，下面是LINQ表达式：

```C#
            var Nums = new List<int>();

            Nums.AddRange(new int[]{
                1,2,3,4,5,6,7,8,9,10
            });
            var qurey = from val in Nums
            where val%2==0&&val>5
            select val;

            foreach (var item in qurey)
            {
                Console.WriteLine(item);
            }
 ```
 还可以使用lambda表达式：
 
 ```C#
            var Nums = new List<int>();

            Nums.AddRange(new int[]{
                1,2,3,4,5,6,7,8,9,10
            });
            //引入lambda表达式用属性扩展
            var qurey = Nums.Where(n=>n>5&&n%2==0).Select(n=>n);

            foreach (var item in qurey)
            {
                Console.WriteLine(item);
            }
 ```
 
 上面是用成员数据条件来筛选数据，还可以使用索引值筛选：
 
 ```C#
            var Students = new List<Student>();

            Students.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",574),
                new Student("Bob",20170212,"男",549),
                new Student("Kay",20170317,"男",567),
                new Student("Karas",20170132,"男",571),
                new Student("Marry",20170209,"女",561),
                new Student("Salfy",20170341,"女",548),
                new Student("Lumnca",20170401,"男",579)
            });
            //传入两个参数，第一个为对象，第二个为索引值
            var Stu = Students.Where((s,index)=>s.StudentName.StartsWith("K")&&index%2==0);

            foreach (var item in Stu)
            {
                Console.WriteLine(item.StudentName);
                //返回Kay，索引值从0开始，按照列表加下来索引值为2的倍速且名字以K开头
            }
```
第三个是基于类型的筛选，使用OfType()扩展方法，这里数组据包含string和int对象，使用OfType扩展方法，把string传给范型参数：

```C#
     Object[] data = {"A","Heelo World",56,new Student("as",12,"ac",56),23.6,"###"};
       //范型约束条件
     var query = data.OfType<string>();

     foreach (var item in query)
     {
         Console.WriteLine(item);
     } 
```
第四个是基于对象数组里面的数组查询，使用from复合语句，如查询一堆学生中一堆成绩中，超过平均分的成绩，先下定义学生类：

```C#
    public class Student
    {
        public string StudentName;
        public int StudentID;
        public string Sex;
        public int[] Score;
        public int Avg{get;set;}=0;

        public Student(string n,int i,string s,int[] c)
        {
            StudentName = n;
            StudentID = i;
            Sex = s;
            Score = c;
            foreach (var item in c)
            {
                Avg+=item;
            }
            Avg=Avg/3;
        }
```
使用from复合语句
```C#
            var Students = new List<Student>();

            Students.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97})
            });
            //s,代表学生类，c代表成绩数据堆
            var Stu = from s in Students
            from c in s.Score
            where c > s.Avg
            //自定义返回格式
            select s.StudentName + "`s over avg:" + c;

            foreach (var item in Stu)
            {
                Console.WriteLine(item);
            }
 ```

与上面一样的结果还可以使用lambda表达式：

```C#
      //使用SelectMany()方法复合
      var query = Students.SelectMany(S=>S.Score,(C,S)=>new{AllScore = S,Stu = C})
      // S 推入 S的分数数组，C推入整个学生类，，构造新的隐式类含两个参数AllScore，Stu，分别代表前面推入的内容，
      .Where(St=>St.AllScore > St.Stu.Avg)
      //使用上面构造的隐式类
      .Select(St=>St.Stu.StudentName+"over avgscore have there :"+St.AllScore);
 ```

还有对筛选的数据排序功能如上面表说的一样，使用orderby降序排布，orderdescending升序排布

```C#
            var Students = new List<Student>();

            Students.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Karas",20170217,"男",new int[]{84,77,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),
                new Student("Rite",20170236,"女",new int[]{86,91,88})
            });

            var Stu = from s in Students
            //排布参数传入多个，如果检索到相等的就检索下一个参数排布，也可以使用二次排布
            orderby s.Score[0],s.Score[2],s.Score[1]
            select s.StudentID+"    "+ s.Score[0]+"  "+s.Score[2]+"   "+s.Score[1];


            foreach (var item in Stu)
            {
                Console.WriteLine(item);
            }
```
接下来于SQL一样的还有分组group子句, 如对上面例子的性别统计如下:

```C#
            var query = from s in Students
            
            //以Sex为分组，一样的分到一组
            group s by s.Sex  into  Stu
            
            orderby Stu.Count()
            select new {
                //Key代表分组值
                Sex = Stu.Key,
                Num = Stu.Count()
            };


            foreach (var item in query)
            {
                Console.WriteLine(item.Sex+"  :  "+item.Num);
            }
```

当然也可以对分组限定,使用lambda表达式：

```C#
     var query = Students.GroupBy(n=>n.Avg).Where(n=>n.Key>80).OrderBy(s=>s.Key).Select(s=> new{
         Sex = s.Key,
         Num = s.Count(),
     });
```
### leaves:2.2对嵌套的对象分组 ###

如果分组的对象包括一个可以分组的对象，可以在select语句里面添加匿名类型：

```C#
     var query = from r in Students
     group r by r.StudentName into r1
     let count = r1.Count()
     select new {
                 Name = r1.Key,
                 Num = count,
                 //嵌套
                 AllScore = from r2 in r1
                 orderby r2.Score[0],r2.Score[1]
                 select r2.Score[0]+"   "+r2.Score[1]+"    "+r2.Score[2]
     };


     foreach (var item in query)
     {
          Console.WriteLine(item.Name+"  ");
          //嵌套循环
          foreach (var val in item.AllScore)
          {
               Console.WriteLine(val);
          }
    }
```
### leaves:2.3内链接 ###

使用join子句可以根据特定的条件合并两个数据源，但之前要获得两个连接的列表:

```C#
            //两个必须要有一个相同的索引值
            var Address = new Dictionary<int,string>(){
                [20170105] = "成都",
                [20170212] = "重庆",
                [20170317] = "武汉",
                [20170132] = "长沙",
                [20170217] = "南昌",
                [20170347] = "杭州",
                [20170234] = "上海",
                [20170119] = "北京",
                [20170236] = "南京"
            };
            var top = from t in Address
            select new {
                ID = t.Key,
                Add = t.Value
            };
            var stu = from s in Students
            select new{
                AvgNum = s.Avg,
                ID = s.StudentID,
                Name = s.StudentName
            };

            var StudentTop = from t in top join s in stu on t.ID equals s.ID
            select new{
                Name = s.Name,
                Address = t.Add,
                ID = s.ID
            };

            foreach (var item in StudentTop)
            {
                Console.WriteLine(item.Name+"   "+item.ID+"   "+item.Address);
            }
```

当然看个人习惯可以写在一个LINQ语句里面。

使用take属性可以限制输出个

```C#
    var StudentTop = (from t in top join s in stu on t.ID equals s.ID
    select new{
          Name = s.Name,
          Address = t.Add,
          ID = s.ID
    }).Take(4);
```

### leaves:2.4左外连接 ###

上面的内链接只能匹配到具有相同ID的学生，如果还有没有匹配学生的ID，则不会现实，使用左外连接可以实现没有匹配的数据:

```C#
            var Students = new List<Student>();

            Students.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Karas",20170217,"男",new int[]{84,77,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),
                new Student("Rite",20170236,"女",new int[]{86,91,88}),
                new Student("Helar",20170219,"男",new int[]{86,97,83}),
                new Student("Jaer",20170209,"女",new int[]{81,87,83}),
                new Student("Helar",20170333,"女",new int[]{78,97,99})
            });

            var Address = new Dictionary<int,string>(){
                [20170105] = "成都",
                [20170212] = "重庆",
                [20170317] = "武汉",
                [20170132] = "长沙",
                [20170217] = "南昌",
                [20170347] = "杭州",
                [20170234] = "上海",
                [20170119] = "北京",
                [20170236] = "南京",
                [20170403] = "西安",
                [20170423] = "福州"
            };
            var top = from t in Address
            select new {
                ID = t.Key,
                Add = t.Value
            };
            var stu = from s in Students
            select new{
                AvgNum = s.Avg,
                ID = s.StudentID,
                Name = s.StudentName
            };

            var StudentTop = (from s in stu join t in top on s.ID equals t.ID into st//为匹配别名
            from address in st.DefaultIfEmpty()//address为匹配参数
            orderby s.Name
            select new{
                Name = s.Name,
                Address = address ==null ? "没有匹配到地址" : address.Add,//未匹配参数返回null ，
                ID = s.ID
            }).Take(20);

            foreach (var item in StudentTop)
            {
                Console.WriteLine(item.Name+"   "+item.ID+"   "+item.Address);
            }
```

当然也有右连接在表的合并时交换顺序即可完成右连接


### leaves:2.5 组连接###

上面的连接都是基于一对一的属性，如果需要多对属性匹配，则需要使用组连接用法与上面的类似，这里就简单给出一个为代码：

```C#
    var q = (from r in XXX join r2 in XXXX on 
    new{
      one = XXX.valueone,
      two = XXX.valuetwo,
      ...
    }
    equals
    new{
    one= XXXX.valueone,
    two = XXXX.valuetwo,
    ...
    }
    into item
    select new{
    ...
    };
```
像这样就可以匹配两个列表的多个匹配项,并将他们传入item中

 ***
<p id = "three"></p>  
  
## :four_leaf_clover:集合操作##

<a href="#title">:arrow_up:返回目录</a>

扩展方法Distinct()，Union(),Intersect(),Except(),Zip()都是集合操作下表说明作用：

|集合操作符|说明|
|:--:|:--:|
|Disinct()|返回序列中通过使用指定的非重复元素，去掉重复元素，子需要一个集合就可以|
|Union()|可以连接两个集合，组合其结果|
|Intersect()|返回两个组合都有的元素|
|Except()|返回只出现在其中一个集合中的元素|
|Zip()|把两个集合合并为一个|

* Disinct() 去除单个集合中重复的元素

```C#
            var SomeStudents = new List<Student>();

            var p1 = new Student("Tom",20170234,"男",new int[]{85,76,86});
            var p2 = p1;
            SomeStudents.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                p2,
                p1,
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Karas",20170217,"男",new int[]{84,77,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
            });
            //去重只对相同引用有效，对于属性相同不能去除如p1与p2为相同引用，所以只能得到一个。
            var newstudents = SomeStudents.Distinct<Student>();
            foreach (var item in newstudents)
            {
                Console.WriteLine(item.StudentName);
            }
```

* Unoin()连接两个集合，组合其结果，去掉重复元素,两个集合要为同类型,

```C#
            var SomeStudents = new List<Student>();
            var OtherStudents = new List<Student>();
            var p1 = new Student("Tom",20170234,"男",new int[]{85,76,86});
            var p2 = p1;
            SomeStudents.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                p2,
                p1,
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Karas",20170217,"男",new int[]{84,77,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
            });
            OtherStudents.AddRange(new Student[]{
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),
                new Student("Rite",20170236,"女",new int[]{86,91,88}),
                new Student("Helar",20170219,"男",new int[]{86,97,83}),
                new Student("Jaer",20170209,"女",new int[]{81,87,83}),
                new Student("Helar",20170333,"女",new int[]{78,97,99}),
                p1
            });
            //添加另一个集合
            var AllStudents = SomeStudents.Union(OtherStudents);

            foreach (var item in AllStudents)
            {
                Console.WriteLine(item.StudentName);
            }
```

* Intersect()，返回两个集合中共有元素组合的集合

```C#
            var SomeStudents = new List<Student>();
            var OtherStudents = new List<Student>();

            var p1 = new Student("Tom",20170234,"男",new int[]{85,76,86});
            var p2 = p1;
            SomeStudents.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                p2,
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Karas",20170217,"男",new int[]{84,77,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
            });

            OtherStudents.AddRange(new Student[]{
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),
                new Student("Rite",20170236,"女",new int[]{86,91,88}),
                new Student("Helar",20170219,"男",new int[]{86,97,83}),
                new Student("Jaer",20170209,"女",new int[]{81,87,83}),
                new Student("Helar",20170333,"女",new int[]{78,97,99}),
                p1
            });
            //只返回p1与p2这两个相同的引用
            var AllStudents = SomeStudents.Intersect(OtherStudents).OrderBy(r=>r.StudentName);
            foreach (var item in AllStudents)
            {
                Console.WriteLine(item.StudentName);
            }
```

* Zip()合并两个集合，两个不同类型的集合

```C#
            var SomeStudents = new List<Student>();
            var OtherStudents = new List<Student>();

            var p1 = new Student("Tom",20170234,"男",new int[]{85,76,86});
            var p2 = p1;
            SomeStudents.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                p2,
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),
                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Karas",20170217,"男",new int[]{84,77,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
            });

            OtherStudents.AddRange(new Student[]{
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),
                new Student("Rite",20170236,"女",new int[]{86,91,88}),
                new Student("Helar",20170219,"男",new int[]{86,97,83}),
                new Student("Jaer",20170209,"女",new int[]{81,87,83}),
                new Student("Helar",20170333,"女",new int[]{78,97,99}),
                p1
            });
            var Address = new Dictionary<int,string>(){
                [20170105] = "成都",
                [20170212] = "重庆",
                [20170317] = "武汉",
                [20170132] = "长沙",
                [20170217] = "南昌",
                [20170347] = "杭州",
                [20170234] = "上海",
                [20170119] = "北京",
                [20170236] = "南京",
                [20170403] = "西安",
                [20170423] = "福州"
            };

            var AllStudents = SomeStudents.Union(OtherStudents);
            var AllInformation = AllStudents.Zip(Address,(s,a)=>s.StudentName + "  Address:   "+a.Value);
            
            //以两边从前到后的顺序排布
            foreach (var item in AllInformation)
            {
                Console.WriteLine(item);
            }

```

 ***
<p id = "four"></p>  
  
## :four_leaf_clover:分区##

<a href="#title">:arrow_up:返回目录</a>

扩展方法Take()和Skip()等的分区操作可用于分页，比如要对信息进行4个一排布：

```C#
            
            var SomeStudents = new List<Student>();
 
            SomeStudents.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),

                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),

                new Student("Rite",20170236,"女",new int[]{86,91,88}),
                new Student("Helar",20170219,"男",new int[]{86,97,83}),
                new Student("Jaer",20170209,"女",new int[]{81,87,83}),
                new Student("Helar",20170333,"女",new int[]{78,97,99}),
            });

            int PageSize = 4;
            int StudentNum = SomeStudents.Count()/PageSize;

            for(int i=0;i<StudentNum;i++)
            {
                int x = 1;
                Console.WriteLine($"-------------{i+1}-------------");
                var s = SomeStudents.Skip(i*PageSize).Take(PageSize);
                foreach (var item in s)
                {

                    Console.WriteLine($"{x++}  :   {item.StudentName}");
                }
            }
```

Skip(i)表示跳过i个数据，Take(i)表示显示i个数据。像上面那样就可以实现规定输出。


 ***
<p id = "five"></p>  
  
## :four_leaf_clover:其他操作符##

<a href="#title">:arrow_up:返回目录</a>

### leaves:4.1聚合操作符 ###

聚合操作符不返回一个序列，而返回一个值。

* Count() 返回集合的项数

```C#
  let num = SomeStudents.Count();
  //或者
  int num =SomeStudents.Count();
```

* Sum()汇总序列中所以数字的和,对前面的例子中的分数数组进行求和

```C#
            var StudentSum =   from s in SomeStudents
            select new{
                Name = s.StudentName,
                Sum = s.Score.Sum()
            };

            foreach (var item in StudentSum)
            {
                Console.WriteLine($"{item.Name}  SumScore:  {item.Sum}");
            }
```
当然还有Max(),Min(),Average()的使用方式与Count()或Sum()相同，这里就不一一列举了。

### leaves:4.2转换操作符 ###

前面说过查询可以推迟到访问数据项时进行，在使用迭代中使用查询时，查询会执行。而使用转换操作符会立即执行查询操作，把查询结果放在数组，列表或者字典中。

```C#
            var SomeStudents = new List<Student>();
 
            SomeStudents.AddRange(new Student[]{
                new Student("Sarry",20170105,"女",new int[]{74,89,86}),
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Bob",20170212,"男",new int[]{79,86,67}),
                new Student("Kay",20170317,"男",new int[]{88,96,58}),

                new Student("Karas",20170132,"男",new int[]{84,71,97}),
                new Student("Poacs",20170347,"女",new int[]{84,72,91}),
                new Student("Tom",20170234,"男",new int[]{85,76,86}),
                new Student("Griop",20170119,"男",new int[]{88,90,77}),

                new Student("Rite",20170236,"女",new int[]{86,91,88}),
                new Student("Helar",20170219,"男",new int[]{86,97,83}),
                new Student("Jaer",20170209,"女",new int[]{81,87,83}),
                new Student("Helar",20170333,"女",new int[]{78,97,99}),
            });

            var StudentSum =(from s in SomeStudents
            select new{
                Name = s.StudentName,
                Sum = s.Score.Sum()
            }).ToList();

             SomeStudents.AddRange(new Student[]{
                 new Student("Lmc",20170329,"男",new int[]{79,99,99})
             });
            foreach (var item in StudentSum)
            {
                Console.WriteLine($"{item.Name}  SumScore:  {item.Sum}");
            }
```
像这样上面在LINQ语句后面添加的语句就无效了，同样的还有其他转换语句.

```C#

