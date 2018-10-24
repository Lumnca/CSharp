# :maple_leaf:C#链接MySQl

<p id="title"></p>

## 目录点击链接
:point_right:<a href="#one" >1连接准备<a><br>
:point_right:<a href="#two" >2连接MySql<a><br>
:point_right:<a href="#three" >3使用MySql命令<a><br>
:point_right:<a href="#four" >4查询显示与结果<a><br>
:point_right:<a href="#five" >5数据库的其他操作<a><br>

 ***
<p id = "one"></p>  
  
## :four_leaf_clover:连接准备 ##

<a href="#title">:arrow_up:返回目录</a>

C#中没有直接链接MySQl的方法，需要下载用MySQl链接引用工具[下载地址](https://dev.mysql.com/downloads/connector/net/6.6.html#downloads)，下载后解压得到.dll引用文件，使用VS中项目一栏添加这些引用，就可以使用`using MySql.Data.MySqlClient`引用MySql方法，如果出现框架版本不符，可以在项目属性中把版本改成相应的版本。

 ***
<p id = "two"></p>  
  
## :four_leaf_clover:连接MySql ##

<a href="#title">:arrow_up:返回目录</a>

连接MySQl使用 MySqlConnection连接类

* 直接命名

```C#
     String mysqlStr = "Database=sgz;Data Source=127.0.0.1;User Id=root;Password=12345678;pooling=false;CharSet=utf8;port=3306";
     MySqlConnection mysql = new MySqlConnection(mysqlStr);
```

接下来介绍使用方法：

 mysqlStr为添加字段，该字段里面的数据参数成为连接成功的保证。
 
 这里只说明必要参数
 
 * server=localhost 连接地址本地，或者使用Source=127.0.0.1等价
 
 * user id=root 连接用户名为root
 
 * password=root 连接用户的密码
 
 * database=abc 连接数据库的名字

对于参数数据没有顺序之分，但是要以分号结束。但对于连接方式，最好采用方法连接

* 使用方法连接

```C#
    // 返回一个MySqlConnection类
    public static MySqlConnection getMySqlCon()
    {
        String mysqlStr = "Database=sgz;Data Source=127.0.0.1;User Id=root;Password=12345678;pooling=false;CharSet=utf8;port=3306";
        
        //建立连接
        MySqlConnection mysql = new MySqlConnection(mysqlStr);
        // String mySqlCon = ConfigurationManager.ConnectionStrings["MySqlCon"].ConnectionString;
        //返回对象
        return mysql;
    }
    //主方法调用
     MySqlConnection mysql = getMySqlCon();
        if(mysql!=null)
        {
            Console.WriteLine("链接成功");
        }
        else
        {
            Console.WriteLine("链接失败");
        }
```

* 也可以才外面传入参数进行连接匹配

```C#
    //参数方法
    public static MySqlConnection getMySqlCon(string s,string d,string i,string p)
    {
        String mysqlStr = $"Database={d};server={s};User Id={i};Password={p};pooling=false;CharSet=utf8;port=3306";
        // String mySqlCon = ConfigurationManager.ConnectionStrings["MySqlCon"].ConnectionString;
        MySqlConnection mysql = new MySqlConnection(mysqlStr);
        return mysql;
    }
    //调用
        string server = "localhost";
        string Database = "sgz";
        string Id = "root";
        string Password = "12345678";
        MySqlConnection mysql = getMySqlCon(server,Database,Id,Password);
        if(mysql!=null)
        {
            Console.WriteLine("链接成功");
        }
        else
        {
            Console.WriteLine("链接失败");
        }
```
像这样可以使用外部参数，还可以改为自己输入参数连接，这样就可以不让密码不被看到。

 ***
<p id = "three"></p>  
  
## :four_leaf_clover:MySQl使用命令 ##

<a href="#title">:arrow_up:返回目录</a>

连接成功后，就可以使用MySQl命令,这个要使用 MySqlCommand类的构造方法，含有两个参数，一为命名语句，二为连接数据库名称。

```C#
        string server = "localhost";
        string Database = "sgz";
        string Id = "root";
        string Password = "chuan.868";

        MySqlConnection mysql = getMySqlCon(server,Database,Id,Password);
        if(mysql!=null)Console.WriteLine("链接成功");
        else Console.WriteLine("链接失败");
        //使用连接命令
        MySqlCommand mySqlCommand = new MySqlCommand("show tables", mysql);
        if (mySqlCommand != null) Console.WriteLine("使用命令成功");
        else Console.WriteLine("未使用命令");
```

和上面的一样还可以使用方法连接

```C#
    public static MySqlCommand getSqlCommand(String sql,MySqlConnection mysql)
    {
        MySqlCommand mySqlCommand = new MySqlCommand(sql, mysql);
        // MySqlCommand mySqlCommand = new MySqlCommand(sql);
        // mySqlCommand.Connection = mysql;
        return mySqlCommand;
    }
    //主方法调用
    MySqlCommand mySqlCommand = getSqlCommand(sqlSearch, mysql);
```

***
<p id = "four"></p>  
  
## :four_leaf_clover:显示结果 ##

<a href="#title">:arrow_up:返回目录</a>

在上面使用命令后我们并看不到输出的结果，所以要使用下面几种方法：

* ExecuteScalar() 执行命令。返回结果集合中第一行第一列的元素的值
* ExecuteReader() 执行命令，返回一个类型化的IDataReader。
* ExecuteNonQuery() 执行命令但不返回结果

使用语法：

```C#
    object reader = mySqlCommand.ExecuteNonQuery();
    object reader = mySqlCommand.ExecuteReader();
    object reader = mySqlCommand.ExecuteScalar();
```

大多时候要返回一个值就要使用ExecuteScalar()

```C#
        string server = "localhost";
        string Database = "sgz";
        string Id = "root";
        string Password = "chuan.868";

        MySqlConnection mysql = getMySqlCon(server,Database,Id,Password);
        if(mysql!=null)Console.WriteLine("链接成功");
        else Console.WriteLine("链接失败");
        //打开数据库，才能进行操作
        mysql.Open();
        //使用命令
        MySqlCommand mySqlCommand = new MySqlCommand("show tables", mysql);
        if (mySqlCommand != null) Console.WriteLine("使用命令成功");
        else Console.WriteLine("未使用命令");
        //读取首个元素
        object reader = mySqlCommand.ExecuteScalar();
        Console.WriteLine(reader);
        //关闭数据库
        mysql.Close();
```

如果是是对表的出来需要大量数据时要使用ExecuteReader()

```C#
        string server = "localhost";
        string Database = "sgz";
        string Id = "root";
        string Password = "chuan.868";

        MySqlConnection mysql = getMySqlCon(server,Database,Id,Password);
        if(mysql!=null)Console.WriteLine("链接成功");
        else Console.WriteLine("链接失败");
        mysql.Open();
        //表的查询语句
        MySqlCommand mySqlCommand = new MySqlCommand("select * from wj", mysql);
        if (mySqlCommand != null) Console.WriteLine("使用命令成功");
        else Console.WriteLine("未使用命令");
        //建立查询对象
        var reader = mySqlCommand.ExecuteReader();
        //循环读取
        while(reader.Read())
        {
            //有一行读取一行
            if(reader.HasRows)
            {
                //匹配表的第一列数据并强制转换string类型
                string s1 = reader.GetString(0);
                //匹配表的第二列数据并强制转换string类型
                string s2 = reader.GetString(1);
                //匹配表的第三列数据并强制转换int类型
                int s3 = reader.GetInt16(2);
                //匹配表的第四列数据并强制转换int类型
                int s4 = reader.GetInt16(3);
                
                Console.WriteLine($"{s1}    {s2}     {s3}    {s4}");
            }
        }
        mysql.Close();
```

对于匹配表的参数，要自己在已经知道了表的结构组成下，使用最合适，才不会有匹配的缺漏，已经数据类型的错误。如果对列的数据类型不是熟悉，也可以使用数组索引的方法显示：

```C#
        var reader = mySqlCommand.ExecuteReader();
        while(reader.Read())
        {
            if(reader.HasRows)
            {
                //直接输出
                Console.WriteLine($"{reader[0]}    {reader[1]}       {reader[2]}    {reader[3]}    ");
            }
        }
 ```
***
<p id = "five"></p>  
  
## :four_leaf_clover:其他操作 ##

<a href="#title">:arrow_up:返回目录</a>

出来上述所说，还有数据库的增删改，这些都是MySQl命令的延时，与上面使用命令方法一致，只要改变语法即可。

```C#
        //MySQl插入语句
        string SqlInsert = @"insert into wj values('自由人','蜀',99,99,99,99)";

        MySqlConnection mysql = getMySqlCon(server,Database,Id,Password);

        mysql.Open();
        //建立插入语句
        MySqlCommand mySqlCommand1 = new MySqlCommand(SqlInsert, mysql);
        //执行操作
        var useSql = mySqlCommand1.ExecuteNonQuery();
```
ExecuteNonQuery()对象为执行语句不返回语句，适合执行MySQl语句。同理删除和修改一样用这个方法实现

```C#
        String sql= "update wj set name='神人' where name='自由人';";

        MySqlConnection mysql = getMySqlCon(server,Database,Id,Password);

        mysql.Open();
        MySqlCommand mySqlCommand1 = new MySqlCommand(sql, mysql);
        var useSql = mySqlCommand1.ExecuteNonQuery();  
```
总的来说就是要语句正确，数据库使用打开，并使用方法完成操作
