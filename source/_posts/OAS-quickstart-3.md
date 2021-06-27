---
title: OpenAPI Specification 快速入门（三）
date: 2021-06-27 22:11:46
tags: OpenAPI 快速入门
---

如果你英语不错，请阅读官方的[The OpenAPI Specification Explained](https://oai.github.io/Documentation/specification.html)系列文章

上一讲我们介绍了OAS中的EndPoints，这节我们来学习一下`content`字段和它的那些“子子孙孙”们。  

<br>

## The content Field
`content`字段可以同时出现在[Response Object](https://spec.openapis.org/oas/v3.1.0#responseObject)和[Request Body Objects](https://spec.openapis.org/oas/v3.1.0#requestBodyObject)中，`Response Object`我们在上一讲已经讲过，而`content`字段允许我们定义response和request的格式  
```yaml
content: 
  application/json:
    ...
  text/html:
    ...
  text/*:
    ...
```
`content`字段下面紧跟请求或响应的格式，其格式就像HTTP请求中的Content-Type一样。  
<br>

## The Schema Object
有了URL,有了HTTP method,也有了Content-Type,还缺什么呢?看看下面这个例子:
```yaml
content:
  application/json:
    schema:
      type: integer # 类型为integer
      minimum: 1    # 最小值为1
      maximum: 100  # 最大值为100
```
`schema`中的`type`字段定义了这次请求的类型,它可以是`number`, `string`, `boolean`, `array`和`object`,根据所选的类型，还有许多其他的字段可以进一步的指定数据的格式。  
例如，对于`string`类型，可以用`minLength`和`maxLength`来限制字符串的长度。同样，`integer`类型、接受`minimum`和`maximum`值。无论是哪种类型，如果数据的选项数量限制在某个集合内，都可以用enum数组来指定。所有这些属性都列在[Schema Object](https://spec.openapis.org/oas/v3.1.0#schemaObject)规范中。

比如下面这个例子,字符串只能有三种值`Alice`,`Bob`和`Carl`:
```yaml
content:
  application/json:
    schema:
      type: string # 类型为string
      enum:  # 值只能为enum列出的值
      - Alice 
      - Bob
      - Carl
```

如果请求类型是一个数组，那么它必须要有一个`item`字段，用来标识数组元素的类型。此外，数组长度也可以使用`minItems`和`maxItems`来限制。
```yaml
content:
  application/json:
    schema:
      type: array  # 请求类型为数组
      minItems: 1  # 最少包含一个元素
      maxItems: 10 # 最多包含十个元素
      items:
        type: integer # 数组的元素为整数类型
```

最后，`object`类型必须有一个`properties`列出`object`的属性。这允许根据需要构建复杂的数据类型。  
下面是一个具有两个字段的对象的示例：一个productName字符串和一个productPrice数字：
```yaml
content:
  application/json:
    schema:
      type: object     # 请求对象为object类型
      properties:      # object所具有的属性
        productName:   # 属性叫productName
          type: string # productName的类型为string
        productPrice:  # 第二个字段名叫productPrice
          type: number # 它是number类型的
```

## 示例
让我们复习一下本章所学的知识，一起看一下下面这个例子。
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
            application/json: # 响应体格式
              schema:
                type: object # 响应体是个object
                properties:  # 它的属性有:
                    winner:  # winner,类型为string
                      type: string
                      enum: [".", "X", "O"] # winner的值只能为. X 或 O中的一个
                      description: |
                        Winner of the game. `.` means nobody has won yet.
                    board: # board属性
                      type: array # 类型为数组
                      maxItems: 3 # 最大3个元素
                      minItems: 3 # 最小3个元素
                      items:
                          type: array # 数组的元素依然是数组
                          maxItems: 3 # 最大三个元素
                          minItems: 3 # 最小三个元素
                          items:
                            type: string # 这个数组的元素为string
                            enum: [".", "X", "O"] # 值只能为. X或O
                            description: |
                              Possible values for a board square.
                              `.` means empty square.
  ...
```
完整的示例可以在[这里](https://oai.github.io/Documentation/examples/tictactoe.yaml)找到，上面这个文件描述的是`井字棋游戏`的API接口，`board`字段是个大小为3的数组，而每个元素又是一个大小为3的数组，正好组成了一个`3x3`的棋盘，而其中每个格子的值只能为`.`,`X`或`O`中的任意一个。  

<br>

## 总结
本章讲述了请求/响应的类型说明，现在我们已经可以详细的制定请求啦，但要**注意区分**本章描述的对象和`Parameter`对象，我们将在下章描述`Parameter Object`。  
  
<br>  
  
**我是赵不贪,欢迎关注我的微信公众号:阿贪爱学习**