---
title: 代码加载器
---

# 代码加载器

CatLib For ILRuntime 扩展包使用代码加载器来加载您的热更新代码，您只需要实现代码加载器接口，并将接口注册到框架，代码加载器就会自动运行。

## 必须实现的接口

您必须实现:`CatLib.API.ILRuntime.ILoaderAssembly`接口。
