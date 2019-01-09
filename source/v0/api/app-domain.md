---
title: AppDomain
---

# AppDomain

您可以通过继承的方式来扩展基础AppDomain的行为，下面是`CatLib.ILRuntime.AppDomain`封装的一些方法。

## 获取ILRuntime AppDomain

通过Domain属性您可以获取ILRuntime原始的AppDomain。

```csharp
protected ILRuntimeDomain Domain { get; }
```

## 调用热更新主入口

在您的热更新代码都已经加载完成，一切都已经准备就绪的情况下，您需要调用热更新主入口来启动热更新。就如同所有的程序都有一个入口函数一样。

```csharp
public void Init(string main);
```

主入口接收一个参数`IApplication`，为主工程的CatLib应用程序。

示例:

- 热更新的代码：

```csharp
namespace Hotfix
{
    public static class Program
    {
        public static void Main(IApplication application)
        {
        }
    }
}
```

- 主工程代码：

```csharp
appDomain.Init("Hotfix.Program.Main");
```

## 加载热更新程序集

您在[和项目关联](../usage/link-project.html)的时候，需要加载热更新程序集，这样热更新代码才能够被执行。

```csharp
public virtual void LoadAssembly(Stream dll, Stream symbol = null);
```

> 在调试等级为：`DebugLevels.Production`的情况下，`symbol`调试符将无论传入何值，都将是无效的。

## 调用热更新的函数

您可以使用`Invoke`方法来调用一个热更新的函数，一般情况下您不会用到这个方法，因为您和热更新的交互都应该通过CatLib来进行。

```cssharp
public virtual object Invoke(string type, string method, object instance, params object[] @params);
```

## 生成热更新的实例

您可以使用`CreateInstance`来生成热更新中的实例，一般情况下您不会用到这个方法，因为您和热更新的交互都应该通过CatLib来进行。

```csharp
public virtual object CreateInstance(string type, object[] args = null);
```

## 注册Action和Func委托

- `RegisterActionDelegate`
- `RegisterFuncDelegate`

您可以通过以上两个函数来向ILRuntime[注册跨域委托](https://ourpalm.github.io/ILRuntime/public/v1/guide/delegate.html)。