---
title: day1
date: 2022-03-09 15:35:44
categories:
 - [计算机科学,Python,fast-api]
tags: 
 - 编程框架
 - Python
 - 软件技术
 - fastApi
---

# 第一讲

## FastAPI的主要特点
- 性能优越
- 开发效率高200% ~ 300%
- 减少约40%人为BUG
- 直观
- 易学易用
- 精简编码 代码重复率低
- 自带API交互文档 开发成果随时交付
- API开发标准化

## Starlett、Pydantic与FastAPI的关系
- Python的类型提示 type hints
- Pydantic是一个基于Python类型提示来定义数据验证，序列化和文档(使用JSON模式)库
- Starlette是一种轻量级的ASGI框架/工具包, 是构建高性能Asyncio服务的理想选择。
![fastapi](/assets/python/fastapi/fastapi1.png)

![fastapi](/assets/python/fastapi/fastapi2.png)

[ASGI](http://huatianbiao.top/computer-science/network-server/about-server/ASGI/)

## 搭建fastapi开发环境

### pydantic模块
Data validation and settings management using python type annotations.
使用Python的类型注解来进行数据校验和setting管理

pydantic enforces type hints at runtime, and provides user friendly errors when data verification failure.
pydantic可以在代码运行时提供类型提示，数据校验失败时提供友好的错误提示

Define how data should be in pure, canonical python; validate it with pydantic.
定义数据应该如何在纯规范的代码中保存，并用Pydantic验证它。

