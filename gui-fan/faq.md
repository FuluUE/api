## FAQ

> 如何实现接口跨域且携带 Cookie （或自定义头）？

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

> 如何实现接口授权机制？

可通过设置 Cookie 或 自定义请求头实现。

> 如何实现超时机制？

超时控制分为 请求超时 和 访问超时，对于请求超时，客户端设置：

```js
axios.defaults.timeout= 1000;
```

当指定请求超时前的毫秒数，如果请求时间超过超时时间，就终止请求。

然而，访问超时可以通过设置 session 过期时间达到目的，创建的 session\_id 通过设置 Cookie 在请求过程中携带，避免使用请求主体携带。

> 如何实现配置化（非硬编码）的消息提示机制？

服务端返回业务返回码 error\_code：

```js
{
    "data": {},
    "status": 200,
    "error_code": "001"
}
```

客户端设置：

```js
{
    "status": {
        "001": "服务器端代码错误 [0101001]"
    }
}
```

通过 Code - 提示文本 的映射关系将消息文本转向前端配置。这样，后续如需修改提示文本内容，不需要重新发布后端代码了，直接前端修改发布即可。

