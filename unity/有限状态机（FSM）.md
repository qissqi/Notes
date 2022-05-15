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

