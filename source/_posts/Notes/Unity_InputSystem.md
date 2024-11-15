---
title: 【Unity-Input System】学习笔记
tags:
  - Unity
  - Input System
  - Note
categories:
  - Notes
  - Unity
description:
  - "Input System是Unity新推出的输入系统(好像也不是很新了)<br>这里是一些根据官方文档与各类资料整理的其使用方法，希望对你有帮助\U0001F60B"
cover: 'https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303300143145.jpeg'
abbrlink: 105
date: 2023-03-29 00:00:00
---

# Input System：浅尝Unity新输入系统

> 官方文档：https://docs.unity3d.com/Packages/com.unity.inputsystem@1.5/manual/index.html

## 前言

Input System是Unity新推出的输入控制系统（好像也不是很新了），我之前对其的使用只能说是一知半解，本文是我根据网上的一些资料与官方文档对Input System的使用方法的学习笔记，希望对你有帮助。😉

---

## Input System的导入

于Unity自带的`Windows->Package Manager`安装即可，安装后需要重启项目

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303291955069.png" alt="image-20230329195514972" style="zoom:67%;" />

之后将`Project Settings->Player->Others->Configuration->Active Input Handling`

设置为`Both/Input System Package(New)`以启用Input System

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303291959502.png" alt="image-20230329195944467" style="zoom:67%;" />

---

## Action概述

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303292017544.png" alt="image-20230329201723515" style="zoom:67%;" />

**Action**可以说是Input System中最重要的新引入的概念之一

Input System通过Action对用户的输入方式进行了映射，即一个Action可以关联多个输入，而原先的Input系统不同的是，与具体逻辑关联的也是Action，而非用户的具体输入

相当于Action成为了输入与逻辑之间的中间商，用户输入->Action->具体逻辑

Input System通过Action系统实现了输入与逻辑的分离，使得对于用户输入的管理与多端设备兼容更加容易

如我们可以将多个不同设备的移动输入绑定到同一个Action上，这样无论我们在手柄上移动摇杆，还是按下键盘的WASD，Action都会为我们返回统一的返回值，我们在编写移动逻辑时，便只需要关心Action的返回值即可，而无需分平台分设备地单独编写逻辑

---

## Action的创建

在Input System中存在着多种创建Action的方式，以下介绍通过序列化单独创建，与通过ActionMap统一创建管理两个方式：

1. **通过序列化创建单一Action**

可以通过在继承了`Monobehavior`的脚本中声明`InputAction`类型的变量(需要引入`UnityEngine.InputSystem`命名空间)

```c#
using UnityEngine;
using UnityEngine.InputSystem;

public class Test : MonoBehaviour
{
    public InputAction moveAction;
}
```

挂载脚本后便可以在Inspector界面看到声明的`moveAction`

![image-20230329203921581](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303292039605.png)

点击齿轮符号便可对`moveAction`进行编辑，设置Action的属性

点击加减号可以为Action添加/删除按键绑定，为Action绑定对应按键后，按下按键便可触发与Action相关联的方法

2. **通过ActionMap文件统一创建Action**

成功安装Input System包之后，于Assets界面`右键->Create->Input Actions`便可创建Input Actions Asset文件

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303292046474.png" alt="image-20230329204610432" style="zoom: 50%;" />

双击创建出的InputActions即可于专门的界面中统一创建管理Actions

其中ActionMaps可以理解为Action的集合，我们可以创建多个Map进行随时切换，如UI和Game

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303292049942.png" alt="image-20230329204920905" style="zoom: 50%;" />

---

## Action的属性

![](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303292052522.png)

一个Action有着多种属性，其影响着Action的触发方式、返回值等信息

### Types

Action共有三种默认的Type，**其影响的是Action对于输入值的响应规则**

|                 |          Value           |             Button             |              Pass Through              |
| :-------------: | :----------------------: | :----------------------------: | :------------------------------------: |
|  phase.started  | 返回值从初始状态被改变时 | 按钮开始被按下，但尚未超过阈值 | 第一个控制器的返回值从初始状态被改变时 |
| phase.performed |       返回值改变时       |      按钮被按下超过阈值时      |              返回值改变时              |
| phase.canceled  |     返回值不再改变时     |           按钮被释放           |            返回值不再改变时            |

Pass Through与Value基本一致，区别是Value只会响应一个最活跃的输入，而Pass Through会响应所有绑定了这一Action的按键的输入

### Interactions

Action共有6种Interactions，**其影响的是Action对于输入行为的响应规则**

当不设置时会设定为Default，响应与Types相同，其余Interactions理解并不困难，可见官方文档:

https://docs.unity3d.com/Packages/com.unity.inputsystem@1.5/manual/Interactions.html#predefined-interactions

---

## Action与方法的绑定

创建Actions后，便可以将Actions与具体的方法进行绑定，同时也存在着有多种方式来进行

**主要可以分为两个步骤：**

1. **Action的获取**
2. **方法的触发**

我们有多种方法可以进行以上两步，以下是两个例子：

### 通过Player Input组件进行绑定

![](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303300101984.png)

在导入Input System包导入后，我们可以为GmaeObject添加Player Input组件对ActionMap进行管理

**Action的获取**：将我们的InputActionAsset拖入组件中Actions之后，PlayerInput组件便会自动获取其中的Action

**方法的触发**：PlayerInput组件为我们提供了4种方式触发方法，如上图所示，在Behavior属性中可以进行选择

- Send Messages：通过Unity本身的消息机制触发方法，只要在对应GameObject挂载的脚本中编写名为OnT（T为对应Action的名字，在选择Send Messages方法后会在下方显示应该编写的方法名）的方法即可，这种方法应用方便，但是Unity的消息利用的是反射机制，效率较差

![image-20230523192728237](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202305231927303.png)

- Broadcast Messages：通过Unity的消息广播触发方法，本质与Send Messages并不不同，区别是可以将消息广播到子物体，但效率更差了
- Invoke Unity Events：利用UnityEvents触发方法，与Button等利用UnityEvents的方法一样，在Inspector中对应Action的EventsList中添加事件即可，缺点是需要拖来拖出，非常繁琐，在需要处理大量事件时操作麻烦
- Invoke C Sharp Events：利用C#的Events在脚本中手动注册回调，触发方法，效率较高，利用C#的Events写起来也会比较优雅，需要在脚本中先获取PlayerInput组件，通过其中的onActionTriggered触发事件，缺点是只有onActionTriggered一个事件，若有多个Action需要进行单独判断

*在使用PlayerInput时，其实我们并不一定要采取其提供给我们的四种方法进行操作，我们可以直接在脚本中获取PlayerInput组件的实例，直接在实例中获取特定Action，后通过Action直接进行方法的绑定，这种使用方法与下面要说的利用InputActionAssets生成C#类类似，故不再赘述*

### 通过ActionMap生成C#类进行绑定

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202305232310377.png" style="zoom: 50%;" />



在创建完成InputActionAsset后，点击创建的文件，在Inspector界面中便可以看到Generate C# Class选项，勾选该选项，点击Apply便可以生成与InputActionAsset文件同名的C#类，这个类中包含了InputActionAsset中的所有数据

之后我们便可以直接在脚本中通过new对刚刚生成的C#类进行实例化，调用其中的数据与回调函数、

*记得一定要先Enable，不然不会起作用*

测试代码如下：

```c#
public class Test : MonoBehaviour
{
    // IS_Test 为InputActionAsset与生成的C#类的名字
    private IS_Test _inputAction;

    private void Start()
    {
        // 实例化
        _inputAction = new IS_Test();
        // 注册回调
        _inputAction.Games.Move.performed += OnMove;
    }

    private void OnEnable()
    {
        _inputAction.Enable();
    }

    private void OnDestroy()
    {
        _inputAction.Disable();
    }
    
    // 回调函数，注意*回调函数的参数必须为InputAction.CallbackContext
    private void OnMove(InputAction.CallbackContext obj)
    {
        Debug.Log(obj.ReadValue<Vector2>());
    }
}
```

---

## 基于Input System的InputManager的实现

在上文中，我梳理了一遍Input System的使用方法，而现在让我们实现一个基于Input System的输入管理器模块吧！

*摆了！以后再写*
