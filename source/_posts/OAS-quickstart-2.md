---
title: OpenAPI Specification 快速入门（二）
date: 2021-06-27 13:47:16
tags: OpenAPI 快速入门
---

上一节介绍了OpenAPI Specification的概念和必要的组成部分，这节我们来学习API Endpoints这一概念
<br>
<br>

## 什么是Endpoints?
我们之前说过,OAS就是一份接口文档，每个Endpoints可以理解为一个接口,在path路径下可以有许多的Endpoints,整个OAS几乎都是围绕着Endpoints这一概念来展开的。
<br>
<br>

## 从一个例子开始
```yaml
openapi: 3.1.0 
info:
  title: Tic Tac Toe
  description: |
    This API allows writing down marks on a Tic Tac Toe board
    and requesting the state of the board or of individual squares.
  version: 1.0.0
paths:    # Path Object
  /board: # Path Item Object
    ...
  /foo:   # Path Item Object
    ...
  /bar:   # Path Item Object
    ...
```
我们来看一下上面这份示例文档,其中`openapi`,`info`和`path`字段我们在上一节已经讲过,值得注意的是,`info`字段下增添了`description`字段，用来描述整个文档。而path下也新增了三个接口，`/board`,`/foo`,`bar`这三个就是本节要介绍的Endpoint。  
例如:我们的域名为`example.com`,那么这个文档就描述了`example.com/board`,`example.com/foo`和`example.com/bar`这三个接口。  
<br>

**注:在yaml文件中,#后面的内容为注释**
<br>
<br>

## Path Item Object
通过上文我们知道,path字段下可以定义许多Endpoint,每个Endpoint又叫做[Path Item Object](https://spec.openapis.org/oas/v3.1.0#pathItemObject),而每个Endpoint又可以定义不同的HTTP Method，叫做[Operation Object](https://spec.openapis.org/oas/v3.1.0#operation-object)

示例:
```yaml
paths:
  /board: # Path Item Object
    get:  # Operation Object 
      ...
    put:  # Operation Object
      ..
```

<br>
<br>

## Operation Object
在`Path Item Object`的基础上，我们又扩充了`Operation Object`  

示例:
```yaml
paths: # Path Object
  /board: # Path Item Object
    get:  # Operation Object
      summary: Get the whole board
      description: Retrieves the current state of the board and the winner.
      parameters:
        ...
      responses: # Responses Object
        ...
```
`Operation Object`对于接口进行了更加详细的补充和说明,每个`Path Item Object`可以分为许多`Operation Object`，你可以定义同一个接口针对不同HTTP请求头的不同行为。  
可以看到,`Operation Obejct`包括对HTTP Method的概要、描述、参数和响应的介绍。
<br>
<br>

## Responses Object
```yaml
paths: 
  /board:
    get: # Operation Object
      responses: # Responses Object
        "200": # response status code
          description: Everything went fine.
          content:  
            ...
```
顾名思义，`Responses Object`描述的就是响应的内容,包括状态码等内容，这部分内容展开太长，我们将在以后的内容中介绍。
<br>
<br>

## 通过实例学习
上面几部分的内容都是对OAS中的对象进行解释，尽管给出了示例，但没有一点经验的读者依然可能会觉得一头雾水，难以理解，下面我们给出一份使用“到目前为止”所学的所有知识而组成的例子吧。

```yaml
openapi: 3.1.0
info:
  title: Tic Tac Toe
  description: |
    This API allows writing down marks on a Tic Tac Toe board
    and requesting the state of the board or of individual squares.
  version: 1.0.0
paths:
  # Whole board operations
  /board:
    get:
      summary: Get the whole board
      description: Retrieves the current state of the board and the winner.
      responses:
        "200":
          description: "OK"
          content:
            ...
```
这个文件叫做`tictactoe.yaml`，第一行的`openapi: 3.1.0`说明这个文件使用的是OAS 3.1.0来书写的。如果你学过`Python`,这行的作用就相当于说明这个文件是使用python2还是使用python3语法进行书写的(众所周知,python2和3的语法是不兼容的)。  

第二行的`info`则对文档进行了概括，包括文档的**标题**(`info`下的`title`字段)，文档的**总体描述**(`info`下的`description`字段),而第7行的version则说明了这份文档的版本。

接下来的path字段定义了一个`/board`接口，并且文档详细描述了，当我们使用`get`方法访问这个接口时，可能产生的响应以及响应的内容格式(content字段)。  

细心观察你会发现，大部分字段里都有`description`字段，它用于描述对应的字段，在**使用工具将文档生成为代码或者接口文档时**，这些描述可能会变为对应的注释或者文档上的说明文字。
关于`tictactoe.yaml`的完整代码可以在[这里](https://oai.github.io/Documentation/examples/tictactoe.yaml)找到.
<br>
<br>

## 总结
这一讲我们介绍了如何定义Endpoints以及它的基本属性。关于这些对象完整的属性列表你可以在[这里](https://spec.openapis.org/oas/v3.1.0)找到。下一讲我们将具体介绍`content`字段。



**我是赵不贪,欢迎关注我的微信公众号:阿贪爱学习**