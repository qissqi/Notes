# *.关于其他教程



## -冲锋与残影

使用组件sprite render，

获得变量transform；SpriteRender thisSprite与playerSprite；

color颜色，控制时间的参数（起始与持续）；不透明度变化（初始为1；变化乘以小于一的数）


### 第一个项目代码部分

####使用`OnEnable(){}` ：组件启用时

1.为transform赋值为player的当前transform

2.为组件本身的 thisSprite获得SpriteRender组件，获得playerSprite的SpriteRender组件

3.获得不透明度

>4.
>thisSprite.sprite = playerSprite.sprite;
>
>transform.position = player.position; +localscale  +rotation

5.起始时间设置为当前时间Time.time，设置持续时间透明度

#### update部分

1.设置透明度变化，颜色：new Color(红，绿，蓝，不透明度)

2.时间判断

if(Time.time>= activeStart + activeTime) 

3.返回对象池ShadowPool.instance.ReturnPool(this.gameObject);(见下)

**最后将该项目存为预置即可**

###第二个项目代码部分（控制所有对象池）
创建脚本ShadowPool
####设为单例
	public static ShadowPool instance;

​	void awake(){ instance = this; }

----

1.获得之前的残影预制体GameObject，定义残影数量的数组，定义GameObject的队列

`private Queue<GameObject> availabkeObjects = new Queue<GameObject>();`

2.初始化对象池

​	public void FillPool()
​	循环：var newShadow = Instantiate(shadowPrefab);
​		newShadow.transform.SetParen(transform) --设置为当前项目的子级
​	取消启用，返回对象池
​	ReturnPool(newShadow)(在下面);

在awake中调用

3.取消启用，返回对象池

​	public void ReturnPool(GameObject gameObject){
​	gameObject.SetActive(false);
​	availableObjects.Enqueue(gameObject);

4.生成

​	public GameObject GetFromPool(){
​	
​	if(availableObjects.Count == 0) { FillPool(); }	

​	var outShadow = availableObjects.Dequeue();
​	outShadow.SetActive(true);
​	return outShadow

#### 调用

`ShadowPool.instance.GetFromPool();`

### 冷却CD

`if(Time.time >= (lastDash + dashCoolDown)) { 执行冲锋 } `

## -背包系统

创建于UI中

背包格子需要添加Grid Layout Group组件，调整间距尺寸

在物品图片添加button组件，设置物品描述

### 数据存储

#### 物品数据

创建C#，但是继承于ScriptableObject，用于直接创建物品



```
[CreateAssetMenu(fileName="aaa",menuName="Inventory/New Item")]//设置文件默认生成名和菜单选项名字
public class Item : ScriptableObject
{
	public string itemName;
	public Sprite itemImage;
	...
	[TextArea]//多行文本
	public string itemInfo;
}
```



#### 背包列表

同上，但是用`public List<Item> itemList = new List<Item>();`



#### 添加物品

略

注意：scriptableObject文件不复原



### 背包显示

用C#写manager脚本，通过此赋值

![image-20220407104648543](https://s2.loli.net/2022/04/30/kKpsaM6TevVLdmF.png)

![image-20220407104955443](https://s2.loli.net/2022/04/30/1hjvks26py8uXmN.png)

![image-20220407105030436](https://s2.loli.net/2022/04/07/fXS2uOy8IProQt9.png)



### 拖拽功能

#### 移动物品

新建C#脚本实现拖拽

使用：`Using UnityEngine.EventSystems;`

将物品脱离父级以优先渲染



![image-20220407111115100](https://s2.loli.net/2022/04/07/Cc39K7uitbUpTNa.png)

![image-20220407112948643](https://s2.loli.net/2022/04/07/oEpWcHCV1Jk7iNP.png)

![image-20220407113038882](https://s2.loli.net/2022/04/30/XvsLCSo4TJFKUfk.png)



使用CanvasGroup组件，取消勾选block raycast，获取鼠标射线下的物件

DragEnd时：如果为图片，则交换；



#### 移动背包

anchorPosition：中央锚点位置

![image-20220407112801893](https://s2.loli.net/2022/04/30/ozGiKxeEaAtOI6J.png)



### 数据传递

移动物品时记得交换物品的数据，略



## -点击响应事件（非button）

```
using UnityEngine.EventSystems;

public class : mono, IPointerDownHandler
{
	public OnPointerDown(PointerEventData eventData)
	{
	
	}
}
```









## -Unity存档系统

### PlayerPrefs(简单，功能少)

```
1.PlayerPrefs.SetString(string key,string value)
2.PlayerPrefs.SetInt(string key,int value)
3.PlayerPrefs.SetFloat(string key,float value)
PLayerPrefs.Save()
```

设定对应key的参数值，保存

保存于注册表中，安全性低

```
PlayerPrefs.GetString(string key,string default)
```

根据key读取对应参数value，如果没有对应key则读取default值（即默认）

---

#### 配合Json功能

可先写一个类，封装玩家数据，例：

```
[Sysytem.Serializable]class SaveData
{
	public string Name;
	...
}
```

数据存于**SaveData**中

---

存档功能使用静态类，例子： 

```
public static class SaveSystem
{
	public static void SaveByPlayerPrefs(string key,object data)
	{
		var/string json = JsonUtility.ToJson(data)
		PlayerPrefs.SetString(key,json);
		PlayerPrefs.Save();
	}
	
	读档功能类似上，但记得return
	
	public static string LoadFromPlayerPrefs(string key)
	{
		return PlayerPrefs.GetString(key,null);
	}
}
```

---

所有功能组合调用，例：

```
	var saveData = new SaveData()//这是前面的SaveData类
//注意saveData大小写的不同
	saveData.Name = Name;
	...
//调用之前的SaveSystem存档功能
	SaveSystem.SaveByPlayerPrefs("PlayerData",saveData);

//读档：
	var json = SaveSystem.LoadFromPlayerPrefs("PlayerData");
	var saveData = JsonUtility.FromJson<SaveData>(json)
	
	Name = saveData.Name
	...
	
	//翻译：读取保存的json存档，并转化为saveData,把saveData数据赋给游戏内容变量
```

----

附加：删除存档功能：

```
[UnityEditor.MenuItem(string ItemName)]//将功能展示于Unity菜单栏上，只有staic函数才能生效
public static void DeletePlayerDataPrefs()
{
	Playerprefs.DeleteAll()//删除所有
	Playerprefs.DeleteKey(string key)//删除指定key
	
	
}
```

### Json存档

[教程1](https://www.bilibili.com/video/BV1Cb4y1b71G?spm_id_from=333.999.0.0 "Json存档教程1")

Json存档的转化仅支持**Unity可序列化的数据**（如 类），详情于上方链接

简易分辨方法：将数据标为public，若在Unity编辑界面可以修改则数据支持Json存档。

---

#### 相关代码

```
JsonUtility.ToJson(object obj,bool pp)
```

将obj转换为json格式的文件，pp是优化json文本，函数会返回string值，即json文本内容。



```
JsonUtility.FromJson(string json)
```

将json文件转为unity的序列化支持的对象



```
JsonUtility.FromJsonOverwrite(string json,object obj)
```

将Json文件转换并覆盖原有对象

---

#### 存档系统

存档保存，需要使用file函数，

保存路径可用自带的默认路径

`Application.persistentDataPath`

并用`Path.Combine(Application... , "xxx")`组合新路径

保存例：

```
Using System.IO

...
var json = JsonUtility.ToJson(data);
var path = Path.Combine (Application.persistentDataPath, saveFile);
File.WriteAllText(path,data);

```



读取例：

```
var path = Path.combine(略);
var json = file.ReadAllText(path);
var data = JsonUtility.FromJson<T>(json);
return data;
```



删除存档：

```
File.Delete(string path);
```

---



### ScriptableObject存档

[教程](https://www.bilibili.com/video/BV1CJ41157DR?spm_id_from=333.999.0.0 "麦扣的存档教程")

需要使用的

```
Using system.IO;
Using System.Runtime.Serialization.Formatters.Binary;
```

创建二进制转化

`BinaryFormatter formatter = new BinaryFormatter();`



创建文件，任意扩展名

`FileStream file = File.Create(Application...+"/xxx")`



使用Json保存数据

```
var json = JsonUtility.ToJson(Inventory);

formatter.Serialize(file,json);
file.Close();
```



读取

```
BinaryFormatter bf = new BinartFormatter();
//检测存在if(File.Exists(path))

FileStream file = file.Open(path,FileMode.Open);

JsonUtility.FromJsonOverwrite((string)bf.Deserialize(file) ,Inventory)

file.Close();
```





## -场景异步加载

```
using UnityEngine.SceneManagement
using UnityEngine.UI
public class load
{
	public void LoadLevel()
	{
		StartCoroutine(AsynLoadLevel())
	}
	
	
	IEnumerator AsyncLoadLevel()        //使用协程实现异步加载
	{
		AsyncOperation operation = SceneManager.LoadSceneAsync(index);  //协程操作变量为operation
		operation.allowSceneActivation = false; //加载完成时先不要自动跳转，允许也可以
		
		while(!operation.isDone)   //加载未完成，改变进度条
		{
			float progress = operation.prgress / 0.9f;  //异步加载进程值为 0~0.9，需要除0.9 获得实际进度
			
			slider.value = progress; //控制组件值
			progressText.Text = Mathf.FloorToInt(progress*100f).ToString() + "%" ;
			if(operation.progress>=0.9f)
			{
				operation.allowSceneActivation = true;
			}
			yield return null; //协程别忘了
		}
	}
}

```



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



## -对话系统

[视频]: https://www.bilibili.com/video/BV1WJ411Y71J?spm_id_from=333.999.0.0

### 1.UI

canvas使用World Space 放置场景位置，并调整sorting layer



### 2.TextAsset

使用TextAsset的变量可以传入文本文档：`TextAsset file`，file.text即文本的内容



切割文本：

```
file.text.Split('\n');  //将文本按行（\n）切割，返回字符串数组

可用List 将文本存储，用按键输入修改显示的内容
```



### 3.协程操作（文字逐个显示）

使用协程写法嗯

```
{
	StartCoroutine(SetTExtUI());
}

Ienumerator SetTextUI()
{
	textFinishied = false; //判断该段文本是否输入结束，阻止多次加载文本
	textLable.text = "";  //清空当前文本内容
	
	for (int i=0,i<textList[index].Length;i++)
	{
		textLable.text += textList[index][i]; //让文本框内容文字逐个增加，达到逐个显示效果
		
		yile return new WaitForSeconds(0.1f)
	}
	
	textFinished = true;  //该段文字显示结束
	index++;
	
}

```



### 4.快进显示所有文本

添加bool变量cancelTyping

```
if(Input.GetKey(R))
{
	if(textFinished&&!canceltyping)
	{
		StartCorouine(...)
	}
	else if(!textFinished)
	{
		cancelTyping = true;
	}
}
```

之后在显示内容的循环中增加cancelTyping的判断

```
Ienumerator SetTextUI()
{
	textFinishied = false; //判断该段文本是否输入结束，阻止多次加载文本
	textLable.text = "";  //清空当前文本内容
	
	for (int i=0,i<textList[index] && !cacelTyping.Length;i++)
	{
		textLable.text += textList[index][i]; //让文本框内容文字逐个增加，达到逐个显示效果
		
		yile return new WaitForSeconds(0.1f)
	}
	
	textLabel.text = textList[index];  //  ++  在cancelTyping后显示所有文本
	cancelTyping = false;
	
	textFinished = true;  //该段文字显示结束
	index++;
	
}

```



其他思路：按下按键后增大文本显示速度达到快进的效果







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



### 使用







## -泛型单例

使用：一般作为Manager类脚本的父级脚本



```
//标准写法
public class Singleton<T> : MonoBehaviour where T : Singleton<T>
{
	private static T instance;
	
	public static T Instance
	{
		get {return instance;}
	}
	//以上为单例的属性构造，用于保护单例
	
	protected virtual void Awake()
	{
		if(instance!=null)
			Destroy(gameObject);
		else
			instance = (T)this;
	}
	//此方法在启用时为对应的脚本生成其单例
	
	protected virtual void OnDestroy()
	{
		if(instance == this)
		{
			instance = null;
		}
	}
	//在gameobject被销毁时销除其单例
	
}
```



之后便可以快捷写出Manager脚本：

```
public class GameManager : Singleton<GameManager>
{
	//b
}
```



### --关于单例与静态

[参考网站]: https://www.jb51.net/article/209535.htm

1、静态类不能继承和被继承！（严格点说是只能继承System.Object）也就是说你的静态类不可能去继承MonoBehaviour，不能实现接口。静态方法不能使用非静态成员！如果你大量使用静态方法，而方法里又需要用到这个类的成员，那么你的成员得是静态成员。

静态（静态方法或者静态类），代码编写上绊手绊脚，方法调用很方便，运行效率高一丢丢。逻辑面向过程，不能很好地控制加载和销毁。



2、单例（类的静态实例），代码编写和其他类完全一样，继承抽象模版接口都可以，Unity里也很方便进行参数配置，不过使用麻烦有犯错的可能性（必须通过实例调用方法），效率不如静态（但是也不会有很大影响吧）。

(1、只要你的类需要保存其他组件作为变量，那么就有必要使用单例；

(2、只要你有在Unity编辑器上进行参数配置的需求，那么就有必要使用单例；

(3、只要你的管理器需要进行加载的顺序控制，那么就有必要使用单例（比如热更新之后加载ResourcesManager）



## -通过资源文件加载物体

使用resource类的功能进行加载，加载路径path要在Resource文件夹后。

加载单个资源：`Resource.Load<T>(string path);`



如果需要加载multiple 的资源

```
Resource.LoadAll<T>(string path);
```







