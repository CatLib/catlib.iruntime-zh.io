---
title: 安装
---

# 安装

您需要简单的安装，来让扩展包在您的项目中生效。该扩展包是基于[CatLib核心库](https://catlib.io)研发的，所以在您的项目中必须安装有CatLib核心库。

> 如果您使用的是[CatLib For Unity](https://github.com/catlib/catlib)的引导库，请按照下面进行操作，否则根据自己的引导环境进行操作。

## 安装服务提供者

注册`服务提供者`到您的服务提供者列表。

- `Game/Config/Providers.cs`

```csharp
new ProviderILRuntime();
```

## 安装ILRuntime支持的应用程序

使用`ILRuntimeApplication`来替换`UnityApplication`应用程序。您需要通过覆写`CreateApplication`方法来实现。

- `Game/Main.cs`

```csharp
protected override Application CreateApplication(DebugLevels debugLevel)
{
    return new ILRuntimeApplication(this)
    {
        DebugLevel = debugLevel
    };
}
```