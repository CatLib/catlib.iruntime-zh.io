---
title: 和项目关联
---

# 和项目关联

`CatLib.ILRuntime`扩展库赋予了`CatLib`框架可以正常的在ILRuntime热更新代码中运行，但是扩展包并没有包含如何将热更新的代码文件加载到`ILRuntime.AppDomain`中，这一部分需要您根据项目情况来自行完成。

本文档，简叙了如何使用一种比较优雅的方式来和项目进行关联，并完成代码加载的操作。

除了文档以外，您也可以查看:[demo-how-to-use-catlib-for-ilruntime](https://github.com/CatLib/demo-how-to-use-catlib-for-ilruntime) 示例项目，该项目完整的展现了，如何关联。

## 建立关联服务

如果以CatLib的思维来处理问题，那么建立一个`服务`来解决对应的问题，那么再好不过了。

建议一个针对于项目的`AppDomain`，这个类将会继承自`CatLib.ILRuntime.AppDomain`：

```csharp
using CatLib.ILRuntime.AppDomain;
```

```csharp
public class AppDomainDemo : AppDomain
{
}
```

您可以在这个针对于项目的`AppDomain`中书写和ILRuntime相关的代码，例如[注册委托适配器](https://ourpalm.github.io/ILRuntime/public/v1/guide/delegate.html)，[注册项目相关的CLR绑定](https://ourpalm.github.io/ILRuntime/public/v1/guide/bind.html)等等。

```csharp
public class AppDomainDemo : AppDomain
{
    public AppDomainDemo(IApplication application, DebugLevels debugLevel)
        : base(application, debugLevel)
    {
        CLRBindings.Initialize(Domain);
    }
}
```

> `Domain`属性是ILRuntime的原始AppDomain。

## 建立关联服务的服务提供者

如果需要将关联服务注册到框架，那么您还需要注册对应的服务提供者：

```csharp
using CatLib;
using CatLib.ILRuntime;
```

```csharp
public class ProviderDemoILRuntime : ServiceProvider
{
    public override void Register()
    {
        App.Singleton<IAppDomain, AppDomainDemo>().Alias<AppDomainDemo>();
    }
}
```

在完成服务提供者后，您只需要将这个服务提供者注册到服务提供者列表，对应的支持就会自动生效。

您和ILRuntime相关的内容都应该在这个服务中完成，例如：加载热更新代码到AppDomain等等。

发散您的思维，这个服务几乎可以做到您所需求的任何事情。