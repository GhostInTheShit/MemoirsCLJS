---
categories: Unity
---

# InputSytem

新版输入系统，可视化设计+自动生成C#代码。

包管理`Input System` 下载

项目设置`Player`中默认是开启两种输入系统，这里只用新系统,在`Active Input Handing`中 选择新输入系统，重启

重启后，在游戏世界的gameobject中，将`EventSystem`中的`StandaloneInputModule`(独立输入模块) 换成`Input System UI Input Module`，因为新的输入系统需要新的UI模块，如果你安装了新输入系统，可以一键修改.

新系统不需要判断射线是否在UI上还是gameobject上，它会自己检测并且UI会率先消耗事件.

## 使用

这里使用代码调用的方式

*   利用工具自动生成默认C#.这样他会给我们一套默认的输入映射，已经设置好了一些跨平台输入.

    *   直接在需要控制的`GameObject`下面添加`Player Input`组件
    
    *   然后点击`Create Actions`按钮一键创建Actions,命名为`PlayerControls`
    
    *   然后找到生成的那个`Action`点击创建`C# Class`
    *   之后删除这个`Player Input`
    *   `Player Input`是一种不用写代码就能绑定输入的组件。但里面其实也是帮我们`new`了`Action C# class`

*   在需要控制的那个脚本里面

    ```C#
    //SomeObject.cs 

    private PlayerControls _inputs;

    private void Awake()
    {
        //这里的PlayerControls为你创建的Action的C# Class
        _inputs = new PlayerControls();

        //新输入系统用的也是事件委托的方式
        _inputs.Player.Fire.performed += HandleMouseClick;
    }

    //这里需要传入InputAction.CallbackContext,里面有输入上下文信息
    void HandleMouseClick(InputAction.CallbackContext ctx)
    {
        //something
    }

    //默认的Action中禁用了Map,因为默认开启会让用不到的输入被检测，冗余。
    private void OnEnable()
    {
        _inputs.Player.Enable();
    }

    
    private void OnDisable()
    {
        _inputs.Player.Disable();
    }
    ```

### Control Schemes

一般用来规定输入源,比如键盘或者手柄,摇杆之类的.

### Devices

设备筛选

### Action Map 

一组Action，方便分类

### Action 

具体的输入动作,名字和按键

Action Properties
*   Button 按钮
*   Value 
*   Pass Through 

## Mouse

`Mouse.current.position.ReadValue()` 鼠标当前的位置

### 右键拖动鼠标

Action Maps -> Player ,Actions -> Look , Look -> `add Binding With One Modifier`.

`Modifier` -> `Mouse Right Button`

`Binding` -> `Mouse delta`

modifier其实就是修饰符,比如`ctrl+s`,`ctrl`就是.

```C#
public class CameraManager : MonoBehaviour
{
    //本身摄像机
    public GameObject Camera;
    //Actions
    public PlayerControls playerControl;

    private void Awake()
    {
        instance = this;
        //创建Action脚本
        playerControl=new PlayerControls();
    }

    // Update is called once per frame
    void Update()
    {
        CameraMove();
    }

    void CameraMove()
    {
        Vector2 vector2 = playerControl.Player.Look.ReadValue<Vector2>();
        transform.position += new Vector3(vector2.x,vector2.y)*0.3f;
        
        if(transform.position.x<-20) transform.position = new Vector3(-20, transform.position.y, transform.position.z);
        if (transform.position.x > 20) transform.position = new Vector3(20, transform.position.y, transform.position.z);

        if (transform.position.y < -10) transform.position = new Vector3(transform.position.x, -10, transform.position.z);
        if (transform.position.y > 20) transform.position = new Vector3(transform.position.x, 20, transform.position.z);
    }

    private void OnEnable()
    {
        //启动，默认是关闭的.
        playerControl.Player.Enable();
    }

    private void OnDisable()
    {
        //关闭
        playerControl.Player.Disable();
    }
}
```
