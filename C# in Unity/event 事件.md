## -event事件

事件是一种**类型成员**，起到**“通知”**作用，*事件基于委托*。

Text文本框需要调整Scale大小，调整合适后再调整字体和位置



### 自定义事件

完整声明：

注：事件需要的OrderEventHandler 为委托类型，

`public delegate void OrderEventHandler(Customer customer,EventArgs _e);`



![image-20220502210832445](https://s2.loli.net/2022/05/02/hdmxSgNctBPl4i3.png)

如果一个**委托**是为了**声明某个事件而准备的委托**，一般命名为 **事件名 + EventHandler** 

如果某个**类**是作为**EventArgs类**（事件信息）来使用，一般命名为 **事件名+EventArgs**，并**派生自 EventArgs**



简略声明：

`public event EventHandler OnOrder;`

event 只能用+=、-=操作

*使用System内的EventHandler，通用且复杂，需要创建大量信息*



**事件不是委托类型的字段**