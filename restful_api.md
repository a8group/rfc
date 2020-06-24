

# RESTful API 规范建议

## 1. 摘要

旨在提供一致的API设计规范，以降低理解与使用成本



## 2. 一致原则

### 2.1. 命名

总命名规范：**小写字母或数字，字母开头，多个单词以下划线分开**

正则：

```
[a-z][a-z0-9_]*
```

#### 2.1.1. 命名例子

##### 2.1.1.1. URL路径

```http
/api/v1/orders/a100/sub_orders/1
```

##### 2.1.1.2. URL Query参数

```http
/api/v1/orders?page=1&per_page=10
```

##### 2.1.1.3. json字段Key

```jso
{
	"order": {
		"id": "a100",
		"sub_orders": [
			{
				"id": 1,
				"price": 100
			}
		]
	}
}
```



### 2.2. 支持方法

动作必须使用标准的HTTP methods

| Method  | 描述                                 | 是否幂等 |
| ------- | ------------------------------------ | -------- |
| GET     | 返回单个资源或多个资源               | True     |
| PUT     | 更新整个资源，或者创建一个命名的资源 | True     |
| DELETE  | 删除资源                             | True     |
| POST    | 基于提交的内容创建一个资源           | False    |
| HEAD    | 返回资源的metadata信息               | True     |
| PATCH   | 更新资源的部分字段                   | False    |
| OPTIONS | 获取请求信息                         | True     |



### 2.3. 请求

#### 2.3.1. 请求头部

更多的头部参照 [RFC7231][rfc-7231], 如下是常用的头部

| Header        | Type         | Description                                    |
| ------------- | ------------ | ---------------------------------------------- |
| Authorization | String       | 授权头部信息                                   |
| Content-Type  | Content type | 默认支持：application/json，不建议使用其它格式 |

#### 2.3.2. 请求体

请求体格式：json

```json
{
  "id": "a100",
  "sub_orders": [
    {
      "id": 1,
      "price": 100
    }
  ]
}
```



### 2.4. 响应

#### 2.4.1. 响应头部

更多的头部参照 [RFC7231][rfc-7231], 如下是常用的头部

| Response Header | Type         | Description                                    |
| --------------- | ------------ | ---------------------------------------------- |
| Content-Type    | Content type | 默认支持：application/json，不建议使用其它格式 |

#### 2.4.2. 成功响应体

响应格式：json

```json
{
  "id": "a100",
  "sub_orders": [
    {
      "id": 1,
      "price": 100
    }
  ]
}
```

#### 2.4.3. 错误响应体

响应格式：json

```json
{
  "error": {
    "code": "invalid_parameter",
    "message": "The request is missing a required parameter",
    "details": {
        "missed_parameters": [
          "username"
        ]
    }
  }
}
```

字段解析

| 字段          | 类型   | 是否必返回 | 规范                                               | 描述                                                         |
| ------------- | ------ | ---------- | -------------------------------------------------- | ------------------------------------------------------------ |
| error.code    | string | 是         | 小写字母或数字，单词以下划线分开                   | 唯一代表一类错误，调用方可以依据此错误code做相应的业务处理，比如：未登录就会跳转至登录页面等 |
| error.message | string | 是         | 一句话                                             | 易懂的错误信息，供调用方或用户查看                           |
| error.details | map    | 否         | map的key命名规范：小写字母或数字，单词以下划线分开 | 具体的错误详情，有些错误需要返回特定的信息，以便调用方处理   |

#### 2.4.4. HTTP状态码

使用标准的状态码，2xx表示成功、4xx客户端错误、5xx服务器端错误，调用方判断状态码为2xx即可认为请求成功了。

