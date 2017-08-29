## 范例参考

> 基本的接口返回格式

```js
{
    "data": {},        // 数据内容
    "status": 200,     // 接口状态码
    "code": "001",       // 业务返回码
    "message": ""      // 消息文本（有业务返回码时，此字段可不显示）
}
```

> 各种状态下，返回带分页的列表数据结构

状态一：无数据

```js
//  /api/user-list
{
    "data": {
        "list": [],
        "total": 0,        // 数据总（条）数
        "current": 1,      // 当前页数
        "page_size": 10    // 每页条数
    },
    "status": 200,
    "code": "001",
    "message": ""
}
```

状态二：有一条数据

```js
{
    "data": {
        "list": [{
            "id": "985123a0-7e4f-11e7-9022-fb7190c856e4",
            ...
        }],
        "total": 1,
        "current": 1,
        "page_size": 10
    },
    "status": 200,
    "code": "001",
    "message": ""
}
```

状态三：有多条数据

```js
{
    "data": {
        "list": [{
            "id": "985123a0-7e4f-11e7-9022-fb7190c856e4",
            ...
        },{
            "id": "df7cca36-3d7a-40f4-8f06-ae03cc22f045",
            ...    
        }],
        "total": 20,
        "current": 2,
        "page_size": 10
    },
    "status": 200,
    "code": "001",
    "message": ""
}
```

> 返回带统计信息的列表数据结构

状态一：无数据，无统计

```js
{
    "data": {
        "list": []
    },
    "statistics": {
        "total_price": 0
    },
    "status": 200,
    "code": "001",
    "message": ""
}
```

状态二：有数据，有统计

```js
{
    "data": {
        "list": [{
            "id": "985123a0-7e4f-11e7-9022-fb7190c856e4",
            ...
        },{
            "id": "df7cca36-3d7a-40f4-8f06-ae03cc22f045",
            ...    
        }]

    },
    "statistics": {
       "total_price": 2
    },
    "status": 200,
    "code": "001",
    "message": ""
}
```

> 返回单个数据结构

```js
{
    "data": {
        "id": "985123a0-7e4f-11e7-9022-fb7190c856e4",
        ... 
    },
    "status": 200,
    "code": "001",
    "message": ""
}
```



