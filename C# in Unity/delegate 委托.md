## -delegate 委托

### --Delegate 自定义委托

委托是一个 引用类

用于存储符合其**参数、返回类型相同**的函数(类型兼容)（可以理解为函数指针）



声明方法

`public delegate <returnT> name(<T1>a ,<T2>b ,...) `

声明例子： `public delegate int MyDelegate(int a,int b);`

实例化： 

```
MyDelegate mydelegate;
mydelegate = new MyDelegate(function);
 或 
mydelegate = function;
```

使用： `mydelegate.Invoke();` 或 `mydelegate();`



多播委托：`mydelegate += function2;` //不建议



### --Action委托

需要命名空间System，必须**无**返回值

创建变量： `Action<T1,T2...> action;`  //T即传入参数类型，可以为空



### --Func委托

需要命名空间System，必须**有**返回值

声明： `Func<T1,T2,Tresult> func`  //必须有返回值，返回类型放在尖括号最后一位（Tresult）



Action、Func委托省去了委托声明



### 模板方法 //未理解

常用Func委托

方法中的某项值不确定，通过调用委托方法获得返回值，填补未知空缺



![image-20220502180508294](https://s2.loli.net/2022/05/02/DZJMiaHuqOVsNYL.png)







### 回调方法 //未理解

获得调用某个方法权利，需要时调用



###   Lambda表达式 //???



![image-20220502180652604](https://s2.loli.net/2022/05/02/JOzy6B9dblv1NIu.png)

作用是省去了【创建方法】这一步骤