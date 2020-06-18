# RESTful API Guidelines

## 1. 摘要




## 2. API Path



```http
POST https://api.fake.com/v1/users
```



```http
201 Created
```



## 3. 响应



### 3.1 正常响应

```json
{
  "user": {
    "id": 1
  }
}
```



### 3.2 错误响应

```json
{
  "error": {
    "code": "invalid_parameter",
    "message": "The request is missing a required parameter"
  }
}
```





