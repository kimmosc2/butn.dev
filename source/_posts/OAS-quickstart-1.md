---
title: OpenAPI Specification 快速入门（一）
date: 2021-06-27 10:28:29
tags: OpenAPI 快速入门
---
## 写在前面
如果你英语不错，我建议你阅读官方的:[The Open API Specification Explained](https://oai.github.io/Documentation/specification.html)，笔者水平有限，并且不是这方面的专家，本文仅作为概括参考，如有错误，欢迎指正。 
  
## 什么是Open API Specification(OAS)?
以下内容源自OpenAPI官方的介绍
> The OpenAPI Specification (OAS) defines a standard, language-agnostic interface to HTTP APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined, a consumer can understand and interact with the remote service with a minimal amount of implementation logic.  
> An OpenAPI definition can then be used by documentation generation tools to display the API, code generation tools to generate servers and clients in various programming languages, testing tools, and many other use cases.

如果你还是不能理解,那么我这样说你或许可以更加易懂一些:  

>我们在开发时会写接口文档，来描述接口列表，接口的作用、使用方法、参数和返回值等。而OAS就类似一份接口文档，用json和yaml的格式，包含着上面所说的内容。

那你可能会疑惑，那既然有了接口文档，为什么还需要OAS?它对比传统的接口文档有什么优势吗?

## OpenAPI Vs 接口文档
过去我们在开发完功能或接口后，会使用**手工**的方式来书写文档，而手工文档往往是**滞后**的，不仅时间落后于接口，甚至有可能出现接口做了很大的修改，而文档还停留在最初版本(甚至没有文档)的情况发生。

### 那么OAS是如何解决这个问题的呢?

在开发时,我们会使用OpenAPI规范来制定自己的接口(其实就是使用OpenAPI Specification写了一份普通的yaml或json文件)。之后我们可以使用**符合OpenAPI规范的代码生成工具**来直接生成对应的代码，包括了参数校验等功能，不仅减轻了我们的工作量，让我们能专注于业务代码的开发，并且在接口变更时，我们只需要更改对应的OpenAPI描述文件，并重新生成代码就可以完成接口的变更了。并且我们可以使用工具生成全面美观的接口文档，相比我们过去手动书写代码文档是否规范并简单了许多?

## OpenAPI初探
一份最基础的OpenAPI描述文件必须包含以下几个字段:  
  
 - `openapi`: 这份文档所使用的的OpenAPI版本，不同的OAS支持不同的字段和语法,最新版为3.x.x
 - `info`: 这份接口文档的详细信息,比如描述、用途等
    - `title`: 接口文档的标题
    - `version`: 文档版本,注意这个版本和OAS的版本不同,这个版本描述的是你接口的版本
 - `paths`: 具体的REST API接口列表
  

下面给出了一个**最简单的,符合OAS的**接口描述示例
```yaml
openapi: 3.1.0
info:
  title: A minimal OpenAPI document
  version: 0.0.1
paths: {} # No endpoints defined
```
  

## Wrapping Up
本文讲了OAS的基本概念，对比普通接口文档的优势，并介绍了OAS最基本的三个字段(field),`openapi`,`info`和`path`字段,其中info字段又必须包含`title`和`version`字段。  
  

<hr>
  

**我是赵不贪,欢迎关注我的微信公众号:阿贪爱学习**