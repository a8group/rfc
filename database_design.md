# 数据库表设计规范建议



## 1. 命名规范

总命名规范：**小写字母或数字，字母开头，多个单词以下划线分开**

正则：

```
[a-z][a-z0-9_]*
```

数据库名、表名、字段名只能由小写字母、数字和下滑线“_“组成，多个单词使用下划线分隔。



数据库名：大部分情况下可以使用服务名称

表名： 大部分情况下可以使用资源名称单数，如user：用户表、role：角色等。

关联表名：以"relation_“ 为前缀，加关联的资源表名，如：relation_user_role 用户和角色关联表



字段：

 bool字段：使用is_形容词、is_动词过去式、can_动词，比如：is_verified是否验证，can_buy 是否能购买等。

时间值为：linux时间戳

创建时间：creation_time
更新时间：update_time

删除时间：deletion_time

是否删除：is_deleted



索引：

​     唯一索引：uniq\_字段1\_字段2，如：uniq_type_username

​     普通索引：idx\_字段1\_字段2，如：idx_type_username



## 2. 字段类型

bool字段使用 tinyint(1)，0为false，1为true。



## 3. 索引

每个表必须要创建主键