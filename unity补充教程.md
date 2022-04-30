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


## 粒子效果



## 屏幕震动

camera帧动画
cinemachine用组件



## 骨骼（先挖个坑）





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



使用CanvasGroup组件，获取鼠标射线下的物件

DragEnd时：如果为图片，则交换；



#### 移动背包

anchorPosition：中央锚点位置

![image-20220407112801893](https://s2.loli.net/2022/04/30/ozGiKxeEaAtOI6J.png)



### 数据传递

移动物品时记得交换物品的数据，略







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



