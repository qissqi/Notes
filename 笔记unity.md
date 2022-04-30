[^_^]: # (HELLO!)

[toc]


# markdown
## 笔记
[markdown笔记](https://breezeshane.github.io/BlogBuildingAndUsing/Markdown%E8%AF%AD%E6%B3%95%E6%9D%82%E8%AE%B0/ "title?")

___

### nothing

---
<!--<center>[click here](#ojbk)</center>-->



# 1.*C# in Unity*

## 创建变量
`public`：公开变量，外部可调用，在unity中Inspector窗口可视

`private`： 私有变量，仅用于当前函数

### 变量
```int float bool```：略

`GameObject`:（unity engine自带）所创建的新物件，获得后可使用其所有conponent组件

`transform`：（unity engine自带）位置，用于更改conponent组件

## 函数：使用类似C
---
#### unity自带
>以下函数执行顺序固定

`awake()`：
无论script是否启动，都会在开始时执行，在start之前执行

`OnEnable()`:
在启用该函数启用时执行,在start之前执行

`start()`：
点击play 后立即执行其中所有代码

`FixedUpdate()`:
每帧执行一次，可修改频率，在start()之后执行

`OnTriggerxxx()`:
在角色？？时执行，在update()前执行

`OnCollisionxxx()`:
碰撞体???执行

`OnMousexxx()`:
鼠标？？？时执行
>`OnMouseDown()` 对于移动设备屏幕触摸也有效
>
>`OnMouseUp()` 不能在iPhone设备生效

`Update()`：
每帧执行一次

`LateUpdate()`:
每帧执行，但在当前帧渲染后执行

`OnGUI()`:
每帧渲染图形，update 后执行
>`GUI.Buttom(new Rect(坐标x,y,长,宽),"内容")`

`OnDestroy()`:
最后执行


---

#### 自定义
使用类似C，不需要声明



### 一些语句
#### **Unity的**
`Debug.log(值)/("字符串")/(变量值+"字符串")`：日志

`outputText.text =" "` 输出文本

用于显示你想显示的东西，+用于连接,*记得分号；*
---

`Input.getxxx(按键)`：判断按键

`transform.position`:位置

`new`：赋予一个新的值

`Vector2()`：二维赋值

`Random.Range(min,max)` DDDD

##### *使用例*
transform.position = new Vector2(transform.position.x+1,0)

???

---


#### 普通的
`if(){} else`:同C

for(;;){}
while(){} do{} while()
break;

`switch(){case :}`

---
##### 数组
定义：

`type[] name = new type[number]`
`type[] name = {data1;data2}`
`public type[] name`

**可在unity窗口中对其赋值**

`name.Length` ：数组name的长度

>`public GameObject[] cubes;`
>
>`cubes = GameObject.FindGameObjectsWithTag("cube");`
>>
>将标签为"cube"的GameObject传递给数组cubes

**数组的数量在游戏中不可改**




## 杂项
unity 支持中文变量

public变量可在unity的窗口中修改

# 2.Unity基本
## 一些组件
rigidbody：刚体，赋予物件一些物理量

collider：碰撞器，赋予碰撞体积

animator：动画

transform：物件位置 大小等


## 窗口

1.hierarchy：所有游戏的项目
2.scene：场景
3.game：实际游戏的场景
4.assist store：素材商店
5.inspector：项目检查器

##创建物体

创建sprite，拖动图片，调整图层。

## 编辑地图素材

创建tilemap（瓦片地图）

调整unit大小

将素材导入至tile palete（平铺调色板）

spirit mode设置为motiple

点slice 进行自动切割素材、
或自定义切割

使用笔刷 放置素材



## 编辑动画

添加**animator组件**，创建**animator controller文件**，在animation窗口中创建动画，并调整触发。

## 碰撞触发器

**使用collider**

将物件设置为**is trigger**，

在玩家角色中编辑代码，使用`OnTriggerEnter2D(Collider collision)`
**:括号内为碰撞到的collider**

{
	destroy(collision.gameobject);
}```

## 预置

将左侧编辑好的物体件拖入文件夹，物件所有参数会保存。

## 物理材质

新建文件类型physics material，在friction修改摩擦
（bounciness 弹性？）

## UI
##1
### 创建
新建 UI—> canvas（画布），会附加一个EventSystem事件触发器。

在canvas中新建UI—>text（或别的），按F对焦

### 代码修改

`Using UnityEngine.UI` ，创建text类的变量（或别的类？），

修改：`变量.text = 变量.tostring()` 将变量修改为string类型。

调整位置：在canvas中修改基准点，使UI在各比例中位置保持。

# 3.unity进阶
对话框：新建UI—>panel，调整位置与大小，透明度等

再在子项目添加UI—>text，修改内容

创建animation文件，拖入对话框项目，点击录制编辑动画效果（关键帧）

### 触发器：
获得对话框的Gameobject，使用触发器，判断略；

**xxx.SetActive(true)**，激活组件，关闭同理



##AI移动:
添加空项目作为左右点放置在适合位置，在游戏开始获得其位置后将子项目删除。

切断子项目：`transform.DetachChidren()`

删除子项目：`destroy(xxx.gameObject)`

随机移动`MoveTowards(起点，终点，速度)`

### 追击

`float distance = (transform.position - playerTransform.position).sqrMagnitude`

`sqrMagnitude`：计算两点距离



### 其他AI
创建sprite，创建动画 略；

在代码中修改各类触发事件：`OnCollisionEnter2D(Collision2D other)`

获得tag：`other.gameObject.tag` **！：写法不同于OnTriggerEnter2D()**


## 动画事件

animation窗口中add event，并在其中调用代码里的函数

event将在动画播放结束后调用。 结束后吗？

## class调用
在代码开头可看到对于的class *xxx*，冒号后的monobehavior为父类

调用例 ：` *xxx(其他的类)* yyy(变量名称) = collision.gameObject.GetComponent<???>( xxx )`

`xxx.JumpOn();`调用其代码内函数JumpOn


### 创建Class：
新建script作为父类，将父/子类的**变量**或**函数**前加protected表示仅在父子类中可用。

再加virtual 表示子级可编写。  子级加override进行覆写

将子类的：父类改为你新建的类

`base.start()` 获得父级的start函数

## 声音

### Audio Listener

### Audio Source

### Audio Clips
添加组件 audio resource，apply all应用全部预置

在audio clips添加音频并调整

`xxxaudio.Play(/delay)`播放

``

## 关于下蹲

`Discoll.enable = false` 关闭Discoll组件

`Physics2D.OverlapCirclr/Box etc (基准点，范围，图层)`

检测**基准点范围**内是否碰到**图层**

## 场景变换
前置： `using UnityEngine.SceneManagement`

重置场景：`SceneManager.LoadScene(SceneManager.GetActiveScene()[获得当前场景名字])`

`Invoke(函数名字，延迟时间)` 延迟执行

下一关：

`SceneManager.GetActiveScene().buildIndex+1`获得关卡编号+1

在Unity菜单-->file-->buidsetting-->将场景拖入可看到编号。

将场景切换条件与dialogue关联

### 光效
lightweight插件

## 视觉差
获得图层初始点，摄像机的transfor，定义MoveRate，

将图层的的移动等于 图层初始点+摄像机位置*MoveRate

## 主菜单

创建开始界面的Scene，创建画布，设置原图像为none

新建UI-->button，更改内容

可使用关键帧动画

### 代码
加载下一关参考上，注意using SceneManagement，在event中添加函数

退出：`application.quit()`

寻找对应文件项目 `GameObject.Find("ccc/bbb/ccc").???`

## 暂停菜单
菜单略

暂停：`time.timeScale = 0f`

恢复：`time.timeScale = 1f`

## 音量调整
创建AudioMixer文件，在unity window打开mixer菜单。

在audioResource的output选择mixer，

声音最小-80，最大0；

### 代码
`using UnityEngine.Audio`，声明AudioMixer

在mixer中右键选择你需要改变的值，选Expose输出值(MainVolume)

audiomixer.SetFloat("MainVolume",volme)

在滑动条选择函数位置选择Dynamic（内部生成） 位置的函数

## 音效管理
将AudioSource AudioClips在代码中声明；

播放：`audioSource.clip = xxx`

`audioSource.Play();`

将类实例化：`public static SoundManager xxx`

awake()
{`xxx=this;`}

调用：`SoundManager.xxx.yyy()`

## 射线
`RaycastHit2D 'Ray' =  Physics.Raycast(起点，方向(vector2.Down)，距离，图层)`

判断射线Ray是否接触图层

`'Ray'.distance` ：获得射线Ray距离最近的碰撞体之间的长度

显示射线：`Debug.DrawRay(起点，方向，颜色，距离（持续时间？）)`


## 	取得函数
`other.getcomponent<'C#'>().'xxx()'`


## 延时

### 协程
​	IEnumerator 'xxx' ()
​	{
​	yield return new WaitForSeconds(时间);
​	other;
​	}

启动协程： `StartCoroutine(函数())`

### Invoke
`Invoke("函数名",延时,重复间隔)`

### Time.deltatime
​	if(ColdTime > 0)
​		Coldtime -= Time.deltatime;
​	else
​		other;

## 粒子
新建项目 Effect-->particle System

然后自己摸索吧！

声明gameObject变量，拖入粒子效果，

​	Instantiate(粒子gameObject,位置,Quaternion.Rotation)
​								   /Quaternion.Identify·



