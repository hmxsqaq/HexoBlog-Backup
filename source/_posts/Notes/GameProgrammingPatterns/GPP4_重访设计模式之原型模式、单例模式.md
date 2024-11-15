---
title: 【游戏编程模式】读书笔记四：原型模式、单例模式
tags:
  - 《游戏编程模式》
  - 设计模式
  - Note
categories:
  - Notes
  - Design Pattern
  - 《Game Programming Patterns》
description:
  - 重访设计模式：原型模式、单例模式
cover: 'https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303301905867.jpeg'
abbrlink: 2004
date: 2023-04-05 20:00:00
---

![img](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202304060426139.jpeg)

> **《游戏编程模式》-Bob Nystrom**
>
> **书籍汉化版网址：https://gpp.tkchu.me/**
>
> **笔记目录：[https://hmxs.games/posts/2000/](https://hmxs.games/posts/2000/)**

---

# 原型模式

![一个生产者包含一个对怪物应用的原型字段。 他调用原型的clone()方法来产生新的怪物。](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202304061710457.png)

## 基本定义与概念

原型模式(PrototypePattern)是一种创建型设计模式，设计之初是为了解决"重复创建类似对象"这一问题，其提倡通过预先创建一个"原型对象"，再通过对原型进行克隆的方式创建对象。虽然在今时今日原始的原型模式实现方式已经有些过时，很多语言已经将原型模式纳入了其语言特性中(如Unity中的Instantiate方法)，但其思想仍然有值得学习借鉴的地方，如在我们通过JSON配置对象数据时，我们便可以将一部分相同的数据作为原型部分来提高效率。

---

# 单例模式

## 基本定义与概念

单例模式(Singleton)是最易于使用，同时也存在着许多争议的一种设计模式。其理念是:**保证一个类只有一个实例，并且提供访问该实例的全局访问点。**从另一个角度来说，单例模式是一种将全局状态封装为类的模式。其有着鲜明的优缺点，它是一种方便好用的设计模式，但是它也有着很多缺陷，依据本书作者的话来说，使用它更接近一种“饮鸩止渴”的状态。

### 优点

- 使用懒汉式单例时，惰性加载可以一定程度上节省内存，没人调用它时，其便不会被创建
- 与静态/全局状态相比，其在运行时进行实例化，这让其初始化变得方便，不必担心引用问题
- 因其可以被全局访问，调用它非常轻松惬意

### 缺点

- 全局访问的特性，让理解代码变得困难，同时并不安全，因为所有人都能访问到，也让并行开发变得困难
- 使用懒汉式单例时，惰性加载让准确控制其加载时机变得困难，无法手动控制其内存
- 饿汉式单例随没有惰性加载的问题，但其相比静态类很难突出其优势

## Unity简单实现

Singleton懒汉式泛型基类实现：分为不继承Mono与继承Mono两个版本

```C#
namespace SingletonPattern
{
    public class Singleton<T> where T : Singleton<T>, new()
    {
        private T _instance;

        public T Instance
        {
            get
            {
                if (_instance == null)
                {
                    _instance = new T();
                }
                return _instance;
            }
        }
    }
    
    public class SingletonMono<T> : MonoBehaviour where T : SingletonMono<T>
    {
        private T _instance;
        
        public T Instance
        {
            get
            {
                if(_instance == null)
                {
                    GameObject obj = new GameObject
                    {
                        name = typeof(T).ToString()
                    };
                    DontDestroyOnLoad(obj);
                    _instance = obj.AddComponent<T>();
                }
                return _instance;
            }
        }
    }
}
```

