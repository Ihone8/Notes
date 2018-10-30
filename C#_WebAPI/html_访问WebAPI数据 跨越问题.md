* 移动端访问远程服务时，建议使用 WebAPI

* 用不同浏览器访问WebAPI时返回的文本格式是不同的，Chrome Firefox将在浏览器中以XML形式显示此列表，IE浏览器将获得Json格式的数据，区别的原因在于不同浏览器的请求头不同，分别为：application/XML和application/json

* 客户端发送Get请求访问WebAPI时，框架会查找以Get开头的方法进行匹配，当客户端发送Post请求时，框架会查找以Post开头的方法进行匹配

* 跨域访问WebAPI中的方法时，客户端使用ajax访问时如何返回正确的json数据，解决方法：在Web.config的子节点配置如下：具体修改看注释-->

  ```c#
  <system.webServer>
  <httpProtocol>
  <!--跨域配置开始-->
  <customHeaders>
  <add name="Access-Control-Allow-Origin" value="*" />
  <!--支持全域名访问，不安全，部署后需要固定限制为客户端网址-->
  <add name="Access-Control-Allow-Methods" value="GET, POST, PUT, DELETE, OPTIONS" />
  <!--支持的http 动作-->
  <add name="Access-Control-Allow-Headers" value="Content-Type,X-Requested-With,token" />
  <!--响应头 请按照自己需求添加 这里新加了token这个headers-->
  <add name="Access-Control-Request-Methods" value="GET, POST, PUT, DELETE, OPTIONS" />
  <!--允许请求的http 动作-->
  </customHeaders>
  <!--跨域配置结束-->
  </httpProtocol>
  </system.webServer>
  ```



