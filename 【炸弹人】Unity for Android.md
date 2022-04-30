# Unity for Android

*已学内容大部分不再记录*

*笔记内容根据教程顺序编辑*



unit 1：略

## unit 2

### TileMap插件 **Rule Tile**

![img](https://s2.loli.net/2022/04/14/BUrlZIvjds4P9z6.png)

安装插件后，右键新建Rule Tile 资源文件，

可对其编辑想要的生成方式，文件拖入palette中即可使用。



## unit 3

### 跳跃（优化）

可以在跳跃时修改重力scale

#### 检测功能

以一点为圆心检测图层：`Physyics2D.OverlapCircle(点，半径，图层)`

其他Overlap检测不同形状的范围

查看检测范围

```
public void OnDrawGizmos()
//该函数在Unity窗口Gizmos启用后实时生效
{
	Gizmos,DrawWireSphere(点，范围)
}
```



unit 4：略

## unit 5

### 爆炸检测与爆炸

获得检测范围对应图层的所有物体：

`Colider2D[] aroundObj = Physics2D.OverlapCircleAll(点，半径，图层)`

爆炸后的推力可用

`item.GetComponent<Rigidbody2D>().AddForce(向量，ForceMode2D.xxx)`



## *unit 6*

### -简单移动

`transform.position = Vector2.MoveTowards(transform.position,目标点，speed*Time.deltaTime);`



### *-有限状态机（FSM）*

#### 抽象类继承方法



##### 创建抽象类：

```
//EnemyBaseState.cs
//不需要挂载其他

public abstract class EnemyBaseState
{
	public abstract void EnterState(Enemy enemy);//抽象类只声明方法,传入Enemy类的实例
	
	public abstract void OnUpdate(Enemy enemy);
}
```



##### 写第一种敌人状态

```
//PortrolState.cs 

public class PatrolState : EnemyBaseState  //基本巡逻状态
{
	//实现抽象类
	public override void EnterState(Enemy enemy)  //启用该状态时的方法
	{
		enemy.SwitchPoint();//执行enemy写好的方法
		...
	}
	
	public override void OnUpdate(Enemy enemy)  //在该状态时的Update方法
	{
		if(enemy.transform.position.x ... <..)
		//由于不继承MonoBehaviour和Enemy类，一些变量以及函数实现就需要enemy来调用
		...
	}
	
}

```



##### 使用状态机

```
// Enemy.cs

public class Enemy : HomoBehaviour
{
	//省略了一些不必要的代码，注重理解内涵
	EnemyBaseState currentState;//当前状态，通过修改当前状态以实现不同状态方法
	
	public PatrolState patrolState = new PartrolState() //得到patrolstate的状态模式
	
	public void TransitionToState(EnemyBaseState state) //通过此方法实现状态切换
	{
		currentState = state;
		currentState.EnterState(this);
	}
	
	void Start()
	{
		TransitionToState(patrolState); //确定一开始的状态
	}
	
	void Update()
	{
		currentState.OnUpdate(this); //实现该状态的Update方法
	}
	
}
```



### -动画状态机

tips：可创建int变量state来切换动画。

获得一个动画是否播放

anim.GetCurrentAnimatorStateInfo(动画层).IsName(特定动画名) 

修改动画状态的名字不影响原动画文件名字



新建动画层记得修改权重



### -攻击方式

#### 距离判断

可用`Vector2.Distance(点1，点2)`



### -接口

关键字 interface

#### 接口调用

参考用

`item.GetComponent<IDamageable>().GetHit();`





unit 7略

## Unit 8

### Canvas

 RenderMode: 

1. Screen Space-Overlay : 渲染覆盖整个画面
2. Screen Space-Camera : 渲染覆盖指定相机画面
3. World Space : 显示于场景中的UI



使用UIManager脚本管理UI界面



## Unit 9

使用GameManager脚本管理游戏内容



### 数据保存

使用PlayerPrefs

初始化：

```
if(!PlayerPrefs.HasKey("xxx"))
{
	PlayerPrefs,SetFloat("xxx",3f);
}

保存数据后
`PlayerPrefs.Save();`
```



详情可见Unity笔记存档系统部分



### 观察者模式

各GameObject向GameManager汇报自身，以不用搜索的方式寻找GameObject





## unit 10

### 平台切换

在Build Setting中切换安卓平台

### 平台适配

#### 遥控杆

资源商店搜索joystick，导入素材

使用prefab里的fixed joystick，拖入UI中并修改

按键使用buttom即可



代码方面：

```
private FixedJoystick joystick; //获得遥控杆，方法略

void Movement()
{
	//获得操控杆水平变量
	float horizontalInput = joystick.Horizontal;
	...
	
	//修改翻转方法，保证人物变换正常
	if(horizontalInput>0)
		transform.eulerAngles = new vector3(0,0,0);
	...
	
}
```

在prefernces - external tool -Android 中可用勾上jdk，sdk，ndk





### 安卓测试

在build setting中run device 使用usb调试连接手机



### 广告

在包管理器中安装advertisement

游戏ID为Unity账户的id

查看项目：

unity账户 - 控制面板 - 左上角develop - 创建项目

或在Unity中service创建并连接项目



代码可以参考手册

