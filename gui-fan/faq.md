## FAQ

**1、如何实现接口跨域且携带 Cookie （或自定义头）？**

客户端设置：

```js
axios.defaults.withCredentials = true;
```

服务端设置：

```js
// web.config
<httpProtocol>
    <customHeaders>
        <add name="Access-Control-Allow-Origin" value="http://121.40.115.179:11111" />
        <add name="Access-Control-Allow-Credentials" value="true"/>
        <add name="Access-Control-Allow-Methods" value="POST, DELETE, PUT, GET, OPTIONS" />
        <add name="Access-Control-Allow-Headers" value="Content-Type, Content-Length, Accept, Authorization, X-Requested-With, token" />
        <add name="Access-Control-Expose-Headers" value="token" />
        <add name="Access-Control-Max-Age" value="360000"/>
        <add name="Cache-Control" value="no-cache" />
    </customHeaders>
</httpProtocol>
```

**2、如何实现接口授权机制？**

可通过设置 Cookie 或 自定义请求头实现。

**3、如何实现前端超时处理机制？**

> “超时”处理
>
> * 请求超过指定的时间；终止请求
> * 认证、授权失败，返回定义的 Status（{status: 401}）；跳转到登录页
> * 访问间隔超过指定的时间，返回定义的 Status（{status: 502}）；跳转到登录页

超时控制分为 请求超时 和 访问超时，对于请求超时，客户端设置：

```js
axios.defaults.timeout= 1000;
```

当指定请求超时前的毫秒数，如果请求时间超过超时时间，就终止请求。

然而，访问超时可以通过设置 session 过期时间达到目的，创建的 session\_id 通过设置 Cookie 在请求过程中携带，避免使用请求主体携带。

**4、如何实现前端异常处理机制？**

> “异常”处理
>
> * 跨域请求异常；需浏览器设置的，跳转到异常链接
> * 接口返回值异常，比如有删减返回字段；跳转到异常页
> * 请求发出，但是没有收到任何响应，比如接口服务挂了；跳转到异常页
> * 访问路径没找到；跳转到 404 页
> * 访问接口内部错误；返回定义的 Status（{status: 500}）；跳转到 500 页

全局异常处理：封装一个异步请求的方法：

```js
getJSON(url, options);
```

局部异常处理：在每个请求后捕获并处理异常：

```js
axios
.get('/user/123456')
.catch((error) => {
    if (error.response) {
        // 请求被执行，服务端以状态码来响应
        console.log(error.response.status);
    } else if (error.request) {
        // 请求发出，但是没有收到任何响应
        console.log(error.request);
    } else {
        // 在设置请求时触发错误，发生了一些问题
        console.log('Error', error.message);
    }
    console.log(error.config);
});
```

**5、如何实现配置化（非硬编码）的消息提示机制？**

服务端返回业务返回码 code：

```js
{
    "data": {},
    "status": 200,
    "code": "001"
}
```

客户端设置：

```js
// code.json
{
    "code": {
        "001": "服务器端代码错误 [0101001]"
    }
}
```

通过 code - 提示文本 的映射关系将消息文本转向前端配置。这样，后续如需修改提示文本内容，不需要重新发布后端代码了，直接前端修改发布即可。

如果返回格式中有 message 字段，优先使用 message，否则使用 code 字段匹配前端字段显示。

