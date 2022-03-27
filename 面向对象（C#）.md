[TOP]

# 一.面向对象（C#）

相关视频

[C#开发课程，一整套课程](https://www.bilibili.com/video/BV1FJ411W7e5?p=98)

笔记内容根据视频 面向对象部分 学习并编辑

## 面向对象初级

### L1.面向对象的概念



（ 不同的人有不同理解）

面向对象：意在写出一个通用的代码，屏蔽差异；



在代码中描述一个对象，描述对象的属性与方法，

有同样属性类型与方法 可抽象为类

### L2.类的基本语法

```
public class 类名

{
	字段；
	属性；
	方法；
}
```

使用new实例化 创建对象

this: 表示当前类的对象



### L3.属性

属性作用：保护字段，对字段的赋值和取值进行限定

属性的本质：get和set

```
class person

{
	public string _name;
	public string Name //属性
	{
		get{return _name;}
		set{_name = value;}
	}
}
```

### L4.略

### L5.静态与非静态

#### 非静态类

##### 成员

可以有静态和非静态成员与方法

##### 方法

非静态方法：对象.方法()/字段

静态方法：类名.方法()/字段



静态函数中，只能访问静态成员

实例函数中，既可以使用静态成员和实例成员

#### 静态类

只能有静态成员和方法，且无法实例化

一般当成工具使用，在整个项目中资源共享

### L6.构造函数

#### 构造函数

作用：初始化对象

构造函数是一个特殊的方法：

无返回值，也没有前缀（void）

函数名称与类名一致

必须要public，可重载

```
public class Student
{
	public Student()//构造函数
	{
		//在创建对象时执行
	}
	
	public Student(string name,int age...)
	{
		this.Name=name;
		this.Age=age;
		...
	}
}
```

类当中自带一个无参数的构造函数



#### new关键字

使用new的作用：

1.在内存中开辟空间

2.在空间中创建对象

3.调用构造函数进行初始化



### L7.this关键字

1.代表当前类的对象

#### 2.在类当中显示调用本类的构造函数（：this）



```
public Student(A,B,C,D,E)
{
	...
}

public Student(A,B,D):this(A,B,0,D,0)
{
	
}

```



### L8.析构函数

当程序结束时执行，帮助释放资源

```
public class Student
{
	~Student()//析构函数
	{
		
	}
}
```





## 二.面向对象 继承

### L2.命名空间

没有L1



作用：区分重名类。

如果当前项目没有该类的命名空间，需要手动导入



调用方法：

alt shift f10

using xxx



#### 在一个项目引用另一项目的类

1.引用项目(在工程中)

2.引用命名空间(using xxx)



### L3.值类型和引用类型

区别：

1.存储位置不一样

2.传递方式不一样



#### 值类型

int  double  bool  char  struct  enum  decimal
存储于栈

#### 引用类型

string  自定义类  数组

存储于堆



### L4.字符串的不可变性

（string）

重新赋值时，旧值不销毁，重开辟新空间存放新值



### L5.字符串的方法（1）

s.ToCharArray()：字符串转为char数组（string 内元素只读）

new string (cahr[] chs)：将char数组转为字符串



StringBu ilder 增加string的值很快（？）



Split()：分割字符串，返回字符串类数组



### L6.字符串的方法（2）

Contains()：判断字符串是否包含某字符串

Replace()：将字符串中old value 替换为new value

Substring()：从特定位置截取指定长度字符串

Trim()：去空格

其余略



### L9.*!继承*

相同成员的类可以封装到父类当中

子类不继承父类私有字段和构造函数，

但子类默认调用父类无参构造函数，有参构造可使用关键字：base()



#### 继承的特性

继承的单根性

一个子类只能有一个父类（c++可继承多个）

继承的传递性

子类可调用父类的父类的父类的父类的父类



### L11.关键字new隐藏父类成员

可以隐藏从父类继承的同名成员，使子类无法调用父类同名成员



### @L14.*里氏转化*

#### 1.子类可以赋值给父类



```
Student s = new Student();

Person p = s;

//也可简写为
Person p = new Student();
```



#### 2.如果父类中装的是子类对象，那么父类可以强转为子类

```
Person p = new Person();
Student ss = (Student)p;
```



#### is和as

is:判断类是否可转换，返回bool

```
if(p is Student)
{
	Student ss = (Student)p;
}
```



as:用于转换类型，可以则返回相应的对象，否则返回null

```
Teacher t = p as Teacher;// t = null
```



### L15.protected访问修饰符

可由子类访问



### L16.ArrayList类集合的方法

集合的特点：长度可变，类型不固定

可用方法：略



### L19.Hashtable类集合(类似字典)

类似于字典

添加方法：ht.add(键，值)  或  ht[6] = 'a';

键必须唯一

输出：

```
Hashtable ht = new Hashtable();
ht.Add(键，值);
//值 = ht[键]
```



#### foreach 循环

在集合内循环

```
foreach(var item in ht)
{
	...//循环的是命名空间
}

foreach(var item in ht.Keys)
{
	...//循环键
}
//顺序为倒序

foreach(var item in ht.Value)
{
	...//循环值
}

```





943788231

## 三.面向对象多态

//L2.File类略



### L3.List泛型集合

创建泛型集合对象：`List<type> name = new List<type>();`

方法基本与ArrayList相似

*泛型集合可以转换为数组* `list.ToArray();`，数组也可转换为list`chs.ToArray();`



### L4.拆箱与装箱

装箱：值类型转为引用类型

拆箱：引用类型转为值类型

```
int n=10;
object o = n;//装箱
int nn = (int)o;//拆箱
```

是否发生装/拆箱，要看两种类型是否存在继承



### L5.字典集合(dictionary)

`Dictionary<键type，值type> dic = new Dictionary<type,type>();`

方法基本与Hashtable一致

#### 键值对遍历方法

```
foreach (KeyValuePair<K-type,V-type> kv in dic)
{
	
}
```





L7-9 file类暂略

### @L10.*多态之虚方法*

概念：让一个对象表现处多种状态（类型）

步骤：

1.将父类的方法标记为虚方法，使用关键字virtual，使其可被子类重写

2.子类方法前用override重写



原本父类只能调父类方法，但是重写后可以调子类，相当于实例父类，但是根据对象不同，方法也就不同。比如写一个通用方法，主体是父类，参数是子类，以后拓展代码只要改下参数就好



### L11.抽象类

当父类方法不知如何实现，可将父类写为抽象类，方法写为抽象方法

```
public abstract class Animal
{
	public abstract void Bark();//抽象方法没有方法体(大括号)
}

子类重写需要关键字override
```

抽象类不允许实例化，接口也不允许创建对象

```
Animal a = new Dog();
a.Bark();//会直接调用子类重写的方法
```



### L16.C#中的访问修饰符

public private protected

internal：只能在当前程序集（项目）访问，在同项目中权限同public

protected internal

子类的访问权限不能高于父类





### L18.值传递和引用传递

值类型在赋值时传递值本身

引用类型在赋值时传递的是其引用

<img src="C:\Users\qissqi\AppData\Roaming\Typora\typora-user-images\image-20220325165421445.png" alt="image-20220325165421445"  />



***！注意：string有不可变性，修改值时会开辟新的空间***

传参时使用ref 修饰参数时，相当于引用，会修改原值



### L19.序列化和反序列化

### //可以看游戏存档教程

序列化：将对象转为二进制

反序列化：反过来



#### 序列化

将对象可序列化：[Serializable]

序列化对象：

``` 
FileStream f = new FileStream(@"...",FileMode,FileAccess);
BinaraFormatter bf = new BinaryFormatter();
bf.Serialize(f,bf);
```



#### 反序列化

```
bf.Deserialize(Stream流)//记得接收返回值
```



### L20.部分类

同名类前用partial 修饰，多个部分类组合成一个类（可多人同写一个类的内容）

### L21.密封类

类前加sealed 修饰，不可被继承

### L22.重写父类ToString()

override，然后你懂的



### *L23.接口简介*

接口以关键字 **interface**修饰，可在多继承时被引用

*接口就是一个规范 能力*

```
public intrtface I...able
{
	成员;//成员不允许添加访问修饰符，默认public；不能有方法体。
	//不能写字段，但是可以写 自动属性
	string Name
	{
		get；
		set；
	}
	
	接口中只能有方法，自动属性，索引器，事件
}
```



### L24、25 接口的特点

只要一个类继承了一个接口，这个类必须实现接口中所有成员，实现接口方法不需要加override



接口不允许实例化

接口与接口之间可以继承，且可以多继承

接口不能继承类，类可以继承接口

类可以继承一个类和多个接口，父类写在接口前

---



### L26.显示实现接口

目的：解决方法重名的问题（类方法与接口方法重名）

```
public class bird：IFlyable
{
	public void Fly(){};
	
	void Iflyable.Fly(){};//显示实现接口
}


```

