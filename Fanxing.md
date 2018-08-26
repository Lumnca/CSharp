## 性能
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从值类型转换为引用类型称为**装箱**>.如果方法需要把一个对象作为参数,同时转递一个值类型,装箱操作就会自动运行.另一方面,
装箱的值类型可以使用拆箱操作作转换为值类型.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下面例子显示了System.Collections名称空间的ArrayList类.ArrayList储存对象,Add()方法定义为需要把一个对象作为参数.<br>
<br><br>
`var list = new ArrayList();`<br>
`list.Add(25);`**//装箱操作,并由值类型转化为引用类型.**<br>
`int i1 = (int)list[0];`**//拆箱操作,引用类型转化为值类型.**
`foreach(int i2 in list)
{
  Console.WriteLine(i2);
}`
