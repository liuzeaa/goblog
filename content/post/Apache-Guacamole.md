---
layout: guacamole
title: Apache Guacamole
date: 2020-08-13 08:00:08
tags: Guacamole
keywords: Guacamole
categories: Guacamole
---
# Apache Guacamole
<!--more-->
## 接口

### 获取token

**Url**：http://192.168.2.231:8080/guacamole/api/tokens  
**Method**：POST  
**Content-Type**：application/x-www-form-urlencoded  
**FormData**：
- username:admin
- password:admin  

**Response**:
```json
{
    "authToken": "DB8DCC2E098BE5ED9B200367C1D11BCE9FB6615096B5836BE33F328E8F5C916D",
    "username": "admin",
    "dataSource": "default",
    "availableDataSources": [
        "quickconnect",
        "default"
    ]
}
```

### 获取identifier

**Url**：http://192.168.2.231:8080/guacamole/api/session/ext/quickconnect/create?token=`7579DC8CC7DCD2682A2D66C465EA4FF5B011767F1D876F5F9AC29E5BF9106BBE`  
**Method**：POST  
**Content-Type**：application/x-www-form-urlencoded  
**FormData**：
- uri:ssh://root:vagrant@192.168.2.91:22  

**Response**:
```json
{
    "identifier": "0"
}
```

## 注意事项

- 后端需要将“获取token接口”和“获取identifier接口”作为一个接口返回给前端，将 `token` 和 `identifier` 一并返回
- 获取token接口获取的 `token`，默认超时时间为1小时，后端需要将该token缓存，避免太频繁的生成token。

## 参考

- [第15章临时连接](http://guacamole.apache.org/doc/gug/adhoc-connections.html)
