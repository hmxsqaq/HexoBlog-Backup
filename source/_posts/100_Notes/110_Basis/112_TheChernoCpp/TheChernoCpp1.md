---
title: 「The Cherno-C++」学习笔记一（1-33）
tags:
  - C/C++
  - Note
categories:
  - Notes
  - C/C++
  - TheCherno
description:
  - "TheCherno的C++系列视频被誉为最好的C++教程之一<br>这里记录了我的一些笔记与理解，希望对你有帮助\U0001F61D"
cover: 'https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202304182342702.png'
abbrlink: 11201
date: 2023-04-18 23:37:00
---

![cherno](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202304182344099.jpg)

> **B站up神经元猫翻译版：[https://www.bilibili.com/video/BV1uy4y167h2](https://www.bilibili.com/video/BV1uy4y167h2)**

## 前言

C++因为其优异的性能，是游戏开发中最为常用的语言之一，目前的Unity与Unreal两大商用引擎的内核都是采用C++进行编写，作为一个有志于成为游戏开发者的人，这种语言怎么能不学一学呢？

本文基于油管著名博主，曾任职于EA的The Cherno的C++系列视频，The Cherno作为寒霜引擎的核心开发人员之一，对C++有着非常独到而深刻的理解

这些笔记记录了我通过The Cherno的系列视频学习C++的一些知识点与理解，希望对你有帮助😉

> **第一章（1-33）：[https://hmxs.games/posts/11201/](https://hmxs.games/posts/11201/)**
>
> 第二章（34-66）：[https://hmxs.games/posts/11202/](https://hmxs.games/posts/11202/)
>
> 第三章（67-99）：[https://hmxs.games/posts/11203/](https://hmxs.games/posts/11203/)

---

## C++是如何运行的（5-7）

![img](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202304190000568.webp)

> C++运行的过程实际便是将”.cpp文本文件“转换为”可运行的.exe文件/库文件“

- **预处理**：修改源文本（删除初始，处理宏定义/头文件等）

#为预处理符号，其后可以跟着`include`、`define`、`if`等

#include的作用仅为把对应代码**复制**过来（典中典之大括号），`cout`/`cin`即来源于`iostream`

- **编译**：将每个翻译单元转换为`.obj`目标代码（`.obj`为VS中，不同编译器生成的文件可能不同）

`.cpp`->翻译单元->`.obj`

一个cpp文件可以代表一个翻译单元，也可以由多个cpp代表一个翻译单元，这可以进行自定义

一个翻译单元会被编译为一个obj文件

- **链接**：找到入口函数（当需要生成.exe文件时），链接不同obj文件

每个可执行文件都需要一个入口（*一般是main函数，也可以自定义*），链接器便可以找到程序入口

同时，在一个项目中，我们可能会有多个翻译单元生成多个obj文件，如果没有连接器，它们之间便无法进行交互

当函数a声明在A翻译单元，但实现在B翻译单元中时，链接器便可以通过唯一的函数签名链接它们

（个人理解：一个函数的“名字”实际包含了其返回值+函数名+参数，其中任何一个不一样都是不一样的函数）

**编译与链接**是两个截然不同的阶段，当我们的程序出现问题是，发现问题出现在什么阶段是重要的

---

## 变量（8）

**变量**：变量允许我们命名我们存储在内存中的数据

在C++中我们有一些基本的数据类型，如`int`、`float`、`char`等

每个变量都有其固定用途，但实际上，我们并不一定要按其规定的用途使用它们：

> **不同变量类型之间的唯一区别，就是其所占的内存大小！**

在计算机看来，所谓变量无非就是一些内存空间，里面都是一些数字而已

而变量类型其实是程序员对于变量用途的假设，其影响着计算机的输出逻辑

如数字65，其对`char`类型的变量代表字符A，而对`int`类型的变量代表整数65，他们在内存中并没有什么不同，不同的是我们对其的假设，不同的是其最后呈现给我们的方式

*`sizeof`关键字可以用于查询不同变量类型占的内存大小*

---

## 函数（9）

函数是我们编写的代码块，被设计用来执行特定任务

C++类中的代码块被称为方法，函数指的是类之外的代码块

函数的用处：避免代码的重复编写，便于程序的管理

*但是需要注意的是，无意义地添加过多函数只会让程序变得凌乱，让代码难以管理*

函数的全部特征：

- 有函数头和函数体
- 可以接受参数
- 可以有返回值
- 需要有函数定义

---

## 头文件（10）

C++的函数通常由两部分组成-**声明与定义**

- 声明意味着告诉编译器有这样一个函数存在，我们便可以在此翻译单元中调用该函数，每个翻译单元同一函数只能声明一次
- 定义则为函数的具体代码，程序中的每个函数只能被定义一次

而当我们需要跨文件调用函数时，因为函数不能有二义性，我们便只能在需要调用函数的地方声明其存在

而**头文件便是函数声明的集合**，当我们需要调用某一函数，但其实现在别的地方时，我们便可以引入包含了该函数声明的头文件来声明其存在

这样一来我们便不需要在每次使用函数时都声明一遍了

*需要注意的是，`.h`头文件与`.cpp`源文件实际上并没有本质区别，理论上我们同样可以在头文件中定义函数，但约定俗成的，我们不会这么做*

**#pragma one**：这一预处理语句代表着在同一翻译单元内，这一头文件只会被复制一次

**< >与“ ”**：通常尖括号代表引入内部文件（`iostream`），而引号代表引入外部文件（如自己定义的头文件）

*实际上，“”几乎可以引入任何文件，但我们通常使用< >来引入系统内部文件*

*`iostream`作为C++标准库没有后缀名，这令人疑惑，实际上这是设计者为了和C语言的标准库`stdlib.h`进行区分，没有特别的意义*

---

## 如何进行代码调试（11）

**调试的方法**：设置断点与读取内存

**调试（Debug）的意义**：从代码中清除错误（看上去像是一句废话🤣），调试允许我们逐行逐句运行代码，并且实时检查内存中的数据

**断点**：程序会在运行到断点行时暂停（断点行不会被运行）

在调试界面中，IDE会给我们提供许多的逐行逐句运行代码的方法，如VS提供的step into/over/out，实际应用中灵活使用即可

一个程序是由内存构成的，我们程序中的一切都储存在内存中，通过设定断点，我们可以暂停程序，在特定的时间检查特定的代码行中的内存情况

---

## 条件与分支（12）

`if`语句包含一个condition和其statement，其代表的便是如果condition为真则执行statement

在汇编中，`if`语句实际上是一个`goto`，即如果condition为真，则跳转到某一部分的内存开始运行

**反编译Debug**：当我们进行断点调试时，我们可以右击正在运行的行，点击“转到反汇编”，便可以看到我们的代码的汇编形式了（但是通过这种方式查错会是个nightmare，而且我也不会汇编，所以仅作了解啦🤐）

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202305081957440.png" alt="image-20230508195728282" style="zoom: 33%;" />

另外需要注意的是，`else if`其实并不是C++的关键字，实际上这只是将一个`if`包含在了`else`中，并且写在了一行，有点像一个语法糖🤣

---

## 关于VS的设置（13）

**如何用VS创建一个优雅的C++项目**

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202305082014626.png" alt="image-20230508201433600" style="zoom:50%;" />

我们在解决方案管理器中看到的这些文件夹其实都是虚文件夹，是被称为“filter筛选器”的东西，其对文件在磁盘中的组织没有任何影响，这只是一种标示，实际上你把文件放哪都行，对文件的实际位置没有任何影响

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202305082017726.png" alt="image-20230508201724681" style="zoom:50%;" />

我们可以通过点击“显示所有文件”来查看我们真正的项目文件夹

<img src="https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202305082049460.png" alt="image-20230508204920418" style="zoom:50%;" />

通过设置属性管理项目文件夹

---

## 循环与控制流语句（14，15）

循环的语法不再赘述

需要注意的是，`for`与`while`循环本质上其实并无区别，他们完全可以被用来做一样的事情

何时使用`for`，何时使用`while`更像一种习惯或风格，而不是规则

一般情况下，如果你有一个已经存在的确定的条件，如游戏中的loop，使用`while`更方便，这更接近与一种对状态的监控，因为条件不变，我们不需要在每次循环之后改变这个条件

而当我们需要处理一个状态会不断迭代的循环，如遍历一个确定长度的数组时，用`for`更方便

控制流语句：

- `continue`-进入循环的下一个迭代，如果没有下一个迭代了，则直接跳出循环
- `break`-跳出循环，也会在switch中被用于跳出switch
- `return`-跳出当前函数，根据函数返回值类型返回对应值

---

## 指针（16）

指针是C++中最重要的概念之一，但实际上，它并没有我们想的那么复杂

要讨论指针，便应该先明白何为内存。内存，是计算机编程中最重要的东西，一切程序在运行时都储存在内存中，电脑通过访问这些内存来读取执行指令，同样的，变量也储存在内存中，当我们声明一个变量，其便会在内存中为其开辟一片内存空间。如果我们形象的去理解内存，内存便像一个无限长的纸带，纸带上有很多格子，每个格子包含一个地址与8个二进制位也就是1字节的数据。而指针便是一个储存了指定格子地址的变量。

> **指针是一个整数、一个储存内存地址的数字，指针就像其他变量一样，但其不是保存着某一变量的值本身，而是一个变量的内存地址，而内存地址同样是一个值，是一个整数**
>
> **类型是无意义的！类型只是为了让我们处理数据更方便的一种虚构，一种抽象，所有类型的指针都是保存内存地址的整数**

```c++
void* ptr = 0;
// 使用NULL/nullptr效果相同，NULL是0的宏定义，nullptr是C++11中的关键字代表空指针
```

上述代码创造了一个空指针，这是我们可以创造的最简单的指针

**指针的大小：**也就是这个整数有多大，取决于很多东西，可能是16位/32位/64位的整数

**指针的逆向引用：**在指针前添加一个“*”代表对指针内容的逆向引用（`dereference`），这意味访问指针所指向的内存，如此我们便可以对指针指向的内存中的值进行读写

类型是我们告诉编译器的，我们对数据类型的设想，这样编译器就知道应该分配多大的内存空间了，但需要注意的是，是“我们告诉编译器的”，编译器自己并不知道某一数据的实际意义，以及我们说的到底对不对，**而指针中只有代表着内存地址的数字，而不包含数据，**这意味着一旦我们出错，程序便会出现问题

**Don't over think it!**

---

## 引用（17）

> **根本上，引用只是指针的伪装，只是在指针上的语法糖，使它们更易阅读和理解**

引用（`references`）就像变量的别名或快捷方式，它不像指针一样作为一个新的变量存在与内存之中，其并没有真正的储存空间，其只是一个变量的引用（`reference`）

```C++
int a = 10;
int& ref = a;
// 主要注意的是&符号和前面的变量类型是一体的，而不是取地址
```

上述代码创造了一个变量`a`的引用`ref`

`ref`实际上并不存在，其不是一个真正的变量，它只存在于我们的源代码中，编译运行时，编译器只会为`a`开辟内存空间

那么引用到底有什么用呢？

**下面是一个简单的例子:**

```C++
void Add(int value){
    value++;
}
int main(){
    a = 1;
    Add(a);
}
```

在上述代码中，我们创造一个`Add`函数希望实现将传入值递增的效果，但实际上，这段代码没有任何意义，我们实际上是复制了变量`a`的值给了`value`，然后使`value`递增，这并不会实际影响`a`的值

而如果我们可以通过传入指针来解决这个问题

```c++
void Add(int* value){
    (*value)++
}
int main(){
    a = 1;
    Add(&a)
}
```

改动后，我们将变量`a`的地址传入了`Add`函数，后通过指针的逆向引用改变了变量`a`的值，这可以实现我们想要的效果

但这会让代码添加很多繁杂的修饰，降低代码的可读性，而引用可以更方便的解决这一问题

```c++
void Add(int& value){
    value++;
}
int main(){
    a = 1;
    Add(a);
}
```

我们只需要将传入`Add`函数的参数改为int&引用类型，使`a`作为引用传入即可，十分方便优雅

对于引用来说，没什么是引用能做而指针做不了的，其是指针的封装、语法糖，可以让代码变得更加简洁明了

但使用引用也有几个限制：

- 声明引用必须立刻赋值，无法单独声明

```C++
int& ref; // 这是错误的语法
```

- 引用赋值后引用对象无法改变；当我们需要改变对象时，我们可以使用指针

```C++
int a = 1;
int b = 10;
int& ref = a;
ref = b;
// 这样写的结果是 a = 10 , b = 10
// ref仍然是a的引用
```

---

## 类（18）

**面向对象编程**：一种编写代码的模式

Java/C#是面向对象的语言，而C由于没有类/对象的概念所以无法采用面向对象的模式，而C++并没有限定我们写代码的方式，我们可以像C一样面向过程的编程，同样也可以运用C++新添加的类等特性来使用面向对象的模式组织我们的代码

而“类（`class`）”是面向对象编程中最重要的特性之一，简单的说

> **类是一种将数据与功能组合在一起的一种类型**

以下是一个`class`的实例：

```c++
class Player
{
public:
	int speed_;
	int posX_, posY_;

	void Move(int x, int y)
	{
		posX_ += x * speed;
		posY_ += y * speed;
	}

private:
	int health_;
};

int main()
{
	Player player = new Player();
}
```

在上述代码中我们创建了一个`Player`类，并在`main`函数中将其进行了实例化

`Player`类中包含3个公共变量，1个公共方法（在类中的函数被称为方法），1个私有变量

`player`是类型为`Player`的变量，称为“**对象**”，创建类对象的过程被称为**实例化**

**类的本质是一种数据与方法的组织方式，一种语法糖，可以让我们的程序更加的简洁明了，其并不是万能的**

---

## 类与结构体的区别（19）

> **几乎没区别**

在技术上，类与结构体的唯一区别便是类中的变量与方法默认可见度为`private`，结构体则为`public`

C++中`struct`得以存续的原因很大程度上源于对C的兼容性考虑，因为C中没有类

而在具体使用中，如何使用`struct`与`class`完全取决于每个人编程习惯与风格

---

## 如何写一个C++类（20）

在接下来的几章中，我们将开始编写C++的类，将一个基础版本的类逐渐升级到更高版本的类，体会其中的过程与区别，这会让我们明白什么是”Good Code“

而我们将要编写的类是**Log类**

Log类的功能是将信息输出到控制台，这听起来十分简单，但实际上这对我们的调试与开发非常重要，同时，其可复杂可简单，是一个非常好的例子

**需求分析：**我们将要实现的初级Log类包含`error`、`warning`、`message`三个层级，Log类可以通过这些层级对输出的东西进行控制

让我们开始写代码吧！

```c++
class Log
{
public:
	const int LogLevelError = 0;
	const int LogLevelWarning = 1;
	const int LogLevelMessage = 2;
private:
	int _logLevel = LogLevelMessage; // 当前log等级
public:
	void SetLevel(int level) // 设置log等级
	{
		_logLevel = level;
	}

	void Error(const char* message) // 输出Error等级的信息
	{
		if (_logLevel >= LogLevelError)
			std::cout << "[ERROR]:" << message << std::endl;
	}

	void Warning(const char* message) // 输出Warning等级的信息
	{
		if (_logLevel >= LogLevelWarning)
			std::cout << "[WARNING]:" << message << std::endl;
	}

	void Message(const char* message) // 输出Message等级的信息
	{
		if (_logLevel >= LogLevelMessage)
			std::cout << "[MESSAGE]:" << message << std::endl;
	}
};

int main()
{
	Log log;
	log.SetLevel(log.LogLevelWarning);
	log.Error("Hello World!");
	log.Warning("Hello World!");
	log.Message("Hello World!");
	std::cin.get();
}
```

以上代码实现了我们的基本需求，但是实际上这并不能被称为什么“Good Code”，it just simple code.

之后我们会对Log类进行迭代，使其变得优雅易用

---

## static静态（21，22，23）

取决于上下文，C++中的`static`关键字有多个意思：

- **当static在类/结构体外时：**`static`用于限定作用域，被`static`修饰的变量/函数只会在当前翻译单元的内部链接

```c++
// 翻译单元一
static int a = 10;

// 翻译单元二
extern int a; // extern代表
int main()
{
    std::cout << a << std::endl;
}
```

在上面的代码中，翻译单元二中的`extern`代表外部链接，其会为a变量在其他翻译单元中寻找链接，我们希望它能找到翻译单元一中的`a`，将其链接过来

但如果我们编译上述代码，我们会得到报错，因为翻译单元一中的`a`被`static`修饰了，这意味着其对链接器来说是不可见的，类似于类中的`private`

- **当static在类/结构体内时：**`static`意味这共享实例内存，静态变量在你创建的所有类的实例中都只有一个实例

```c++
struct Entity
{
	static int x, y;

	static void Print()
	{
		std::cout << x << " " << y << std::endl;
	}
};

int Entity::x;
int Entity::y;

int main()
{
	Entity::x = 10;
	Entity::y = 20;
	Entity::Print();
	std::cin.get();
}
```

在上述代码中，我们可以发现我们并没有对结构体`Entity`进行实例化，但是我们仍然可以访问其中的静态变量`x`、`y`，静态方法`Print`

在结构体与类中的静态近似于静态全局变量，但将其放在类中可以更好的帮我们组织代码，满天乱飞的全局变量将是非常灾难性的

需要注意的是，静态成员函数无法访问非静态成员变量，因为静态方法没有类实例，非静态方法都依赖于一个类实例而存在，静态方法的组织方法类似于在外部编写函数，当静态方法访问非静态成员变量时，其无法得知它们到底是什么，依赖于哪个实例存在，亦或者其根本没有实例

- **当static在局部作用域时：**被`static`修饰的变量声明周期会和程序一样长，不会被删除，但作用域仍然为当前局部

```c++
void Add()
{
    static int i = 0;
    i++;
    std::cout << i << std::endl;
}
```

当我们多次调用上述函数时，若变量`i`没有被`static`修饰，那么我们输出的将一直是1，但现在，`i`变量会不断递增

这与将i声明在函数外的效果相似，但区别是只有在`Add`函数的局部作用域中i才可以被访问

局部作用域中的`static`有时可以很大程度上帮我们优化代码，以下为其在单例实现中的应用：

```c++
class Singleton
{
private:
	static Singleton* _instance;
public:
	static Singleton& Instance()
	{
		return *_instance;
	}

	void Hello() {}
};

Singleton* Singleton::_instance = nullptr;

int main()
{
	Singleton::Instance().Hello();
	std::cin.get();
}
```

以上是单例的一种实现，但实际上，运用局部作用域的`static`可以实现一样的效果但大幅优化我们的代码

```c++
class Singleton
{
public:
	static Singleton& Instance()
	{
		static Singleton _instance
		return _instance;
	}

	void Hello() {}
};

int main()
{
	Singleton::Instance().Hello();
	std::cin.get();
}
```

---

## 枚举（24）

枚举`enum`是一个数值集合，是一种给值命名的方式，当我们想用整数来标示某些状态或数值时，我们便可以采用枚举来让其有更好的可读性

同时枚举也可以限定类型，让枚举类型的变量只能是枚举类型，但其背后代表的仍然只是一个整数

```c++
enum Player
{
	Idle,
	Attack,
	Chase
};
```

以上是一个简单的枚举，我们也可以为枚举指定数值，数值可以是任意整数，如果我们不指定则默认从0开始递增

而在我们之前的Log类中，枚举便可以用来表示Log级别，以下是运用了枚举的Log类：

``` c++
class Log
{
public:
	enum Level
	{
		LevelError,
		LevelWarning,
		LevelMessage
	};
private:
	Level _logLevel = LevelMessage;
public:
	void SetLevel(Level level)
	{
		_logLevel = level;
	}

	void Error(const char* info)
	{
		if (_logLevel >= LevelError)
			std::cout << "[ERROR]:" << info << std::endl;
	}

	void Warning(const char* info)
	{
		if (_logLevel >= LevelWarning)
			std::cout << "[WARNING]:" << info << std::endl;
	}

	void Message(const char* info)
	{
		if (_logLevel >= LevelMessage)
			std::cout << "[MESSAGE]:" << info << std::endl;
	}
};
```

---

## 构造函数（25）

> **构造函数是一种特殊类型的方法，他在每次实例化对象时运行**

在构造对象时，初始化对象中的变量是非常常见的需求，我们可以通过编写函数，在每次需要初始化时手动调用一遍该函数来实现这一需求，但这意味着我们需要在每次构造一个新对象时都调用一遍初始化函数，这非常的麻烦，所以**构造函数（constructor）**诞生了，每次你构建对象时构造函数都会被自动地调用

```c++
class Entity
{
public:
    Entity(){ } // 构造函数
}
```

构造函数就像其他方法一样被定义，特殊的是其没有返回值，并且函数名需要与类/结构体名相同；如果我们没有定义构造函数，系统便会为我们自动生成默认的函数体为空的构造函数，即什么也不做

*在Java与一些其他语言中，一些类型的变量如`float`、`int`会自动地被初始化为0，但是C++中并不是，我们需要手动地初始化所有的变量*

需要注意的是，构造函数只有在实例化对象时才会被调用，调用类中的静态变量/函数不会实例化对象，所以也不会触发构造函数

```c++
class Entity
{
public:
    Entity(){ } // 构造函数
    
    static void Hello() { }
}

int main()
{
    Entity::Hello(); // 此时Entity类的构造函数并不会被触发
}
```

同理，我们也可以通过让构造函数私有化或使用`delete`方法删除构造函数的方式，使得类无法被实例化，这在我们只希望用户以静态方法的形式使用类时非常有用

```c++
class Entity1
{
private:
    Entity1(){ } // 私有构造函数
}

class Entity2
{
public:
    Entity1() = delete; // 私有构造函数
}

int main()
{
    Entity1 e1;
    Entity2 e2;
    // 上述两行都无法通过编译
}
```

---

## 析构函数（26）

*析构函数（Destructor）是构造函数的邪恶孪生兄弟（乐）*

> **构造函数（Constructor）在实例化对象时运行，而析构函数（Destructor）则在销毁对象时运行**

而同样的，在功能上，构造函数通常用来初始化变量，声明内存空间，而析构函数则常常用来卸载或销毁变量，释放内存空间

```c++
class Entity
{
public:
    ~ Entity(){ } // 析构函数
}
```

析构函数与构造函数在声明与定义时的唯一区别，就是放在析构函数前需要多加一个波浪号`~`

---

## 继承（27）

面向对象编程（OOP）是一个庞大的编程范式，而类之间的继承是其的一个基本方面，它是我们可以实际利用的最强大的特性之一

> **继承允许我们创建一个存在相互关联的类的层级结构，即我们可以基于一个公共基类创建子类**

继承强大的主要原因是它可以帮助我们避免代码的重复，我们可以将相同的需要重复编写的代码放在一个公共基类中，这样其子类就都拥有了基类中的代码

```c++
class Entity
{
public:
    float X, Y;
    
    void Move(float x, float y) {}
}

class Player : public Entity
{
public:
    const char* Name;
    
    void PrintName() {} 
}

int main()
{
    Player player;
    player.Move(5f,4f);
}
```

在上述代码中，`Player`类继承了`Entity`类，这意味着`Player`类拥有了`Entity`类中的所有代码

同时，继承可以引出OOP的另一个重要概念-**“多态”**

多态即一个类型可以拥有多个类型，在上述例子中，`player`既是`Player`类也是`Entity`类，我们可以在任意需要`Entity`类的地方用`player`来进行，因为`player`拥有`Entity`类中的一切代码

---

## 虚函数（28）

> **虚函数允许我们在子类中重写方法**

```c++
class Entity
{
public:
	std::string GetName() { return "Entity"; }
};

class Player : public Entity
{
public:
	std::string GetName() { return "Player"; }
};

int main()
{
	Entity* e = new Entity();
	std::cout << e->GetName() << std::endl;
	Player* p = new Player();
	std::cout << p->GetName() << std::endl;
    
    Entity* e2 = p;
	std::cout << e2->GetName() << std::endl;
	std::cin.get();
}
```

在上述代码中，`Player`类继承于`Entity`类，它们中都有一个`GetName()`方法会输出一些东西，这看起来非常和谐，最后也确实输出了“Entity”与“Player”

但若我们引入OOP中的多态思想，那么问题便会出现，如果我们创建一个`Entity*`类型的指针`e2`指向`p`，再调用`e2`的`GetName()`方法，我们便会发现其只能输出“Entity”，但实际上该指针实际指向的是`Player`类型，这与我们的预期不符

问题的核心便是多态虽然为类带来了多类型，但是编译器只会依据声明的类型调用方法，但实际上我们传入的参数的类型不一定会是我们声明的类型，也有可能是其子类，这时**虚函数**就应该出场了

虚函数引入了一种叫做动态联编（Dynamic Dispatch）的东西，它通过虚函数表（v表）来实现编译，v表是一个包含了基类中的所有虚函数的映射表，这样我们在其运行时，便可以将虚函数映射到正确的覆写函数（override）上

```c++
class Entity
{
public:
	virtual std::string GetName() { return "Entity"; }
};

class Player : public Entity
{
public:
	std::string GetName() override { return "Player"; }
};
```

如上述代码所示，只需在对应方法前添加关键字`virtual`与`override`便可实现方法的覆写，现在`e2`可以正常输出“Player”了

*其实`override`并不必须，但是为了可读性，咱们最好还是加上*

虚方法会产生一定程度的额外内存与性能开销，但是实际上影响很小，who cares？用就完事了🤣

---

## 纯虚函数/接口（29）

纯虚函数是一种特殊类型的虚函数，其本质上与Java/C#中的抽象方法或接口相同

> **纯虚函数允许我们在基类中定义一个没有实现的函数，然后强制子类实现该函数**

在OOP中，创建一个类，只由未实现的方法构成，然后强制子类实现它们，是非常常见的需求，我们常常称其为**接口**

```c++
class Entity
{
public:
	virtual std::string GetName() = 0；
};
```

将一个虚方法的函数体删除，并使其等于0，便形成了一个纯虚函数，若其没有被某一子类实现，则该子类与其都无法进行实例化

接口（`interface`）通常作为对“类存在某一方法”的检查与保证而存在，C#中有专门的`interface`关键字，但C++中没有，C++的接口只是一个都是纯虚函数的类

```c++
class Printable
{
public:
    virtual std::string PrintName() = 0;
}

void Print(Printable* obj)
{
	std::cout << obj->PrintName() << std::endl;
}
```

如在上面的代码中，`Printable`类作为一个接口存在，其保证了其所有子类都一定实现了`PrintName()`方法，同时多态的特性使得其所有子类同时也是`Printable`类，这样`Print()`函数的参数中，将指向`Printable`类的指针传入即可，无需关心传入的具体类型，只需传入的类继承了`Printable`

---

## 可见性（30）

> **可见性是一个属于OOP的概念，它指的是类的某些成员或方法实际上能被谁调用（类似于访问权限）**

*需要注意的是，可见性是对程序实际运行完全没有影响的东西，不影响性能与内存，它是纯粹存在在语言中的东西，只是为了帮你更好地组织代码*

**C++中有且只有三个可见性修饰符：**

- `public`：都可见
- `protected`：继承体系中都可见
- `private`：只有在类内与其友元（`friend`）可见

**为啥不让一切`public`呢？**

为了让代码更加容易维护、容易理解，不管阅读代码还是拓展代码

代码是个错综复杂的东西，而可见性是一种”警告标示“，当一个方法/字段被标记为`private`时，这意味着这段代码的作者不希望其被外界调用，这告诉了别人“这不是你该碰的东西”，同时，也代表着修改这个可能带来意料之外的灾难

如一个UI类，你想修改一个UI的位置，直接访问这个类中的私有的`posX`，`posY`很可能并不会带来预期的结果，虽然内存中xy的值变了，但是显示器需要刷新，或许一些其他的东西需要修改，这一切你都无从得知

---

## 数组（31）

> **数组是元素的集合，一般是一堆相同类型的变量**

数组的定义非常简单，`类型 数组名[大小]`即可，如`int example[5]`定义了一个可以容纳5个整形变量的数组

*当我们试图访问`example[-1]`/`example[5]`这样超出数组范围的内存时，在Debug模式中，程序会崩溃，但在Release模式中，你可能不会得到报错信息，这意味着你写入了不属于数组的内存，让一些不改被改变的内存改变了，这会导致非常难以Debug的后果，我们需要做好边界安全检查，尽量防止这种情况出现*

关于数组最重要的一点是，其**连续地储存数据**，这意味着其将元素放到了一排进行储存，在内存上它们是连续地，如果我们有一个大小为5的整形数组，其便会连续地占据20个字节的内存，每个整形变量直接有这固定的内存偏移

关于**数组的创建**，存在着两种方法对数组进行创建，它们分别会将数组创建在`栈`或者`堆`中

```c++
int main()
{
	int example[5]; // 栈中
	int* example2 = new int[5]; // 堆中
	std::cin.get();
}
```

创建在栈中的变量在作用域结束时，其内存会被自动地释放

创建在堆中的则不同，需要我们通过`delete`关键字手动地对其内存进行释放

```c++
delete[] example2；// 释放栈中的内存
```

在C++11的库中，存在着内置的数组数据结构`std::array`,它有着许多优点，如边界检查、记录数组大小

而在原始的数组中，动态地取得数组的大小是一件非常困难、危险而不可靠的事，并不存在类似`size`的直接返回数组大小的字段或方法，要做到这件事，我们需要用到`sizeof(example) / sizeof(int)`的方法，但是我们经常采用指针来表示数组，如果传入的是指针而不是实际的数组那么我们将可能不会得到预期的结果；对于原始数组来说，我们通常需要手动维护一个`static`的常量来标示其大小

而对于`std::array`来说事情便会变得简单许多：

```c++
int main()
{
	std::array<int, 5> example;
	std::cout << example.size() << std::endl;
	std::cin.get();
}
```

通过`size()`方法我们可以轻松地获得数组大小，这的确会带来一些额外的开销，但也会让你的代码更加安全，anyway，who cares?🤣

---

## 字符串（32）

> **字符串本质上是一个连着一个的一连串字符，对于我们来说，这是一种能够表示和处理文本的方法**

字符，即为`char`数据类型，而字符串实际上是字符数组，数组是元素的集合，故而字符串便是字符的集合，一组字符组成了字符串文本

```c++
const char* name = "Hmxs";
```

这是C语言风格定义字符串的方式，C++中存在`string`库让一切变得简单，但是从源头了解它们如何工作仍然很重要

实际上在声明中，我们并不一定要加上`const`关键字，但人们通常这样做的原因是，不想改变字符串的值，因为字符串从整体是不可变的，这是一个固定分配的内存块*（在VS中，单独更改其中的某个字符是可行的，如果你希望这样做，就不能加上`const`，但通常我们不应该这样改变字符串，这可能导致未定义行为的错误，在一些编译器如Clang中，这种做法是被禁止的）*，如果你需要一个更大的字符串，你需要执行一个全新的分配并删除旧有的字符串

**在字符串数组的末尾都存在一个`\0`的空终止字符，其用来标示字符串的结束**，因为字符串实际上是一个`char*`类型的指针，编译器并不能知道它有多大，需要一个特定字符作为标示，而当我们手动声明字符串数组时，如`char name[5]`我们需要手动的在其最后添加`‘\0’`或`0`作为空终止字符，否则字符串数组无法成立，并且在我们调用它时，其会不断向后寻址，知到遇到空终止字符，这将导致很多问题

**C++中的字符串：**

在上面的叙述中，我们已经了解了字符串到底是如何工作的了，是时候make things easier了

C++中的模板库`std::string`实际上只是一个`char`数组，其有着许多方法来帮助我们更方便地操作这个数组

```c++
std::string name = "Hmxs";
```

直接使用`std::string`便可以声明一个字符串变量，string有一个构造函数，它接受`char*`或`const char*`参数

在`std::string`中存在着许多方便的方法，如重载过的`+=`运算符可以帮助我们快速链接两个字符串，而`size()`可以返回字符串长度，`find()`在字符串中进行查询，下面是一些用例：

```c++
int main()
{
	std::string name = std::string("Hmxs") + "Hello";// 链接字符串法1
	name += "Hi!"; // 链接字符串法2
	bool contains = name.find("xs") != std::string::npos; // 查找匹配
	std::cin.get();
}
```

当我们传递std::string类型的变量作为参数时，我们通常不会直接传递它们，因为直接传递类对象实际上是一个复制的过程，而复制字符串意味着你需要在堆上重新分配一片内存空间并且进行字符串的复制，这个过程效率很低，故而我们通过采用被`const`修饰的引用来进行传递，`const`意味着不可变，我们承诺不会在这其中对字符串进行修改

```c++
void PrintString(const std::string& string)
{
	std::cout << string << std::endl;
}

int main()
{
	std::string name = "Hmxs";
	PrintString(name);
	std::cin.get();
}
```

---

## 字符串字面量（33）

> **字符字面量是在双引号之间的一串字符**

```c++
"Hmxs";
```

我们可以像这样创建一个字符串字面量，而它最后会变成什么，取决于很多因素

在最基本的情况下，`"Hmxs"`会是一个const char数组，长度5，因为在字符串的最后需要存在空终止字符（若我们在字符串中间添加'\0'，会破坏字符串的构成）

在我们使用字符串字面量时，我们最好为我们的声明加上`const`，虽然VS的编译器允许我们使用`name[2] = ‘a’`这样的代码，但这往往是不可靠的，在release模式下，字符串字面量的内存会被储存在只读内存中，修改它们往往不会成功

C++还提供了许多额外的字符串类型：

```c
const char* name = u8"Hmxs"; // 1字节
const wchar_t* name2 = L"Hmxs"; // 2字节 宽字符
const char16_t* name3 = u"Hmxs"; // 2字节
const char32_t* name4 = U"Hmxs"; // 4字节
```

关于字符串字面量的内存：字符串字面量**永远**保存在内存的只读区域内
