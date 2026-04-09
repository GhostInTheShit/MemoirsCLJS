---
categories: Unity
---

# State Machine

状态机，一般都是有限的。又叫有限状态机.

用来管制由一个状态转变为另一个状态。防止逻辑穿插混乱。

```C#
//状态枚举
public enum StateEnum
{
    Idle,
    Move,
    Attack,
    Die,

    Pick,
    Deposit,

}

public abstract class AIUnit : Unit
{
    //三个核心方法，进入状态，状态进行时，状态退出
    //我这里不用进入和退出所以为空
    public virtual void OnEnter(StateEnum state) { }
    public abstract void OnUpdate(StateEnum state);
    public virtual void OnExit(StateEnum state) { }

    [Header("State")]
    //当前状态和新状态，用来判断是否进行了状态改变
    
    //CurrentState在OnUpdate中随时检测与新状态不同而触发切换
    public StateEnum CurrentState;
    //新状态是可以随时被提供，默认为Idle.
    public StateEnum NewState=StateEnum.Idle;

    //改变状态的方法，尽量不要直接变量赋值，要不然你可能会写的到处都是，用单独的方法包装，保证你不会别的方法里面穿插使用，而是单独调用。
    public void SwitchState(StateEnum state)
    {
        NewState = state;
    }

    //在update里面时刻判断状态的修改
    protected override void Update()
    {
        base.Update();

        //检测是否状态切换了，里面为切换了
        if (NewState != CurrentState)
        {
            //执行退出状态
            OnExit(CurrentState);
            //执行进入新状态
            OnEnter(NewState);
            //将当前状态保存为新
            CurrentState = NewState;

        }
        //执行状态方法.
        OnUpdate(NewState);
       
    }
}

```

具体实现
```C#
//某个单位的状态方法，状态的切换也在这个方法里面。由自己判断。
public override void OnUpdate(StateEnum state)
{
    switch (state) 
    {
        case StateEnum.Idle: 
            GetAttackTarget();
            if(AttackTarget != null) 
                SwitchState(StateEnum.Attack);
            break;

        case StateEnum.Attack:
            Attack();
            if (AttackTarget == null) 
                SwitchState(StateEnum.Idle);
            break;
    }
}
```