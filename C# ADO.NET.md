# :twisted_rightwards_arrows:C# ADO.NET #

<b id='t'></b>

:checkered_flag:[ADO.NET](#a1)

:checkered_flag:[Connection类](#a2)

:checkered_flag:[Command类](#a3)

:checkered_flag:[DataReader 类](#a4)

<b id='a1'></b>

### :leftwards_arrow_with_hook:ADO.NET ###

:arrow_up:[返回目录](#t)

在 C# 语言中 ADO.NET 是在 ADO 的基础上发展起来的，ADO (Active Data Object) 是一个 COM 组件类库，用于访问数据库，而 ADO.NET 是在 .NET 平台上访问数据库的组件。

ADO.NET 是以 ODBC (Open Database Connectivity) 技术的方式来访问数据库的一种技术。

ADO.NET 中的常用命名空间如下表所示。

|命名空间	|数据提供程序|
|:--:|:-------------|
|System.Data.SqlClient	|Microsoft SQL Server|
|System.Data.Odbc |	ODBC|
|System.Data.OracleClient|	Oracle|
|System.Data.OleDb	|OLE DB|

`在使用 ADO.NET 进行数据库操作时通常会用到 5 个类，分别是 Connection 类、 Command 类、DataReader 类、DataAdapter 类、DataSet 类。`

`在接下来的讲解中我们将以连接 SQL Server 为例介绍 ADO.NET 中的对象，引用的命名空间为 System.Data.SqlClient。`

`除了 DataSet 类以外，其他对象的前面都加上 Sql，即 SqlConnection、SqlCommand、SqlDataReader、SqlDataAdapter。`

**1) Connection 类**

该类主要用于数据库中建立连接和断开连接的操作，并且能通过该类获取当前数据库连接的状态。

使用 Connection 类根据数据库的连接串能连接任意数据库，例如 SQLServer、Oracle、MySQL 等。

但是在 .NET 平台下，由于提供了一个 SQL Server 数据库，并额外提供了一些操作菜单便于操作，所以推荐使用 SQLServer 数据库。

**2) Command 类**

该类主要对数据库执行增加、删除、修改以及查询的操作。

通过在 Command 类的对象中传入不同的 SQL 语句，并调用相应的方法来执行 SQL 语句。

**3) DataReader 类**

该类用于读取从数据库中查询出来的数据，但在读取数据时仅能向前读不能向后读， 并且不能修改该类对象中的值。

在与数据库的连接中断时，该类对象中的值也随之被清除。

**4) DataAdapter 类**

该类与 DataSet 联用，它主要用于将数据库的结果运送到 DataSet 中保存。

DataAdapter 可以看作是数据库与 DataSet 的一个桥梁，不仅可以将数据库中的操作结果运送到 DataSet 中，也能将更改后的 DataSet 保存到数据库中。

**5) DataSet 类**

该类与 DataReader 类似，都用于存放对数据库查询的结果。

不同的是，DataSet 类中的值不仅可以重复多次读取，还可以通过更改 DataSet 中的值更改数据库中的值。

此外，DataSet 类中的值在数据库断开连接的情况下依然可以保留原来的值。

<b id='a2'></b>

### :leftwards_arrow_with_hook:Connection类 ###

:arrow_up:[返回目录](#t)

Connection 类概述
Connection 类根据要访问的数据和访问方式不同，使用的命名空间也不同，类名也稍有区别，在这里我们使用的是 SqlConnection 类，以及微软提供的 SQL Server 2014 数据库。

SqlConnection 类中提供的常用属性和方法如下表所示。

|属性或方法|	说明|
|:--:|:--------|
|SqlConnection()|	无参构造方法|
|SqlConnection(string connectionstring)|	带参数的构造方法，数据库连接字符串作为参数|
|Connectionstring|	属性，获取或设置数据库的连接串|
|State	|属性，获取当前数据库的状态，由枚举类型 Connectionstate 为其提供值|
|ConnectionTimeout|	属性，获取在尝试连接时终止尝试并生成错误之前所等待的时间|
|DataSource|	属性，获取要连接的 SQL Server 的实例名|
|Open()|	方法，打开一个数据库连接|
|Close()|	方法，关闭数据库连接|
|BeginTransaction()	|方法，开始一个数据库事务|


`使用 Connection 类连接数据库,在使用 Connection 类连接 SQL Server 2014 时，先要编写数据库连接串。数据库连接串的书写方法有很多，这里介绍两种常用的方法。`

**第1种方式:server = 服务器名称 / 数据库的实例名 ; uid = 登录名 ; pwd = 密码 ; database = 数据库名称**

其中：

* server：用于指定要访问数据库的数据库实例名，服务器名称可以换成 IP 地址或者数据库所在的计算机名称，如果访问的是本机数据库，则可以使用“.”来代替，如果使用的是默认的数据库实例名，则可以省略数据库实例名。例如连接的是本机的默认数据库，则可以写成“server = .”。

* uid：登录到指定 SQL Server 数据库实例的用户名，相当于以 SQL Server 身份验证方式登录数据库时使用的用户名，例如 sa 用户。
* pwd：与 uid 用户对应的密码。
* database：要访问数据库实例下的数据库名。

**第2种方式:Data Source = 服务器名称 \ 数据库实例名 ; Initial Catalog = 数据库名称 ; User ID = 用户名 ; Password = 密码**

其中：

* Data Source：与第1种连接串写法中的 server 属性的写法一样，用于指定数据库所在的服务器名称和数据库实例名，如果连接的是本机的默认数据库实例，则写成“Data Source=. ”的形式。
* Initial Catalog：与第 1 种连接串写法中的 database 属性的写法一样，用于指定在 Data Source 中数据库实例下的数据库名。
* User ID：与第 1 种连接串写法中的 uid 属性的写法一样，用于指定登录数据库的用户名。
* Password：与第 1 种连接串写法中的 pwd 属性的写法一样，用于指定 User ID 用户名所对应的密码。

此外，还可以在连接字符串中使用 Integrate Security = True 的属性，省略用户名和密码，即以 Windows 身份验证方式登录 SQL Server 数据库。

将数据库连接更改如下：

`Data Source = 服务器名称 \ 数据库实例名 ; Initial Catalog = 数据库名称 ; Integrate Security = True`

需要注意的是，由于在使用 Windows 身份验证的方式登录数据库时，会对数据库的安全性造成一定的影响，因此不建议使用 Windows 身份验证的方法，而是使用 SQL Server 验证方式登录数据库，即指定用户名和密码。

提示：在 SQL Server 2014 中更改数据库的身份验证方式并不复杂，只需要在 SQL Server 的 SQL Server Management Studio 2014 中右击数据库的服务器结点，弹出如下图所示的服务器属性界面，并在界面中选择“安全性”选项。

![](http://c.biancheng.net/uploads/allimg/190404/4-1Z404142419559.gif)

在该界面中可以通过选择“服务器身份验证”中的两个选项来切换身份验证方式，默认情况下选中“Windows 身份验证模式”。

在选中任意一种身份验证模式后需要重启 SQL Server 服务器才能完成服务器身份验证模式的更改。

在完成了数据库连接串的编写后即可使用 SqlConnection 类与数据库连接，分以下 3 步完成。

**1) 创建 SqlConnection 类的实例**

对于 SqlConnection 类来说，上表中提供了两个构造方法，通常是使用带一个字符串参数的构造方法来设置数据库的连接串创建其实例，语句形式如下。
SqlConnection 连接对象名 = new SqlConnection( 数据库连接串 )

```C#
  String sqlConString = "server =. ; uid = Lmc ; pwd =123456 ; database = User";
  SqlConnection sqldb = new SqlConnection(sqlConString);
```

如上意为打开本地SQL server服务，并用用户名为Lmc，密码为123456登录，且使用数据库为User。

**2) 打开数据库连接**

在创建 SqlConnection 连接类的实例后并没有连接上数据库，需要使用连接类的 Open 方法打开数据库的连接。

在使用 Open 方法打开数据库连接时，如果数据库的连接串不正确或者数据库的服务处于关闭状态，会出现打开数据库失败的相关异常，因此需要通过异常处理来处理异常。

打开数据库连接的语句形式如下。
连接对象名.Open();


**3) 关闭数据库连接**

在对数据库的操作结束后要将数据库的连接断开，以节省数据库连接的资源。

关闭数据库连接的语句形式如下。
连接对象名.Close();

如果在打开数据库连接时使用了异常处理，则将关闭数据库连接的语句放到异常处理的 finally 语句中，这样能保证无论是否发生了异常都将数据库连接断开，以释放资源。

除了使用异常处理的方式释放资源外，还可以使用 using 的方式释放资源。

具体的语句如下。
using(SqlConnection 连接对象名 = new SQLConnection( 数据库连接串 ))
{
    //打开数据库连接
    //对数据库先关操作的语句
}

using 关键字的用法主要有两个，一个是引用命名空间，一个是创建非托管资源对象。

在 .NET 平台上资源分为托管资源和非托管资源，托管资源是由 .NET 框架直接提供对其资源在内存中的管理，例如声明的变量；非托管资源则不能直接由 .NET 框架对其管理，需要使用代码来释放资源，例如数据库资源、操作系统资源等。

如下为窗体程序测试一个数据库是否打开：

```C#
namespace Test
{
    public partial class Form1 : Form
    {
        private SqlConnection sqldb;
        public Form1()
        {
            InitializeComponent();
        }
        private void laoding(object sender,EventArgs s)
        {
           
        }
        private void button1_Click(object sender, EventArgs e)
        {
            String sqlConString = "server =. ; uid = Lmc ; pwd =123456 ; database = User";
            
            try
            {
                sqldb = new SqlConnection(sqlConString);
                sqldb.Open();
                MessageBox.Show("打开成功！", "提示信息", MessageBoxButtons.OK);
            }
            catch(Exception ex)
            {
                MessageBox.Show(ex.Message, "提示信息", MessageBoxButtons.OK);
            }
            finally
            {
                if (sqldb != null)
                {
                    sqldb.Close();
                }
            }
        }
    }
}
```

<b id='a2'></b>

### :leftwards_arrow_with_hook:Command类 ###

:arrow_up:[返回目录](#t)

`在 System.Data.SqlClient 命名空间下，对应的 Command 类为 SqlCommand，在创建 SqlCommand 实例前必须已经创建了与数据库的连接。SqlCommand 类中常用的构造方法如下表所示。`

|构造方法|	说明|
|:---:|:------:|
|SqlCommand()|	无参构造方法|
|SqlCommand(string commandText,SqlConnection conn)	|带参的构造方法，第 1 个参数是要执行的 SQL 语句，第 2 个参数是数据库的连接对象|

`对数据库中对象的操作不仅包括对数据表的操作，还包括对数据库、视图、存储过程等数据库对象的操作，接下来主要介绍的是对数据表和存储过程的操作。在对不同数据库对象进行操作时，SqlCommand 类提供了不同的属性和方法，常用的属性和方法如下表所示。`

|属性或方法|	说明|
|:---:|:------:|
|CommandText	|属性，Command 对象中要执行的 SQL 语句|
|Connection	|属性，获取或设置数据库的连接对象|
|CommandType	|属性，获取或设置命令类型|
|Parameters	|属性，设置 Command 对象中 SQL 语句的参数|
|ExecuteReader()	|方法，获取执行查询语句的结果|
|ExecuteScalar()	|方法，返回查询结果中第 1 行第 1 列的值|
|ExecuteNonQuery()|	方法，执行对数据表的增加、删除、修改操作|

注意，如果要有返回查询结果的话，是多列多行需要SqlDataReader类型数据。如下所示：

```C#
    public partial class Form1 : Form
    {
        private SqlConnection sqldb;
        public Form1()
        {
            InitializeComponent();
        }
        private void laoding(object sender,EventArgs s)
        {
           
        }
        private void button1_Click(object sender, EventArgs e)
        {
            String sqlConString = "server =. ; uid = Lmc ; pwd =123456 ; database = User";
            
            try
            {
                sqldb = new SqlConnection(sqlConString);
                sqldb.Open();
                SqlCommand sql = new SqlCommand("select * from UserInfor",sqldb);
                //返回结果集
                SqlDataReader str = sql.ExecuteReader();
                String s = "";
                //循环结果集
                while(str.Read())
                {
                    //使用数组代表该行第几个数据
                    s += str[0];
                }
                MessageBox.Show($"打开成功！{s}", "提示信息", MessageBoxButtons.OK);
                
            }
            catch(Exception ex)
            {
                MessageBox.Show(ex.Message, "提示信息", MessageBoxButtons.OK);
            }
            finally
            {
                if (sqldb != null)
                {
                    sqldb.Close();
                }
            }
        }
    }
```

这是对于有具体sql语句参数的查询，如果sql需要程序引入参数，这就不能够使用这种方法。需要使用参数代替方法Parameters.AddWithValue()  该方法含有两个参数，一是需要代替的变量命名，二是代替的变量。如下我们想通过变量id来获取不同信息：


```C#
 private void button1_Click(object sender, EventArgs e)
        {
            String sqlConString = "server =. ; uid = Lmc ; pwd =123456 ; database = User";
            String id = "2017110329";
            try
            {
                sqldb = new SqlConnection(sqlConString);
                sqldb.Open();
                //@id代表代替字符串
                SqlCommand sql = new SqlCommand("select * from  UserInfor Where ID = @id ", sqldb);
                //为@id赋值
                sql.Parameters.AddWithValue("@id", id);

                SqlDataReader str = sql.ExecuteReader();
               
                String s = "";
                while(str.Read())
                {
                    s += str[0];
                }
                MessageBox.Show($"打开成功！{s}", "提示信息", MessageBoxButtons.OK);
                
            }
            catch(Exception ex)
            {
                MessageBox.Show(ex.Message, "提示信息", MessageBoxButtons.OK);
            }
            finally
            {
                if (sqldb != null)
                {
                    sqldb.Close();
                }
            }
        }
    }
```

像这样就完成了参数sql查询。

<b id='a4'></b>

### :leftwards_arrow_with_hook:DataReader 类 ###

:arrow_up:[返回目录](#t)

`DataReader 类在 System.Data.SqlClient 命名空间中，对应的类是 SqlDataReader，主要用于读取表中的查询结果，并且是以只读方式读取的（即不能修改 DataReader 中存放的数据）。`

`正是由于 DataReader 类的特殊的读取方式，其访问数据的速度比较快，占用的服务器资源比较少。`

`SqlDataReader 类中常用的属性和方法如下表所示。`

|属性或方法|	说明|
|:--:|:--------:|
|FieldCount	|属性，获取当前行中的列数|
|HasRows	|属性，获取 DataReader 中是否包含数据|
|IsClosed	|属性，获取 DataReader 的状态是否为已经被关闭|
|Read	|方法，让 DataReader 对象前进到下一条记录|
|Close	|方法，关闭 DataReader 对象|
|Get XXX (int i)	|方法，获取指定列的值，其中XXX代表的是数据类型。例如获取当前行第1列 double 类型的值，获取方法为GetDouble(o)|

`使用 DataReader 类读取查询结果`

`在使用 DataReader 类读取查询结果时需要注意，当查询结果仅为一条时，可以使用 if 语句查询 DataReader 对象中的数据，如果返回值是多条数据，需要通过 while 语句遍历 DataReader 对象中的数据。`

`在使用 DataReader 类读取查询结果时需要通过以下步骤完成：`

**1) 执行 SqlCommand 对象中的 ExecuteReader 方法***

具体代码如下。

`SqlDataReader dr=SqlCommand 类实例 .ExecuteReader();`

**2) 遍历 SqlDataReader 中的结果**

SqlDataReader 类中提供的 Read 方法用于判断其是否有值，并指向 SqlDataReader 结果中的下一条记录。

`dr.Read()`

如果返回值为 True，则可以读取该条记录，否则无法读取。

在读取记录时，要根据表中的数据类型来读取表中相应的列。

**3) 关闭 SqlDataReader**

下面通过实例来演示 SqlDataReader 类的使用。


```C#
        private void button1_Click(object sender, EventArgs e)
        {
            String sqlConString = "server =. ; uid = Lmc ; pwd =123456 ; database = User";
            String id = "2017110329";
            try
            {
                sqldb = new SqlConnection(sqlConString);
                sqldb.Open();
                SqlCommand sql = new SqlCommand("select * from  UserInfor Where ID = @id ", sqldb);

                sql.Parameters.AddWithValue("@id", id);

                SqlDataReader str = sql.ExecuteReader();
               
                String s = "";
                while(str.Read())
                {
                    s += str.GetString(0);
                    s += str["password"];
              
                }
                MessageBox.Show($"打开成功！ 行数为{str.FieldCount}  {s}  ", "提示信息", MessageBoxButtons.OK);
                 str.Close(); //关闭结果集
            }
            catch(Exception ex)
            {
                MessageBox.Show(ex.Message, "提示信息", MessageBoxButtons.OK);
            }
            finally
            {
                if (sqldb != null)
                {
                    sqldb.Close();
                }
            }
        }
    }
 ```



















